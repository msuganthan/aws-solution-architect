### AWS CloudFront

* Content Delivery Network(CDN)
* **Improves read performance, content is cached at the edge**
* Improves users experience
* 216 Point of Presence globally(edge locations)
* **DDoS protection(because worldwide), integration with Shield, AWS Web Application Firewall**

<img src="../images/cloud-front/aws-cloud-front.png" alt="AWS Cloud Front">


#### CloudFront - Origins

* S3 bucket
  * For distributing files and caching them at the edge
  * Enhanced security with CloudFront **Origin Access Control(OAC)**
  * OAC is replacing Origin Access Identity(OAI)
  * CloudFront can be used as an ingress(to upload files to S3)
* **Custom Origin(HTTP)**
  * Application Load Balancer
  * EC2 instance
  * S3 Website(must first enable the bucket as a static S3 website)
  * Any HTTP backend you want

#### CloudFront at a high level

<img src="../images/cloud-front/aws-cloud-front-high-level.png" alt="AWS Cloud Front at high level">

#### CloudFront - S3 as an Origin

<img src="../images/cloud-front/cloud-front-s3-as-an-origin.png" alt="S3 as an Origin">

#### CloudFront vs S3 Cross Region Replication

* CloudFront:
  * Global Edge network
  * Files are cached for a TTL(maybe a day)
  * Great for **static content** that must be available everywhere
* S3 Cross Region Replication:
  * Must be setup for each region you want o replication to happen
  * Files are updated in near real-time
  * Read only
  * Great for **dynamic content** that needs to be available at low-latency in few regions.

#### CloudFront - ALB or EC2 as an origin

* EC2 instance can serve **as an origin** for Cloud-Front
* EC2 instance must allow access to all the **Edge Location's IP**

* Similarly, ALB can serve **as an origin** for Cloud-Front
* ALB must allow access to all the **Edge Location's IP**

<img src="../images/cloud-front/cloud-front-ec2-and-alb-as-origin.png" alt="Allow EC2 instance and ALB as an origin">

#### CloudFront Geo Restriction

* You can restrict who can access your distribution
  * Allowlist: Allow your users to access your content only if they're in one of the countries on a list of approved countries
  * Blocklist: Prevent your users from accessing your content if they're in one of the countries on a list of banned countries
* The "country" is determined using a 3rd party Geo-IP database
* Use-case: Copyright Laws to content access to content

#### CloudFront - Price Classes

* CloudFront Edge locations are all around the world
* The cost of data out per edge location varies

<img src="../images/cloud-front/cloud-front-pricing.png" alt="Cloud Front Pricing">

##### CloudFront - Price Classes

* You can reduce the number of edge locations for **cost reductions**
* Three price classes:
  * Price Class All: all regions - best performance
  * Price Class 200: most regions, but excludes the most expensive regions
  * Price Class 100: only the least expensive regions

<img src="../images/cloud-front/cloud-front-price-class.png" alt="Cloud Front Price Class">

#### CloudFront - Cache Invalidations

* In case you update the backend origin, CloudFront doesn't know about it and will only get the refreshed content after the TTL has expired
* However, you can force an entire or partial cache refresh(thus bypassing the TTL) by performing **CloudFront Invalidation**
* You can invalidate all files(*) or a special path(/images/*)

<img src="../images/cloud-front/cloud-front-cache-invalidation.png" alt="Cloud Front Cache Invalidations">

#### Global users for our application

* You have deployed an application and have global users who want to access it directly
* The go over the public internet, which can add a lot of latency due to many hops
* We wish to go as fast as possible through AWS network to minimize latency

<img src="../images/cloud-front/global-user-for-application.png" alt="Global user for application">

##### Unicast IP vs Anycast IP

* Unicast IP: one server holds one IP address

<img src="../images/cloud-front/unicast.png" alt="Unicast">

* Anycast IP: all server hold th same IP address and the client is routed to the nearest one

<img src="../images/cloud-front/anycast.png" alt="Anycast">

##### AWS Global Accelerator

* Leverage the AWS internal network to route to your application
* **2 Anycast IP** are created for your application
* The Anycast IP send traffic directly to Edge Locations
* The Edge locations send the traffic to your application

<img src="../images/cloud-front/global-accel-working.png" alt="Global Accel Working">

* Works with Elastic IP, EC2 instances, ALB, NLB, public or private
* Consistent Performance
  * Intelligent routing to the lowest latency and fast regional failover
  * No issue with client cache(because the IP doesn't change)
  * Internal AWS network
* Health Checks
  * Global Accelerator performs a health check of you application
  * Helps make you application global(failover less than 1 minute for healthy)
  * Great for disaster recovery(thanks to the health checks)
* Security
  * only 2 external IP need to be whitelisted
  * DDoS protection thanks for to AWS Shield


##### AWS Global Accelerator vs CloudFront

* They both use the AWS global network and its edge locations around the world
* Both services integrate with AWS Shield for DDoS protection
* **CloudFront**
  * Improves performance of both cacheable content(such as images and videos)
  * Dynamic content(suck as API acceleration and dynamic site delivery)
  * Content is served at the edge
* **Global Accelerator**
  * Improves performance for wide range of application over **TCP or UDP**.
  * Proxying packets at the edge to applications running in one or more AWS regions.
  * Good fit for non-HTTP use cases, suck as gaming(UDP), IoT(MQTT) or Voice over IP
  * Good for HTTP use cases that require static IP addresses
  * Good for HTTP use cases that required deterministic, fast regional failover.






========================================================================================================================
### Cloud Front

* **Content Distribution Network(CDN)** creates cached copies of your website at various Edge Locations around the world.
* A CDN is a **distributed network of servers** which delivers web pages and content to users based on their **geographical location**, the **origin of the webpage** and a **content delivery server**.

<img src="../images/cloud-front/old/content-delivery-intro.png" alt="">

* Can be used to **deliver a entire website** including static, dynamic and streaming.
* Requests for content are served from the nearest Edge Location for the best possible performance.

**Core Components**

**Origin:** The location where all of original files are located. For example an S3 Bucket, EC2 Instance, ELB or Route53.
**Edge Location:** The location where web content will be cached. This is different from an AWS Region or AZ.
**Distribution:** A **collection of Edge locations** which defines how cached content should behave.

<img src="../images/cloud-front/old/cdn-core-components.png" alt="core-components">

**Distributions**

A Distribution is a collection of Edge Locations. You specific the Origin eg. S3, EC2, ELB, Route53

**Behaviours:** Redirect to HTTPs, Restrict HTTP Methods, Restrict Viewer Access, Set TTLs.
**Invalidations:** You can manually invalidate cache on specific files via Invalidations.
**Error Pages:** You can serve up custom error pages e.g. 404
**Restrictions:** You can use Geo Restriction to blacklist or whitelist specific countries

<img src="../images/cloud-front/old/distributions.png" alt="">

**Types:**
1. Web(for websites)
2. RTMP(**for streaming media**)

**Lambda@Edge**

We use Lamdda@Edge functions to **override the behavior** of request and responses.

**The 4 Available Lambda@Edge Functions**
1. **Viewer request** => When CloudFront receives a request from a viewer
2. **Origin request** => Before CloudFront forwards a request to the origin
3. **Origin response** => When CloudFront receives a response from the origin
4. **Viewer response** => Before CloudFront returns the response to the viewer.

<img src="../images/cloud-front/old/lambda-edge.png" alt="lambda edge">

<img src="../images/cloud-front/old/lambda-edge-functions.png" alt="">

**CloudFront Protection**

By default, a Distribution **allows everyone to have access.**

**Original Identity Access(OAI)**

A virtual user identity that will be used to give your CloudFront Distribution permission to fetch a private object.

<img src="../images/cloud-front/old/cloud-front-protection.png" alt="cloud front protection">

In order to use Signed URLs or Signed Cookies you need to have an OAI

**Singed URLs**(Not the same thing as S3 presigned URL)
A url with provides temporary access to cached objects.

**Singed Cookies**
A cookie which is passed along with the request to CloudFront. The advantage of using a Cookie is you want to provide access to multiple restricted files. e.g. **Video Streaming**.

### Creating a Distribution

### CheatSheet
* CloudFront is a CDN. It makes website load fast by serving cached content that is nearby
* CloudFront distributes cached copy at Edge Locations
* Edge Locations aren't just not read-only, you can write to them.
* TTL defines how long until the cache expires.
* When you invalidate you cache, you are forcing it to immediately expire
* Refreshing the cache cost money because of transfer costs to update Edge Locations.
* Origin is the address of the where the original copies of your files reside
* Distribution has 2 Types: Web and RTMP
* Origin Identity Access(OAI) is used access private S3 buckets.
* Access to cached content can be protected via Singed URL or Signed Cookies
* Lambda@Edge allows you to pass each request through a Lambda to change the behavior of the response.