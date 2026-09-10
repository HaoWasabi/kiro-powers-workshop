---
title: "Setup Network Infrastructure"
date: "2024-01-01"
weight: 1
chapter: false
pre: "<strong>2.1. </strong>"
---

#### Creating Your VPC Environment

**ℹ️ Information**: In this section, we'll create a Virtual Private Cloud (VPC) with public and private subnets across multiple Availability Zones to ensure high availability for our Auto Scaling architecture.

Navigate to the [AWS Management Console](https://aws.amazon.com/premiumsupport/knowledge-center/sign-in-console/)

- In the search bar, find and select **VPC**

![VPC Console Navigation](/images/2-preparation/2.1-network/2.1.1.png?featherlight=false&width=90pc)

In the **VPC** Console:

- Click **Create VPC**

![Create VPC Button](/images/2-preparation/2.1-network/2.1.2.png?featherlight=false&width=90pc)

In the **Create VPC** interface:

- Select **VPC and more** for the comprehensive setup wizard
- For VPC name, enter **`AutoScaling-Lab`**
- For **IPv4 CIDR block**, enter **`10.0.0.0/16`**

![VPC Configuration](/images/2-preparation/2.1-network/2.1.3.png?featherlight=false&width=90pc)

Configure the VPC architecture:

- Number of Availability Zones: **3** (for maximum resilience)
- Number of public subnets: **3** (one per AZ)
- Number of private subnets: **3** (one per AZ)
- NAT gateways: **None** (we'll keep costs minimal for this lab)

![VPC Architecture Configuration](/images/2-preparation/2.1-network/2.1.4.png?featherlight=false&width=90pc)

Finalize the VPC creation:

- VPC endpoints: **None**
- Click **Create VPC**

![Create VPC Final Step](/images/2-preparation/2.1-network/2.1.5.png?featherlight=false&width=90pc)

#### Configuring Public Subnet Auto-Assignment

**ℹ️ Information**: For EC2 instances in public subnets to receive public IP addresses automatically, we need to enable auto-assign public IPv4 addresses.

To enable auto-assign public IP:

- Select **Subnets** in the left navigation
- Select a **public subnet**
- Click **Edit subnet settings**

![Edit Subnet Settings](/images/2-preparation/2.1-network/2.1.6.png?featherlight=false&width=90pc)

In the subnet settings:

- Check **Enable auto-assign public IPv4 address**
- Click **Save**

![Enable Auto-assign Public IP](/images/2-preparation/2.1-network/2.1.7.png?featherlight=false&width=90pc)

Verify the configuration was successful:

![Verify Subnet Configuration](/images/2-preparation/2.1-network/2.1.8.png?featherlight=false&width=90pc)

**💡 Pro Tip**: Repeat this process for all public subnets to ensure any EC2 instance launched in these subnets automatically receives a public IP address.

#### Creating Application Security Group

**ℹ️ Information**: Security Groups act as virtual firewalls for your instances to control inbound and outbound traffic.

To create a security group for your application:

- In the VPC console, select **Security groups**
- Click **Create security group**

![Create Security Group](/images/2-preparation/2.1-network/2.1.9.png?featherlight=false&width=90pc)

Configure the basic security group details:

- **Security group name**: **`FCJ-Management-SG`**
- **Description**: **`Security Group for FCJ Management`**
- **VPC**: Select the **AutoScaling-Lab** VPC you created

![Security Group Basic Configuration](/images/2-preparation/2.1-network/2.1.10.png?featherlight=false&width=90pc)

Configure the inbound rules:

- Add rule for **SSH** (port **22**) with **Source: My IP** for secure administrative access
- Add rule for **HTTP** (port **80**) with **Source: Anywhere-IPv4** for web traffic
- Add rule for **Custom TCP** (port **5000**) with **Source: Anywhere-IPv4** for the FCJ Management application
- Add rule for **HTTPS** (port **443**) with **Source: Anywhere-IPv4** for secure web traffic

![Security Group Inbound Rules](/images/2-preparation/2.1-network/2.1.11.png?featherlight=false&width=90pc)

**🔒 Security Note**: In a production environment, consider restricting access to specific IP ranges rather than using "Anywhere" for enhanced security.

Review the outbound rules (default allows all outbound traffic) and click **Create security group**

![Security Group Outbound Rules](/images/2-preparation/2.1-network/2.1.12.png?featherlight=false&width=90pc)

#### Creating Database Security Group

**ℹ️ Information**: For database instances, we'll create a separate security group with more restrictive access controls to enhance security.

Configure the database security group:

- **Security Group name**: **`FCJ-Management-DB-SG`**
- **Description**: **`Security Group for DB instance`**
- **VPC**: Select your **AutoScaling-Lab** VPC

Configure the inbound rules:

- Add rule for **MYSQL/Aurora** (port **3306**)
- Set the source to the application security group **FCJ-Management-SG**

![Database Security Group Configuration](/images/2-preparation/2.1-network/2.1.13.png?featherlight=false&width=90pc)

**🔒 Security Note**: By referencing the application security group as the source, you ensure that only EC2 instances with that security group can access the database, following the principle of least privilege.

Review the outbound rules and click **Create Security Group**

![Database Security Group Creation](/images/2-preparation/2.1-network/2.1.14.png?featherlight=false&width=90pc)

**💡 Pro Tip**: This network architecture with separate security groups for application and database tiers follows AWS Well-Architected best practices for security and isolation.
