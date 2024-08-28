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