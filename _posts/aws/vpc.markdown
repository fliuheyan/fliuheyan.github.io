## VPC

* IGW
* Route Table
* ACL
* Subnet
* Security Group stateful ; ACL stateless(must both inbound/outbound)
* NO transitive peering VPC //TODO. VPC need to peer one to one(support cross regions)

`1Subnet = 1 AZ`

#### Create VPC
1. default Route Table
2. default ACL
3. Security Group

* It won't create any subnets nor IGW.

### IGW
* 1 IGW per VPC.

### Subnet
* Subnet must be associated with a `ACL`. Default the `VPC default ACL`
* `US-East-1A` in differnt accounts could be different. The AZs are randomized.
* Amazon always reserve 5 IPs within your subnets.

### ACL
* `the default ACL allow all inbound/outbound traffic`
* `create custome network ACL, deny all inbound/outbound traffic`
* deny rule
* block IP address & port
* 1 ACl to Many Subnet(1 subnet only 1 ACL)
* associate ACL with a subnet, the previous auto removed.
* ACL contain list of rules.Rule start with the lowest number
* stateless

### ACL vs Security Group
* Block `IP` using `ACL`
* `SG` is stateful. `ACL` is stateless

### Security Group
* Cannot span VPC


### NAT
NAT cross multi AZ ????
#### Nat instance
1. Disable source/destination check when creating NAT instance.
2. NAT instance must be in `public subnet` //??????
3. the amount of traffic depends on the instance size.

#### VPC Flow Logs
* Cannot enable VPC not in your account.
* once created, cannot change configuration;
* Not all IP Traffic is monitored.
    * instance access AWS DNS server.
    * traffic to 169.254.169.254 for instance metadata.
    * DHCP
    * traffic to the reserved IP address for the default VPC router.(??????)

#### NAT Gateway
![nat](nat.png)
1. redundant
2. 5Gbps - 45Gbps
3. no need to patch
4. not associated with SG
5. auto assign public IP
6. `Need to manual update Route Table`
7. No need to disable Source/Destination checks
8. Not behind security group
9. for `HA`, create `NAT Gateway` in each `AZ`

#### NAT instance vs. NAT Gateway
* NAT Gateway
 redundant inside the AZ
 Starts at 5GB - 45GB
 auto assgin to `public ip`

### Bastions
* cannot use NAT as `Bastion`. (`ELB` sames OK)

### Direct Connect
* connect your data center to `AWS`
* `thoughput workloads` & `stable`, `reliable` secure connection
#### How to create a Direct Connect
1. Create a virtual interface(public virtual interface)
2. Go to VPC console and then to VPN connections. Create a `Customer Gateway`
3. Create `Virtual Private Gateway`
4. Attach `Virtual Private Gateway` to desired VPC
5. Create `VPN Connection`
6. Setup `VPN` on the `Customer Gateway`

### Global Accelerator
> AWS Global Accelerator is a service which can improve availability 
> and performance of your application for local and global users.
* assign two static IPs
* with `endpoint group` control traffic(traffic dials)
* control weight 

### VPC Endpoint //TODO
> `VPC Endpoint` enables you to privately connect your `VPC` to
> supported AWS services.

#### Type
* Interface Endpoint
* Gateway Endpoint
    * S3
    * DynamoDB

### AWS PrivateLink
* Peering VPC to tens, hundreds, thousands of customer VPCs.
* No VPC peering, no route table, NAT, IGW etc.
* Require `Network Load Balancer` on the service VPC, and `ENI` on the customer VPC.


### Transit Gateway
* peering between VPCs and on-promises data centers.
* work on `hub-and-spoke` mode.
* regional base, but can cross multiple regions.
* With `Resource Access Manager` to cross account.
* `Route Table` to limit VPCs connections.
* work with `Direct Connect` and `VPN Connection`
* support `IP multicast`

### AWS VPN CloudHub
* Multi sites , each with its own VPN Connection. Can use `VPN CloudHub`
to connect together.
* `Hub-and-spoke` model
* lower cost & easy to manage

### AWS Network costs
* private IP address (AWS backbone network free ?????)
* Group all EC2 in same AZ and use private IP address.( cost free,
but single point of failure issue)
 
