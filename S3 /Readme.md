# Simple Cloud Storage - S3
* It is object based.
* Files can be from **0 to 5 TB**.
* Files are stored in buckets.
* Unlimited storage.
* Resdponds wtih **HTTP 200** on successful upload of a file.
* Guranteed for 99.99% platfomr availability and 99.99999999999% for durability.
* All public access is blocked by default.
* Provides following features
    * Tiered Storage
    * Lifecycle Mangement
    * Versioning
    * Encryption
    * MFA Delete
    * Security of objects provided via ACL or bucket policies

> It is a **universal namespace**. The bucket names must be unique globally.

Objects in S3 comprise of 
* Key/value pair
* Version Id
* Metadata
* Subresources
    * ACL
    * Torrent

## Data consistency
* Read after write for PUTS of new objects.
* Eventual consistency for overwrite PUTS and DELETES.

## Storage Classes
1. Standard
    - 99.99% availability and 11X9s durability
    - Can survice 2 site crashes
2. IA (Infrequently Accessed)
   - Has a retrieval fees
3. One Zone - IA
    - No multi site storage
4. Intelligent Tiering
5. Glacier
    - Used for data archiving
    - Retrieval time from minutes to hours
6. Glacier Deep Archieve
    - Cheapest
    - Retrieval time of 12 hrs acceptable

> All storage classes span across **at least 3 AZs** except for One Zone - IA

> _First byte latency_ is in milliseconds except for Glacier(selected minutes to hours) and Glacier Deep Archive (Selected hours atleast 12)

## Billing
Billing is calculated based on
- Storage
- Requests & Retrievals
- Storage management
- Data Transfer
- Transfer Acceleration
- Cross Region Replication 

## Security & Encryption
All newly created buckets are **Private**. The access can be controlled using:
* Bucket Policies
* Access Control Lists

> Buckets can be configured to create access logs which logs all requests made to the bucket. These logs can be sent to another bucket or more so another bucket in different account.

### Ways of Encryption
1. Encryption **In Transit** can be achieved via 
* **SSL/TLS**

2. Encryption **At Rest** is achieved by
    * SSE-S3: Amazon manages the keys for encryption
    * SSE-KMS: Amazon and customer manage the keys together. Customer never sees the keys. The keys are region specific
    * SSE-C: Customer managed keys. Keys are provided by customer to AWS.

3. Client Side Encryption: Client encrypts the data

## Versioning
* Stores all versions of object
* Once **enabled** caanot be **disabled** but can be **suspended**
* Integrates with lifecycle rules
* MFA delete adds additional security layer for deletion.
* New version is **private** by default, Old version's access is not altered when new version is uploaded.

## Lifecycle management
* Allows for automating the move of objects between storage tiers.
* Can be used with in conjuction with versioning.
* Can be used for current and previous versions.

## S3 Object locks
* Allows for storing objects using **WORM (Write once Read many)** model. Can be applied to individual objets or to the entire buket

* Governance Mode: Can be deleted by special users
* Compliance Mode: Cannot be deleted or modified by anyone for the retention period. 

## S3 Glacier Vault locks
* Enforce complicance control on Vaults using Vault lock policy.

## S3 Performance

**S3 Prefix**: The component between bucket name and file name is prefix.
> For `bucketname/folder1/nested/file.jpg` the prefix is `/folder1/nested`

* S3 can give `3,500 requests for PUT/COPY/POST/DELETE` and `5,500 for GET/HEAD` requests per scond per prefix.
    > It is advisible to spread reads on to multiple prefixes for better performance.

* If SSE is enabled with KMS, there may be a hit to KMS limit while uploading/downloading files.

* For files `> 5 GB` use` multi-part upload` and for downloading use `byte-range fetch`

## S3/Glacier Select
Allows for fetching the selective data from S3/Glacier using SQL expressions.

## AWS Organization
It is an account management service to consolidate multiple AWS accounts into an organization to feliciate central management.

Service Control Policies can be enfored on to individual accounts or an OU.

## Sharing buckets 
| Way | Granularity | Access level |
|:---------:|:----------|:----------|
| Bucket Policies & IAM | Across the Bucket | Programmatic access only |
| Bucket ACLs & IAM | Individual objects | Programmatic access only |
| Cross Account IAM Roles | Individual objects| Programmatic and Console access only |

## Cross Region Replication
* Versioning must be enabled
* Existing files are not replicated automatically
* Subsequent files will be replicated automatically.
* Delete markers are not replicated.

## AWS DataSync
* Used for moving large amount of data from onpremise to AWS.
* Used with NFS and SMB compatible file system
* Used for syncing data to and from AWS and on-premise.
* Data sync agent is required for replication.