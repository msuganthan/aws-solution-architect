### AWS Organizations

* Global service
* Allows to manage multiple AWS accounts
* The main account is the management account
* Other accounts are member accounts
* Member accounts can only be part of one organization
* Consolidated Billings across all accounts - single payment method
* Pricing benefits from aggregated usage(volume discount for EC2, S3...)
* **Shared reserved instances and Saving Plans discounts across accounts**
* API is available to automate AWS account creation

<img src="../images/identity-and-access-management/aws-organizations.png" alt="AWS Organizations">

<img src="../images/identity-and-access-management/aws-organization-units.png" alt="AWS Organization Units Examples">

#### Advantages

* Multi Account vs One Account Multi VPC
* Use tagging standards for billing purposes
* Enable CloudTrail on all accounts, send logs to central S3 account
* Send CloudWatch Logs to central logging account
* Establish Cross Account Roles for Admin purposes

#### Security: Service Control Policies

* IAM policies applied to OU or Accounts to restrict Users and Roles
* They do not apply to the management account(full admin power)
* Must have an explicit allow from the root through each OU in the direct path to the target account(does not allow anything by default - like IAM)

#### SCP Hierarchy

<img src="../images/identity-and-access-management/security-control-policy-hierarchy.png" alt="SCP hierarchy">

* Management Account
    * Can do anything(no SCP apply)
* Account A
    * Can do anything
    * Except S3
    * Execpt EC2
* Account B & C
    * Can do anything
    * Except S3
* Account D
    * Can access EC2
* Prod OU & Account E & F


### IAM - Advanced Policies

#### aws:SourceIp

* restrict the client IP from which the API calls are being made

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "*",
      "Resource": "*",
      "Condition": {
        "NotIpAddress": {
          "aws:SourceIp": ["192.0.2.0/24", "203.0.113.0/24"]
        }
      }
    }
  ]
}
```

#### aws:RequestedRegion

* restrict the region the API calls are made to

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "ec2:*",
        "rds:*",
        "dynamodb:*"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:RequestedRegion": ["eu-central-1", "eu-west-1"]
        }
      }
    }
  ]
}
```

#### ec2:ResourceTag

* restrict based on tags

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:startInstances",
        "ec2:StopInstances"
      ],
      "Resource": "arn:aws:ec2:us-east-1:123456789:instance/*",
      "Condition": {
        "StringEquals": {
          "ec2:ResourceTag/Project": "DataAnalytics",
          "aws:PricipalTag/Department": "Data"
        }
      }
    }
  ]
}
```

#### aws:MultiFactorAuthPresent

* to force MFA

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:*",
      "Resource": "*"
    },
    {
      "Effect": "Deny",
      "Action": [
        "ec2:StopInstances",
        "ec2:TerminateInstances"
      ],
      "Resource": "*",
      "Condition": {
        "BoolIfExists": {
          "aws:MultiFactorAuthPresent": false 
        }
      }
    }
  ]
}
```

#### IAM for S3

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket"
      ],
      "Resource": "arn:aws:s3:::test"
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject",
        "s3:DeleteObject"
      ],
      "Resource": "arn:aws:s3:::test/*"
    }
  ]
}
```

#### Resource Policies & aws:PrincipalOrgID

* **aws:PrincipalOrgId** can be used in any resource policies to restrict access to accounts that are member of an AWS organization

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::2022-financial-data/*",
      "Condition": {
        "StringEquals": {
          "aws:PrincipalOrgId": ["o-yyyyyyyyyyyy"]
        }
      }
    }
  ]
}
```


### Resource-based Policies vs IAM Roles

* Cross Account
  * attaching a resource-based policy to a resource(example: S3 bucket policy)
  * OR using a role as a proxy
    
<img src="../images/identity-and-access-management/cross-account-policies.png" alt="Cross Account Policies">

* **When you assume a role(user, application or service), you give up your original permissions and take the permissions assigned to the role**
* When using a resource-based policy, the principal doesn't have to give up his permission
* Example: user in account A need to scan a DynamoDB table in Account A and dump it in an S3 bucket in Account B
* Supported by: Amazon S3 buckets, SNS topics, SQS queues, etc...

#### Amazon EventBridge - Security

* When a rule runs, it needs permission on the target
* **Resource-based policy: Lambda, SQS, SNS, S3, API Gateway**
* **IAM role: Kinesis stream, Systems Manager Run Command, ECS task...**