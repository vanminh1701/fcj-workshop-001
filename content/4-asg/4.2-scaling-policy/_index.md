---
title : "Scaling Policy"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 4.2 </b> "
---

Scaling is the ability to increase or decrease the compute capacity of your application. Scaling starts with an event, or scaling action, which instructs an Auto Scaling group to either launch or terminate Amazon EC2 instances.

Considerations for scaling policy:
- What role should Amazon EC2 Auto Scaling play in your application's architecture ? increase or decrease capacity, or just keep the desired capacity
- What cost constraints are important to you? Knowing your cost constraints helps you decide when to scale your applications, and by how much.
- What metrics are important to your application? Amazon CloudWatch supports a number of different metrics that you can use with your Auto Scaling group: depend on CPU, RAM, data transfer, or custom metric.

There are 4 types of scaling policy:
- [Manual scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-scaling-manually.html): you change the desired capacity on Console or CLI
- [Scheduled scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-scheduled-scaling.html): you can set scaling based on predictable load changes in specific time, e.g: increase capacity from 9am to 5pm every day.
- [Dynamic scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-scale-based-on-demand.html): you defined the scaling rule to follow some desired state (target tracking) or just meet a threshold and increase/decrease the capacity.
- [Predictive scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-predictive-scaling.html): AWS will scale depend on analyzing historical data to detect daily or weekly patterns in traffic flows, then forecast future capacity so ASG proactively increase the capacity to match the anticipated load. 

In this lab, we will use Dynamic Scaling with target tracking rule. It is recommend by AWS because it is a easy to use and make your architecture simple. After you can deeply understand the system, you can change to another policy for better optimization. The script creates `TargetTrackingScaling` rule with average CPU on all instances in ASG. For example, the current ASG has 2 EC2 instances, each CPU workload is 60% and 70%, so the average CPU value is 65%, so the scaling process won't work. When the first instance's CPU increase to 80%, now the average CPU value is 85%, and scaling process will work.

**Choose metrics**

You can create target tracking scaling policies with either predefined metrics or custom metrics. AWS has 4 predefined metrics that you can choose when create ASG on Console: `ASGAverageCPUUtilization`, `ASGAverageNetworkIn`, `ASGAverageNetworkOut`, `ALBRequestCountPerTarget`.
```
resource "aws_autoscaling_policy" "cpu" {
  name                      = "cpu-auto-scaling"
  autoscaling_group_name    = aws_autoscaling_group.web_asg.name
  policy_type               = "TargetTrackingScaling"
  estimated_instance_warmup = 60
  enabled                   = true

  target_tracking_configuration {
    predefined_metric_specification {
      predefined_metric_type = "ASGAverageCPUUtilization"
    }

    target_value = 70
  }
}
```

![VPC-result](/images/4.asg/4.2-asg.png) 
