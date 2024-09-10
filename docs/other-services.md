### What is CloudFormation

* CloudFormation is a declarative way of outlining your AWS Infra, for any resources(most of them are supported)
* For example within a CloudFormation template, you say:
  * I want a security Group
  * I was two EC2 instances using this security group
  * I want an S3 bucket
  * I want a load balancer(ELB) in front of these machines
* Then CloudFormation creates those for you, in the **right order**, with the **exact configuration** that you specify

#### Benefits of AWS CloudFormation

* Infrastructure as code
  * No resources are manually created, which is excellent for control
  * Changes to the infrastructure are reviewed through code
  
* Cost
  * Each resource within the stack is tagged with an identifier so you easily see how much a stack costs you
  * You can estimate the cost of your resources using the CloudFormation template
  * Saving strategy: In Dev you could automate deletion of templates at 5 PM and recreated at 8 AM safely
  
* Productivity
  * Ability to destroy and re-create an infrastructure on the cloud on the fly
  * Automated generation of Diagram for your templates
  * Declarative programming(no need to figure out ordering and orchestration)
  
* Don't re-invent the wheel
  * Leverage existing templates on the web
  * Leverage the documentation
  
* Supports all AWS resources
  * You can use "custom resources" for resources that are not supported

#### CloudFormation + Application Composer

* We can see all the resources
* We can see the relations between the components

<img src="../images/other-services/cloud-formation-plus-application-composer.png" alt="CloudFormation + Application Composer">


### CloudFormation - Service Role

* IAM role that allows CloudFormation to create/update/delete stack resources on your behalf.
* Give ability to users to create/update/delete the stack resources even if they don't have permissions to work with the resources in the stack
* Use cases:
  * You want to achieve the least privilege principle
  * But you don't want to give the user all the required permissions to create the stack resources
* User must have **iam:PassRole** permissions

<img src="../images/other-services/cloud-formation-service-role.png" alt="CloudFormation Service Roles">

### Amazon Simple Email Service (Amazon SES)

* Fully managed service to send emails securely, globally and at scale
* Allows inbound/outbound emails
* Reputation dashboard, performance insights, anti-spam feedback
* Provides statistics such as email deliveries, bounces, feedback loop results, email open
* Supports DomainKeys Identified Mail(DKIM) and Sender Policy Framework(SPF)
* Flexible IP deployment: shared, dedicated and customer-owned IPs
* Send emails using you application using AWS Console, APIs, or SMT

* **Use cases**: transactional, marketing and bulk email communications

<img src="../images/other-services/amazon-simple-email-service.png" alt="Amazon Simple Email Services">

### Amazon Pinpoint

* Scalable **2-way(outbound/inbound)** marketing communications service
* Supports email, SMS, push, voice, and in-app messaging
* Ability to segment and personalize messages with the right content to customers
* Possibility to receive replies
* Scales to billions of messages per day
* Use cases: run campaigns by sending marketing, bulk, transaction SMS messages
* **Versus Amazon SNS or Amazon SES**
  * In SNS or SES you managed each message's audience, content and delivery schedule
  * In Amazon Pinpoint, you creat message templates, delivery schedules, highly-targeted segments and full campaigns

<img src="../images/other-services/amazon-pin-point.png" alt="Amazon PinPoint">

### System Manager - SSM Session Manager

* Allow you to start a secure shell on your EC2 and on-premises servers
* No SSH access, bastion hosts, or SSH keys needed
* No port 22 needed(better security)
* Support Linux, macOS, and Windows
* Send session log data to S3 or CloudWatch Logs

<img src="../images/other-services/system-session-manager.png" alt="System Session Manager">