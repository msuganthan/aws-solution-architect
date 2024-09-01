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

### AWS Secrets Manager

* Newer service, meant for storing secrets
* Capability for force **rotation of secrets** every X days
* Automate generation of secrets on rotation(uses Lambda)
* Integration with **Amazon RDS**(MySQL, PostgreSQL, Aurora)
* Secrets are encrypted using KMS
* Mostly meant for RDS integration

#### AWS Secrets Manager - Multi-Region Secrets

* Replicate Secrets across multiple AWS regions
* Secrets Manager keeps read replicas in sync with the primary Secret
* Ability to promote a read replica Secret to a standalone Secret
* Use cases: multi-region apps, disaster recovery strategies, multi-regions DB

<img src="../images/aws-security-encryption/aws-secrets-manager-multi-region-secrets.png" alt="Multi Region Secrets">

### AWS Certificate Manager(ACM)

* Easily provision, manage and deploy **TLS Certificates**
* Provide in-flight encryption for websites(HTTPS)
* Supports both public and private TLS certificates
* Free of charge for public TLS certificates
* Automatic TLS certificate renewal
* Integration with(load TLS certificates on)
  * Elastic Load Balancer(CLB, ALB, NLB)
  * CloudFront Distributions
  * APIs on API Gateway
* Cannot use ACM with EC2(can't be extracted)

<img src="../images/aws-security-encryption/aws-certificate-manager.png" alt="AWS Certificate Manager">

#### ACM - Requesting Public Certificates

* List domain names to be included in the certificates
  * Fully Qualified Domain Name(FQDN): corp.example.com
  * Wildcard Domain: *.example.com
* Select Validation Method: DNS Validation or Email validation
  * DNS Validation is preferred for automation purposes
  * Email Validation will send emails to contact addresses in the WHOIS database
  * DNS Validation will leverage a CNAME record to DNS config(ex: Route 53)
* It will take a few hours to get verified
* The Public Certificate will be enrolled for automatic renewal
  * ACM automatically renews ACM-generated certificates 60 days before expiry

#### ACM - Importing Public Certificates

* Option to generate the certificates outside of ACM and then import it
* **No automatic renewal,** must import a new certificate before expiry
* **ACM sends daily expiration events** starting 45 days prior to expiration
  * The # of days can be configured
  * Events are appearing in EventBridge
* **AWS Config** has a managed rule named acm-certificate-expiration-check to check for expiring certificates(configurable number of days)

<img src="../images/aws-security-encryption/acm-importing-public-certificates.png" alt="Importing public Certificates">

#### ACM - Integration with ALB

<img src="../images/aws-security-encryption/acm-integration-with-alb.png" alt="Integration with ALB">

#### API Gateway - Endpoint Types

* **Edge-Optimized(default)**: For global clients
  * Requests are routed through the CloudFront Edge Locations(improve latency)
  * The API Gateway still lives in only one region
* **Regional**:
  * For clients within the same region
  * Cloud manually combine with CloudFront(more control over the caching strategies and the distribution)
* **Private**:
  * Can only be accessed from your VPC using an interface VPC endpoint(ENI)
  * Use a resource policy to define access

#### ACM - Integration with API Gateway

* Create a **Custom Domain Name** in API Gateway
* **Edge-Optimized(default)**: For global clients
  * Requests are routed through the CloudFront Edge locations
  * The API Gateway still lives in only one region
  * **The TLS Certificates must be in the same region as CloudFront, in us-east-1**
  * Then setup CNAME or (better) A-Alias record in Route 53
* **Regional**:
  * For clients within the same region
  * **The TLS Certificates must be imported on API Gateway, in the same region as the API Stage**
  * Then set CNAME or (better) A-Alias record in Route 53

<img src="../images/aws-security-encryption/acm-integration-with-api-gateway.png" alt="Integration with API Gateway">

### AWS WAF - Web Application Firewall

* Protects your web applications from common web exploits(Layer 7)
* **Layer 7 is HTTP**(vs Layer 4 is TCP/UDP)
* Deploy on
  * **Application Load Balancer**
  * **API Gateway**
  * **CloudFront**
  * **AppSync GraphQL API**
  * **Cognito User Pool**

* Define Web ACL(Web Access Control List) Rules:
  * **IP Set: up to 10,000 IP addresses** - use multiple Rules for more IPs
  * HTTP headers, HTTP body, or URI strings Protects from common attack - **SQL injection** and **Cross-Site Scripting(XSS)**
  * Size constraints, **geo-match(block countries)**
  * **Rate-based rules**(to count occurrences of events) - **for DDoS protection**

* **Web ACL are Regional except for CloudFront**
* A rule group is a **reusable set of rules that you can add to a web ACL**

#### WAF - Fixed IP while using WAF with a Load Balancer

* WAF does not support the Network Load Balancer(Layer 4)
* We can use Global Accelerator for fixed IP and WAF on the ALB

<img src="../images/aws-security-encryption/waf-fixed-ip-solution-with-load-balancer.png" alt="WAF fixed IP Solution">

### Shield - DDoS Protection

* DDoS: Distributed Denial of Service - many requests at the same time
* AWS Shield Standard:
  * Free service that is activated for every AWS customer
  * Provides protection from attacks such as SYN/UDP floods, Reflection attacks and other layer 3/layer 4 attacks
* AWS Shield Advanced:
  * Optional DDoS mitigation service($3,000 per month per organization)
  * Protect against more sophisticated attack on _Amazon EC2, Elastic Load Balancing(ELB), Amazon CloudFront, AWS Global Accelerator and Route 53_
  * 24/7 access to AWS DDoS response team(DRP)
  * Protect against higher fees during usage spikes due to DDoS
  * Shield Advanced automatic application layer DDoS mitigation automatically creates, evaluates and deploys AWS WAF rules to mitigate layer 7 attacks.

### AWS Firewall Manager

* **Manage rules in all accounts of an AWS Organization**

* Security policy: common set of security rules
  * WAF rules(Application Load Balancer, API Gateways, CloudFront)
  * AWS Shield Advanced(ALB, CLB, NLB, Elastic IP, CloudFront)
  * Security Groups for EC2, Application Load Balancer and ENI resources in VPC
  * AWS Network Firewall(VPC Level)
  * Amazon Route 53 Resolver DNS Firewall
  * Policies are created at the region level
  
* **Rules are applied to new resources as they are created(good for compliance) across all and future accounts in your organization.**

#### WAF vs Firewall Manager vs Shield

* **WAF, Shield, and Firewall Manager are used together for comprehensive protection**
* Define your Web ACL rules in WAF
* For granular protection of your resources, WAF alone is the correct choice
* If you want to use **AWS WAF across accounts, accelerate WAF configuration, automate the protection of new resources**, use Firewall manager with AWS WAF
* Shield Advanced adds additional features on top of AWS WAF, such as dedicates support from the Shield Response Team(SRT) and advanced reporting.
* If you're prone to frequent DDoS attacks, consider purchasing Shield Advanced.

### AWS Best Practices for DDoS Resiliency

<img src="../images/aws-security-encryption/aws-best-practices-for-ddos.png" alt="AWS Best Practices for DDoS Resiliency">

#### Edge Location Mitigation(BP1, BP3)

* **BP1 - CloudFront**
  * **Web Application delivery at the edge**
  * Protect from DDoS Common Attacks(SYN floods, UDP reflections...)
* **BP1 - Global Accelerator**
  * Access your application from the edge
  * **Integration with Shield from the edge**
  * Helpful if your backend is **not compatible** with CloudFront
* **BP3 - Route 53**
  * Domain Name Resolution at the edge
  * DDoS Protection mechanism on your DNS

#### Best practices for DDoS Mitigation

* **Infrastructure layer defense(BP1, BP3, BP6)**
  * Protect Amazon EC2 against high traffic
  * That includes using Global Accelerator, Route 53, CloudFront, Elastic Load Balancing
* **Amazon EC2 with Auto Scaling(BP7)**
  * Helps scale in case of sudden traffic surges including a flash crowd or a DDoS attack
* **Elastic Load Balancing(BP6)**
  * Elastic Load Balancing scales with the traffic increases and will distribute the traffic to many EC2 instances

#### Application Layer Defense

* **Detect and filter malicious web requests(BP1, BP2)**
  * CloudFront cache static content and serve it from edge locations, protecting your backend
  * AWS WAF is used on top of CloudFront and Application Load Balancer to filter and block requests based on request signatures
  * WAF rate-based rules can automatically block the IPs of bad actors
  * Use managed rules on WAF to block attacks based on IP reputation or block anonymous IPs
  * CloudFront can block specific geographies
* **Shield Advanced(BP1, BP2, BP6)**
  * Shield Advanced automatic application layer DDoS mitigation automatically creates, evaluates and deploy AWS WAF rules to mitigate layer 7 attacks

#### Attack surface reduction

* **Obfuscating AWS resources(BP1, BP4, BP6)**
  * Using CloudFront, API Gateway, Elastic Load Balancing to hide your backend resources(Lambda functions, EC2 instances)
* **Security groups and Network ACLs (BP5)**
  * Use security groups and NACLs to filter traffic based on specific IP at the subnet or ENI-level
  * Elastic IP are protected by AWS Shield Advanced
* **Protecting API endpoints (BP4)**
  * Hide EC2, Lambda, elsewhere
  * Edge-optimized mode, or CloudFront + regional mode(more control for DDoS)
  * WAF + API Gateway: burst limits, header filtering, use API keys

### Amazon GuardDuty

* Intelligent Threat discovery to protect your AWS account
* Uses Machine Learning algorithms, anomaly detection, 3rd party data
* One click to enable(30 days trail), no need to install software
* Input data includes:
  * CloudTrail Events Logs - unusual API calls, unauthorized deployments
  * VPC Flow Logs - usual internal traffic, unusual IP address
  * DNS Logs - compromised EC2 instances sending encoded data within DNS queries
  * Optional Features - EKS Audio Logs, RDS & Aurora, EBS, Lambda, S3 Data Events...
* Can setup **EventBridge rules** to be notified in case of findings
* EventBridge rules can target AWS Lambda or SNS
* **Can protect against CryptoCurrency attacks (has a dedicated "finding" for it)**

<img src="../images/aws-security-encryption/amazon-guard-duty.png" alt="Amazon GuardDuty">