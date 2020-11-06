# Fundamentals

## Regions 
* Are cluster of data-centers
* names start with us-east-1, eu-west-3
> Many services are locked to regions

## Availability Zones
* Comprises of `1+ disjoint and isolated` data centers connected via high bandwidth ultra low-latency networking
* Each region comprises of more than 2 availability zones `(>=2 <= 6>)`
* names start with us-east-1a, eu-west-3b

## Edge Locations
* Endpoints for caching content (Cloudfront, AWS's CDN). Objects are cached for the TTL (Time to live)
* Cache can be cleared but will incur a charge
> There are more edge locations than regions

## Distribution
* Collection of Edge location used by CloudFront

# CloudFront
AWS's content delivery network.

**Origin** can be S3 Bucket, EC2 instance, ELB or Route 53.

* Premium content can be delivered using signed URl(for a file) or Cookie (for multiple files)

# Storage Gateway
 Used for caching S3 locally. It is virtual or physical device used for replicating data into AWS

## Types
* File Gateway (NFS & SMB): For transferring files stored directly on S3.
* Volume Gateway (iSCSI): For replicating disks
    - Stored Volume: Primary Data is store locally, provide low-latency access to the data. 1GB - 16TB
    - Cached Volume: Most frequently acceced data is stored locally. 1GB-32GB
* Tape Gateway (VTL): For backing up tapes

# Athena
* Interactive query service which allows for querying data in S3 using SQL. 
* Serverless, pay/query/TB scanned
* Commonly used for analysing logs

# Macie
Security service which uses ML and Natural language processing to discover, classify and protect sensitive data stored in S3. Helps identity PII (Personal identifiable information)

# Security Groups
 - are stateful, any inbound rule automatically creates corresponding outbound rule.
 - Cannot specify deny rules
 - Can be attached to multiple instances.
 - Can be conceived as a firewall for EC2 instance, lives outside of EC2. They regulate
    * Port Access
    * Authorised IP ranges
    * Inbound/outbound traffic
 - Region/VPC combination specific
 - Should have a separate sg for SSH
 - All inbound traffic is blocked
 - All outbound traffic is authorised
 - Can refer other security groups
 - Network security in AWS
- Firewall for EC2 machines
- Regulates access to
    - Port
    - IP Ranges
    - Control of in/out bound traffic to EC2
- Can be attached to multiple instances
- Locked down to a region/VPC combination
- All inbound traffic is blocked by default
- All outbound traffic is allowed
- Can refer to another security group

> Network Access Control List are stateless. Inbound rule does not automatically creates outbound rules.

> Elastic Ip are static Ips which can be attached to any instance. Max of 5 is allocated by default but more can be requested

# Services which can cache
1. CloudFront: 
    > Caches the origin. Maintained at nearest edge location
2. API Gateway
    > Also provides request caching
3. Elasticache (Memcache|Redis)
    > Can be used to cache data from Database
4. DAX
    > Can be used with DynamoDB

> 1 -> 4 less to more latency
---

# Acronyms in AWS

| Acronym | Synopsis |
|:---------:|:----------|
| AZs               | Availability Zones - Cluster of data centers |
| IAM               | Identity and access management - Security and access management |
| S3                | Simple Cloud Storage - File storage system |
| EC2               | Elastic compute cloud - Virtual machines |
| ASG               | Auto scaling group - Used for scaling the infrastructure |
| ELB               | Elastic load balancer - Load balancer |
| EBS               | Elastic Block storage - Block storage |
| EFS               | Elastic File system - Network file system |
| ENI               | Elastic Network Interface - Virtual network card |
| VPC               | Virtual private Cloud |
| OAI               | Origin access identity |
| TTL               | Time to live |
| ACL               | Access control list |
| SSE               | Server side encryption |
| KMS               | Key Management Service |
| SCT               | Schema conversion tool used for hetrogenous database migration |
| S3/Glacier Select | Used for retrieve data from S3/Glacier using Sql statement |
| Athena            | SQL query to S3 |
| AWS DataSync      | Used for syncing large amount of data from onremise to AWS |
| EMR               | Elastic Map Reduce|
| RDS               | Relational Database Service |
| DAX               | DynamoDB Accelerator |
| DMS               | Database Migration Service |
| PITR              | Point in time recovery |
| ARN               | Amazon Resource Name |
| RAM               | Resource Access Manager |
| IGW               | Internet gateway |
| NAT               | Network Address Translation |