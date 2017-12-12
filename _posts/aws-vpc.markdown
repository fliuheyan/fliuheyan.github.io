## AWS VPC

### Concept
CIDR

### ACL

### Security Group
创建ec2 instance时候选择
inbound rules
outbound rules

bastion的设置
ICMP，22端口

????? security group VS ACL
security绑定到instance
ACL绑定到

### Compare security group and ACL
ACL is a general firewall. 可以定义allow和deny rule
通过security group，可以进行更精细的访问控制grant access to a specific CIDR range，允许某个sg进行访问。只能定义allow rule
1. security group is instance level,
2. Security groups can grant access to a specific CIDR range, or to another security group in the VPC or in a peer VPC (requires a VPC peering connection)
3. An instance can be assigned 5 security groups with each security group having 50 rules
4. Security groups are associated with ENI

### Subnet
Availability Zone
#### Route Table

#### NetWork ACL

private subnet

public subnet: Auto-assign Public IP

### IP
private ip
public ip
EIP

### IGW   ??? NAT



### ENI

VPC Peering

Direct Connect

### REA实践
bastion
ssh key放在rattic
