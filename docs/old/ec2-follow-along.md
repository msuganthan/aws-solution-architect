### Launching an Instance
**Steps**
1. Choosing AMI(Amazon Machine Image):
   Is a template that contains the software configuration(operating system, application server and applications) required to launch your instance. You can select an AMI provided by AWS, user community, or the AWS marketplace: or you can select one of your own AMIs.
<img src="../images/ec2/ami-selection.png">
2. Choose Instance Type:
   Amazon EC2 provides a wide selection of instance types optimized to fit different use cases. Instances are virtual servers that can run application. They have varying combination of CPU, memory, storage, and networking capacity, and give you the flexibility to choose the appropriate mix of resources for your applications.
<img src="../images/ec2/instance-type-selection.png">
3. Configure Instance:
   Configure the instance to suit your requirements. You can launch multiple instances from the same AMI, request Spot instances to take advantage of the lower pricing, assign an access management role to the instance and more.
<img src="../images/ec2/configure-instance-details.png">
   Important options: Number of Instances, Purchasing Option, Placement group,  Capacity Reservation, IAM role, shutdown behavior, monitoring, Enable termination protection.
4. Add Storage:
   You can attach **additional EBS volumes** and instance store volumes to your instances, or edit the settings of the root volume. You can also **attach additional EBS volumes after launching an instance, but not instance store volumes**.
<img src="../images/ec2/add-storage.png">
5. Add Tags
6. Configure Security Group
   A security group is a set of firewall rules that control the traffic for your instance. On this page, you add rules to allow specific traffic to reach you instance. For example, if you want to set up a web server and allow internet traffic to reach your instance, add rules that allow unrestricted access to the HTTP and HTTPs ports. You can create a new security group or select from an existing one below.
<img src="../images/ec2/security-group.png">
7. Review
### SSH Into Instance

* ssh ec2-user@<public-ip> -i <path to .pem file>
* If the pem file is having more access, reduce it down(chmod 400 <path to .pem file>)
* Here you can do `curl http://169.254.169.254/latest/meta-data`

### Encrypted Snapshots

* If we want to launch an instance with encrypted root volume.
* Go to volumes
* Create a snapshot
* Go a snapshot
* Go to actions and make a copy of this snapshot
  * Use the default aws/ebs as master key and click `Copy` button.
* To launch the Encrypted one now.
* Go to action, Create Image 
* Go ahead and launch it.

### Creating an AMI

* Go to action, click on `Image` -> `Create Image` -> `Enter the image name` and click `Create Image`.

### Working with ASGs

* Create a Launch Configuration
  * Choose AMI(Choose one from my AMI)
  * Choose Instance Type
    * Choose t2.micro
  * Configure details
    * IAM role to `MyEC2Role`
    * Name `my-server-lc-000`
  * Add Storage
  * Configure the Security Groups
* Launch Auto Scaling Group
  * Configure Auto Scaling group details
    * Name it `my-server-asg`
    * Choose the subnets `us-east-1a`, `us-east-1b`, `us-east-1c`
  * Configure scaling policies(leave it alone)
  * Configure Notification(leave it alone)
  * Configure Tags(leave it alone)
  * Review(leave it alone)
  * Create it.
  * **After the creation, just note Desired - 1, Min - 1, Max - 1**
* Create Image
  * Name it `my-server-001`
* Update the Launch Configuration to add `my-server-000` to ASG
  * Create a copy of the exising launch configuration.
  * Now choose the AMI from the `Choose AMI` wizard
  * In the `Configure Details` make sure to change the name.
* Choose the security group.
* Edit the Auto Scaling Group and choose the new Launch Configuration.
* Create a LB
  * Choose application LB
  * Choose multiple AZ's
  * Configure the Security Group
  * Configure Routing
    * Name it `tg-prod`
  * Register Target Groups
* Add Listener to LB
  * Go with default for now.

### Register a Domain

* Go to service
* Route53
* Domain Registration
* Register Domain
* Choose a domain name and buy it
* It will take some time for the new domain to get activated
* Create Record Set by choosing the `Alias Target`

**Securing it**
* Go to ACM
* Click Provision Certificate
* Give the domain name and sub-domains
* Select DNS validation
* Create record in Route 53
* Go to EC2
* Go to Listener
* Add Listener
* Protocol
  * HTTPS
  * Forward to `tg-prod`
  * Default SSL certificate, paste the certificate
  * Go to Description
  * Go to Security Group
  * Create an `inbound` rules 
  * select `HTTPS` 