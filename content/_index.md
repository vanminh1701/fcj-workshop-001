---
title : "Session Management"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
---
# Work with Amazon System Manager - Session Manager

### Overall
 In this lab, you'll learn how to setup and monitor a high scalability website with Terraform tool.
![ConnectPrivate](/images/arc-log.png) 

### Content
 1. [Introduction ](1-introduce/)
 2. [Preparation](2-prerequiste/)
 3. [VPC](3-vpc/)
 4. [AutoScaling Group](4-asg/)
 5. [Elastic Load Balancer](5-elb/)
 6. [Route 53](6-route53/)
 7. [Cleanup](7-cleanup/)


<!---
1. VPC with 2 public subnet and 2 private subnet
   
2. Autoscaling Group in private subnet: EC2 instance with cloudWatch agent managed by SSM service
  configuration file to match log group with each instance in ASG
  - Config CloudWatch group to receive application log from instance (Nginx logs)
  - save CW configuration to ssm parameter store
  - create launch template to Add user_data to download cloudwatch agent and configuration from ssm
  - create ASG and scaling policy
  - Setup VPC private link for cloudwatch endpoint => add dns link to cw configuration   
1. CloudWatch alarm on specific error and send notification to Slack
  - setup metric filter and SNS topic 
  - Lambda function to send request to webhook
2. Config ALB + ACM and Cloudflare DNS
  - Route 53
  - TODO: ALB => nginx server
*/
-->
