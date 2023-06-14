# Question 1
An online medical system hosted in AWS stores sensitive Personally identifiable Information of the users in an Amazon S3 bucket. Both the master keys and the unencrypted data should never be sent to AWS to comply with the strict compliance and regulatory requirements of the company.

## Explanation

Missing

## Answer

Use S3 client-side encryption with a client side master key

# Question 2

A content-management system is hosted on a fleed of auto-scaled, on-demand EC2 instances that use Amazon Aurora as its database. Currently, the system stores the file documents that the users uploads in one of the attached EBS Volumes. Your manager noticed that the system performance is quite slow and he has instructed you to improve the architecture of the system.

In this scenario, what will you do to implement a scalable, high-available POSIX compliant shared file system?

## Answer

Use EFS

# Question 3

An application consists of multiple EC2 instances in private subnets in different AZ. The application uses a single NAT Gateway for downloading software patches from the internet to the instances. There is a requirement to protect the application from a single point of failure when the NAT gateway encounters a failure or if its availability zone goes down.

## Answer

Create a NAT Gateway in each availability zone. Configure the route table in each private subnet to ensure that instances use the NAT Gateway in the same availability zone.

# Question 4 

An online cryptocurrency exchange platform is hosted in AWS which uses ECS Cluster and RDS in Multi-AZ Deployments configuration. The application is heavily using the RDS instance to process complex read and write database operations. To main the reliability, availability, and performance of your systems, you have to closely monitor how the different processes or threads on a DB instance use the CPU, including the percentage of the CPU bandwidth and total memory consumed by each process.

What is the most suitable solution to properly monitor your database.

## Answer

Enable Enhanced Monitoring in RDS

# Question 5

A company is in the process of migrating their applications to AWS. One of their systems requires a database that can scale globally and handle frequent schema changes. The application should have any downtime or performance issues whenever there is schema changes in the databases. It should also provide a low latency response to high traffic queries.

What is the most suitable database solution to use to achieve this requirement?

## Answer

Amazon DynamoDB

# Question 6

An application hosted in EC2 consumes message from an SQS queue and is integrated with SNS to send out an email to you once the process is complete. THe operations team received 5 orders but after a few hours, they say 20 email notifications in their inbox.

What would have caused this problem?

## Answer

The web application is not deleting the messages in the SQS queue after it has processed them.

# Question 7

A company is using a combination of API Gateway and Lambda for the web services of the online web portal that is being accessed by hundreds of thousands of clients each day. They will be announcing a new revolutionary product and it is expected that the web portal will receive a massive number of visitor all around the globe.

How can you protect the backend systems and applications from traffic spikes?

## Answer

Use throttling limits in API Gateway

# Question 8

An application that records weather data every minute, is deployed in a fleet of Spot EC2 instances and uses a MySQL RDS database instance. Currently there is only one RDS instance running in one Availability Zone. You plan to improve the database to ensure  high availability by synchronous data replication to another RDS instance.

Which performs synchronous data replicate in RDS

## Answer 

RDS DB instance running as a Multi-AZ deployment

# Question 9

A car dealership website hosted in Amazon EC2 stores car listing in an Amazon Aurora database managed by Amazon RDS. Once a vehicle has been sold, its data must be removed from the current listings and forwarded to a distributed processing system.

What satisfy the given requirement?

## Answer

Create a native function or a stored procedure that invokes a lambda function. Configure the Lambda function to send event notifications to an Amazon SQS queue for the processing system to consume.

# Question 10

A government agency plans to store confidential tax documents on AWS. Due to the sensitive information in the files, the solution Architect must restrict the data access requests made to the store solution to a specific Amazon VPC only. The solution should also prevent the file from being deleted or overwritten to meet the regulator requirement of having a write0one-read-many storage model.

## Answer

* Create a new Amazon S3 bucket with **S3 Object Lock feature enabled**. Store the documents in the bucket and set the **Legal Hold** option for object retention.
* Configure an **Amazon S3 Access Point** for the S3 bucket to restrict data access to a particular Amazon VPC only.

# Question 11

A Solution Architect need to set up a relational database and come up with a disaster recovery plan to mitigate multi-region failure. The solution requires a `Recovery Point Objective` of 1 second and a `Recovery Time Objective (RTO)` of less than 1 minute.

What AWS service can fulfill this requirement?

## Answer 

Amazon Aurora Global Database

# Question 12

An online shopping platform is hosted on an Auto Scaling group of Spot EC2 instances and uses Amazon Aurora PostgreSQL as its database. There is a requirement to optimize your database workloads in your cluster where you have to direct the write operations of the production traffic to your high-capacity instances and point the reporting queries sent by your internal staff to the low-capacity instances

What is the most suitable configuration for this application as well as your Aurora database cluster to achieve this requirement?

## Answer

Create a custom endpoint in aurora based on the specified criteria for the production traffic and another custom endpoint to handle the reporting queries.

# Question 13

A newly hired Solution Architect is assigned to manage a set of CloudFormation templates that are used in the company's cloud architecture in AWS. The Architect accessed the templates and tired to analyze the configured IAM policy for an S3 bucket.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:Get*",
        "s3:List*"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::boracay/*"
    }
  ]
}
```

## Answer

* An IAM user with this IAM policy is allowed to write objects into the `boracay` S3 bucket
* An IAM user with this IAM policy is allowed to read objects from the `boracay` S3 bucket.
* An IAM user with this IAM policy is allowed to read objects from all S3 buckets owned by the account.

# Question 14

A company plans to migrate its on-premises workload to AWS. The current architecture is composed of a **Microsoft SharePoint server** that uses a **Windows shared file storage**. The Solution Architect needs to use a cloud storage solution that is highly available and can be integrated with Active Directory for access control and authentication

## Answer

Create a file system using **Amazon FSx for Windows File Server** and join it to an Active Directory Domain in AWS.

# Question 15:

A tech company has a CRM application hosted on an Auto Scaling group of On-Demand EC2 instances with different instance types and sizes. The application is extensively used during the office hours from 9 in the morning to 5 in the afternoon. Their users are complaining that the performance of the application is slow duing the start of the day but then works normally after a couple of hours.

What is the MOST operationally efficient solution to implement to ensure the application works properly at the beginning of the day?

## Answer

Configure a Scheduled scaling policy for the Auto Scaling group to launch new instances before the start of the day.

# Question 16

A media company has an Amazon ECS Cluster, which uses the Fargate launch type, to host its news website. The application data are all stored in Amazon Keyspaces(for Casandra) with data-at-rest encryption enabled. The database credentials should be supplied using environment variables, to comply with strict security compliance. As the Solution Architect, you have to ensure that the credentials are secure and that they cannot be viewed in plaintext on the cluster itself.

What is the most suitable solution in this scenario that you can implement with minimal effort?

## Answer

Use the **AWS Systems Manager Parameter Store** to keep the database credentials ad then encrypt them using **AWS KMS**. Create an IAM Role for your **Amazon ECS** task execution role(`taskRoleArn`) and reference it with your **task definition**, which allows access to both KMS and the Parameter Store. Within your container definition, specify secrets with the **name of the environment variable to set in the container** and the **full ARN of the System Manager Parameter Store** parameter containing the sensitive data to present to the container.

# Question 17

A travel photo sharing website is using Amazon S3 to serve high-quality photos to visitors of your website. After a few days, you found out that there are other travel websites linking and using your photos. This resulted in financial loses for your business

What is the MOST effective method to mitigate the issue?

## Answer

Configure your S3 bucket to remove public read access and use pre-signed URLs with expiry dates.

# Question 18:

An AI-powered Forex trading application consumer thousands of data sets to train its machine learning model. The application's workload requires a high performance, parallel hot storage to process the training datasets concurrently. It also needs cost-effective cold storage to archive those datasets that yield low profit.

What is the Amazon Storage service should the developer use?

## Answer:

Use Amazon FSx For Lustre and Amazon S3 for hot and cold storage respectively

# Question 19:

A company wishes to query data that resides in multiple AWS accounts from a central data lake. Each account has its own Amazon S3 bucket that stores data uniqeu to its business function. Users from different accounts must be granted access to the data lake based on their roles.

What solution will minimize overhead and costs while meeting the required access patters?

## Answer:

Use **AWS Lake Formation** to consolidate data from multiple accounts into a single account.

# Question 20:

An online learning company hosts its Microsoft .NET e-learning application on a Windows Server in its on-premises data center. The application uses an Oracle Database Standard Edition as its backend database.

The company wants a high performing solution to migrate this workload to the AWS cloud to take advantage of the cloud's high availability. The migration process should  minimize development changes, and the environment should be easier to manage.

What should be implemented to meet the company requirements?

## Answer:

* Migrate the Oracle database to Amazon RDS for Oracle in a Multi-AZ deployment by using AWS Database Migration Service(AWS-DMS)
* Rehost the on-premises .NET application to an AWS Elastic Beanstalk Mulit-AS environment which run in multiple Availability Zones.

# Question 21:

A startup is using Amazon RDS to store data from a web application. Most of the time, the application has how user activity but it receives burst of traffic within seconds whenever there is a new product announcement. The Solution Architect needs to create a solution that will allow the users around the globe to access the data using an API.

What should the Solution Architect do meet the above requirement?

## Answer:

Create an API using Amazon API Gateway and use AWS lambda to handle the bursts traffic in seconds.

# Question 22:

A Solution Architect identified a series of DDoS attacks while monitoring the VPC. The Architect needs to fortify the current cloud infrastructure to protect the data of the clients.

What is the most suitable solution to mitigate these kinds of attacks?

## Answer:

Use AWS Shield Advanced to detect and mitigate DDoS attacks.

# Question 23:

A popular social media website uses a CloudFront web distribution to serve their static contents to their millions of users around the globe. They are receiving a number of complaints recently that their users take a lot of time to log to their website. There are also occasions when their users are getting HTTP 504 errors. You are instructed by your manager to significantly reduce the user's login time to further optimize the system.

What are the options you use together to set-up a cost effective solution that can improve your application's performance?

## Answer:

* Customize the content that the `CloudFront` web distribution delivers to your users using `Lambda@Edge`, which allows your Lambda functions to execute the authentication process in AWS location closer to the users.

* Set up an `origin failover` by creating an origin group with two origins. Specify one as the primary origin and the other as the second origin which `CloudFront` automatically switches when the primary origin returns specific HTTP status code failure responses.

# Question 24:

A Docker application, which is running on an Amazon ECS cluster behind a load balancer, is heavily using DynamoDB. You are instructed to improve the database performance by distributing the workload evenly and using the provisioned throughput efficiently.

What would you consider to implement for your DynamoDB table?

## Answer:

Use partition keys with high-cardinality attributes, which has a large number of distinct values for each item.

# Question 25:

A company is using Amazon S3 to store frequently accessed data. When an object is created or deleted, the S3 bucket will send an event notification to the Amazon SQS queue. A solution architect needs to create a solution that will notify the development and operations team about the created and deleted objects

What would satisfy this requirement?

## Answer:

Create an Amazon SNS topic and configure two Amazon SQS queues to subscribe to the topic. Grant Amazon S3 permission to send notification to Amazon SNS and update the bucket to use the new SNS topic.

# Question 26:

A company uses an Application Load Balancer(ALB) for its public-facing multi-tier web applications. The security team has recently reported that there has been a surge of SQL injection attacks lately, which causes critical data discrepancy issues. The same issue is also encountered by its web application in other AWS accounts that are behind an ALB. An immediate solution is required to prevent the remote injection of unauthorized SQL queries and protect their application hosted across multiple accounts.

What solution would you recommend.

## Answer:

Use AWS WAF and set up a managed rule to block request **patterns** associated with the exploitation of SQL databases, like SQL injection attacks. Associate it with the Application Load Balancer. Integrate AWS WAF with AWS Firewall Manager to re-use the rules across all the AWS accounts.

# Question 27:

A company has a web application that uses Amazon CloudFront to distribute its images, videos, and other static contents stored in its S3 bucket to its users around the world. The company has recently introduced a new member-only access feature to some of its high-quality media files. There is a requirement to provide access to multiple private media diles only to their paying subscribers with having to change their current URLS.

What is the most suitable solution that you should implement to satisfy this requirement?

## Answer:

Use Singed Cookie to control who can access the private files in your CloudFont distribution by modifying you application to determine whether a use should have access to your content. For members, send the required `Set-Cookie` headers to the viewer which will unlock the content only to them.

# Question 28:

A popular social network is hosted in AWS and is using a DynamoDB table as its database. There is a requirement to implement a `follow` feature where uses can subscribe to certain updates made by a particular uses and be notified via email. Which of the following is the most suitable solution that you should implement to meet the requirement?

## Answer:

Enable DynamoDB Stream and create an `AWS Lambda trigger` as well as the IAM roles which contains all the permissions that the Lambda function will need at runtime. The data from the stream record will be processed by the Lambda function which will then publish a message to SNS Topic that will notify the subscribes via email.

# Question 29:

A software development company is using serverless computing with AWS Lambda to build and run applications without having to set up or manage servers. They have a Lambda function that connects to a MongoDB Atlas. which is a popular Database as a Service(DBaas) platform and also uses a third party API to fetch certain data for their application. On of the developers was instructed rto create the environment variables for the MongoDN database hostname, username and password as well as the API credentials that will be used by the Lambda function for DEV, SIT, UAT, and PROD environments. 

Considering that the Lambda function is storing sensitive database and API credentials, how can this information can be secured to prevent other developers in the team or anyone, from seeing these credentials in plain text? 

## Answer:

Create a new KMS key and use it to enable encryption helpers that leverage on AWS Key Management Service to store and encrypt the sensitive information.

# Question 30:

A company needs to deploy at least 2 EC2 instances to support the normal workloads of its application and automatically scale up to 6 EC2 instances to handle the peak load. The architecture must be highly available and fault-tolerant as its processing mission-critical workloads.

As a SA what would you do to meet the above requirement?

## Answer:

Create an Auto Scaling group of EC2 instances and set the minimum capacity to 4 and maximum capacity to 6. Deploy 2 instances in AZ A and another 2 instances in AZ B.

# Question 31:

A company conducted a surprise IT audit on all the AWS resources being used in the production environment. During the audit activities, it was noted that you are using a combination of **Standard** and **Convertible** Reserved EC2 instances in your applications.

What are the characteristics and benefits of using these two types of Reserved EC2 instances?

## Answer:

* Convertible Reserved Instances allow you to exchange for another convertible reserved instance of a different instance family.
* Unused standard Reserved instances can be later sold at the Reserved Instance Marketplace.

# Question 32:

A financial application is composed of Auto Scaling group of EC2 instances, an Application Load Balancer, and a MySQL RDS instance in a Multi-AZ Deployments configuration. To protect the confidential data of your customers, you have to ensure that your RDS database can only be accessed using the **profile credentials specific to your EC2 instances via an authentication token**.

## Answer:

Enable the IAM DB Authentication

# Question 33:

There was an incident in your production environment where the user data stored in the S3 bucket has been accidentally deleted by one of the Junior DevOps Engineers. The issue was escalated to your manager and after a few days, you were instructed to improve the security and protection of your AWS resources.

What are option will protect the S3 objects in your bucket from both accidental deletion and overwriting?

## Answer:

Enable versioning
Enable Multi-factor Authentication Delete

# Question 34:

A company collects atmospheric data such as temperature, air pressure, and humidity from different countries. Each site location is equipped with various weather instruments and a high-speed Internet connection. The average collected data in each location is around 500 GB and will be analyzed by a weather forecasting application hosted in teh Northern Virgina. As the Solution Architect, you need to aggregate all the data in the fastest way.

## Answer:

Enable **Transfer Acceleration** in the destination bucket and upload the collected data using Multipart Upload.

# Question 35:

A business has recently migrated its applications to AWS. The audit team must be able to assess whether the service the company is using meet common security and regulatory standards. A solutions architect needs to provide the team with a report of all compliance-related documents for their account.

## Answer:

Use **AWS Artifact** to view the security reports as well as other AWS compliance-related information.

# Question 36:

A company has a web application that user Internet Information Services(IIS) for Windows Server. A file share is used to store the application data on the network-attached storage of the company's on-premises data center. To achieve a highly available system, they plan to migrate the application and file share to AWS.

## Answer:

Migrate the existing file share configuration to Amazon FSx for Windows File Server.

# Question 37:

A retail website has intermittent, sporadic, and unpredictable transaction workloads throughout the day that are hard to predict. The website is currently hosted on-premises and is slated to be migrated to AWS. A new relational database is needed that autoscales capacity to meet the needs of the application's peak load and scales back down when surge of activity is over.

What is most cost-effective and suitable database setup in this scenario?

## Answer:

Launch an **Amazon Aurora Serverless DB cluster** then set the minimum and maximum capacity for the cluster.

# Question 38:

An organization needs to **persistent block storage volume** that will be used for mission-critical workloads. The backup data will be stored in an object storage service and after 30 days, the data will be stored in a data archiving storage service.

## Answer:

Attach an EBS volume in your EC2 instance. Use Amazon S3 to store your backup data and configure a life cycle policy to transition you objects to Amazon S3 Glacier.

# Question 39:

A telecommunications company is planning to give AWS Console access to developers. Company policy mandates the use of **identity federation and role-based access control**. Currently, the roles are already assigned using groups in the corporate Active Directory.

## Answer:

* IAM Roles
* AWS Directory Service AD Connector

# Question 40:

A Solution Architect is working for a company which has multiple VPCs in various AWS regions. The Architect is assigned to set up a logging system which will track all the changes made to their AWS resources in all regions, including the configuration made in the IAM, CloudFront, AWS WAF and Route 53. In order to pass the compliance requirements, the solution must ensure the security, integrity and durability of the log data. It should also provide an event history of all API calls made in AWS Management Console and AWS CLI.

## Answer:

Set up a new CloudTrail trail in a new S3 bucket using the AWS CLI and also pass both the `--is-multi-region-trail` and `--include-global-service-events` parameters then encrypt log files using KMS encryption. Apply Multi Factor Authentication(MFA) Delete on the S3 bucket and ensure the only authorized users can access the logs by configuring the bucket policies.

# Question 41:

A cryptocurrency trading platform is using an API built in **AWS Lambda and API Gateway**. Due to the recent news and rumors about the upcoming price surge of BitCoin, Ethereum and other cryptocurrencies, it is expected that the trading platform would have a significant increase in site visitors and new users in the coming days ahead.

In this scenario, how can you protect the backend systems of the platform from traffic spikes?

## Answer:

Enable throttling limits and result caching in API Gateway.

# Question 42:

A government entity is conducting a population and housing census in the city. Each household information uploaded on their online portal is stored in the encrypted files in the Amazon S3. The government assigned its Solution Architect to set compliance policies the verify data containing personally identifiable information(PII) in a manner that meets their compliance standards. They should also be alerted if htere are potential policy violations with the privacy of their S3 buckets.

What should the SA implement to satisfy this requirement?

## Answer:

Set up and configure **Amazon Macie** to monitor their Amazon S3 data.

# Question 43:

A global IT company with offices around the world has multiple AWS accounts. To improve efficiency and drive costs down, the CIO wants to set up a solution that centrally manages their AWS resources. This will allow them to procure AWS resources centrally and share resources such **AWS Transit Gateways**, **AWS License Manager configurations**, or **Amazon Route 53 Resolver** rules across their various accounts.

## Answer:

* Consolidate all the company's accounts using AWS Organizations
* Use the **AWS Resource Access Manager(RAM)** service to easily and securely share your resources with your AWS accounts.

# Question 44:

An application that records weather data every minute is deployed in a fleet of spot **EC2 instances** and uses a MySQL RDS database instance. Currently, there is only one RDS instance running in one AZ. You plan to improve the database to ensure **high availability by synchronous data replication to another RDS instance**.

What performs synchronous data replication in RDS

## Answer:

RDS DB instance running as a Multi-AZ deployment.

# Question 45:

A government agency plans to store confidential tax documents on AWS. Due to the sensitive information in the files, **the SA must restrict the data access requests made to the storage solution to a specific Amazon VPC only**. Ths solution should also prevent the files from being deleted or overwritten to meet the regulatory requirement of having a **write-once-read-many(WORM)** storage model.

## Answer:

* Create a new Amazon S3 bucket with the S3 Object Lock feature enabled. Store the documents in the bucket and set the Legal Hold option for object retention.
* Configure an Amazon S3 Access Point for the S3 bucket to restrict data access to a particular Amazon VPC only.

# Question 46:

A media company has an Amazon ECS Cluster, which uses the Fargate Launch type, to host its news website. The application data are all stored in Amazon Keyspaces(for Apache Cassandra) with data-at-rest encryption enabled. The database credentials should be supplied using env variables, to comply with strict security compliance. As a SA, you have to ensure that the credentials are secure and that they cannot be viewed in plaintext on the cluster itself.

## Answer:

Use the **AWS Systems Manager Parameter Store** to keep the database credentials and then encrypt them using **AWS KMS**. Create an IAM Role for your **Amazon ECS** task execution role(`taskRoleArn`) and reference it with your task definition, which allows access to both KMS and the Parameter Store. Within your container definition, specify secrets with the name of the **environment variable** to set in the container and the full ARN of the **Systems Manager Parameter Store** parameter containing the sensitive data to present to the container.

# Question 47:

An online medical system hosted in AWS stores sensitive Personally identifiable Information of the users in an Amazon S3 bucket. Both the master keys and the unencrypted data should never be sent to AWS to comply with the strict compliance and regulatory requirement of the company.

## Answer:

Use S3 client-side encryption with a client-side master key.

# Question 48:

A company is using AWS Fargate to run a batch job whenever an object is uploaded to an Amazon S3 bucket. The minimum ECS task count is initially set to 1 to save on costs and should be increased based on new objects uploaded to the S3 bucket.

Which is the most suitable option to implement with the LEAST amount of effort?

## Answer:

Set up an Amazon EventBridge rule to detect S3 object PUT operations and set the target to the ECS cluster to run a new ECS task.

# Question 49:

A startup is using Amazon RDS to store data from a web application. Most of the time, the application has now user activity but it receives burst of traffic within seconds whenever there is a new product announcement. The SA needs to create a solution that will allow users around the globe to access the data using an API.

## Answer:

Create an API using Amazon API Gateway and use AWS Lambda to handle the bursts of traffic in seconds.

# Question 50:

A company is in the process of migrating their applications to AWS. One of their systems **requires a database the can scale globally and handle frequent schema changes**. The application should not have any downtime or performance issues whenever there is a schema change in the database. IT should also provide a low latency response to high-traffic queries.

## Answer:

Amazon DynamoDB

# Question 51:



## Answer:

# Question 52:

## Answer:

# Question 53:

## Answer:

# Question 54:

## Answer:

# Question 56:

## Answer:

# Question 57:

## Answer:

# Question 58:

## Answer:

# Question 59:

## Answer:

# Question 60:

## Answer: