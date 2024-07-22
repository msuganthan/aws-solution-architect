### AWS Snow Family

* Highly secure, portable device to **collect and process data at the edge, and migrate data into and out of AWS**
* **Data migration:**
  * Snowcone
  * Snowball Edge
  * Snowmobile
* **Edge Computing**
  * Snowcone
  * Snowball Edge

#### Data Migrations with AWS Snow Family


|          | Time to transfer |           |           |
|----------|------------------|-----------|-----------|
|          | 100 Mbps         | 1Gbps     | 10Gbps    |
| -------- | ---------------- | --------- | --------- |
| 10 TB    | 12 days          | 30 hours  | 3 hours   |
| 100 TB   | 124 days         | 12 days   | 30 hours  |
| 1 PB     | 3 years          | 124 days  | 12 days   |

Challenges:
* Limited connectivity
* Limited bandwidth
* High network cost
* Shared bandwidth(can't maximize the line)
* Connection stability

**AWS Snow Family: offline devices to perform data migrations**
If it takes more than a week to transfer over the network, use Snowball devices!!!


=============================================================================================================

### Snowball

* **Petabyte-scale** data transfer service. 
* Move data onto AWS via physical briefcase computer.

* **Low Cost:** It cost a **thousand of dollars** to transfer 100TB over high speed internet. Snowball can reduce that cost by **1/5th**.
* **Speed:** It can take 100TB over 100 days to transfer over high speed internet. Snowball can reduce that transfer time by less than a week.

#### **Snowball features and limitations:**

* E-Ink display(shipping information) 
* Tamper and weatherproof.
* Data is encrypted end-to-end(256-bit encryption)
* Uses **Trusted Platform Module(TPM).** (A specialized chip on an endpoint device that stores RSA encryption keys specific to the host system for hardware authentication)
* For security purposes, data transfer must be completed **within 90 days** of the Snowball being prepared.
* Snowball can Import and Export from S3.

#### **Snowball come in two sizes:**

* 50TB(42 TB of usable spaces)
* 80TB(72 TB of usable spaces)


### **AWS Snowball Edge:**

* Petabyte-scale data transfer service. 
* Move data onto AWS via physical briefcase computer. 
* More storage and on-site compute capabilities. 
* Similar to Snowball but with more storage and with local processing.

#### **Snowball Edge features and limitations:**

* **LCD display**(shipping information and other functionality)
* Can undertake **local processing and edge computing** workloads.
* **Can use in a cluster in groups of 5 to 10 devices**.
* three options for device configurations:
  * storage optimized(24 vCPUs)
  * compute optimized(54 vCPUs)
  * GPU optimized (54 vCPUs)

#### **Snowball Edge come in two sizes:**

* 100 TB(83 TB of usable space)
* 100 TB clustered(45 TB per node)

### **Snowmobile**

* a 45 foot long ruggedized shipping container, pulled by a semi-trailer truck.
* transfer up to 100PB per Snowmobile. 
* AWS personal will help you connect your network to the snowmobile and when data transfer is complete they'll drive it back to AWS to import into S3 or Glacier.

#### **Security Features:**

* GPS tracking
* Alarm monitoring
* 24/7 video surveillance
* an escort security vehicle while in transit(optional)
* It comes in one size: 100PB
