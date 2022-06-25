### Elastic Load Balancer

* Distributes incoming application traffic across multiple targets, such as Amazon EC2 instances, containers, IP addresses and Lambda functions.

**Intro**

* Load Balancers can be **physical hardware or virtual software** that accepts incoming traffic, and then distributes the traffic to multiple targets. This can balance that load via different rules. These rules vary based on types of load balancers.
* **Elastic Load Balancer(ELB)** is the AWS solution for load balancing traffic, and there are 3 types available.
  * Application Load Balancer ALB(HTTP/HTTPS)
  * Network Load Balancer NLB(TCP/UDP)
  * Classic Load Balancer CLB(Legacy)

**The Rules of Traffic**

**Listeners:** 

* **Incoming traffic is evaluated against listeners.** Listeners evaluate any traffic that is matches the Listener's port. For Classic Load Balancer, EC2 instances are directly registered to the Load Balancer.

**Rules:(Not available for Classic Load Balancer)**

* **Listeners will then invoke rules to decide what to do with the traffic.** Generally the next step is to forward traffic to a Target Group.

**Target Groups:(Not available for Class Load Balancer)**
  
* EC2 instances are registered as targets to a Target Group.

* For **Application Load Balancer(ALB) or Network Load Balancer(NLB)** traffic is sent to the Listeners. When the port matches it then checks the rules what to do. The rules will forward the traffic to a Target Group. The target group will evenly distribute the traffic to instances registered to that target group.

<img src="../images/elb/the-rules-of-traffic.png" alt="">

<img src="../images/elb/rules-of-traffic-1.png" alt="">

* For **Classic Load Balancer(CLB)** traffic is sent to the listeners. When the **port matches it then forwards the traffic to any EC2 instances** that are registered to the Classic Load Balancer. CLB does not allow you to apply rules to listeners.

<img src="../images/elb/classic-rules-of-traffic.png" alt="">

**Application Load Balancer(ALB)**

* This is designed to balance **HTTP and HTTPs** traffic.
* They operate at **Layer 7 of the OSI model**
* ALB has a feature called **Request Routing** which allows you to add routing rules to your listeners based on the HTTP protocol.
* Web Application Firewall can be attached to ALB.
* Great for Web Applications.

<img src="../images/elb/application-lb-layer.png" alt="">

**Network Load Balancers**

* It is designed to balance **TCP/UDP.** 
* They **operate at Layer 4 of the OSI Model**
* Can handler **millions of requests per second** while still maintaining extremely low latency.
* Can perform Cross-Zone Load Balancing
* Great for multiplayer video games or when network performance is critical.

<img src="../images/elb/network-lb-layer.png" alt="">

**Classic Load Balancer**

* It was AWS first load balance(legacy)
* Can balance **HTTP, HTTPS or TCP** traffic(but not at the same time)
* It can use **Layer 7-specific features** such as sticky sessions.
* It can also use **strict Layer 4** balancing for purely TCP applications.
* Can perform Cross-Zone Load Balancing
* It will respond with a 504 error(timeout) of the underlying application is not responding(at the web server or database level)
* Not recommended for use, instead use NLB or ALB 

<img src="../images/elb/classic-lb-layers.png" alt="">

**Sticky Session**

* Is an advanced load balancing method that allows you to bind a user's session to a specific EC2 instance.
* Ensures all requests from that session are **sent to the same instance**
* Typically utilized with a **Classic Load Balancer**
* **Can be enabled for ALB** though can only be set on a **Target Group not individual EC2 instances**.
* Useful when specific **information is only stored locally on a single instance**

<img src="../images/elb/sticky-sessions.png" alt="">

**X-Forwarded-For(XFF) Header**

* If you **need the IPv4 address** of a user, check the **X-Forwarded-For** header.
* The **X-Forwarded-For(XFF)** header is a command method for identifying the **originating IP address** of a client connecting to a web server thought an HTTP proxy or a load balancer.

**Health Checks**

* Instances that are monitored by the Elastic Load Balancer(ELB) report back Health Checks as **InService, or OutOfService**
* Health Checks communicate directly with the instance to determine its state.
* ELB **does not terminate unhealthy instance.** It will just redirect traffic to health instances.
  
<img src="../images/elb/health-checks.png" alt="">

* For ALB and NLB the Health checks are found on the **Target Group**

<img src="../images/elb/nlb-alb-health-checks.png" alt="">

**Cross-Zone Load Balancing**

* Only for **Classic** and **Network** Load Balancer

**Cross-Zone Load Balancing _Enabled_** requests are distributed evenly across the instances in all enabled Availability Zones.

<img src="../images/elb/enable-cross-zone-lb.png" alt="">

**Cross-Zone Load Balancing _Disabled_** requests are distributed evenly across the instances **in only** it Availability Zone.

<img src="../images/elb/disable-cross-zone-lb.png" alt="">

**Request Routing**

* Apply rules to incoming request and the **forward** or **redirect** traffic.
  * Host Header
  * Source IP
  * Path
  * Http Header
  * Http Header method
  * Query String

<img src="../images/elb/request-routing.png" alt="">
<img src="../images/elb/request-routing-rules.png" alt="">

**CheatSheet**

* There are three ELBs: **Network, Application, and Classic** Load Balancer
* A Elastic Load Balancer must have **at least two** Availability Zones.
* Elastic Load Balancers **cannot go cross-region.** You must create one per region.
* ALB has **Listeners, Rules and Target Groups** to route traffic.
* NLB use **Listeners and Target Groups to route traffic**
* CLB use **Listeners and EC2 instances are directly registered** as targets to CLB
* Application Load Balancer is for HTTP(S) traffic and the name implies it is good for Web Applications.
* Network Load Balancer is for TCP/UDP is good for network throughput eg. Video Games
* Classic Load Balancer is legacy and It's recommended to use ALB or NLB
* Use X-Forwarded-For to get original IP of incoming traffic passing through ELB
* You can attach Web Application Firewall to ALB but to NLB or CLB
* You can attach Amazon Certification Manager SSL to any of the Elastic Load Balancers for SSL
* **ALB has advanced Request Routing riles where you can route based on subdomain header, path and other HTTP(S) information**
* Sticky Sessions can be enabled for CLB or ALB and sessions are remembered via Cookie.