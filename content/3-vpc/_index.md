---
title : "VPC"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

In this step, we will a standard VPC service:
-  2 public subnets contains Application Load Balancer
-  2 private subnets contains Auto Scaling Group with 2 desired EC2 instances
-  Allow VPC connect to internet by attaching a Internet Gateway, 
-  NAT Gateway for private EC2 instance connect to outside internet

For EC2 instance in Private subnet, we will create a EC2 Instance Connect Endpoint, which attached to our VPC. So that we can connect to instance for checking.

We will use CloudWatch service for collect application logs by installing CloudWatch Logs agent on each instance. To make the traffic go through AWS internal network, we will create a CloudWatch Private Endpoint and attaches to VPC.

### Content
3.1. [VPC Common resources](3.1-common/)\
3.2. [EC2 Instance Connect Endpoint](3.2-ec2-endpoint/)\
3.2. [CloudWatch PrivateLink](3.2-ec2-endpoint/)