---
title: "Create Load Balancer"
date: "2024-01-01"
weight: 2
chapter: false
pre: "<strong>4.2. </strong>"
---

#### Creating an Application Load Balancer

**ℹ️ Information**: Application Load Balancers operate at the application layer (Layer 7) and are ideal for HTTP/HTTPS traffic routing. They provide advanced request routing capabilities, support for containerized applications, and integration with AWS services.

Navigate to the Load Balancer creation interface:

1. In the EC2 management console:
   - Select **Load Balancers** from the left navigation pane
   - Click the **Create Load Balancer** button

![Load Balancer Creation Interface](/images/4-setup-load-balancer/4.2.1.png?featherlight=false&width=90pc)

2. In the "Compare and select load balancer type" panel:
   - Locate the **Application Load Balancer** section
   - Click **Create**

![Load Balancer Type Selection](/images/4-setup-load-balancer/4.2.2.png?featherlight=false&width=90pc)

#### Configuring Load Balancer Settings

In the "Create Application Load Balancer" configuration panel:

1. Basic configuration:
   - Load balancer name: `FCJ-Management-LB`
   - Scheme: **Internet-facing**
   - IP address type: **IPv4**

![Basic Load Balancer Configuration](/images/4-setup-load-balancer/4.2.3.png?featherlight=false&width=90pc)

2. Network mapping:
   - VPC: **AutoScaling-Lab**
   - Availability Zones: Select all three public subnets in **ap-southeast-1a**, **ap-southeast-1b**, and **ap-southeast-1c**

![Network Mapping Configuration](/images/4-setup-load-balancer/4.2.4.png?featherlight=false&width=90pc)

**⚠️ Warning**: Ensure you select only public subnets for internet-facing load balancers. Private subnets cannot receive traffic from the internet.

3. Security and routing configuration:
   - Security groups: **FCJ-Management-SG**
   - Listeners and routing: Set default action to forward to **FCJ-Management-TG**

![Security and Routing Configuration](/images/4-setup-load-balancer/4.2.5.png?featherlight=false&width=90pc)

4. Review the configuration summary:
   - Verify all settings are correct
   - Click **Create load balancer**

![Configuration Review](/images/4-setup-load-balancer/4.2.6.png?featherlight=false&width=90pc)

#### Verifying Load Balancer Deployment

After successful creation:

1. Select **FCJ-Management-LB** from the load balancers list
2. Review the load balancer details, including DNS name, state, and listeners

![Load Balancer Details](/images/4-setup-load-balancer/4.2.7.png?featherlight=false&width=90pc)

3. Explore the load balancer's connections:
   - Select **Resource map - new** to visualize the load balancer architecture
   - Verify connections to target groups and registered targets

![Load Balancer Resource Map](/images/4-setup-load-balancer/4.2.8.png?featherlight=false&width=90pc)

**💡 Pro Tip**: Application Load Balancers support content-based routing, allowing you to route requests to different target groups based on URL paths, host headers, or query parameters. This enables microservices architectures where different services handle specific request patterns.

**🔒 Security Note**: For production workloads, consider implementing HTTPS listeners with certificates from AWS Certificate Manager (ACM) to ensure encrypted communication between clients and your load balancer.
