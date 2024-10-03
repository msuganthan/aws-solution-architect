<!-- TOC -->
* [EBS volumes](#ebs-volumes)
  * [To migrate an EBS volume AZ](#to-migrate-an-ebs-volume-az)
  * [Root EBS Volumes of instances get terminated by default if the EC2 instance gets terminated(you can disable that)](#root-ebs-volumes-of-instances-get-terminated-by-default-if-the-ec2-instance-gets-terminatedyou-can-disable-that)
* [EFS](#efs)
<!-- TOC -->

### EBS volumes

* One instance(except multi-attach io1/io2)
* are locked at the AZ level
* gp2: IO increases if the disk size increases.
* gp3 & io 1: can increase IO independently

#### To migrate an EBS volume AZ

* Take a snapshot
* Restore the snapshot to another AZ
* EBS backups use IO and you shouldn't run them while your application is handling a lot of traffic 

#### Root EBS Volumes of instances get terminated by default if the EC2 instance gets terminated(you can disable that)

### EFS

* Mounting 100s of instances across AZ
* EFS share website files 
* Only for linux Instances(POSIX)

* EFS has a higher price point than EBS
* Can leverage storage tiers for cost savings
