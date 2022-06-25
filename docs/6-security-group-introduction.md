### Security Groups Introduction:
	
* A **virtual firewall** that controls the traffic **to and from EC2 instances**.

* Security Group acts as a **virtual firewall** at the instances level. 
* Security Groups are associated with EC2 instances. 
* Each Security Group contains a set of rules that filter traffic coming into(inbound) and out of(outbound) EC2 instances.

<img src="../images/security-group/security-group-rules.png" alt="">

* There are no `Deny` rules. **All traffic is block by default** unless a rule specifically allow it.
* Multiple instances across multiple subnets can belong to a Security Group.

<img src="../images/security-group/security-group-multiple-instances.png" alt="">

**Security Group Use cases:**

* You can specify the source to be an **IP range or a specific ip**(/32 is a specific IP address)

<img src="../images/security-group/security-group-by-ip-addr.png" alt="">

* You can specify the source to be **another security group**.

<img src="../images/security-group/security-grp-by-another-sec-grp.png" alt="">

* An instance can **belong to multiple Security Group**, and rules are permissive. Meaning if you have one security group which has no Allow and you can add an allow to another than it will Allow.

<img src="../images/security-group/sec-grp-override.png" alt="">

**Limits:**

* You can have **upto 10, 000 Security Groups in a Region**. 
* You can have **60 inbound rules** and **60 outbound rules** per security group. 
* **16 Security Groups** per Elastic Network Interface(ENI)(default is 5)