---
title : "Introduction"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---
This workshop aims to acquaint you with a standard scalability web application with applying Infrastructure as Code.

Infrastructure as Code (IaC) becomes a important process to manage and provision infrastructure nowadays. With IaC, we can manage the infrastructure with configuration files, which makes it easier to review, edit and distribute configurations. Imagine if you have a architecture with hundreds of services, you can clone it with a few commands in minutes. The workshop uses [Terraform](https://www.terraform.io/) to setup IaC for all the steps, the benefit of it for this workshop is that you can destroy services any time and continue later and you don't need to do all steps in one time.

AWS Auto Scaling Group (ASG) and Elastic Load Balancer (ELB) is a popular architecture to setup a scalability applications. Auto Scaling Group is service help you spin more new EC2 instances to handle high traffic and terminate instances when traffic decrease. It makes your application scalable and optimize the cost for extra resources used in scaling time. Elastic Load Balancer service automatically distributes incoming application traffic to targets in ASG. You don't need to manually manage connection when ASG spin new instances or terminate instances.

CloudWatch is a monitor service that monitors applications, provides insights to operational health. By collecting metrics and logs data, CloudWatch gives visibility into system.

Amazon Route 53 is a highly available and scalable Domain Name System (DNS) web service. This workshop use Route 53 private domain registration feature to create a domain name, so you don't need to purchase for a public domain name.

Beside the above main services, some AWS services is used to work with the main services and will be introduce in detail steps.

**You can refer to the full Terraform script in this repo [https://github.com/vanminh1701/fcj-workshop-001-iac](https://github.com/vanminh1701/fcj-workshop-001-iac)**

![Web Architecture](/images/ws-001-web-apps.svg) 
