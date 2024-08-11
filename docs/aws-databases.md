### Database Types

* **RDBMS (= SQL / OLTP)**: RDS, Aurora - great for joins.
* **NoSQL database - no joins, no SQL**: DynamoDB (~JSON), ElasticCache (Key / value pairs), Neptune (graphs), DocumentDB (for MongoDB), Keyspaces (for Apache Cassandra)
* **Object Store**: S3 (for big objects) / Glacier (for backups / archives)
* **Data Warehouse**(= SQL Analytics / BI): Redshift (OLAP), Athena, EMR
* **Search**: OpenSearch(JSON) - free text, unstructured searches
* **Graphs**: Amazon Neptune - display relationships between data
* **Ledger**: Amazon Quantum Ledger Database
* **Time series**: Amazon Timestream 

#### Amazon RDS

* Managed PostgreSQL / MySQL / Oracle / SQL Server / DB2 / MariaDB / Custom
* Provisioned RDS Instance Size and EBS Volume Type & Size
* Auto-scaling capability for Storage
* Support for Read Replicas and Multi-AZ
* Security through IAM, Security Groups, KMS, SSL in transit
* Automated Backup with Point i time restore feature(up to 35 daysM
* Manual DB Snapshot for longer term recovery
* Managed and Scheduled maintenance downtime
* Support for IAM Authentication, integration with Secrets Manager
* RDS Custom for access to and customize the underlying instance(Oracle & SQL Server)

* **Use case**: Store relational dataset(RDBMS / OLTP), perform SQL queries, transactions