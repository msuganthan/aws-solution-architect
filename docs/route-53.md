<!-- TOC -->
  * [What is DNS?](#what-is-dns)
    * [DNS Terminologies](#dns-terminologies)
    * [How DNS Works](#how-dns-works)
  * [Amazon Route 53](#amazon-route-53-)
    * [Route 53 - Records](#route-53---records)
    * [Route 53 - Record Types](#route-53---record-types)
    * [Route 53 - Hosted Zones](#route-53---hosted-zones)
    * [Route 53 - Records TTL(Time to Live)](#route-53---records-ttltime-to-live)
  * [CNAME vs Alias](#cname-vs-alias)
    * [Route 53 - Alias Records](#route-53---alias-records)
  * [Route 53 - Alias Records Targets](#route-53---alias-records-targets)
  * [Route 53 - Routing Policies](#route-53---routing-policies)
    * [Routing Policies - Simple](#routing-policies---simple)
    * [Routing Policies - Weighted](#routing-policies---weighted)
    * [Routing Policies - Latency](#routing-policies---latency)
    * [Route 53 - Health Checks](#route-53---health-checks)
    * [Health Checks - Monitor an Endpoint](#health-checks---monitor-an-endpoint)
    * [Route 53 - Calculated Health Checks](#route-53---calculated-health-checks)
    * [Health Checks - Private Hosted Zones](#health-checks---private-hosted-zones)
  * [Routing Policies - Failover(Active-Passive)](#routing-policies---failoveractive-passive)
    * [Routing Policies - Geolocation](#routing-policies---geolocation)
    * [Routing Policies - GeoLocation](#routing-policies---geolocation-1)
    * [Geo proximity Routing Policy](#geo-proximity-routing-policy)
    * [Routing Policies - IP-based Routing](#routing-policies---ip-based-routing)
    * [Routing Policies - Multi-Value](#routing-policies---multi-value)
  * [Domain Registrar vs DNS Service](#domain-registrar-vs-dns-service)
    * [GoDaddy as Registrar & Route 53 as DNS Service](#godaddy-as-registrar--route-53-as-dns-service)
  * [Beanstalk Overview](#beanstalk-overview)
    * [Elastic Beanstalk - Components](#elastic-beanstalk---components)
    * [Web Server Tier vs Worker Tier](#web-server-tier-vs-worker-tier)
    * [Elastic Beanstalk Deployment Modes](#elastic-beanstalk-deployment-modes)
<!-- TOC -->

### What is DNS?

* Domain Name System which translates the human friendly hostname into the machine IP addresses
* www.google.com => 171.217.18.36
* DNS is the backbone of the internet
* DNS uses hierarchical naming structure
  * .com
  * example.com
  * www.example.com
  * api.example.com
  
#### DNS Terminologies

* Domain Registrar: Amazon Route 53, GoDaddy,...
* DNS Records: A, AAAA, CNAME, NS,...
* Zone File: contains DNS records
* Name Server: resolves DNS queries(Authoritative or Non-Authoritative)
* Top Level Domain (TLD): .com, .us, .in, .gov, .org, ...
* Second Level Domain (SLD): amazon.com, google.com, ...

<img src="../images/route53/dns-terminologies.png" alt="DNS Terminologies">

#### How DNS Works

<img src="../images/route53/dns-work.png" alt="How DNS Works">

### Amazon Route 53 

* A highly available, scalable, fully managed and Authoritative DNS
  * Authoritative = the customer (you) can update the DNS records
* Route 53 is also a Domain Registrar
* Ability to check the health of your resources
* The only AWS service which provides 100% availability SLA
* Why Route 53? 53 is a reference to the traditional DNS port.

<img src="../images/route53/amazon-route-53.png" alt="Amazon Route 53">

#### Route 53 - Records

* How you want to route traffic for a domain
* Each record contains:
  * **Domain/subdomain Name** - e.g., example.com
  * **RecordType** - e.g., A or AAAA
  * **Value** - e.g., 12.34.56.78
  * **Routing Policy** - how Route 53 respond to queries
  * **TTL** - amount of time the record cached at DNS Resolvers
* Route 53 supports the following DNS record types:
  * (must know) A / AAAA / CNAME / NS
  * (advanced) CAA / DS / MX / NAPTR / PTR / SOA / TXT / SPF / SRV

#### Route 53 - Record Types

* **A** - maps a hostname to IPV4
* **AAAA** - maps a hostname to IPV6
* **CNAME** - maps a hostname to another hostname
  * The target is a domain name which must have an A or AAAA record
  * Can't create a CNAME record for the top node of a DNS namespace(Zone Apex)
  * Example: you can't create for example.com, but you can create for www.example.com
* **NS** - Name Servers for the Hosted Zone

#### Route 53 - Hosted Zones

* A container for records that define how to route traffic to a domain and its subdomains
* **Public Hosted Zones** - contains records that specify how to route traffic on the Internet(public domain names) application1.mypublicdomain.com
* **Private Hosted Zones** - contains records that specify how to route traffic within one or more VPCs (private domain names) application1.company.internal

* You pay $0.50 per month per hosted zone

<img src="../images/route53/public-vs-private-hosted-zones.png" alt="Public vs Private Hosted zones">

#### Route 53 - Records TTL(Time to Live)

* High TTL - e.g., 24 hr
  * Less traffic on Route 53
  * Possibly outdated record
* Low TTL - e.g, 60 sec
  * More traffic on Route 53($$)
  * Records are outdated for less time
  * Easy to change records
* Except for Alias records, TTL is mandatory for each DNS record

<img src="../images/route53/route-53-ttl.png" alt="Route 53 TTL">

### CNAME vs Alias

* AWS Resources(Load Balancer, CloudFront...) expose an AWS hostname:
  * lb1-1234.us-east-1.elb.amazonaws.com and you want myapp.mydomain.com

* CNAME:
  * Point a hostname to any other hostname(app.mydoamin.com => blabla.anything.com)
  * ONLY FOR NON ROOT DOMAIN(something.mydomain.com)
* Alias:
  * Points a hostname to an AWS Resource(app.mydomain.com => blabla.amazonaws.com)
  * Works for ROOT DOMAIN and NON ROOT DOMAIN(aka mydomain.com)
  * Free of charge

#### Route 53 - Alias Records

* Maps a hostname to an AWS resource
* An extension to DNS functionality
* Automatically recognizes changes in the resource's IP addresses
* Unlike CNAME, it can be used for the top node of a DNS namespace(Zone Apex) e.g: example.com
* Alias Record is always of type A/AAAA for AWS resources(IPv4/IPv6)
* **You can't set the TTL**

<img src="../images/route53/route-53-alias-records.png" alt="Alias records">

### Route 53 - Alias Records Targets

* Elastic Load Balancers
* CloudFront Distributions
* API Gateway
* Elastic Beanstalk environments
* S3 Websites
* VPC Interface Endpoints
* Global Accelerator accelerator
* Route 53 record in the same hosted zone

* **You cannot set an ALIAS record for an EC2 DNS name**

<img src="../images/route53/route-53-alias-records-targets.png" alt="Alias Records Target">

### Route 53 - Routing Policies

* Define how Route 53 responds to DNS queries
* Don't get confused by the word "Routing"
  * It's not the same as Load balancer routing which routes the traffic
  * DNS does not route any traffic, it only responds to the DNS queries
* Route 53 Supports the following Routing Policies
  * Simple
  * Weighted
  * Failover
  * Latency Based
  * Geolocation
  * Multi-Value Answer
  * Geo proximity(using Route 53 Traffic Flow Feature)

#### Routing Policies - Simple

* Typically, route traffic to a single resource
* Can specify multiple values in the same record
* **If multiple values are returned, a random one is chosen by the client**
* When Alias enabled, specify only one AWS resource
* Can't be associated with Health Checks

<img src="../images/route53/route-53-routing-policies-simple.png" alt="Routing Policies Simple">

#### Routing Policies - Weighted

* Control the % of the requests that go to each specific resource
* Assign each record a relative weight
  * traffic(%) = Weight for a specific record / Sum of all the weights for all records
* DNS records must have the same name and type
* Use-cases: load balancing between regions, testing new application versions...
* **Assign a weight of 0 to a record to stop sending traffic to a resource**
* If all records have weight of 0, the all records will be returned equally.

<img src="../images/route53/route-53-routing-policies-weighted.png" alt="Route 53 - Weighted">

#### Routing Policies - Latency

* Redirected to the resources that has the least latency close to us
* Super helpful when latency for users is a priority
* **Latency is based on traffic between users and AWS regions**
* German users may be directed to the US(if that's the lowest latency)
* Can be associated with Health Checks(has a failover capability)

<img src="../images/route53/route-53-routing-policies-latency.png" alt="Route 53 - Latency">

#### Route 53 - Health Checks

* HTTP Health Checks are only for **public resources**
* Health Check => Automated DNS Failover:
  * Health checks that monitor an  endpoint(application, server, other AWS resource)
  * Health checks that monitor other health checks(Calculated Health Checks)
  * Health checks that monitor CloudWatch Alarms(full control !!!) - e.g., throttles of DynamoDB, alarms on RDS, custom metrics, (helpful for private resources)
* Health Checks are integrated with CW metrics

<img src="../images/route53/route-53-health-checks.png" alt="Health Checks">

#### Health Checks - Monitor an Endpoint

* **About 15 global health checkers will check the endpoint health**
  * Healthy/Unhealthy Threshold - 3(default)
  * Interval - 30 sec (can set to 10 sec - higher cost)
  * Supported protocol: HTTP, HTTPS and TCP
  * If > 18% of health report the endpoint is health, Route 53 considers it **Healthy**. Otherwise, it's **Unhealthy**
  * Ability to choose which location you want Route 53 to use
* Health Checks pass only when the endpoint responds with the 2xx and 3xx status codes
* Health Checks can be setup to pass / fail based on the text in the first **5120 bytes** of the response
* Configure you router/firewall to allow incoming requests from Route 53 Health Checkers

<img src="../images/route53/route-53-monitor-an-endpoint.png" alt="Monitor an endpoint">

#### Route 53 - Calculated Health Checks

* Combine the results of multiple Health Checks into a single Health Check
* You can use **OR**, **AND**, or **NOT**
* Can monitor up to 256 Child Health Checks
* Specify how many of the health checks need to pass to make the parent pass
* Usage: perform maintenance of your website without causing all health checks to fail

<img src="../images/route53/route-53-calculated-health-checks.png" alt="Calculated Health Checks">

#### Health Checks - Private Hosted Zones

* Route 53 health checkers are outside the VPC
* They can't access private endpoints(private VPC or on-premises resources)
* You can create a **CloudWatch Metric** and associate a **Cloudwatch Alarm**, then create a Health Check that checks the alarm itself

<img src="../images/route53/route-53-private-hosted-zones.png" alt="Private hosted zones">

### Routing Policies - Failover(Active-Passive)

<img src="../images/route53/route-53-failover-policies.png" alt="Route 53 Failover policies">

#### Routing Policies - Geolocation

* Different from latency-based
* **This routing is based on user location**
* Specify location by Continent, Country, or by US State(if there's overlapping, more precise location selected)
* Should create a **Default** record(in case there's no match on location)
* Use cases: website localization, restrict content distribution, load balancing,...
* Can be associated with Health Checks

<img src="../images/route53/route-53-routing-policies-geolocation.png" alt="Geolocation">

#### Routing Policies - GeoLocation

* Different from latency based
* This routing is based on user location
* Specify location by Continent, Country or by US state(if there's overlapping, most precise location selected)
* Should create a "Default" record(in case there's no match on location)
* Use cases: website localization, restrict content distribution, load balancing,...
* Can be associated with Health Checks

<img src="../images/route53/route-53-geo-location.png" alt="Geo Location">

#### Geo proximity Routing Policy

* Route traffic to your resources based on the geographic location of users and resources
* Ability to shift more traffic to resource based on the defined bias:
  * To expand(1 to 99) - more traffic to the resource
  * To shrink(-1 to -99) - less traffic to the resource

* Resource can be:
  * AWS resources(specify AWS region)
  * Non-AWS resource(specify Latitude and Longitude)
* You must use Route 53 traffic flow(advanced) to use this feature.

<img src="../images/route53/route-53-geo-proximity-routing.png" alt="Geo proximity routing">

* Higher bias in us-east-1

<img src="../images/route53/route-53-geo-proximity-routing-higher-bias.png" alt="Higher bias">

#### Routing Policies - IP-based Routing

* Routing is based on client's IP addresses
* You provide a list of CIDR for your clients and the corresponding endpoints/locations user-IP-to-endpoint mappings)
* Use-cases: Optimize performance, reduce network costs...
* Example: route end users from a particular ISP to a specific endpoint

<img src="../images/route53/route-53-ip-based-routing.png" alt="IP based routing">

#### Routing Policies - Multi-Value

* Use when routing traffic to multiple resources
* Route 53 return multiple values/resources
* Can be associated with Health Checks(return only values for health resources)
* Up to 8 healthy records are return for each Multi-Value query
* Multi-Value is not a substitute for having an ELB(because it is a client side balancing)

<img src="../images/route53/route-53-multi-value-policy.png" alt="Multi Value policy">


### Domain Registrar vs DNS Service

* You buy or register your domain name with a Domain Registrar typically by paying annual charges(e.g GoDaddy, Amazon registrar Inc.)
* The Domain Registrar usually provides you with a DNS service to manage your DNS records
* But you can use another DNS service to manage you DNS records
* Example: purchase the domain from GoDaddy and use Route 53 to manage your DNS records

<img src="../images/route53/route-53-go-daddy-to-amazon.png" alt="GoDaddy to amazon">

#### GoDaddy as Registrar & Route 53 as DNS Service

<img src="../images/route53/route-53-go-daddy-as-registrar.png" alt="Go Daddy as registrar">

* If you buy your domain on a 3rd party registrar, you can still use Route 53 as the DNS service provider

1. Create a Hosted Zone in Route 53
2. Update NS Records on the 3rd party website to use Route 53 **Name Servers**

* Domain registrar != DNS Service
* But every Domain registrar usually comes with some DNS features

### Beanstalk Overview

* Elastic Beanstalk is a developer centric view of deploying an application on AWS
* It uses all the component's we have seen before: EC2, ASG, ELB, RDS,...
* Managed service
  * Automatically handles capacity provisioning, load balancing, scaling, application health monitoring, instance configuration,...
  * Just the application code is the responsibility of the developer
* We still have full control over the configuration
* Beanstalk is free but you pay for the underlying instances

#### Elastic Beanstalk - Components

* **Application:** collection of Elastic Beanstalk components(environments, versions, configurations,...)
* **Application Version:** an iteration of your application code
* **Environment**
  * Collection of AWS resources running an application version(only one application version at a time)
  * **Tiers:** Web server environment tier & worker environment tier
  * You can create multiple environments(dev, test, prod,...)

<img src="../images/route53/bean-stalk-components.png" alt="Bean Stalk Components">

#### Web Server Tier vs Worker Tier

<img src="../images/route53/web-server-vs-worker-tier.png" alt="Web server vs worker tier">

* Scale based on the number of SQS message
* Can push message to SQS queue from another Web Server Tier

#### Elastic Beanstalk Deployment Modes

* Single instance great for dev

<img src="../images/route53/bean-stalk-single-instance.png" alt="Bean stalk single instance deployment">

* High availability with load balancer great for prod

<img src="../images/route53/high-availability-with-lb.png" alt="high availability great for prod">