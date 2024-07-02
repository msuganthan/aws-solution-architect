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