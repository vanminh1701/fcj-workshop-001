---
title : "Launch Template"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 4.1 </b> "
---

When you create an Auto Scaling group, you must specify the necessary information to configure the EC2 instances, the Availability Zones and VPC subnets for the instances, the desired capacity, and the minimum and maximum capacity limits.

To configure EC2 instances that are launched by your Auto Scaling group, you can specify a launch template or a launch configuration [(DEPRECATED)](https://docs.aws.amazon.com/autoscaling/ec2/userguide/create-auto-scaling-groups-launch-configuration.html). The following script how to create an Auto Scaling group using a ASG and launch template.


The `data.aws_ami` block will fetch the latest Amazon Linux 2 AMI, if you can use this block to fetch your custom AMI, AWS Market Place.

```
data "aws_ami" "linux_amazon" {
  most_recent = true

  filter {
    name   = "name"
    values = ["amzn2-ami-hvm-*-x86_64-gp2"]
  }

  filter {
    name   = "virtualization-type"
    values = ["hvm"]
  }
}
```

This script will create a `aws_autoscaling_group` and `aws_launch_template`. For Launch Template, we will create some custom configurations: EC2 keypair, user data for installing nginx web server, launch CloudWatch agent, security group

```
resource "aws_autoscaling_group" "web_asg" {
  desired_capacity = 1
  max_size         = 2
  min_size         = 1

  launch_template {
    id      = aws_launch_template.web_instance_template.id
    version = "$Latest"
  }

  vpc_zone_identifier = module.vpc.private_subnets
}

resource "aws_launch_template" "web_instance_template" {
  name_prefix   = "web_instance_template"
  image_id      = data.aws_ami.linux_amazon.id
  instance_type = "t3.micro"
  key_name      = aws_key_pair.cw_agent_instance.key_name
  user_data     = base64encode(templatefile("user-data.sh", { cw-configuration-path = "cw-agent-configuration-file" }))

  vpc_security_group_ids = [module.instance_sg.security_group_id]

  iam_instance_profile {
    name = aws_iam_instance_profile.instance_connect_profile.name
  }

  tag_specifications {
    resource_type = "instance"
    tags = {
      Name = "web_instance"
    }
  }
}
```

The security group for allowing connection from EC2 Instance Connect Endpoint and connect from Load Balancer which we will create in the next step
```
############### Security Group ###############
module "instance_sg" {
  source = "terraform-aws-modules/security-group/aws"

  name        = "lab_instance-sg"
  description = "lab_instance-sg"
  vpc_id      = module.vpc.vpc_id

  ingress_with_source_security_group_id = [
    {
      # Allow traffic from the Instance Connect Endpoint
      source_security_group_id = module.instance_connect_sg.security_group_id
      rule                     = "all-all"
    },
    {
      # Allow traffic from the Application Load Balancer
      source_security_group_id = module.alb_sg.security_group_id
      rule                     = "http-80-tcp"
    }
  ]

  egress_rules       = ["all-all"]
  egress_cidr_blocks = ["0.0.0.0/0"]
}
```

Terraform provides a resource for creating SSH keypair, the private key is saved in [Terraform state](https://developer.hashicorp.com/terraform/language/state). It can be saved to the local file by command `terraform output -raw private_keypair > PRIVATE_KEY`
```
############### Keypair ###############
resource "tls_private_key" "cw_agent_instance" {
  algorithm = "RSA"
  rsa_bits  = 4096
}

resource "aws_key_pair" "cw_agent_instance" {
  key_name   = "cw_agent_instance"
  public_key = tls_private_key.cw_agent_instance.public_key_openssh
}

# Output: "terraform output -raw private_keypair > cw_agent_instance" to save private key
output "private_keypair" {
  value     = tls_private_key.cw_agent_instance.private_key_pem
  sensitive = true
}


```
This block create IAM role for our EC2 instance, which has permission for Instance Connect Endpoint (**inline_policy**), CloudWatchAgentServerPolicy permission (**managed_policy_arns**) 


```
############### IAM role ###############
resource "aws_iam_role" "instance_connect_role" {
  name = "instance-connect-iam-role"
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = "sts:AssumeRole"
        Effect = "Allow"
        Sid    = ""
        Principal = {
          Service = "ec2.amazonaws.com"
        }
      }
    ]
  })
  inline_policy {
    name = "instance-connect-role-inline-policy"

    policy = jsonencode({
      Version = "2012-10-17"
      Statement = [
        {
          "Sid" : "EC2InstanceConnect",
          "Action" : "ec2-instance-connect:OpenTunnel",
          "Effect" : "Allow",
          "Resource" : aws_ec2_instance_connect_endpoint.lab_instance.arn
        },
        {
          "Sid" : "SSHPublicKey",
          "Effect" : "Allow",
          "Action" : "ec2-instance-connect:SendSSHPublicKey",
          "Resource" : "*"
        }
      ]
    })
  }

  managed_policy_arns = [
    "arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore",
    "arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy",
  ]

  tags = {
    tag-key = "tag-value"
  }
}

resource "aws_iam_instance_profile" "instance_connect_profile" {
  name = "instance_connect_role-profile"
  role = aws_iam_role.instance_connect_role.name
}
```



