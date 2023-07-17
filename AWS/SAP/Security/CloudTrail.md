# AWS CloudTrail

- **Provide governance, compliance and audit for your AWS Account**
- CloudTrail is enabled by defualt
- Get **an history of events / API calls made within your AWS Account ** by:
  - Console
  - SDK
  - CLI
  - AWS Services
- Can put logs from CloudTrail into CloudWatch Logs or S3
- **A trail can be applied to All Regions (default) or a single Region**
- If a resource is deleted in AWS, investigate CloudTrail first

# CloudTrail Events

- Management Events:
  - Operations that are performed on resources in your AWS account
  - Examples:
    - Configuring security ( IAM **AttachRolePolicy**)
    - Configuring rules for routing data (AWS CloudTrail **CreateTrail**)
    - Setting up logging (AWS CloudTrail **CreateTrail**)
  - **By default, trails are configured to log management events**
  - Can separate **Read Events** (that don't modify resources) from **Write Events** (that may modify resources)

- Data Events:
  - **By default, data events are not logged (because high volume operations)**
  - Amazon S3 object-level activity (ex:**GetObject, DeleteObject, PutObject**): can separate Read and Write Events
  - AWS Lambda function execution activity (the **Invoke** API)


# CloudTrail Insights

- Enable **CloudTrail Insights to detect unusual activity** in your account:
  - inaccurate resource provisioning
  - hitting service limits
  - Bursts of AWS IAM actions
  - Gaps in periodic maintenance activity
- CloudTrail Insights analyzes normal management events to create a baseline
- And then **continuously analyzes write events to detect unusual patterns**
  - Anomalies appear in the CloudTrail console
  - Event is sent to Amazon S3
  - An EventBridge event is generated (for automation needs)

# CloudTrail Events Retention

- Events are stored for 90 days in CloudTrail
- To keep events beyond this period, log them to S3 and use Athena

# CloudTrail - Solution Architecture: Delivery to S3

- S3 Enhancements:
  - Enable Versioning
  - MFA Delete Protection
  - S3 Lifecycle Policy (S3 IA, Glacier ..)
  - S3 Object Lock
  - SSE-S3 or SSE-KMS encryption
  - Feature to perform CloudTrail Log File Integrity validation (SHA-256 for hashing and signing)

# CloudTrail - Solution Architecture: Multi Account, Multi Region Logging

- Observations:
  - The S3 bucket policy is necessary for cross-account delivery
  - If Account A wants to access its CloudTrail files:
      - Option 1: create a cross-account role and assume the role
      - Option 2: edit the bucket policy
   
# CloudTrail - Solution Architecture: Alert for API calls 

CloudTrail &rarr; CW Logs &rarr; Metric Filters &rarr; CW Alarm &rarr; SNS

- Log filter metrics can be used to detect a high level of API happening
- Ex: Count occurrences of EC2 **TerminateInstances** API
- Ex: Count of API calls per user
- Ex: Detect high level of Denied API calls

# CloudTrail: How to react to evens the fastest?

Overall, CloudTrail may take up to 15 minutes to deliver events

1. EventBride:
   - Can be triggered for any API call in CloudTrail
   - The fastest, most reactive way
2. ClouddTrail Delivery in CloudWatch Logs:
   - Events are streamed
   - Can perform a metric filter to analyze occurrences and detect anomalies
3. CloudTrail Delivery in S3
   - Events are delivered every 5 minutes
   - Possibility of analyzing logs integrity, deliver cross account, long-term storage
