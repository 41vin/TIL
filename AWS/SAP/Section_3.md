# IAM - What should you know by now
- Users: long term credentials
- Groups
- Roles: short-term, credentials, uses STS
  - EC2 instance Roles: uses the EC2 metadata service. One role at a time per instance
  - Service Roles: API Gateway, CodeDeploy, etc...
  - Cross Account Roles
- Policies
  - AWS Managed
  - Customer Managed
  - Inline Policies
- Resource Based Policies (S3 Bucket, SQS queue, etc...)
<br><br><br>

# IAM Policies Deep Dive
- Anatomy of a policy: JSON doc with Effect, Action, Resource, Conditions, Policy Variables
- Explicit DENY has precedence over ALLOW
- Best practice: use least privilege for maximum security
  - Access Advisor: See permissions granted and when last accssed
  - Access Analyzer: Analyze resources that are shared with external entity

<br><br><br>
# IAM AWS Managed Policies
1. AdministratorAccess
2. PowerUserAccess

<br><br><br>

# IAM Policies Conditions
- Operators:
  - String (StringEquals, StringNotEquals, ...)
  - Numeric (NumericEquals, NumericNotEquals, ...)
  - Date (DateEquals, DateNotEquals, DateLessThan ...)
  - Boolean (Bool)
  - (Not) IpAddress
  - ArnEquals, ArnLike
  - Null: "Condition": {"Null":{"aws:TokenIssueTime": "true}}

&rarr; 연산자들을 모두 암기할 필요 없음

<br><br><br>

# IAM Policies Variables and Tags
- Example:${aws:username}
- AWS Specific:
  - aws:CurrentTime, aws:TokenIssueTime etc...
- Service Specific:
  - s3:prefix, s3:max-keys, etc...
- Tag Based:
  - iam:ResourceTag/key-name, aws:PrincipalTag/key-name ...

<br><br><br>
# IAM Roles vs Resource Based Policies
- Attach a policy to a resource (example: S3) versus attaching of a using role as a proxy
<br>
- When you assume a role (user,application or service), you give up to your original permissions and take the permissions assigned to the role
- When using a resource-based policy, the principal doesn't have to give up any permissions
- Example: User in account A needs to scan a DynamoDB table in Account A and dump it in an S3 bucket in Account B


<br><br><br>

# IAM Permission Boundaries
- IAM Permission Boundaries are supported for users and roles(not groups)
- Advanced feature to use a managed policy to set the maximum permissions an IAM entity can get
- Can ve used in combinations of AWS Organizations SCP

    Usecases
    - Delegate reponsibilities to non administrators within their permission boundareis, for example create new IAM users
    - Allow developers to self-assign policies and manage their own permissions, while making sure they can't 'escalate' their privileges (=make themselves admin)
    - Useful to restrict one specific user(instead of a whole account using Organizations & SCP)

<br><br><br>
# IAM Access Analyzer
- Find out which resources are shared externally
  - S3 Buckets
  - IAM Roles
  - KMS Keys
  - Lambda Functions and Layers
  - SQS queues
  - Secrets manager Secrets
- Define **Zone of Trust** = AWS Account or AWS Organization
- Access outside zone of trusts &rarr; findings

<br><br><br>
# IAM Access Analyzer
- IAM Access Analyzer Policy Validation
  - Validates your policy iagainst IAM policy grammar and best practices
  - General warnings, security warnings, errors, suggestions
  - Provides actionable recommendations

- IAM Access Analyzer Policy Generation
  - Generates IAM policy based on access activity
  - CloudTrail logs is reviewed to generate the policy with the **fine-grained permissions and the appropriate Actions and Services**
  - Reviews CloudTrail logs for up to 90 days

<br><br><br>
# Using STS to Assume a Role
- Define an IAM Role within your account or cross-account
- Define which principals can access the IAM Role
- Use AWS STS(Security Token Service) to retrieve credentials and impersonate the IAM Role you have access to (**AssumeRole API**)
- Temporary credentials can be valid between 15 minutes to 12 hours\
  <br><br><br>
# Assuming a Role with STS
- Provide access for an IAM user in one AWS account that you own to access resources in another account that you own
- Provide access to IAM users in AWS accounts owned by third parties
- Provide access for services offered by AWS to AWS resources
- Provide access for externally authenticated users(identify federation)

- Ability to revoke activity sessions and credentials for a role(by adding a policy using a time statement - AWSRevokeOlderSessions)

**When you assume a role(user,application or service), you give up your original permissions and take the permissions assigned to the role**

<br><br><br>

# Providing Access to an IAM User in Your or Another AWS Account That You Own

- You can grant your IAM users permission to switch to roles within your AWS account or to roles defined in other AWS accounts **that you own**.
- ___Benefits___
  - You must explicitly grant your users permission to assume the role
  - Your users must actively switch to the role using the AWS Management Console or assume the role using AWS CLI or AWS API
  - You can add multi-factor authentication(MFA) protection to the role so that only users who sign in with an MFA device can assume the role
  - Least privilege + auditing using CloudTrail

<br><br><br>

- Cross account access with STS
<그림 첨부하기>

<br><br><br>

# Providing Access to AWS Accounts Owned by Third Parties
- Zone of Trust = accounts, organizations that you own
- Outside Zone of Trust = 3rd parties
- Use IAM Access Analyzer to find out which resources are exposed
- For granting access to a 3rd party:
  - the 3rd party AWS account ID
  - An **External** ID
  - Define permissions in the IAM policy

<br><br><br>

# Session Tags in STS
  
- Tags that you pass when you assume an IAM Role or federate user in STS
- aws:PrincipalTag Condition
  - Compares the tags attached to the principal making the request with the tag you specified the policy

<br><br><br>

# STS Important APIs

1. **AssumeRole**: access a role within your account or cross-account
2. **AssumeRoleWithSAML**: return credentials for users logged with SAML
3. **AssumeRoleWithWebIdentity**: return creds for users logged with an IdP
    - AWS recommends using Cognito instead
4. **GetSessionToken**: for MFA, from a user or AWS account root user
5. **GetFederationToken**: obtain temporary creds for a federated user, usually a proxy app that will give the creds to a distributed app inside a corporate network


<br><br><br>

# Identity Federation in AWS

- Give users outside of AWS permissions to access AWS resources in your account
- **You don't need to create IAM Users(user management is outside AWS)**
- Use cases:
  - A corporate has its own idetity system (e.g., Active Directory)
  - Web/Mobile application that needs access to AWS resources

<br>

- Identity Federation can have many flavors:
  - SAML 2.0
  - Custom Identity Broker
  - Web Identity Federation With(out) Amazon Cognito
  - Single Sign-On (SSO)

<br><br><br>

# SAML 2.0 Federation

- Security Assertion Markup Language (SAML 2.0
- Open standard used by many identity providers(e.g., ADFS)
  - Supports integration with Microsoft Active Directory Federations Services(ADFS)
  - Or any SAML 2.0-compatible idPs with AWS
- Access to AWS Console, AWS CLI, or AWS API using temporary credentials
  - No need to create IAM Users for each of your employees
  - Need to setup a trust between AWS IAM and SAML 2.0 Identity Provider (both ways)

- Under-the-hood: Uses the STS API **AssumeRoleWithSAML**
- SAML 2.0 Federation is the "old way", **Amazon Single Sign-On (AWS SSO)** Federation is the new managed and simpler way

<br><br><br>

# Custom Identity Broker Application

- Use only Identity Provider is NOT compatible with SAML 2.0
- The Identity Broker Authenticates users & requests temporary credentials from AWS
- **The Identity Broker must determine the appropriate IAM Role**
- Uses the STS API **AssumeRole** or **GetFederationToken**

<br><br><br>

<div style="display: flex;">
  <div style="flex-basis: 50%;">
  <h3>Web Identity Federation - With Cognito</h3>
    <ul>
    <li>Preferred over for Web Identity Federation</li>
        <ul>
        <li> Create IAM Roles using Cognito with the least privileged needed</li>
        <li> Build trust between the OIDC IdP and AWS</li>
        </ul>
    <li>Cognito benefits:</li>   
        <ul>
        <li>Supports anonymous users</li>
        <li>Supports MFA</li>
        <li>Data Synchronization</li>
        </ul>
    <li>Cognito replaces a Token Vending Machine(TVM)</li>
  </div>
  
  <div style="flex-basis: 50%;">
  <h3>Web Identity Federation - Without Cognito</h3>
    <ul>
      <li>Not recommended by AWS</li>
        <ul>
          <li>use Cognito instead</li>
        </ul>  
  </div>
</div>

<br><br><br>

# Web Identity Federation - IAM Policy

- After being authenticated with Web Identity Federation, you can Identity Federaiton, you can identify the user with an IAM policy variagle
- Examples:
  - cognito-identity.amazonaws.com:sub
  - www.amazon.com:user_id
  - graph.facebook.com:id
  - accounts.google.com:sub

<br><br><br>

# What is Microsoft Active Directory (AD)?

- Found on any Windows Server with AD Domain Services
- Database of **objects**: User Accounts, Computers, Printers, File Shares, Security Groups
- Centralized security management, create account, assign permissions
- Objects are organized in **trees**
- A group of trees is a **forest**

<br><br><br>

# What is ADFS (AD Federation Services)?
  
- ADFS provides Single Sign-On across applications
- SAML across 3rd party: AWS Console, Dropbox, Office365 etc...

<br><br><br>

# AWS Directory Services

- AWS Managed Microsoft AD
  - Create your own AD in AWS, manage users locally, supports MFA
  - Establish "trust" connections with your on-premises AD
- AD Connector
  - Directory Gateway(proxy) to redirect to on-premises AD, supports MFA
  - Users are managed on the on-premise AD
- Simple AD
  - AD-compatible managed directory on AWS
  - Cannot be joined with on-premise AD

<br><br><br>

# AWS Directory Services 
  ## 1. AWS Managed Microsoft AD

- Managed Servie: Microsoft AD in your AWSVPC
- EC2 Windows Instance:
  - EC2 Windows instances can join the domain and run traditional AD applications (sharepoint,etc)
  - Seamlessly Domain Join Amacon EC2 Instances from Multiple Accounts & VPCs
- Integrations:
  - RDS for SQL Server; AWS Workspaces, Quicksight ...
  - AWS SSO to provide access to 3rd party applications
- Standalone repository in AWS or joined to on-premise AD
- Multi AZ deployment of AD in 2 AZ, # of DC(Domain Controllers) can be increased for scaling
- Automated backups
- Automated Multi-Region replication of your directory


### Connect to on-premises AD

- Ability to connect your on-premises Active Directory to AWS Managed MicrosoftAD
- Must establish a Directory Connect(DX) or VPN connection
- Can setup three kinds of forest trusts:
  - **One-way trust**:<br>
  AWS => on-premises 
  - **One-way trust**:<br>
  on-premises => AWS
  - **Two-way forest trust**:<br>
  AWS <-> on-premises
- Forest trust is different than synchronization
  
### Solution Architecture: Active Directory Replication
- You may want to create a replica of your AD on EC2 in the cloud to minimize latency of in case DX or VPN goe sdown
- Establish trust between the AWS Managed Microsoft AD and EC2

<br><br>

## 2. AD Connector

- **AD Connector** is a directory gateway to redirect directory requests to your on-premises Microsoft Active Directory
- No caching capability
- Manage users soley on-premises, no possibility of setting up a trust
- VPN or Direct Connect
- Doesn't work with SQL Server, doesn't do seamless joining, can't share directory


<br><br>

## 3. Simple AD

- **Simple AD** is an inexpensive Active Directory-compatible service with the common directory features.
- Supports joining EC2 instances, manage users and groups
- Does not support MFA, RDS SQL server, AWS SSO
- Small: 500 users, large:5000 users
- Powered by Samba 4, compatible with Microsoft AD

<br><br><br>

# AWS Organizations - OrganizationAccountAccessRole

- IAM role which grants full administraor permissions in the Member account to the Management account
- Used to perform admin tasks in the Member accounts(e.g., creating IAM users)
- Could be assumed by IAM users in the Management account
- Automatically added to all new Member accounts created with AWS Organizations
- Must be created manually if you invite an existing Member account
<br><br><br>

# Multi Account Strategies

- Create accounts per **department**, per **cost center**, per **dev/test/prod**, based on **regulatory restrictions**(using SCP), for **better resource isolation** (ex:VPC). to have **separate per-account service limits**, isolated account for **logging**
- Multi Account vs. One Account Multi VPC
- Use tagging standards for billing purposes
- Enable CloudTrail on all accounts, send logs to central S3 account
- Send CloudWatch Logs to central logging account
- Strategy to create an account for security
<br><br><br>

# AWS Organization - Feature Modes
- Consolidated billing features:
  - Consolidated Billing across all accounts - single payment method
  - Pricing benefits from aggregated usage (volume discount for EC2, S3 ...)
- All Features(Default):
  - Includes consolidated billing features, SCP
  - Invited accounts must approve enabling all features
  - Ability to apply an SCP to prevent member accounts from leaving the org
  - Can't switch back to Consolidated Billing Features only
<br><br><br>

# AWS Organizations - Reserved Instances
- For billing purposes, the consolidated billing feature of AWS Organizations treats all the accounts in the organizations as one account.
- This means that **all accounts** in the organization can receive the hourly cost benefit of Reserved Instances that are purchased **by any other account.**
- **THe payer account (Management account) of an organization** can turn off Reserved Instances(RI) discount and Savings Plans discount sharing for any accoutns in that organization, including payer account
- This means that RIs and Savings Plans discounts aren't shared between any accounts that have sharing turned off
- To share an RI or Savings Plans discount with an account, both accounts must have sharing turned on
<br><br><br>

# AWS Organizations - Moving Accounts
1. Remove the member account from the AWS Organizations
2. Send an invite to the member account from the AWS Organization
3. Accept the invite to the new Organizations from the member account
<br><br><br>

# Service Control Policies(SCP)
- Define allowlist or blocklist IAM actions
- Applied at the **OU** or **Account** level
- Does not apply to the Management Account
- SCP is applied to all the **User and Roles** in the account, including Root user
- The SCP does not affect Service-linked roles
  - Service-linked roles enable other AWS services to integrate with AWS Organizations and can't be restricted by SCPs
- SCP must have an explicit Allow(does not allow anything by default)
- Use Cases:
  - Restrict access to certain services(for example: can't use EMR)
  - Enforce PCI compliance by explicitly disabling services
<br><br><br>

# Restricting Tags with IAM Policies
- You can restrict specific Tags on AWS resources
- Using the **aws:TagKeys** Condition Key
  - Validate the Tag Keys attached to a resource against the Tag Keys in the IAM Policy
- Example: allow IAM users to create EBS Volumes only if it has the "Env" and "CostCenter" Tags
- Use either **ForAllValues**(must have all keys) or **ForAnyValue**(must have any of these keys at a minimum)
<br><br><br>

# Using SCP to Restrict Creating Resources without appropriate Tags
- Prevent IAM Users/Roles in the affected Member accounts from creating resources if they don't have a specific Tags
- Example: restrict launching an EC2 instance if it doesn't have the "Project" and "CostCenter" Tags
<br><br><br>

# AWS Organizations - Tag Policies
- Helps you standardize tags across resources in an AWS Organizations
- Ensure consistent tags, audit tagged resources, maintain proper resources categorization, ...
- You define Tag keys and their allowed values
- Helps with AWS Cost Allocation Tags and Attribute-based Access Control
- Prevent any non-compliant tagging operations on specified services and resources
- Generate a report that lists all tagged/non-compliant resources
- Use Amazon EventBridge to monitor non-compliant tags
<br><br><br>

# AWS Organizations - AI Services Opt-out Policies
- Certain AWS AI services may use your content for continuous improvement of Amazon AI/ML services
- Example: Amazon Lex, Amazon Comprehend, Amazon Polly, ...

- You can opt-out of having your content stored or used by AWS AI services
- Create an Opt-out Policy that enforces this setting across all Member accounts and AWS Regions
- You can opt-out all AI services or selected services
- Can be attached to Organization Root, specific OU, or individual Member account
<br><br><br>

# AWS Organizations - Backup Policies
- AWS Backup enables you to create Backup Plans that define how to backup your AWS resources
- JSON documents that define Backup Plans across an AWS Organization
- Gives you granular control over backing up your resources(e.g., backup frequency, time window, backup region, ...)
- Can be attached to Organization Root, specific OU, or individual Member account
- **Immutable** Backup Plans appear in Member accounts (view ONLY)
<br><br><br>

# AWS IAM Identity Center(successor to AWS Single Sign-On)
- One login (single sign-on) for all your
  - **AWs accounts in AWS Organizations**
  - Business cloud applications (e.g., Salesforce, Box, Microsoft 365, ...)
  - SAML2.0-enabled applications
  - EC2 Windows Instances

- Identity providers
  - Built-in identity store in IAM Identity Center
  - 3rd party: Active Directory(AD), OneLogin, Okta ...
