### Blocking an IP Address

<img src="../../images/solutions/blocking-ip-address/blocking-ip-addr-simple-arch.png" alt="Blocking an IP address in simple arch">

* Using NACL can block IP addresses
* Using Security Group one can allow a subset of IP of address
* Optionally using firewall software you block request from client

<img src="../../images/solutions/blocking-ip-address/blocking-ip-addr-in-alb-arch.png" alt="Blocking an IP address - with an ALB">

* Using NACL can block IP addresses
* Using Security Group one can allow a subset of IP of address

<img src="../../images/solutions/blocking-ip-address/blocking-ip-addr-in-alb-cloud-front.png" alt="Blocking an IP address - ALB, CloudFront WAF">

* Here NACL is not useful
* Use CloudFront Geo Restriction to block a country
* Can use WAF for IP address filtering