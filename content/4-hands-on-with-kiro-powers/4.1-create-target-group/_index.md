---
title: "Create Target Group"
date: "2024-01-01"
weight: 1
chapter: false
pre: "<strong>4.1. </strong>"
---

#### Creating an Application Load Balancer Target Group

**ℹ️ Information**: Target groups route requests to individual registered targets such as EC2 instances. When creating a load balancer, you define at least one target group, then register targets to that group.

Navigate to the Target Group creation interface:

1. In the EC2 management console, locate the **Load Balancing** section in the left navigation pane
2. Select **Target Groups**
3. Click **Create target group**

![Target Group Creation Interface](/images/4-setup-load-balancer/4.1.1.png?featherlight=false&width=90pc)

#### Configuring Target Group Details

In the **Specify group details** section, configure the following settings:

1. Basic configuration:
   - Target type: **Instances**
   - Target group name: `FCJ-Management-TG`

![Basic Target Group Configuration](/images/4-setup-load-balancer/4.1.2.png?featherlight=false&width=90pc)

2. Network settings:
   - Protocol: **HTTP**
   - Port: **5000**
   - IP address type: **IPv4**
   - VPC: **AutoScaling-Lab**
   - Protocol version: **HTTP1**

![Network Settings Configuration](/images/4-setup-load-balancer/4.1.3.png?featherlight=false&width=90pc)

3. Click **Next** to proceed to target registration

![Proceed to Target Registration](/images/4-setup-load-balancer/4.1.4.png?featherlight=false&width=90pc)

#### Registering Targets

In the **Register targets** section:

1. Select your instance from the Available instances list
2. Verify port **5000** is specified for the selected instance
3. Click **Include as pending below**

![Registering Target Instances](/images/4-setup-load-balancer/4.1.5.png?featherlight=false&width=90pc)

4. Review the pending targets in the list
5. Click **Create target group** to complete the process

![Finalizing Target Group Creation](/images/4-setup-load-balancer/4.1.6.png?featherlight=false&width=90pc)

#### Verifying Target Group Creation

After successful creation, you can view and manage your new target group:

1. Select the newly created **FCJ-Management-TG** from the target groups list
2. Review the target group details, health status, and registered targets

![Target Group Verification](/images/4-setup-load-balancer/4.1.7.png?featherlight=false&width=90pc)

**💡 Pro Tip**: Target groups support health checks to ensure traffic is only routed to healthy instances. You can customize health check settings including path, port, threshold counts, and timeout values to match your application's specific requirements.

**🔒 Security Note**: When configuring target groups, consider using HTTPS protocol with proper certificate management for production workloads to ensure encrypted communication between clients and your load balancer.
