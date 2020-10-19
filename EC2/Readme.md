# Elastic Compute Cloud - EC2
This service offers
- Renting VMs (EC2)
- Virtual drives for data storage (EBS)
- Load Balancer (ELB)
- Scaling the services using auto-scaling group (ASG) 

## Pricing Models
### On Demand: 
- Pay as you go.
- Low cost and no contract.
- Useful for short time spike and unpredictable but uninterrupted workload.
- Dev and test

### Reserved: 
- Allows for reserving the instance, contractual over period of 1 or 3 years.
- Application with steady state and predictable usage
- Make upfront payments to reduce cost
#### Standard
_ Upto 75% less than on demand
#### Convertible
- Can change the instance type as long as the creation results in equal or greater value.
#### Scheduled Reserves
- Periodic reservation. Useful for predicatable recurring schedule that requires a fraction of a period.
### Spot: 
- Allows for bidding. 
- Can set max and min price range. 
- Options to terminate or stop when the max price is hit. 2 minutes grace perios.
- **Spot block**: Allows for blocking instance for a specified time frame. 

#### Termination of Spot request
1. Cancel the spot request
2. terminate the instances

> Spot request which are **open, active or disbaled** can be **terminated**. Cancelling a sport request does not terminate the instances.

### Dedicated Host: 
- Physical dedicated server.
- Generally required when using any server bound license or compliance which do not support multi-tenancy or could deploymnents.

### Spot Fleet
Combination os Spot instances + on-demand instances
_Strategies_
- lowestPrice
- diversified
- capacityOptimized

## Placement Groups
### Cluster
- Instances placed close to each other on **the same rack** in a **single availability zone**.
- Ideal for **low latency or high throughput** applications.
 
### Spread 
- Instances placed on distinct underlying hardware.
- Pleaced on different availability zones under single region.
- Ideal for critical applicatioms.
- Instances are isolated from each other.
- 7 instances per AZ

### Partitioned Placement
- Groups are devided in partitions.
- Each partition can have multiple instances.
- Each partition is placed on its own distinct hardware.
- Ideal for applications like cassandra, haddop kafka.
- 7 partitions per AZ
- 100s of EC2 instances

## Elastic Network Interfaces - ENI
- Logical component in a VPC representing a virtual network card.
- An instance can have 1 primary and ENI and multiple secondary ENIs.
- They are bound to a specific AZ.
- ENIs can be created and moved independatly and can be attached or removed from an EC2 instance on the fly.
- ENI has the following attributes:
    - Description
    - Private IP Address
    - Elastic IP Address
    - MAC Address
    - Security Group(s)
    - Source/Destination Check Flag
    - Delete on Termination Flag

## EC2 Hibernate
- Allows for preserving the in-memory state between reboots.
- The state of RAM is dumped on to the EBS root volume.
- The root volume should be encrypted.
- The RAM size must be less than 150GB.
- Only for on-demand and Reserved instances.
- Can only be hibernated for max 60 days.

## Instance types:
R: RAM
C: CPU
M: Medium
I: I/O
G: GPU
T2/T3: General purpose, burstable

## Connecting to EC2 Instance
Use `SSH` to connect to EC2 terminal alternatively `EC2 Instance connect` can be used from the browser.
> Use **chmod 0400** _Keyname_ to change the key permission (bad permission problem)

> SSH -I filename.pem ec2-user@publicipofinstance

> Termination of an instance will delete the root volume by default but will not delete any additional EBS volume.

# User Data
- Used for bootstrapping the instances.
- only run once at the instance first start

# Meta data
Meta data about an instance can be retrieved from http://169.254.169.254/latest/meta-deta/



