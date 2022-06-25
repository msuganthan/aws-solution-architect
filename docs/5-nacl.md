### Network Access Control List(NACLs)

* An (optional) layer of security that acts as a firewall for controlling traffic **in and out** of subnet(s).
* NACL acts as a virtual firewall at the **subnet level**.
* VPCs automatically get a default NACL.

<img src="../images/nacl/nacl-dashboard.png" alt="">

**Use case**

* We determine there is a malicious actor at a specific IP address is trying to access our instances, so we block their IP.
* We never need to SSH into instances, so we add DENY for these subnets. This is just an additional measure in case our Security Group SSH port is left open.

<img src="../images/nacl/nacl-use-case.png" alt="">
