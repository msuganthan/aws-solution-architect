Security Groups Introduction:
=============================
	
	A virtual firewall that controls the traffic to and from EC2 instances.

	Security Group acts as a virtual firewall at the instances level.

	Security Groups are associated with EC2 instances.

	Each Security Group contains a set of rules that filter traffic coming into(inbound) and out of(outbound) EC2 instances.

	There are no `Deny` rules. All traffic is block by default unless a rule specifically allow it.

	Multiple instances accross multiple subnets can belong to a Security Group.

Security Group Use cases:
=========================

	You can specify the source to be an IP range or A specific ip(/32 is a specific IP address)

	You can specify the source to be another security group.

	An instance can belong to mulitple Security Group, and rules are permissive. Meaing if you have one security group which has no Allow and you can add an allow to another than it will Allow.

Limits:
========

	You can have uptp 10, 000 Security Groups in a Region.

	You can have 60 inbound rules and 60 outbound rules per security group.
	
	16 Security Groups per Elastic Network Interface(ENI)(default is 5)

	

CheatSheet:
===========

	* Security Groups are Stateful(if any traffic group is inbound it is allowed Outbound)

	* EC2 Instances can belong to multiple security groups.

	* Security Groups can contain multiple EC2 instances.

	* You cannot block specific IP addresses with Security Groups, for this you would need a Network Access Control List(NACL).

	
