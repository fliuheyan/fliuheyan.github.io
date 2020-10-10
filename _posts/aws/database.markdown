## Database

### RDS (Online Transaction Processing)

#### Types
* SQL
* MySQL
* PostgreSQL
* Oracle
* Aurora
* MariaDB
#### Features
* RDS runs on Virtual Machines
* Cannot SSH
* Patching OS & DB is Amazon's responsibility (Fully Managed)
* RDS is NOT Serverless
* Aurora Serverless is Serverless
#### Backups
1. Automated Backups
2. Database Snapshots

#### Read Replicas
* Multi-AZ
* Must have backups turned on
* Cross Region
* Can be promoted to master.

#### Encryption
`KMS`

### DynamoDB (No SQL)
* Stored on SSD storage
* Spread accross 3 geographically distinct data centres
* Eventual Consistent Reads(Default)
* Strongly Consistent Reads

### Red Shift (Data warehouse)
* used for business intelligence
* available in only 1 AZ

#### Backups
* enabled by default with 1day retention period.
* maximum retention period is 35 days.
* maintain at least 3 copies(1 original , 1 replica, 1 back in S3)
* async replicate to S3 in another region for DR

### Aurora

* 2copies in each AZ (at least 3 AZs, 6copies of data)
* share Aurora Snapshots with other AWS accounts.
* 3 types of replica, Aurora/Mysql/PostgresQL. `Only Aurora supports automated failover`
* automated backups turned on by default.
* Aurora Serverless : simple, cost-effective for infrequent, intermittent, unpredictable workloads.

### Elasticache
* Memcached
* Redis (Multi AZ & Backup)
 


