---
layout: post
categories: AWS-Hhigh-Traffic
date: 2022-01-16
---





## Idea:

Users -----------> ALB -----------> Target Group (EC2, ECS, Fargate)

ref: https://ithelp.ithome.com.tw/articles/10276378



# ELB

![image-20220117224124822](https://tva1.sinaimg.cn/large/008i3skNgy1gyh1tehx9oj313a0l80us.jpg)

#### Setting Required:

1. Listener -- HTTP protocol
2. SG on ELB VPC
3. TargetGroup's protocol
4. all my VPC's EC2 , to register to TargetGroup, and the SG should make sure all requests able to get in the EC2s

![image-20220117223134440](https://tva1.sinaimg.cn/large/008i3skNgy1gyh1j6252aj311s0ky76l.jpg)

- After Target Group registers, the monitoring not yet started, since the one who does the monitoring task is ELB



![image-20220117223346353](https://tva1.sinaimg.cn/large/008i3skNgy1gyh1lg7ucdj31500lktc9.jpg)

- NLB - for high efficacy
- ALB - for web-application, able to process info in request info and setting





## ELB + ASG Network Structure



- requets come in, through ELB's DNS name
- ELB is followed by a TargetGroup, to which ASG is assigned
  - ELB listener monitors the protocol & port 
  - ELB send commands to TargetGroup
- ASG is set in a TargetGroup, which is placed in a ELB
- ASG dynamically adjusts according to the traffic 
  - 2 Main settings
    1. Launch Template
    2. Scaling Policy



#### Launch Template 

(Security Group & User data)

- helps modulize a project (frontend or backend)

##### AMI

image

##### EC2 Config

第一次的啟動指令。第一次啟起來一台EC2 的時候，aws幫我們執行我們放到user_data中的所有指令



#### Scaling Policy

to increase or decrease EC2 amount

##### Dynamic -- CW (CloudWatch)

most useful

##### Scheduled

##### Predicted -- CW



![image-20220118140601137](https://tva1.sinaimg.cn/large/008i3skNgy1gyhsjgjz8kj31cw0mwdjl.jpg)

![image-20220118140652694](https://tva1.sinaimg.cn/large/008i3skNgy1gyhskc8d8fj316c0modic.jpg)



- Min/Max EC2 amount within a TargetGroup

  ASG decides the "desired" number, letting EC2 instances within a TargetGroup reaches "desired" capacity count![image-20220118140957285](https://tva1.sinaimg.cn/large/008i3skNgy1gyhsnjjw58j31a20komyj.jpg)

- ASG dynamically lower down "desired" capacity accroding to the traffic. "now" may also goes down 3 mins later.
  ![image-20220118141048736](https://tva1.sinaimg.cn/large/008i3skNgy1gyhsoft2svj311i0ju75j.jpg)



# ALB + EC2

![image-20220119005306019](https://tva1.sinaimg.cn/large/008i3skNgy1gyib8rrqqqj310y01ujrk.jpg)









![image-20220119192100040](https://tva1.sinaimg.cn/large/008i3skNgy1gyj79hnn9vj31aa0q6goc.jpg)

![image-20220119192200544](https://tva1.sinaimg.cn/large/008i3skNgy1gyj7ajmwzzj316m0u0tc4.jpg)

![image-20220119192213748](https://tva1.sinaimg.cn/large/008i3skNgy1gyj7arlqcjj31730u0djj.jpg)

![image-20220119192234342](https://tva1.sinaimg.cn/large/008i3skNgy1gyj7b4orj8j31as0q0q5s.jpg)
