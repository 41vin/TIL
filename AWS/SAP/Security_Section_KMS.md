
# AWS KMS (Key Management Service)

- Anytime you hear "encryption" for an AWS service, it's most likely KMS
- Easy way to control access to your data, AWS manages keys for us
- Fully integrated with IAM for authorization
- Seamlessly integrated into:
  - Amazon EBS: encrypt volumes
  - Amazon S3: Server-side encryption of objects
  - Amazon Redshift: encryption of data
  - Amazon RDS: encryption of data
  - Amazon SSM: Parameter store
  - Etc...
- But you can also use the CLI/SDK
<br>
# KMS - KMS Key Types

- Symmetric (AES-256 keys)
  - First offering of KMS, single encryption key that is used to Encrypt and Decrypt
  - AWS service that are integrateed with KMS use Symmetric KMS keys
  - Necessary for envelope encryption
  - You never get access to the KMS key unencrypted (must call KMS API to use)
- Asymmetric (RSA & ECC key pairs)
  - Public (Encrypt) and Private Key(Decrypt) pair
  - Used for Encrypt/Decrypt, or Sign/Verify operations
  - The public key is downloadable, but you can't access the Private Key unencrypted
  - Use case: encryption outside of AWS by users who can't call the KMS API
<br>
# Types of KMS Keys

- Customer Managed Keys
  - Create, manage and use, can enable or disable
  - Possibility of rotation policy (new key generated every year, old key preserved)
  - Can add a Key Policy (resource policy) & audit in CloudTrail
  - Leverage for envelope encryption
 - AWS Managed Keys
   - Used by AWS service (aws/s3, aws/ebs, aws/redshift)
   - Managed by AWS (automatically rotated every 1 year)
   - View Key Policy & audit in CloudTrail
 - AWS Owned Keys
   - Created and managed by AWS, use by some AWS services to protect your resources
   - Used in multiple AWS accounts, but they are not in your AWS account
   - You can't view, use, track, or audit



|KMS Key|Customer Managed Key|AWS Managed Key|AWS Owned Key|
|---|---|---|----|
|Can view metadata?|O|O|X|
|Can manage?|O|X|X|
|Used only for my AWS account?|O|O|X|
|Automatic Rotation|Optional(every 1 year)|Required(every 1 year)|Varies|

<br>

# KMS Key Material Origin

- Identifies the source of the key material in the KMS key
- Can't be changed after creation
  <br>
- **KMS(AWS_KMS)** - default
  - AWS KMS creates and manages the key material in its own key store
- **External(EXTERNAL)**
  - You import the key material into the KMS key
  - You're responsible for securing and managing this key material outside of AWS
- **Custom Key Store (AWS_CLOUDHSM)**
  - AWS KMS creates the key material in a custom key store (CloudHSM Cluster)
<br>

# KMS Key Source - Custom Key Store(CloudHSM)

- Integrates KMS with CloudHSM cluster as a Custom Key Store
- Key materials are stored in a CloudHSM cluster that you own and manage
- The cryptographic operations are performed in the HSMs
- Use cases:
  - You need direct control over the HSMs
  - KMS keys need to be stored in a dedicated HSMs
<br>

# KMS Key Source - External

- Import your own key material into KMS key, Bring Your Own Key (BYOK)
- **You're responsible for key material's security, availability, and durability outside of AWS**
- Must be 256-bit **Symmetric** key (Asymmetric is NOT supported)
- Can't be used with Custom Key Store (CloudHSM)
- Manually rotate your KSM key (Autoamtic Key Rotation is NOT supported)
<br>

# KMS Multi-Region Keys

- A set of identical KMS keys in different AWS Regions that can be used interchangeably ( ~same KMS key in multiple Regions)
- Encrypt in one Reegion and decrypt in other Regions (No need to re-encrypt or making croos-Region API calls)
- Multi-Region keys have the same key ID, key material, automatic rotation, ...
- KMS Multi-Region are NOT global (Primary + Replicas)
- Each Multi-Region key is managed **independently**
- Only one primary key at a time, can promote replicas into their own primary
- Use cases: Disaster Recovery, Global Data Management, Active-Active Applications that span multiple Regions, ...
