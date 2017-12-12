## AWS EC2

### AMI
DR ????
### ELB
schema: 1. internal 2. internet facing
ConnectionDrainingPolicy ??????
多个subnet

#### Health Check





### AutoScaling Group

#### Lanuch Config
min size
desired size
max size
MetricsCollection
#### Update Policy
1. AutoScalingRollingUpdate (ScheduledAction)
一个instance一个instance替换
##### SuspendProcesses
suspend Auto Scaling processes to avoid making unexpected changes to the group

WaitOnResourceSignals + PauseTime

2. AutoScalingReplacingUpdate
整个asg替换
####

### Route53

### Key pair
