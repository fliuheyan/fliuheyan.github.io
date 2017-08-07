---
layout: post
title: "AWS IAM"
date: 2017-07-01 12:00:00
---

## Concepts

### Policy
apply permission to User, Group, Role

* inline-policy
embeded directly into a single User,Group,Role

* managed-policy
standalone policies that you can attach to multiple Users,Groups,Roles
** AWS managed policies
** Customer managed policies

### Role
1. Which account or AWS Service can assume this role
trust relationship
2. policies

* instance profile
Applications must sign their API requests with AWS credentials
Cannot attach multiple IAM roles to a single instance
[example](https://s3.amazonaws.com/cloudformation-templates-us-east-1/ec2_instance_with_instance_profile.template)

### Identity providers
> Giving external user identities permissions to use AWS resources.
name-metadata.xml
```
<EntityDescriptor entityID="urn:your_name.auth0.com" xmlns="urn:oasis:names:tc:SAML:2.0:metadata">
  <IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
    <KeyDescriptor use="signing">      
    </KeyDescriptor>
  </IDPSSODescriptor>
</EntityDescriptor>
```

### Group
### User
* root User
* IAM user
> It is a security best practice to not use your root account because the root account grants access to all services and resources. Grant users the minimum amount of privilege necessary, which is known as least privilege.

credential
under the path `~/.aws/`
* `credential` file
* `config` file
associated with group
### KMS
### STS
assume role
cannot assume role from root user

## Intergration with Auth0
* create identity providers
* create role & attach policy
* create rule in auth0 //用来绑定aws role和auth0 client

