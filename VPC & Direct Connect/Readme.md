# Virtual Private Cloud - VPC
- Max 5 VPC per region can be allowed. But can be increased by raising AWS support ticket
- Max 5 CIDR block per VPC, each CIDR can have 
    - Min size /28 = 16 IPs
    - Max size /16 = 65536 IPs
- Only private IP ranges are allowed
- A CIDR block size must be between a `/16` netmask and `28 ` netmask.

## Private IP Ranges
| CIDR | Range | Usage |
| :-: | :- | :- |
| `10.0.0.0/8`     | 10.0.0.0   - 10.255.255.255 | Big private network |
| `172.16.0.0/12`  | 172.16.0.0 - 172.16.255.255 | Default AWS |
| `192.168.0.0/16` | 192.168.0.0 - 192.168.255.255 | Home networks|

- All other IPs can be used as public IP.

- 5 IPs are reserved by AWS, first 4 and last 1 from each subnet.
    - 1st -> Network address
    - 2nd -> VPC Router
    - 3rd -> Mapping to Amazon provided DNS
    - 4th -> Reserved for future use
    - nth (last) -> Network broadcasr address

## CIDR - XXX.XXX.XXX.XXX
|Value|What Can change|
| :-: |:-|
|/32 | no IP number can change |
|/24 | last IP number can change |
|/16 | last IP 2 numbers can change | 
|/8  | last IP 3 numbers can change |
|/0  | all IP numbers can change |
 
## Internet Gateway
- Helps connect VPC instances to internet
- By default new VPC do not have IGW
- `1 VPC -> 1 IGW` and vice versa  
- Serves as `NAT device` for instances which have `Public ipv4`
- Do not allow internet access on their own and will require some route table changes

### EC2 in Public subnet to internet
1. Create an Internet Gateway to VPC
2. Create a Route table and associate it with public subnet.
3. Add a route entry to associate `0.0.0.0/0` to internet gateway.

### EC2 in private subnet to internet
These instances cannot use Internet Gateway, which will expose them to internet as well.

#### Option 1 - Network Address Translation Instances (Obsolete)
- Allow instances in the private subnet to connect to the internet.
- They must be launched in Public subnet
- EC2 flag `source/destination` check should be disabled
- Should have elastic IP
- Route table must be configured to allow traffic from private subnet to NAT instances

##### Steps
1. Create a NAT instance in public subnet.
2. Associate an elastic IP to NAT instance
3. Configure the public route table to allow traffic from Nat instance to IGW.
4. Configure the private route table to allow traffic from private subnet to Nat instance.

#### Option 2 - Network Address Translation Gateway 
- Managed Service, higher bandwidth and no administration 
- Cannot be used by instances in the same subnet
- Pay by the hour for usage and bandwidth
- Requires IGW. `Private subnet -> NATGW -> IGW -> WWW` 
- Locked to an AZ.
- 5 Ggps to 45 Gbps

## DNS Resolution
Controlled by following properties
### enableDnsSupport
- Default True
- if true, queries the AWS DNS server at `169.254.169.253`
### enableDnsHostname
- False for manually created ones, true for the default and created via wizard.
- Allows for assigning hostnames to public EC2 instances.

> Private hosted zones require thee flags to be set to `true`.

## VPC Peering
- Used for connecting two VPC privately using AWS network
- Must not have overlapping CIDRs
- Is not `transitive`
- Can be done with VPC in another AWS account
- Route table on each VPC's subnet need to be updated to allow the traffic
- Works inter-region as well as cross account
- Can refer a security group of a peered VPC

## VPC Endpoint
- To `connect to AWS services` using `private network` instead of public network.
- Scale horizontally and are redundant
- No need for IGW, NATGW

### Types
#### Interface
- Provisions an ENI as an entry point. 
- Must attach a security group.
- Used by most Services

#### Gateways
- Provision a target
- Must use a route table
- Required by S3 and DynamoDB


## Flow logs
IP traffic information which can be captured and logged into S3 /CloudWatch logs.
### Types
- VPC: All the traffic
- Subnet: Traffic in subnet
- Elastic Network Interface: One network interface

## Network ACL & Security Groups

> Network ACL are defined and Operate `at subnet level`

- Is `stateless`, both inbound and outbound rules need to be defined explicitly.
- `Control` traffic to and from `subnet`, 
- `Default NACL allows` everything outbound/inbound by default
- One `NACL/subnet`, new subnet have default NACL
- `Newly` created NACL will `deny` everything
- Can be used for blocking specific IP at subnet level

|Security Group|Network ACL|
|:-|:-|
|Instance level|Subnet level|
|Only allow rules|Both allow/deny rules|
|Stateful, if traffic is allowed in, it will be allowed out|Stateless, return traffic must be explicitly allowed|
|All rules are evaluated|Rules are evaluated in precedence and lower presecedence are not evaluated if a match is found|
|Applies only to instance|Applies to all instances in the subnet|

### Rule definition
- The `lower` the rule no, the `higher` the precedence.
- Rule nos can range from `1 to 32766`
- `*` is last rule and denies everyhting in case no rule match
- Preferably rules should be added in `increment of 100`

## Bastion Host
Its an EC2 instance within a public subnet, to allow SSH into private instances. 

## Connecting corporate network to VPC
1. Create a `Customer Gateway` at `corporate data center`
2. Create a `VPN Gateway` at `VPC`
3. Join the two using `Site to Site VPN Connection`

## Direct Connect
- Provides a `dedicated` private connection `between corporate DC to AWS VPC`
- Dedicated connection is required between `corporate DC` and `AWS Direct connect locations`.
- Need a `Virtual Private Gateway` on VPC.
- Access public resources (S3) and private resources (EC2) on same instance
- Takes a month to establish the connection 
- Data is not encrypted.
- `AWS Direct Connect + VPN` will give a secured connection

### Direct Connect Gateway
- Allows a direct connect to one or more VPC.

### Types of connections
- Dedicated
    - 1 to 10 Gbps
    - Physical ethernet port dedicated to customer
    - Request made to AWS and then completed by `AWS Direct Connect Partners`
- Hosted 
    - 50 Mbps to 500 Mbps upto 10Gbps
    - Requests are made `AWS Direct Connect Partners`
    - Bandwidth can be increased or decreased on demand

## Egress only Internet gateway
- Used for provifing internet access to private instances having IPV6 address.
- Same as NAT, Nat us for IPV and Egress is for IPV6.

## AWS Private Link - VPC endpoint service
- Used for exposing a service in one VPC to another VPCs securily and privately

## VPN Cloud Hub
 - Used for connecting multiple customer data centers to VPC
 - Allows for multiple VPN connection

## Transit Gateways
- Provides transitive peering connection between VPCs 
- Regional but can work across regions
- Works with most of the services
- Support IP Multicat