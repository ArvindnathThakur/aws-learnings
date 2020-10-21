# Route 53
Is DNS service by AWS
* ELBs do not have pre defined IPv4 addresss. They are resolved using DNS name.
* CNAME cannot be used with naked domain names(Zone apex records).
* Alias record allows for naked domain names.

## Routing Policies
### Simple Routing Policy
* 1 DNS record => Multiple IP Addresses.
* Route 53 will return all recoreds in random order

### Weighted Routing Policy
* Allows to navigate traffic to different areas or regions based on the configured weight.
* Health checks can be set on individual ecord sets
* A failed record set will be removed from Route53 until its recovery.
* SNS can be set for failed checks notification

### Latency-Based Routing
* Allows for latency based routing, the one with least response time is served by Route53

### Failover Routing Policy
* Allows for seamless navigation to `secondary` site when `primary` fails.

### Geolocation Routing Policy
* Allows for navigating traffic based on geo location os the user.

### Geoproximity Routing Policy (Traffic flow only)
* Allows for navigating traffic based on user's and AWS resource's geo location.
* Works with a `bias` defined. Bias is to expand or shrink the size of the geographic region.
* Route 53 `Traffic flow should be enabled`. 

### Multivalue Answer Policy
* Same as Simple routing, allows for having a health check on each recordset.
* Unhealthy recordset are not returned.