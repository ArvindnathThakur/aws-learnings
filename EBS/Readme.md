# Elastic Block Storage - EBS
- Network drive which can be attached to EC2 instances.
- By default all the volumes are deleted on instance termination but can be configured to persist the volumes post termination.
- Highly available and durable but may have a latency.
- Can be detached and attached to instances.
- Capacity can be increased over time.
- Persistent Block storage
- Replicated within its AZ.

## Types

| Volume Type | General purpose SSD | Provisioned IOPS SSD| Throughput Optimized HDD | Cold HDD |  Magnetic |
|:---------:|:----------|:----------|:----------|:----------|:----------|
|**Description**| Used for wide variety of transcational workloads| high performance and mission critical workloads| Low cost HDD, for frequently accessed, throughput intensive workloads| Lowest cost HDD, for less frequently accessed workloads| Previous genration HDD|
|**Use Cases**| Mostl workloads | Databases | Big Data and Warehousing, Streaming| File Servers| Data is infrequently accessed|
|**API Name**| gp2 | io1 | st1 |sc1 | Standard |
|**Boot Volumes**| Yes | Yes | No |No | No |
|**Volume Size**| 1gb - 16tb | 4gb - 16tb | 500gb - 16tb | 500gb - 16tb | 1gb - 1tb|
|**Max IOPS/Volume**| 16,000 |  32,000 - 64,000(Nitro instances) |  500 | 250 | NA | 
|**Max Throughput**| NA | NA  | 500MiB/s | 250 MiB/s | NA |
|**IOPS/GB**| 3 |  50:1 |  500 | 250 |40-200| 

## Notes
- Snapshots are point in time copies of the volume.
- Snapshot lifecycle manager service can be used for scheduling and manging the snapshots
- Snapshots are incremental.
- Stored in S3
- Snapshot can be created while instance is running or stopped. Advisable is stopped.
- AMi's can be create dfrom Snapshot.
- Volume size and type can be changed on fly.
- Volumes are created on the same AZ as the EC2.

## Migration

### From 1 AZ to another
1. Take a snapshot of the EC2 instance
2. Crate an AMI from the snapshot
3. Use the AMI to launch instance in new AZ.


### From 1 region to other
1. Take a snapshot of the EC2 instance
2. Crate an AMI from the snapshot
3. Copy the AMI to new region.
3. Use the copied AMI to launch instance in new region.

## Encryption
An encrypted EBS volume provide has the following
- data at rest is encrypted
- data in flight between the instance and volume is encrypted
- snapshots are encrypted.
- volume created from snapshots are encrypted.
- Has minimal impact onlatency
- Uses keys from KMS (AES-256) 
 
### Encrypting and unecrypted EBS Volume
1. Create an EBS snapshot of the volume
2. Encrypt the EBS snapshot
3. Create new EBS Volume from the snapshot
4. Attach the encrypted volume.

# Instance Store Volumes
* Are ephemeral volumes. The volumes are lost when the instance is terminated.
* These volume instance cannot be stopped but can be re booted.
* Are physical hard drives attached to the machine.

**Pros**
- Better I/O
- Good for Buffer/ Caching/ content
- Data survived reboot

**Cons**
- Lost on termination
- Can't resize on the fly
- No auto backup

## EBS RAID
### RAID 0 - Increased performance
- Combination of 2 or more volumes into one logical disk space and I/O.
- The data is written on only one disk.
- If one disk fails the entire data fails.
- Used for applications which require high IOPS and have fault- tolerance in built.

### RAID 1 - Increased Fault Tolerance
- The data is replicated on all the disks in the RAID.


# EFS - Elastic File System
- Network file system
- Can be used across multi-az
- Highly available, scalable but expensive
- Access is managed via security groups
- Only going to work with (POSIX) linux based EMI.
- 1000s of EC2 instances can be connected concurrently
- Performance mode
    - General purpose: Latency sensitive, can be used for web-server, CMS etc.
    - MAX I/O - higher latency, highly parallel, can be used for big data.
- Storage tiers
    - Standard: For frequently accessed files
    - Infrequently access (EFS-IA): lower cost but cost to retrieve

## Usages
### EFS
Ideal for distributed, highly resilient storage for **Linux instances and linnux based applications**.

### FSX for windows
Ideal for windows based applications or windows OS which cannot connect to EFS.

### FSX for Lustre
Ideal for high speed, high capacity distributed storage. Useful for High performance compute, machine learning, financial modelling.
