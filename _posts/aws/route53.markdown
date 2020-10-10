## Route53

* ELBs donnot have pre-dined IPv4 address; should resolve using a DNS name

### Difference between Alias Record & CNAME & Address Record
* A record maps name to IP
* `CNAME` maps a name to another name. there are no other records on that name
* `Alias` can coexist with other records.

`choose Alias Record over a CNAME`
### Common DNS Types
* SOA Records
* NS Records
* A Records
* CNAME
* MX Record
* PTR Record

### Route53 routing policies
* Simple Routing
    * 1 record with multiple IP addresses.
    * round robin , no health check
* Weighted Routing
* Latency-based Routing
* Failover Routing
    * active & passive
* Geolocation Routing
* Geoproximity Routing(must use Route53 Traffic Flow) //TODO
* Multivalue Answer Routing
    * simple routing with health check

### Health Checks
* Can set health checks on individual record set
* If a record set fails a health check, it will be removed from Route53,
  until it pass the health check.
* SNS when health check fails 
