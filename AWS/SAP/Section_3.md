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
