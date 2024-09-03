### CIDR - IPv4

* **Classless Inter-Domain Routing** - a method of allocating IP addresses
* Used in **Security Groups** rules and **AWS networking** in general
* They help in define an IP address range:
  * We've seen ww.xx.yy.zz/32 => one IP
  * We've seen 0.0.0.0/0 => all IPs

#### Understanding CIDR - IPv4

* A CIDR consists of two component
* Base IP
  * Represents an IP contained in the range(XX.XX.XX.XX)
  * Example: 10.0.0.0, 192.168.0.0,...
* Subnet Mask
  * Define how many bits can change in the IP
  * Example: /0, /24, /32
  * Can take two forms:
    * /8 <=> 255.0.0.0
    * /16 <=> 255.255.0.0
    * /24 <=> 255.255.255.0
    * /32 <=> 255.255.255.255

#### Understanding CIDR - Subnet Mask

* The Subnet Mask basically allows part of the underlying IP to get additional next values from the base IP

* 192.168.0.0/32 => allows for **1** IP (2^0)       ==> 192.168.0.0
* 192.168.0.0/31 => allows for **2** IP (2^1)       ==> 192.168.0.0 -> 192.168.0.1
* 192.168.0.0/30 => allows for **4** IP (2^2)       ==> 192.168.0.0 -> 192.168.0.3
* 192.168.0.0/29 => allows for **8** IP (2^3)       ==> 192.168.0.0 -> 192.168.0.7
* 192.168.0.0/28 => allows for **16** IP (2^4)      ==> 192.168.0.0 -> 192.168.0.15
* 192.168.0.0/27 => allows for **32** IP (2^5)      ==> 192.168.0.0 -> 192.168.0.31
* 192.168.0.0/26 => allows for **64** IP (2^6)      ==> 192.168.0.0 -> 192.168.0.63
* 192.168.0.0/25 => allows for **128** IP (2^7)     ==> 192.168.0.0 -> 192.168.0.127
* 192.168.0.0/24 => allows for **256** IP (2^8)     ==> 192.168.0.0 -> 192.168.0.255
* ...
* 192.168.0.0/16 => allows for **65,536** IP (2^16) ==> 192.168.0.0 -> 192.168.255.255
* ...
* 192.168.0.0/0  => allows for All IPs              ==> 0.0.0.0 => 255.255.255.255

##### Quick Memo
  _Octets_
__.__.__.__

* **/32** - no octet can change
* **/24** - last octet can change
* **/16** - last 2 octets can change
* **/8**  - last 3 octets can change
* **/0**  - all octets can change

#### Public vs Private IP(IPv4)

* The Internet Assigned Numbers Authority(IANA) established certain blocks of IPv4 addresses for the use of private(LAN) and public(Internet) addresses

* **Private IP** can only allow certain values:
  * 10.0.0.0 - 10.255.255.255 (10.0.0.0/8) <- in big networks
  * 172.16.0.0 - 172.31.255.255 (172.16.0.0/12) <- AWS default VPC in that range
  * 192.168.0.0 - 192.168.255.255 (192.168.0.0/16) <- e.g., home networks

* All the rest of the IP addresses on the Internet as Public

### Default VPC Overview

* All new AWS accounts have a default VPC
* New EC2 instances are launched into the default VPC if no subnet is specified
* Default VPC has Internet connectivity and all EC2 instances inside it have public IPv4 addresses.
* We also get a public and a private IPv4 DNS names

### VPC in AWS - IPv4

* VPC = Virtual Private Cloud
* You can have multiple VPCs in an AWS region(max. 5 per region - soft limit)
* Max. CIDR per VPC is 5, for each CIDR:
  * Min. size is /28 (16 IP addresses)
  * Max. size is /16 (65536 IP addresses)
* Because VPC is private, only the Private IPv4 ranges are allowed:
  * 10.0.0.0 - 10.255.255.255 (10.0.0.0/8)
  * 172.16.0.0 - 172.31.255.255 (172.16.0.0/12)
  * 192.168.0.0 - 192.168.255.255 (192.168.0.0/16)

* **Your VPC CIDR should NOT overlap with your other networks (e.g., corporate)**

### Adding Subnet(IPv4)

* AWS reserves **5 IP addresses(first 4 & last 1)** in each subnet
* These 5 IP addresses are not available for use and can't be assigned to an EC2 instance
* Example: if CIDR block 10.0.0.0/24, then reserved IP addresses are:
  * 10.0.0.0 - Network Address
  * 10.0.0.1 - reserved by AWS for the VPC routed
  * 10.0.0.2 - reserved by AWS for mapping to Amazon-provided DNS
  * 10.0.0.3 - reserved by AWS for future use
  * 10.0.0.255 - Network Broadcast Address. AWS does not support broadcast in a VPC, therefore the address is reserved.
* **Exam Tip,** if you need 29 IP addresses for EC2 instances:
  * You can't choose a subnet of size /27 (32 IP addresses, 32 - 5 = 27 < 29)
  * You need to choose a subnet of size /26 (64 IP addresses, 64 - 5 = 59 > 29)

### Internet Gateway(IGW)

* Allow resources(e.g., EC2 instances) in a VPC connect to the internet
* It scales horizontally and is highly available and redundant
* Must be created separately from a VPC
* One VPC can only be attached to one IGW and vice versa

* Internet Gateways on their own do not allow Internet access...
* Route tables must also be edited!!!

### Bastion Hosts

* We can use a Bastion Host to SSH into our private EC2 instances
* The bastion is in the public subnet which is then connected to all other private subnets
* **Bastion Host security group must allow** inbound from the internet on port 22 from restricted CIDR, for example the _public CIDR_ of your corporation
* **Security Group of the EC2 Instances** must allow the Security Group of the Bastion Host, or the _private IP_ of the Bastion Host.

<img src="../images/net-working-vpc/bastion-hosts.png" alt="Bastion Hosts">

### NAT Instance(outdated, but still at the exam)

* Network Address Translation
* Allow EC2 instances in private subnets to connect to the internet
* Must be launched in a public subnet
* Must disable EC2 setting: **Source / destination check**
* Must have Elastic IP attached to it.
* Route Tables must be configured to route traffic from private subnets to the NAT Instance

<img src="../images/net-working-vpc/nat-instance.png" alt="Nat Instance">

#### NAT Instance - Comments

* Pre-configured Amazon Linux AMI is available
  * Reached the end of standard support on December 31, 2020
* Not highly available / resilient setup out of the box
  * You need to create an ASG in multi-AZ + resilient user-data script
* Internet traffic bandwidth depends on EC2 instance type
* You must manage Security Group & rules
  * Inbound:
    * Allow HTTP / HTTPS traffic coming from Private Subnets
    * Allow SSH from your home network(access is provided through Internet Gateway)
  * Outbound:
    * Allow HTTP / HTTPS traffic to the Internet

### NAT Gateway

* AWS-managed NAT, higher bandwidth, high availability, no administration
* Pay per hour of usage and bandwidth
* NATGW is created in a specific AZ, uses an Elastic IP
* Can't be used by EC2 instance in the same subnet(only from other subnets)
* Requires an IGW(Private Subnet => NATGW => IGW)
* 5 Gbps of bandwidth with automatic scaling up to 100 Gbps
* No Security Group to manage / required

#### NAT Gateway with High Availability

* NAT Gateway is resilient within a single Availability Zone.
* Must create multiple **NAT Gateways** in **multiple AZs** for fault-tolerance
* There is no cross-AZ failover needed because if an AZ goes down it doesn't need NAT

<img src="../images/net-working-vpc/nat-gateway-wtih-high-avail.png" alt="NAT Gateway with High Availability">

### NAT Gateway vs NAT Instance

|                     | NAT Gateway                                    | NAT Instance                                      |
|---------------------|------------------------------------------------|---------------------------------------------------|
| Availability        | Highly available with AZ(create in another AZ) | Use a script to manage failover between instances |
| Bandwidth           | Upto 100 Gbps                                  | Depends on EC2 instance type                      |
| Maintenance         | Managed by AWS                                 | Managed by you(e.g., software, OS patches)        |
| Cost                | Per hour & amount of data transferred          | Per hour, EC2 instance type and size, + network $ |
| Public IPv4         | Yes                                            | Yes                                               |
| Private IPv4        | Yes                                            | Yes                                               |
| Security Groups     | No                                             | Yes                                               |
| Use as Bastion Host | No                                             | Yes                                               |


### Security Groups & NACL

<img src="../images/net-working-vpc/incoming-outgoing-request-using-nacl.png" alt="Incoming and Outgoing Request using NACL">

* NACL are like a firewall which control traffic from and to **subnets**
* **One NACL per subnet,** new subnets are assigned the **Default NACL**
* You define **NACL Rules**:
  * Rules have a number(1-32766), higher precedence with a lower number
  * First rule match will drive the decision
  * Example: if you define #100 ALLOW 10.0.0.10/32 and #200 DENY 10.0.0.10/32, the IP address will be allowed because 100 has a higher precedence over 200
  * The last rule is an asterisk (*) and denies a request in case of no rule match.
  * AWS recommends adding rules by increment of 100
* Newly created NACLs will deny everything
* NACL are a great way of blocking a specific IP address at the subnet level

#### Default NACL

* Accepts everything inbound/outbound with the subnets it's associated with
* Do NOT modify the Default NACL, instead create custom NACLs

##### Inbound Rules

| Rule# | Type             | Protocol | Port Range | Source    | Allow/Deny |
|-------|------------------|----------|------------|-----------|------------|
| 100   | All IPv4 Traffic | All      | All        | 0.0.0.0/0 | ALLOW      |
| *     | All IPv4 Traffic | All      | All        | 0.0.0.0/0 | DENY       |

##### Outbound Rules

| Rule# | Type             | Protocol | Port Range | Destination | Allow/Deny |
|-------|------------------|----------|------------|-------------|------------|
| 100   | All IPv4 Traffic | All      | All        | 0.0.0.0/0   | ALLOW      |
| *     | All IPv4 Traffic | All      | All        | 0.0.0.0/0   | DENY       |

#### Ephemeral Ports

* For any two endpoints to establish a connection, they must use ports
* Clients connect to a **defined port,** and expect a response on an **ephemeral port**
* Different Operating Systems use different port ranges, examples:
  * IANA & MS Windows 10 => 49152 - 65535
  * Many Linux Kernels => 32768 - 60999

<img src="../images/net-working-vpc/ephemeral-ports.png" alt="Ephemeral Ports">

#### NACL with Ephemeral Ports

<img src="../images/net-working-vpc/nacl-with-ephemeral-ports.png" alt="NACL with Ephemeral Ports">

<img src="../images/net-working-vpc/nacl-for-each-target-subnet.png" alt="NACL with each target subnet">

#### Security Group vs NACLs

| Security Group                                                             | NACL                                                                                                     |
|----------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| Operates at the instance level                                             | Operates at the subnet level                                                                             |
| Supports allow rules only                                                  | Support allow rules and deny rules                                                                       |
| Stateful: return traffic is automatically allowed, regardless of any rules | Stateless: return traffics must be explicitly allowed by rules                                           |
| All rules are evaluated before deciding whether to allow traffic           | Rules are evaluated in order(lowest to highest) when deciding whether to allow traffic, first match wins |
| Applies to an EC2 instance when specified by someone                       | Automatically applies to all EC2 instances in the subnet that it's associated with                       |

### VPC Peering

* Privately connect two VPCs using AWS network
* Make them behave as if they were in the same network
* Must not have overlapping CIDRs
* VPC Peering connection is **NOT transitive** (must be established for each VPC that need to communicate with one another)
* You must update route tables in each VPC's subnet to ensure EC2 instances can communicate with each other

<img src="../images/net-working-vpc/vpc-peering.png" alt="VPC Peering">

#### VPC Peering - Good to know

* You can create VPC Peering connection between VPCs in **different AWS accounts/regions**
* You can reference a security group in a peered VPC(works cross account - same region)

### VPC Endpoints(AWS PrivateLink)

* Every AWS service is publicly exposed (public URL)
* VPC Endpoints(powered by AWS PrivateLink) allows you to connect to AWS services using a **private network** instead of using the public network
* They're redundant and scale horizontally
* They remove the need of IGW, NATGW,... to access AWS services
* In case of issues:
  * Check DNS Setting Resolution in your VPC
  * Check Route Tables

<img src="../images/net-working-vpc/vpc-endpoints.png" alt="VPC Endpoints">

#### Types of Endpoints

* _Interface Endpoints(powered by PrivateLink)_
  * Provisions an ENI(private IP address) as an entry point(must attach a Security Group)
  * Supports most AWS services because there is an ENI
  * $ per hour + $ per GB of data processed
  
* _Gateway Endpoints_
  * Provisions a gateway and must be used **as a target in a route table(does not use security groups)**
  * Supports both **S3** and **DynamoDB**
  * Free

<img src="../images/net-working-vpc/type-of-endpoints.png" alt="Types of endpoints">

#### Gateway or Interface Endpoint for S3?

* **Gateway is most likely going to be preferred all the time at the exam**
* Cost: free for Gateway, $ for interface endpoint
* Interface Endpoint is preferred access is required from on-premises(Site to Site VPN or Direct Connect), a different VPC or a different region

<img src="../images/net-working-vpc/gateway-or-interface-endpoints-for-s3.png" alt="Gateway or Interface Endpoint for S3">