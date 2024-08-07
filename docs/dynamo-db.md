### Dynamo-DB

* Fully managed, highly available with replication across multiple AZs
* NoSQL database - not a relation database - **with transaction support**
* Scales to massive workloads, distributed database
* **Millions of requests per seconds, trillions of row, 100s of TB of storage**
* Fast and consistent in performance(single digit millisecond)
* Integrated with IAM for security, authorization and administration
* Low cost and auto-scaling capabilities
* **No maintenance or patching, always available.**
* **Standard & Infrequent Access(IA) Table Class.**

#### DynamoDB - Basics

* DynamoDB is made of **Tables**
* Each table has a **Primary Key** (must be decided at creation time)
* Each table can have an infinite number of items(=rows)
* Each item has **attributes**(can be added over time - can be null)
* Maximum size of an item is **400KB**
* Data types supported are:
  * **Scalar Types**: String, Binary, Number, Boolean, Null
  * **Document Types**: List, Map
  * **Set Types**: String Set, Number Set, Binary Set
* **Therefore, in DynamoDB you can rapidly evolve schemas**