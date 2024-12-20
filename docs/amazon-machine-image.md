<!-- TOC -->
  * [Amazon Machine Image(AMI): A template to configure new instances.](#amazon-machine-imageami-a-template-to-configure-new-instances)
    * [AMI Process(from an EC2 instance)](#ami-processfrom-an-ec2-instance)
    * [An AMI holds the following information:](#an-ami-holds-the-following-information)
  * [AMI Use cases](#ami-use-cases)
  * [AMI Marketplace:](#ami-marketplace)
  * [Creating an AMI:](#creating-an-ami)
  * [Choosing an AMI](#choosing-an-ami)
    * [Amazon Machine Images can be selected based on:](#amazon-machine-images-can-be-selected-based-on)
  * [Copying an AMI](#copying-an-ami)
<!-- TOC -->

### Amazon Machine Image(AMI): A template to configure new instances.

* AMI is a customization of an EC2 instance
  * You add your own software, configuration, operating system, monitoring.
  * Faster boot/configuration time because all your **software is pre-packaged**.
* AMI is built for a **specific region** (and can be copied across regions)
* You can launch EC2 instances from:
  * **A Public AMI**: AWS provided
  * **You own AMI**: you make and maintain them yourself
  * **An AWS Marketplace AMI**: an AMI someone else made (and potentially sells)

#### AMI Process (from an EC2 instance)

* Start an EC2 instance and customize it
* Stop the instance(for data integrity)
* Build an AMI—this will also create EBS snapshots
* Launch instances from other AMIs

#### An AMI holds the following information:

* A template for the root volume for the instance(**EBS Snapshot or Instance Store template**) e.g. an operating system, an application server and applications.
* Launch permissions that control which AWS accounts can use the AMI to launch instances.
* A block device mapping that specifies the volumes to attach to the instance which it's launched.
* AMIs are **Region Specific!**

<img src="../images/ami/ami.png" alt="">

### AMI Use cases

AMIs help you keep incremental changes to your OS, application code and system packages

* web-server-000: Ruby, Node, Postgres Client Installed
* web-server-001: Redis for Sidekiq Installed
* web-server-002: ImageMagick for Image Processing Installed
* web-server-003: CloudWatch Agent Installed

* Using **System Manager Automation** you can routinely patch your AMIs with security updates and bake those AMIs.
* AMIs are used with **LaunchConfigurations.** When you want to roll out updates to multiple instances you make a copy of your LaunchConfiguration with new AMI.

### AMI Marketplace:

* The AWS Marketplace lets you **purchase subscription** to vendor maintained AMIs.

<img src="../images/ami/aws-market-place.png" alt="">

### Creating an AMI:

* You can **create an AMI** from an existing EC2 instance that's either **running** or **stopped**.

<img src="../images/ami/create-ami.png" alt="">

### Choosing an AMI

* AWS has hundreds of AMIs you can search and select from.
* **Community AMI are** free AMIs maintained by the community
* **AWS Marketplace** free and paid AMIs maintained by vendors.

<img src="../images/ami/choosing-an-ami.png" alt="">

#### Amazon Machine Images can be selected based on:
* Region
* Operating System
* Architecture(32-bit or 64-bit)
* Launch Permissions
* Root Device Volume
  * Instance Store(Ephemeral Storage)
  * EBS Backed Volumes

<img src="../images/ami/choosing-an-ami-based-on.png" alt="">

AMIs are categorized as either backed by Amazon EBS, or backed by Instance Store.

<img src="../images/ami/choosing-an-ami-device-type.png" alt="">

### Copying an AMI

* AMIs are region specific. If you want to use an AMI from another region. You need to **Copy the AMI** and then select the destination region.

<img src="../images/ami/copying-ami.png" alt="">