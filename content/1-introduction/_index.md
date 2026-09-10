---
title: "Introduction"
date: "2024-01-01"
weight: 1
chapter: false
pre: "<strong>1. </strong>"
---

#### Introduction

**ℹ️ Information**: In this workshop, we will deploy an application using Amazon EC2 Auto Scaling to ensure dynamic scalability based on user demand. Additionally, we will implement Elastic Load Balancing to distribute traffic and efficiently manage user requests to our application tier.

Before proceeding, please review the document [Deploying FCJ Management Application on a Windows/Amazon Linux Virtual Machine] to understand the base deployment process. We will use the virtual machine with **FCJ Management** already deployed as the foundation for our Auto Scaling Group configuration.

#### Amazon EC2 Auto Scaling

**ℹ️ Information**: When applications go into production, user traffic naturally fluctuates over time. To maintain optimal performance and cost efficiency, we need to adjust (scale) the number of instances accordingly. Amazon EC2 Auto Scaling provides an automated solution that makes this scaling process flexible and hands-free.

Amazon EC2 Auto Scaling automatically adjusts the number of EC2 instances based on application demand. It can scale out (add instances) when traffic increases and scale in (remove instances) when traffic decreases, optimizing resource utilization and reducing costs. It also enhances high availability by distributing instances across multiple Availability Zones, ensuring continuous operation even if part of the infrastructure experiences issues.

#### Scaling Strategies

In this workshop, we will explore the following scaling strategies:

1. **Manual Scaling**: Manually adjusting the number of EC2 instances in the Auto Scaling group based on anticipated demand. This method requires human intervention and does not automatically respond to metrics.

2. **Dynamic Scaling**: Automatically adjusting capacity based on real-time metrics such as CPU utilization, network traffic, or custom CloudWatch metrics. Dynamic scaling includes three primary approaches:
   - **Target Tracking Scaling**: Maintains a specific metric value (e.g., 50% CPU utilization)
   - **Step Scaling**: Responds to metric thresholds with proportional capacity adjustments
   - **Simple Scaling**: Adjusts capacity based on a single metric threshold

3. **Scheduled Scaling**: Configuring specific times to automatically scale instances up or down, such as increasing capacity during business hours and decreasing it during off-hours. This works well for predictable traffic patterns.

4. **Predictive Scaling**: Forecasts future capacity needs by analyzing historical load patterns. This proactive approach allows Amazon EC2 Auto Scaling to scale out in advance of anticipated traffic increases, ensuring optimal performance during peak periods.

#### Launch Templates

**ℹ️ Information**: A Launch Template is a configuration that contains all the necessary parameters to launch EC2 instances. It stores specifications such as instance type, Amazon Machine Image (AMI), key pair, network settings, security groups, and other EC2 configuration details. Launch Templates simplify the instance creation process and enable Auto Scaling groups to consistently deploy properly configured instances.

**💡 Pro Tip**: Launch Templates support versioning, allowing you to iterate on your configuration while maintaining the ability to roll back if needed.

#### Elastic Load Balancing

**ℹ️ Information**: Elastic Load Balancing automatically distributes incoming application traffic across multiple targets, such as EC2 instances, containers, and IP addresses. It helps ensure high availability and fault tolerance by preventing any single resource from becoming overwhelmed with traffic. If one server experiences issues, traffic is automatically redirected to healthy servers, maintaining a seamless user experience.

**🔒 Security Note**: Elastic Load Balancing integrates with AWS Certificate Manager to provide SSL/TLS termination, enhancing security for your applications without adding computational overhead to your instances.

#### Target Groups

**ℹ️ Information**: A Target Group is a component of Elastic Load Balancing that identifies and manages the destinations (targets) to which the load balancer routes traffic. Target Groups allow you to register multiple EC2 instances, configure health checks, and set routing rules. They serve as the bridge between your load balancer and the application instances, enabling intelligent traffic distribution based on health and availability.