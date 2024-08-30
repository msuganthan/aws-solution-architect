### Encryption in Flight (TLS/SSL)

* Data is encrypted before sending and decrypted after receiving
* TLS certificates help with encryption(HTTPS)
* Encryption in flight ensures no MITM(man in the middle attack) can happen

<img src="../images/aws-security-encryption/encryption-in-flight.png" alt="Encryption in flight">

### Server-side encryption at rest

* Data is encrypted after being received by the server
* Data is decrypted before being sent
* It is stored in an encrypted form thanks to a key(usually a data key)
* The encryption / decryption key must be managed somewhere, and serer must have access to it.

<img src="../images/aws-security-encryption/server-side-encryption.png" alt="Server side encryption">

### Client-side encryption

* Data is encrypted by the client and never decrypted by the server
* Data will be decrypted by a receiving client
* The server should not be able to decrypt the data
* Could leverage Envelope Encryption

<img src="../images/aws-security-encryption/client-side-encryption.png" alt="Client side encryption">

### KMS Overview

* Anytime you hear "encryption" for an AWS service it's most likely KMS
* AWS manages encryption keys for us
* Fully integrated with IAM for authorization
* **Easy way to control access to your data**
* Able to audit KMS key usage using **CloudTrail**
* Seamlessly integrated into most AWS service(EBS, S3, RDS, SSM...)
* **Never ever store your secrets in plaintext, especially in your code!!!**
  * KMS Key Encryption also available through API calls(SDK, CLI)
  * Encrypted secrets can be stored in the code / environment variables

#### KMS Keys types

* KMS Keys is the new name of KMS Customer Master Key
* **Symmetric(AES-256 Keys)**
  * Single encryption key that is used to Encrypt and Decrypt
  * AWS services that are integrated with KMS use Symmetric CMKs
  * You never get access to the KMS Key encrypted(must call KMS API to use)
* **Asymmetric(RSA & ECC Key pairs)**
  * Public(Encrypt) and Private Key(Decrypt) pair
  * Used for Encrypt / Decrypt or Sign / Verify operations
  * The public key is downloadable, but you can't access the Private key unencrypted
  * Use case: **encryption outside of AWS by users who can't call the KMS API**

#### AWS KMS(Key Management Service)

* Types of KMS Keys:
  * AWS Owned keys(free): SSE-S3, SSE-SQS, SSE-DDB(default key)
  * AWS Managed Key: free(aws/service-name, example: aws/rds or aws/ebs)
  * Customer managed keys created in KMS: **$1 / month**
  * Customer managed keys imported: **$1 / month**
  * + pay for API call to KMS($0.03 / 10000 calls)
  
* Automatic Key rotation:
  * AWS-managed KMS key: **automatic every 1 year**
  * Customer-managed KMS key: (must be enabled) **automatic & on-demand**
  * Imported KMS Key: **only manual rotation possible using alias**

#### Copying snapshots across region 

<img src="../images/aws-security-encryption/copying-snapshots-across-regions.png" alt="Copying Snapshots across regions">

#### KMS Key Policies

* Control access to KMS keys, similar to S3 bucket policies
* Difference: you cannot control access without them

* **Default KMS Key Policy:**
  * Created if you don't provide a specific KMS Key policy
  * Complete access to the key to the root user = entire AWS account
* **Custom KMS Key Policy:**
  * Define users, roles that can access the KMS key
  * Define who can administer the key
  * Useful for cross-account access of your KMS key

#### Copying Snapshots across accounts

* Create a snapshot, encrypted with your own KMS key(Customer Managed Key)
* **Attach a KMS Key Policy to authorize cross-account access**
* Share the encrypted snapshot
* In target: Create a copy of the Snapshot, encrypt it with a CMK in your account
* Create a volume from the snapshot

### KMS - Multi Region Keys

* Identical KMS keys in different AWS Regions that can be used interchangeably
* Multi-Region keys have the same key ID, Key material, automatic rotation...

* Encrypt in one Region and decrypt in other Regions
* No need to re-encrypt or making cross-Region API calls

* KMS Multi-Region are NOT global(Primary + Replicas)
* Each Multi-Region key in managed **independently**

* **Use-cases**: global client-side encryption, encryption on Global DynamoDB, Global Aurora

#### DynamoDB Global Tables and KMS Multi-Region Keys Client-side encryption

* We can encrypt specific attributes client-side in our DynamoDB table using the **Amazon DynamoDB Encryption Client**
* Combined with Global Tables, the client-side encrypted data is replicated to other regions
* If we use a multi-region key, replicated in the same region as the DynamoDB Global table, the clients in these regions can use low-latency API calls to KMS in their region to decrypt the data client-side
* Using client-side encryption we can protect specific fields and guarantee only decryption if the client has access to API key

<img src="../images/aws-security-encryption/dynamo-db-global-table-and-kms-multi-region.png" alt="DynamoDB Global Tables and KMS Multi-Region Keys Client-side encryption">

* We can encrypt specific attributes client-side in our Aurora table using the **AWS Encryption SDK**
* Combined with Aurora Global Tables, the client-side encrypted data is replicated to other regions
* If we use a multi-region key, replicated in the same region as the Global Aurora DB, then clients in these regions can use low-latency API calls to KMS in their regions to decrypt the data client-side
* Using client-side encryption we can protect specific fields and guarantee only encryption if the client has access to an API key, **we can protect specific fields even from database admin**

<img src="../images/aws-security-encryption/client-side-encryption.png" alt="Client side encryption">

### S3 Replication and Encryption Consideration

* **Unencrypted objects and objects encrypted with SSE-S3 are replicated by default**
* Objects encrypted with SSE-C(customer provided key) can be replicated

* **For objects encrypted with SSE-KMS**, you need to enable the option
  * Specify which KMS key to encrypt the objects within the target bucket
  * Adapt the KMS Key Policy for the target key
  * An IAM Role with kms:Decrypt for the source KMS key and kms:Encrypt for the target KMS Key
  * You might get KMS throttling errors, in which case you can ask for a Service Quotas increase

* **You can use multi-region AWS KMS keys, but they are currently treated as independent keys by Amazon S3(the object will still be decrypted and then encrypted)**

### AMI Sharing Process Encrypted via KMS

* AMI in Source Account is encrypted with KMS Key from Source Account
* Must modify the image attribute to add a **Launch Permission** which corresponds to the specified target AWS account
* Must share the KMS keys used to encrypted the snapshot the AMI references with the target account / IAM Role
* The IAM Role/User in the target account must have the permissions to DescribeKey, ReEncrypted, CreateGrant, Decrypt
* When Launching an EC2 instance from the AMI optionally the target account can specify a new KMS key in its own account to re-encrypt the volumes.

<img src="../images/aws-security-encryption/ami-sharing-process-via-kms.png" alt="AMI Sharing Process Encrypted via KMS">

### SSM Parameter Store Overview

* Secure storage for configuration and secrets
* Optional Seemless Encryption using KMS
* Serverless, scalable, durable, easy SDK
* Version tracking of configuration / secrets
* Security through IAM
* Notifications with Amazon EventBridge
* Integration with CloudFormation

<img src="../images/aws-security-encryption/ssm-parameter-store.png" alt="SSM Parameter Store">

#### SSM Parameter Store Hierarchy

* /my-department/
  * my-app/
    * dev/
      * db-url
      * db-password
    * prod/
      * db-url
      * db-password
  * other-app/
* /other-department/
* /aws/reference/secretsmanager/secret_ID_in_Secrets_Manager
* /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-ap2 (public)

#### Standard and advanced parameter tiers

|                                                        | Standard             | Advanced                               |
|--------------------------------------------------------|----------------------|----------------------------------------|
| Total number of parameters allowed(per account/region) | 10,000               | 100,000                                |
| Maximum size of a parameter value                      | 4KB                  | 8KB                                    |
| Parameter policies avilable                            | No                   | Yes                                    |
| Cost                                                   | No additional charge | Charges apply                          |
| Storage Pricing                                        | Free                 | $0.05 per advanced parameter per month |

#### Parameter Policies(for advanced parameters)

* Allow to assign a TTL to a parameter to force updating or deleting sensitive data such as parameter
* Can assign multiple policies at a time

##### Expiration(to delete a parameter)

```json
{
  "Type": "Expiration",
  "Version": "1.0",
  "Attributes": {
    "Timestamp": "2020-12-02T21:34:33.000Z"
  }
}
```

```json
{
  "Type": "ExpirationNotification",
  "Version": "1.0",
  "Attributes": {
    "Before": 15,
    "Unit": "Days"
  }
}
```

```json
{
  "Type": "NoChangeNotification",
  "Version": "1.0",
  "Attributes": {
    "After": "20",
    "Unit": "Days"
  }
}
```