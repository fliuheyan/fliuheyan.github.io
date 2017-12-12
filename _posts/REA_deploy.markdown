REA的里面的项目部署主要有三种方式：
第一种是那些还部署在data center中的老项目。
REA采用的data center主要是在melbourne的eqnix,和在荷兰的eyr，它采用地理位置这么远的地方主要是考虑到DR。
RPM包上传到KOJI。
CI/CD工具 jenkins

第二种就是使用AWS进行部署
cloudformation
pattern:
生成的主要是AMI


### 第三种Docker部署
REA-shipper ：DSL生成cloudformation file
pattern:
Akkamai
Route53
ELB
ASG
Cloudformation
监控和报警splunk和pagerduty

#### Steps:
1. 准备docker base image(eginx, splunk forwarder,)
2. 准备AWS vpc, kms, deploy role, pagerduty sns,
3.
4. 通过SAML得到一个base role，这个role可以assume到任意一个deploy role当中
5. cloudformation create-stack/update-stack

1. Puppet,Chelf
2. AMI

pattern
3. Docker
pattern


HA和DR都是在做冗余
HA的冗余
DR的冗余
1.AMI
2.Docker

一些实践

cron task
1. ec2 cron.d
2.
