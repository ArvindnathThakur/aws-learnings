## Relational database service - RDS (OLTP)
**Following are the supported RDS**
* SQL Server - No Read Replica
* MySQL Server
* Oracle
* PostgreSQL
* Aurora
* MariaDB

> Patching and maintenance is done by Amazon. No access to the server. It is not a serverless service but for _Aurora_.

## Aurora
- Is on-demand serverless
- MYSql & PostgreSql compatible relational database.
- 5X more efficent than MySQl and 3X than PostgreSql.
- Min 10Gb , scales to 64TB
- Will auto scale up/down based on the load
- 2 copies of data can be replicate to minimum 3 AZ. At max giving 6 copies of data.
- Can handle loss of upto 2 copies of data without affecting writes and 3 copies of data without affecting reads.
- Storage is self healing.
- No replication delay.
- Backups do not impoact performance
- Snapshots can be taken and shared with other AWS accounts
- Ideal for infrequent, intemittent or unpredictable workload 

## Redshift
- Fully manged data warehouse service
- Used for OLAP (Data Warehousing)
- Can be Single/Multi node
- Can be only in 1 AZ
- 1 day retention period backup
- Max retention period is 35 days
- Attempts to maintain 3 copies of data. Original + replica on compute note and a backup on S3.
- Can take snapshot and Redshift will asynchronously replicate the snapshot to S3 in another region.

## DynamoDB
- Is Serverless
- Used for NoSQL
- Fully managed database. 
- Stored on SSD
- Spread across 3 distinct data centers
- Supports read models
    * Strongly consistent reads: Data available less than a second of write.
    * Eventual consistent reads: Data available after 1 second of write. Default behaviour.

#### DynamoDB Accelerator (DAX)
- Fully managed and highly available in-memory cache.
- 10x performance improvement
- Compatible with DynamoDB API calls.
- No need to mange caching logic.
- Provides Read/Write cache.
- Replicated in another AZ. Helps in auto failover.
- Supports two phase commit
- Transaction can operate on 25 items or 4 MB od data at any time.

#### Capacity 
##### On-Demand
- pay/request
- DynamoDB accomodates the workload by adjusting to the traffic load.
- No minimum capacity is required.
- No read/write charge, only charged for per request.
- Use for new launches

##### Provisioned (default)
- For more predictable traffic

#### Backup and restores
##### Backup
- Full backup at tany time
- No performance impact during backup.
- Are consistent within seconds and retained until deleted.
- Operates within the same region as the source table. Cannot do backup across regions.

##### Point in time Recovery
- Protects against accidental reads or writes
- Can be restore to any time in past 35 days.
- Stores incremental backups
- Not enabled by default
- latest restorable timestamp is 5 moinutes in past.

#### Streams
- Time-ordered sequence of item-level changes in a table.
- Cross region replication (Global tables)
- Can be hooked to lambda function to get stored procedure like functionality.

#### Global tables
- Managed multi-master, multi-region replication
- Globally distributed applications
- Based on DynamoDb streams.
- Multi-region redundancy
- Replication latency is less than a second.

## Database Migration Service - DMS
- To migrate any type of database to another type of database.
- DynamoDB cannot be used as source DB but can be used as target DB
- The source and target Database can be on-premise, EC2, RDS or another cloud provider
- The data transformation logic is encapsulated in DMS.
- The source database is completely operational during the migration,
- Supports homogenous(same Db type) as well as hetrogenous( one type to other)

## ElastiCache
- Used for in-memory caching in the clouid. Can be used for caching most frequently accessed data.
- Supports `memcached` (simple, horizontal & multi threaded) & `Redis` (advanced usage, multi AZ, back up and restore, pub-sub, not multi threaded)

## Elastic Map Reduce -  EMR
- Used for big data hosted within AWS.
- Comprises of cluster (collection of EC2)
- Master Node -> Core Node -> Task Node (optional) 
- Master node lost can result in loss of nodes.
- Configure the cluster to archieve the log files on master node to Amazon S3. EMR archives the log files at 5 minutes interval. Can only be done when setting up the cluster.

### Backups
#### Automated backups
- Backups happen during a defined window. The storage I/O to DB may be suspensed and users may experience some latency.
- No suspension for multi-az, since backup is taken from a standby.
- Backups are retained between **1-35** days. Default is 7 days.
- A full daily snapshot is taken during the maintenance window and transaction logs are captured all throughout the day. Helps in point in time recovery. 
- Backups are stored in S3(free). These s3 are automatically provisioned to the size of DB.

#### Database Snapshots
- Has to be triggered manually.
- Are kept until manually deleted.

> Restoration will always create a new instance of DB with new endpoint.

> Automated backups are deleted when the DB instance is deleted. Only manually created DB Snapshots are retained after the DB Instance is deleted.

## Multi-AZ
For disaster recovery. Automatic failover to the standby DB. Standby DB is in sync with the primary DB.

## Read Replicas
- For performance
- Can have 5 copies
- Each replica has its own DNS endpoint
- Can be across multiple AZs.
- Can be promoted to become a full db.
- Replica can also be created in another region
- Need automatic backup to be enabled

