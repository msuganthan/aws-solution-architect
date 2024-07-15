### Amazon S3 Use cases

* Backup and storage
* Disaster recovery
* Archive
* Hybrid Cloud storage
* Application hosting
* Media hosting
* Data lakes & big data analytics
* Software delivery
* Static website

### Amazon S3 - Buckets

* Amazon S3 allows people to store objects(files) in "buckets" (directories)
* Buckets must have a **globally unique name(across all regions all accounts)**
* Buckets are defined at the region level
* S3 looks like a global service but buckets are created in a region
* Naming convention
  * No uppercase, No underscore
  * 3-63 characters long
  * Not an IP
  * Must start with lowercase letter or number
  * Must NOT start with the prefix **xn--**
  * Must NOT end with the suffix **-s3alias**

#### Amazon S3 - Objects

* Objects(files) have a Key
* The key is the **FULL** path:
  * s3://my-bucket/my_file.txt
  * s3://my-bucket/my_folder1/another_folder/my_file.txt
* The key is composed of prefix + object name
  * s3://my-bucket/my_folder1/another_folder/my_file.txt
* There's no concept of "directories" within buckets
  (although the UI will trick you to think otherwise)
* Just keys with very long names that contain slashes("/")

#### Amazon S3 - Objects(cont.)

* Object values are the content of the body:
  * Max. Object Size is 5TB(5000GB)
  * If uploading more than 5GB, must use "multi-part upload"
* Metadata(list of text key / value pairs - system or user metadata)
* Tags (Unicode key / value pair - up to 10) - useful for security / lifecycle
* Version ID (if versioning is enabled)

#### Amazon S3 - Security

* User-Based
  * IAM Policies - which API calls should be allowed for a specific user from IAM
  
* Resource-Based
  * Bucket Policies - bucket wide rules from the S3 console - allows cross account
  * Object Access Control List(ACL) - finer grain(can be disabled)
  * Bucket Access Control List(ACL) - less common(can be disabled)

* Note: an IAM principal can access an S3 object if
  * The user IAM permission ALLOW it OR the resource policy ALLOWS it
  * AND there's no explicit DENY

* Encryption: encrypt objects in Amazon S3 using encryption keys

#### S3 Bucket Policies

* JSON based policies
  * Resources: buckets and objects
  * Effect: Allow/Deny
  * Actions: Set of API to Allow or Deny
  * Principal: The account or user to apply the policy to
  
* Use S3 bucket for policy to
  * Grant public access to the bucket
  * Force objects to be encrypted at upload
  * Grant access to another account(Cross Account)

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicRead",
      "Effect": "Allow",
      "Principal": "*",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": [
        "arn:aws:s3:::examplebucket/*"
      ]
    }
  ]
}
```

#### Public Access - Use Bucket Policy

* Use can you bucket policy to allow public access for a bucket or to allow user from different account to access the bucket

<img src="../images/s3/s3-public-access-bucket-policy.png" alt="Public Access">

#### User Access to S3 - IAM Permissions

* To allow a user to access the bucket use IAM user policy

<img src="../images/s3/s3-user-access-to-s3.png" alt="User access">

#### EC2 instance access - Use IAM Roles

* To allow EC2 instance to access the S3 bucket, use IAM roles

<img src="../images/s3/s3-ec2-instance-iam-roles.png" alt="EC2 instance access using IAM roles">

#### Bucket settings for Block Public Access

<img src="../images/s3/s3-block-all-public-access.png" alt="Block all public access.">

* These settings were created to prevent company data leaks
* If you know your bucket should never be public, leave these on
* Can be set at the account level