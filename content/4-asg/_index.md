---
title : "Auto Scaling Group"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 4. </b> "
---

[Auto Scaling Group](https://docs.aws.amazon.com/autoscaling/ec2/userguide/auto-scaling-groups.html) is an important service in the architecture to build a scalable applications. Both maintaining  the number of instances in an Auto Scaling group and automatic scaling are the core functionality of the Amazon EC2 Auto Scaling service.

The size of an Auto Scaling group depends on the number of instances that you set as the **desired capacity**. You can adjust its size to meet demand, either manually or by using automatic scaling. The limitation is depended on 2 parameters: **minimum capacity** and **maximum capacity**, so we can prevent unintentional scaling process and then keep control the usage cost.

When working with ASG, we will need to setup 2 main components:
- Launch template: configurations for which EC2 instance will be launched, e.g: instance type, instance profile, keypair, user data (the script will be run for initializing the instance), ...
- Scaling policy: configuration rule for ASG know when to scale out or in to meet the traffic demand.


### Content:

   - [Launch Template](./4.1-launch-template/)
   - [Scaling Policy](./4.2-scaling-policy/)