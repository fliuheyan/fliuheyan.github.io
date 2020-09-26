## EC2

### Instance role
* attach
* replace

### type by charging

* on demand
pay a fixed rate by the hour

* reserved
Offer a significant discount on the hourly charge.
Contract term 1 or 3 years 

* spot
bid price. 
If AWS terminate your instance, you will not be charged for partial hour of usage.
If you terminate the instance yourself, you will be charged.

* dedicated hosts
physical ec2 server

### EBS
* termination protection (default disabled)

* EBS-backed instance, the root EBS volume to be deleted when instance is terminated.
   The additional attached volume would not be deleted.
   
* EBS root/additional volume can be encrypeted (when creating AMIs)

* Can change EBS size & type when instance is running

* Volumes will always be the same AZ with EC2

* When migrate EC2 to another AZ
    * take snapshot from ebs volume
    * create AMI from snapshot
    * launch instance from AMI
* move ebs from one region to another
    * take snapshot
    * create AMI
    
* encrypt `an unencrypted root device`
    * create snapshot of the unencrypted root device
    * create a copy of snapshot & select the encrypt option
    * create AMI from the encrypted snapshot
    * launch AMI

#### Types

* EBS backed

| General Purpose SSD | IOPS SSD | Thought Optimized HDD | Cold HDD|


* instance stored volume (ephemeral storage)
    * hosts fails loose your data

### Snapshot

* exists on S3
* point in time copy of volumes
* incremental (like docker) only sync change part to S3
* can create AMI from both Volume & Snapshot

* snapshot of encrypted EBS is encrypted automatically

* only `unencrypted` snapshot can be shared

* To delete an EBS Snapshot attached to a registered AMI,
 first remove the AMI,
 then the snapshot can be deleted

### Security Group

* all inbound traffic is blocked by default
* outbound allow
* no limitation for numbers of instances within same SG  

* stateful. ACL is stateless ???????
* If allow one inbound traffic in , that traffic is automatically allowed outbound.

* `Cannot` block specific IP address (should use ACL)
* default blocked, only specific allow rules.

### Network

#### ENI
basic

#### EN (enhanced network)
10Gbps - 100Gbps reliable, high throughput

#### EFA (Elastic Fabric Adapter)
when you need to accelerate HPC & Machine Learning

### CloudWatch
monitoring performance

* with EC2 will monitor every 5 mins by default

* 1min intervals by turning on detailed monitoring

* CloudWatch -> performance , CloudTrail -> auditing

#### What Can I do with CloudWatch

* Dashboards
* Alarms
* Events
* Logs


#### BootStrap Scripts (user data ?????)
* run when instance first boots

#### Meta data
```
curl http://169.254.169.254/latest/meta-data/
curl http://169.254.169.254/latest/user-data/
```

#### EFS 
* support NFSv4
* pay for used part
* can scale to PB
* support thousands of NFS connections
* stored cross AZ
* read after write consistency

#### EFS vs. FSX for windows vs. FSX for lustre
* EFS
distributed, highly resilient storage for linux based app

* FSX for windows
centralised storage for windows-based app : SQL Server, IIS

* FSX for lustre (can store directly on S3)
high-speed , high-capacity distributed storage.
`HPC`, financial modeling


#### TODO 

* cluster placement group

* SR-IVO

* What is the underlying Hypervisor for EC2?

* Spread Placement Groups
