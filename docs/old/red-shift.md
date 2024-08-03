### Amazon Redshift

* Fully Managed **Petabyte-size** Data Warehouse. Analyze(Run complex SQL queries) on massive amount of data Columnar Store database.

**What is a Database Transaction?**

* A transaction symbolizes a unit of work performed within a database management system.

| Database                                                                                                                             | Data Warehouse                                                                                                                  |
|--------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Online **Transaction** Processing                                                                                                    | Online **Analytical** Processing                                                                                                |
| A database was built to store current transaction and enable **fast access to specific transaction** for ongoing business processes. | A data warehouse is built to store large quantities of historical data and **enable fast, complex queries across all the data** |
| Adding items to your shipping cart                                                                                                   | Generating reports                                                                                                              |
| Single source                                                                                                                        | Multiple source                                                                                                                 |
| short transaction with an emphasis on writes                                                                                         | Long transaction with an emphasis on reads.                                                                                     |

### Intro

* AWS Redshift is the AWS managed, petabyte-scale solution for **Data Warehousing.**
* Pricing starts at just $0.25 per hour with no upfront costs or commitments.
* Scale up to petabytes for $1000 per terabyte per year.
* Redshift is price is less than 1/10 cost of most similar services.

* Redshift is used for Business Intelligence.
* Redshift uses OLAP(Online Analytics Processing System)
* Redshift is **Columnar Storage** Database.

**Columnar Storage** for database tables is an important factor in optimizing analytic query performance because it drastically reduces the overall disk I/O requirement and reduces the amount of data you need to load from disk.

### Use-cases

We want to continuously COPY data from 
1. EMR,
2. S3 and 
3. DynamoDB
to power a custom Business Intelligence tool.
   
Using a third-party library we can connect and query Redshift for data.

<img src="../images/red-shift/use-cases.png" alt="use-cases">

### Columnar Storage

Stores data together as columns instead of rows.

<img src="../images/red-shift/columnar-storage.png" alt="">

* **OLAP** applications look at multiple records at the same time. You save memory because you fetch just columns of data you need instead of whole rows. 
* Since data is stored via column, that means all data is of the same data-type allowing for easy compression.

### Configuration

**Single Node** comes in sizes of **160 GB** You can launch a single node to get started with Redshift

**Multi-Node** You can launch a cluster of nodes with Multi-Node mode
    **Leader Node** - manages client connection and receiving queries
    **Compute Node** - stores data and performs queries up to **128 compute nodes**

### Node Types and Sizes

**Dense Compute(DC):** best for high performance, but they have less storage
**Dense Storage(DS):** cluster in which you have a lot of data

### Compression

* Redshift uses **multiple compression techniques** to achieve significant compress relative to traditional relational data stores
* Similar data is stored sequentially on disk
* Does not require indexes or materialized views, which saves a lot of space.
* When loading data to an empty table, data is sampled and the most appropriate compression scheme is selected automatically.

### Processing

* Redshift uses **Massively Parallel Processing(MPP)**
* Automatically distributes data and query loads across all nodes.
* Lets you easily add new nodes to you data warehouse while still maintaining fast query performance.

### Backups

* Backups are enabled by default with a 1 days retention period. Retention period can be **modified up to 35 days.**
* Redshift always attempts to maintain at least 3 copies of your data. 
  * The original copy 
  * Replica on to compute nodes. 
  * Backup copy in S3
* Can asynchronously replicate your snapshots to S3 **in a different region**

<img src="../images/red-shift/red-shift-back-ups.png" alt="">

### Billing

**Compute Node Hours**
* THe total number of hours ran across all nodes in the billing period
* Billed for 1 unit per node, per hour.
* **Not Charged for leader node hours,** only compute nodes incur charges

**Backup**

* Backup are stored on S3 and you are billed the S3 storage fees.

**Data Transfer**

* Billed for only transfer within a VPC, not outside of it.

### Security

**Data-in-transit** Encrypted using SSL
**Data-at-rest** Encrypted using AES-256 encryption

* Database Encryption can be applied using 
  * Key Management Service(KMS) multi-tenant HSM
  * CloudHSM single-tenant HSM

<img src="../images/red-shift/security.png" alt="">

### Availability

* Redshift is **Single-AZ.** To run in Multi-AZ you would have to run multiple Redshift.
* Clusters in different AZs with same inputs.
* Snapshots **can be restored to a different AZ** in the event an outage occurs

<img src="../images/red-shift/redshift-az.png" alt="">

### Cheatsheet
* Data can be loaded from S3, EMR, DynamoDB, or multiple data sources on remote hosts.
* Redshift is Columnar Store database which can SQL-like queries and is an OLAP.
* Redshift can handle petabytes worth of data. Redshift is for Data Warehousing.
* Redshift most common use case is Business Intelligence.
* Redshift can only run in a 1 availability zone(**Single AZ**)
* Redshift can run via a single node or multi-node(clusters)
* A single node is 160 GB in size
* A multi-node is comprised of a leader node and multi compute nodes.
* You are bill oer hour for each node(excluding leader node in multi-node)
* You are not billed for the leader node
* You can have up to 128 compute nodes
* Redshift has two kinds of Node Type **Dense Compute** and **Dense Storage**
* Redshift attempts to back up 2 copies of your data, the original, on compute node and on S3
* Similar data is stored on disk sequentially for faster reads.
* Redshift database can be encrypted via KMS or CloudHSM
* Backup Retention is default to 1 day and can be increase to maximum of 35 days
* Redshift can asynchronously back up your snapshot to Another Region delivered to S3
* Redshift uses **Massively Parallel Processing(MPP)** to distribute queries and data across all loads
* In the case of empty table, when importing Redshift will sample data to create a Schema