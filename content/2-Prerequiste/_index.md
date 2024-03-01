---
title : "Preparation "
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

{{% notice info %}}
This lab is created on Linux Mint machine and only introduce command lines compatible with Linux Debian OS.\
Amazon Linux 2 OS is used for EC2 instances.
{{% /notice %}}
{{% notice info %}}
All the steps are built by Terraform script and only show the result with screenshot from Amazon Console.
{{% /notice %}}

The workshop requires you to have a terminal tool which has installed:
- [AWS Command Line Interface](https://aws.amazon.com/cli/): run SSH connection to EC2 instance, setup [IAM credential](https://aws.amazon.com/iam/features/managing-user-credentials/) for authenticating actions to the cloud.
- [Terraform CLI](https://developer.hashicorp.com/terraform/cli): help you to run Terraform commands.  


### Content
  - [IAM credential](2.1-iam-credential/)
  - [AWS CLI](2.2-aws-cli/)
  - [Terraform](2.3-terraform/)