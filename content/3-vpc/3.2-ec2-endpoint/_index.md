---
title : "EC2 Instance Connect Endpoint"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---

[EC2 Instance Connect](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connect-linux-inst-eic.html) is a solution for SSH connection to private instance without requiring the instance to have a public IPv4 address. We will use it to connect to private instances.

The requirements for EC2 Instance Connect (EIC) are security group that can connect the instance SG, and the instance has attached permission for EIC. The configuration for EC2 instance will be presented when we create [Launch Template](/4-asg/4.1-launch-template) for Auto Scaling Group later.


To limit connectivity to only the instances in the VPC, we recommend that the outbound rule only allows traffic to the specified destination. In this case we choose limit to VPC CIDR.


```
module "instance_connect_sg" {
  source      = "terraform-aws-modules/security-group/aws"
  name        = "instance_connect_sg"
  description = "instance_connect_sg"
  vpc_id      = module.vpc.vpc_id

  egress_rules       = ["all-all"]
  egress_cidr_blocks = ["0.0.0.0/0"]
}

resource "aws_ec2_instance_connect_endpoint" "lab_instance" {
  subnet_id          = module.vpc.private_subnets[0]
  security_group_ids = [module.instance_connect_sg.security_group_id]
}
```

Result in Console
![ec2-instance-connect-endpoint](/images/3.connect/3.2-vpc-eice.png) 
