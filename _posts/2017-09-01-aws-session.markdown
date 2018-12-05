---
layout: post
title: "AWS Session"
date: 2017-09-01 12:00:00
---

## AWS Components Overall

### EC2
> Elastic Compute Cloud

* EC2
* EBS
* AMI
* SecurityGroup
* ELB
* AutoScalingGroup

### CloudWatch

> Monitoring System

### CloudTrail

> Monitoring ,Logging your AWS options

### SQS

> Messaging Queue

### Database

* RDS
* RedShift
* SimpleDB
* DynamoDB

### AWS API Gateway

> Facade pattern, scale single endpoint

### AWS CloudFront

> CDN

### AWS Lambda

> Serverless

### S3

> file storage service & static web hosting

### Glacier

> Archive your S3 data to save money

### Route53

> DNS Service

### CloudFormation

> Immutable deploy

## IAM
> AWS Identity and Access Management aims to control users' access to AWS services

### Policy
> apply permission to User, Group, Role

* inline-policy
embedded with other User,Group,Role

* managed-policy
standalone policies that you can attach to multiple Users,Groups,Roles
  * AWS managed policies
  * Customer managed policies

### Role
* a bunch of policies
* trust relationship
Which user or role can assume this role

### User
* root user
* IAM user

credential under the path `~/.aws/`
* `credential` file
* `config` file

### Group
> A bundle of User and you can attach policy

#### differences between User and Role
> An IAM role is similar to a user actually

* User associated with a person. Role is intended to be assumable by anyone who needs it.
* Role doesn't have any credentials.

#### instance profile
> Grant permission to your EC2 instance

Cannot attach multiple IAM roles to a single instance

[example](https://s3.amazonaws.com/cloudformation-templates-us-east-1/ec2_instance_with_instance_profile.template)

### Identity providers
> Granting external user or service permissions to use AWS resources.

#### SAML Identity Provider

```
<EntityDescriptor entityID="urn:your_name.auth0.com" xmlns="urn:oasis:names:tc:SAML:2.0:metadata">
  <IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
    <KeyDescriptor use="signing">      
    </KeyDescriptor>
  </IDPSSODescriptor>
</EntityDescriptor>
```

### Group
> a collection of IAM users

### KMS
> AWS Key Management Service (AWS KMS) is a managed service that makes it easy for you to create and control the encryption keys used to encrypt your data

`aws kms encrypt --key-id your_key --plaintext your_content`

### STS
>The AWS Security Token Service (STS) is a web service that enables you to request temporary, limited-privilege credentials for AWS Identity and Access Management (IAM) users

`aws sts assume-role --role-arn your_role --role-session-name free_input_session`

* default session is 3600 seconds
* cannot assume role from root user

### Integration with authentication service(Ldap, SSO)

[login](https://fliuheyan.auth0.com/login?client=BxmOE1EeLS5R7AlNb3FNrW4P5xF0mGST&protocol=samlp&connection=Username-Password-Authentication&state=Jlh84P6RI0TH-w3d3SmapgxCDvFDFFr4)

* create identity providers
* create role & attach policy
* create rule in auth0

### Using Temporary Security Credentials instead of sharing Credential file



## AWS VPC

## Description
> Amazon Virtual Private Cloud (Amazon VPC) lets you provision a logically isolated section of the AWS Cloud where you can launch AWS resources in a virtual network that you define.

* help you manage your network
* security

### Region & Available Zone

* one region locate in one country
* one region contains tons of AZ.
* each AZ is physical isolated

### Subnet
* VPC spans AZ
* Subnet resides in one AZ

#### Public Subnet or Private Subnet

#### IGW
> a logical connection between an Amazon VPC and the Internet

### NAT
* Uses an EIP

* Exists in a subnet

* Cannot be used by an instance in that subnet (you need to route to the NAT gateway from another subnet)

* Requires an IGW (the NAT gateway routes to an IGW to access the internet)

#### NAT instance
> a EC2 instance configured to forward traffic to the Internet.
#### NAT Gateway
> AWS full managed NAT instance.
### RouteTable
> assigned to subnets and define how traffic routes to different destinations.

0.0.0.0/0 route towards IGW

### ACL
> Access Control List. Control your inbound and outbound traffic

#### ACL vs Security Group
* ACL default allow all traffic while SG bans all traffic
* ACL works at subnet level while SG works for instance

## Practice in the real world

### High Availability

### Bastion

#### ElasticIP
> attach static IP to a ENI


### Planing your network

#### arrange subnets for different layer
#### Specify CIDR

### How to visit resources which inside another VPC
* VPC Peer Connection

### A typical architecture
<figure>
	<img src="{{ site.baseurl }}/assets/typical-architecture.png" alt="hehe">
	<figcaption>
		A typical architecture
	</figcaption>
</figure>
