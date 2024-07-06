### Relational Database service(RDS)

* RDS stands for Relational Database Service
* It's a managed DB service for DB use SQL as a query language.
* It allows you to create database in the cloud that are managed by AWS
  * Postgres
  * MySQL
  * MariaDB
  * Oracle
  * Microsoft SQL Server
  * IBM DB2
  * Aurora

#### Advantage over using RDS versus deploying DB on EC2

* RDS is a managed service:
  * Automated provisioning, OS patching
  * Continuous backups and restore to specific timestamp(Point in time Restore)
  * Monitoring dashboards
  * Read replicas for improve read performance
  * Multi AZ setup for **Disaster Recovery**(DR)
  * Maintenance windows for upgrades
  * Scaling Capability(vertical and horizontal)
  * Storage backed by **EBS**(gp2 or io 1)
* But you can't SSH into your instances.

#### RDS - Storage Auto Scaling

* Helps you increase storage on your RDS DB instance dynamically.
* When RDS detects you are running out of free database storage, it scales automatically.
* Avoid manually scaling your database storage.
* You have to set **Maximum Storage Threshold** (maximum limit for DB storage)
* Automatically modify storage if:
  * Free storage is less than 10% of allocated storage
  * Low storage lasts at least 5 minutes
  * 6 hours have passed since last modification
* Useful for application with unpredictable workloads
* Supports all RDS database engines

<img src="../images/rds/rds-auto-scaling.png" alt="RDS auto scaling">

#### RDS Read Replicas for read scalability

* Up to 15 read replicas
* Within AZ, Cross AZ or Cross Region
* Replication is **ASYNC** so reads are eventually consistent
* Replicas can be promoted to their own DB
* Applications must update the connection string to leverage read replicas

<img src="../images/rds/rds-read-replicas.png" alt="RDS read replicas">

##### RDS Read replicas - Use Cases

* You have a production database that is taking on normal load
* You want to run a reporting application to run some analytics
* You create a Read Replica to run the new workload there
* The production application is unaffected
* Read replicas are ise for SELECT(=read) only kind of statement(not INSERT, UPDATE, DELETE)

<img src="../images/rds/read-replicas-use-cases.png" alt="Read replicas use cases">

##### RDS Read Replicas - Network Cost

* In AWS there's a network cost when data goes from one AZ to another
* For RDS Read replicas within the same region you don't pay that fee

<img src="../images/rds/read-replicas-network-cost.png" alt="Network Cost">

##### RDS Multi AZ(Disaster Recovery)

* SYNC replication
* One DNS name - automatic app failover to standby
* Increase **availability**
* Failover in case of loss of AZ, loss of network, instance or storage failure
* No manual intervention in apps
* Not used for scaling
* Note: The Read Replicas be setup as Multi-AZ for Disaster Recovery(DR)

<img src="../images/rds/rds-disaster-recovery.png" alt="Disaster Recovery">

##### RDS - From Single-AZ to Multi-AZ

* Zero downtime operation(no need to stop the DB)
* Just click on "modify" for the database
* The following happens internally:
  * A snapshot is taken
  * A new DB is restored from the snapshot in a new AZ
  * Synchronization is established between the two databases.

<img src="../images/rds/rds-single-az-multi-az.png" alt="RDS single az to multi az">

### RDS Custom

* Managed Oracle & Microsoft SQL Server Database with OS and database customization
* RDS: Automates setup, operation, and scaling of database in AWS
* Custom: access to the underlying database and OS so you can
  * Configure settings
  * Install patches
  * Enable native features
  * Access the underlying EC2 instance using **SSH** or **SSM Session Manager**
* De-activate Automation Mode to perform your customization, better to take a DB snapshot before
* RDS vs RDS Custom
  * RDS: entire database and the OS to be managed AWS
  * RDS: Custom: full admin access to the underlying OS and the database

### Amazon Aurora

* Aurora is a proprietary technology from AWS(not open sourced)
* Postgres and MySQL are both supported as Aurora DB(that means your drivers will work as if Aurora was a Postgres or MySQL database)
* Aurora is "AWS Cloud optimized" and claims 5x performance improvement over MySQL on RDS, over 3x the performance of Postgres on RDS
* Aurora storage automatically grows in increments of 10GB, up to 128 TB.
* Aurora can have up to 15 replicas and the replication process is faster than MySQL(sub 10ms replica tag)
* Failover in Aurora is instantaneous. It's HA native
* Aurora cost more than RDS(20% more) - but is more efficient

#### Aurora High Availability and Read Scaling

* 6 copies of your data across 3 AZ:
  * 4 copies out of 6 needed for writes
  * 3 copies out of 6 need for reads
  * Self-healing with peer-to-peer replication
  * Storage is striped across 100s of volumes
* One Aurora Instance takes writes(master)
* Automated failover for master is less than 30 seconds
* Master + up to 15 Aurora Read Replicas serve reads
* Support for Cross Region replication

<img src="../images/rds/high-availability-and-read-scaling.png" alt="High Availability and read scaling">

#### Aurora DB Cluster

<img src="../images/rds/aurora-db-cluster.png" alt="Aurora DB Cluster">

#### Features of Aurora

* Automatic fail-over
* Backup and Recovery
* Isolation and security
* Industry compliance
* Push-button scaling
* Automated Patching with Zero Downtime
* Advanced Monitoring
* Routine Maintenance
* Backtrack: restore data at any point of time with using backups

### Aurora Replicas - Auto Scaling

Auto-scaling due to high CPU usage.

<img src="../images/rds/aurora-auto-scaling.png" alt="Aurora Auto Scaling">

#### Aurora - Custom Endpoints

* Define a subset of Aurora Instance as a Custom Endpoint
* Example: Run analytical queries on specific replicas
* The Reader Endpoint is generally not used after defining Custom Endpoints

<img src="../images/rds/aurora-custom-end-point.png" alt="Aurora Custom end point">

#### Aurora Serverless

* Automated database instantiation and auto-scaling based on actual usage
* Good for infrequent, intermittent or unpredictable workloads.
* No capacity planning needed
* Pay per second, can be more cost-effective

<img src="../images/rds/aurora-serverless.png" alt="Aurora Serverless">

#### Global Aurora

* Aurora Cross Region Read Replicas
  * Useful for disaster recovery
  * Simple to put in place
* Aurora Global Database(recommended)
  * 1 Primary Region(read/write)
  * Up to 5 secondary (read-only) regions, replication lag is less than 1 second
  * Up to 16 Read Replicas per secondary region
  * Helps for decreasing latency
  * Promoting another region(for disaster recovery) has an RTO of less than one minute
  * Typical cross-region replication takes less than 1 second

<img src="../images/rds/aurora-global-database.png" alt="Aurora Global Database">

#### Aurora Machine Learning

* Enables you to add ML-based predictions to your application via SQL
* Simple, optimized and secure integration between Aurora and AWS ML services
* Supported services
  * Amazon SageMaker(use with any ML model)
  * Amazon Comprehend(for sentiment analysis)
* You don't need to have ML experience
* Use cases: fraud detection, ads targeting, sentiment analysis, product recommendations

<img src="../images/rds/aurora-machine-learning.png" alt="Aurora Machine Learning">