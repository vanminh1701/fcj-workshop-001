---
title : "CloudWatch PrivateLink"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3.3 </b> "
---

We use CloudWatch to collect application logs from EC2 instances. To use this feature, we will need to install a CloudWatch Log agent in each EC2 and then this service will read logs files and send to CloudWatch service on AWS cloud. For default behavior, the instance need to access internet and the agent will send a request to AWS Log endpoint through internet. To secure our log data from transfer outside internet and have a better latency, we will create a [CloudWatch Log Interface Endpoint](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/cloudwatch-logs-and-interface-VPC.html#create-VPC-endpoint-for-CloudWatchLogs) attached to our VPC. 

```
data "aws_region" "current" {}

module "vpc_endpoint_sg" {
  source = "terraform-aws-modules/security-group/aws"

  name        = "SG VPC Cloudwatch Endpoint"
  description = "SG VPC Cloudwatch Endpoint"
  vpc_id      = module.vpc.vpc_id

  ingress_rules       = ["all-all"]
  ingress_cidr_blocks = [local.vpc.cidr]
}

resource "aws_vpc_endpoint" "cw-endpoint" {
  vpc_id            = module.vpc.vpc_id
  service_name      = "com.amazonaws.${data.aws_region.current.name}.logs"
  vpc_endpoint_type = "Interface"

  security_group_ids = [module.vpc_endpoint_sg.security_group_id]
  subnet_ids         = module.vpc.private_subnets
}
```
{{< highlight go "linenos=inline" >}}
TODO: Controlling access to your CloudWatch Logs VPC endpoint
{{< / highlight >}}
