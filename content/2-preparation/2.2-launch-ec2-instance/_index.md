---
title: "Launch EC2 Instance"
date: "2024-01-01"
weight: 2
chapter: false
pre: "<strong>2.2. </strong>"
---

#### Launching an EC2 Instance

**ℹ️ Information**: In this section, we'll launch an Amazon EC2 instance that will serve as the foundation for our Auto Scaling configuration. This instance will be used to create an Amazon Machine Image (AMI) for our Launch Template.

Access the [AWS Management Console](https://aws.amazon.com/console/):

- In the search bar, find and select **EC2**

![EC2 Console Navigation](/images/2-preparation/2.2-launch-ec2/2.2.1.png?featherlight=false&width=90pc)

In the **EC2** console:

- Click **Launch instances**

![Launch Instances Button](/images/2-preparation/2.2-launch-ec2/2.2.2.png?featherlight=false&width=90pc)

#### Configure Basic Instance Details

For the instance name:
- Enter **`FCJ-Management`**

![Instance Naming](/images/2-preparation/2.2-launch-ec2/2.2.3.png?featherlight=false&width=90pc)

#### Select Amazon Machine Image (AMI)

For the operating system:
- Select **Quick Start**
- Choose **Amazon Linux**
- Select **Amazon Linux 2023 AMI**

![AMI Selection](/images/2-preparation/2.2-launch-ec2/2.2.4.png?featherlight=false&width=90pc)

#### Configure Instance Type and Key Pair

For compute resources:
- Select **t2.micro** instance type
- Click **Create new key pair**

![Instance Type Selection](/images/2-preparation/2.2-launch-ec2/2.2.5.png?featherlight=false&width=90pc)

**🔒 Security Note**: The key pair is essential for secure SSH access to your instance. Store the private key securely and never share it.

Configure the key pair:
- Name: **`fcj-key`**
- Key pair type: **RSA**
- Private key format: **.pem**
- Click **Create key pair**

![Key Pair Configuration](/images/2-preparation/2.2-launch-ec2/2.2.6.png?featherlight=false&width=90pc)

#### Configure Network Settings

For network configuration:
- Click the **Edit** button
- VPC: Select the VPC you created in the previous step
- Subnet: Choose a **Public subnet**
- Ensure **Auto-assign public IP** is enabled

**💡 Pro Tip**: Placing this instance in a public subnet allows direct internet access, which is necessary for initial setup and configuration.

![Network Configuration](/images/2-preparation/2.2-launch-ec2/2.2.7.png?featherlight=false&width=90pc)

#### Configure Security Group

For access control:
- Select **Select existing security group**
- Choose **FCJ-Management-SG**
- Click **Launch instance**

![Security Group Selection](/images/2-preparation/2.2-launch-ec2/2.2.8.png?featherlight=false&width=90pc)

#### Review and Launch

Verify your instance is launching:

![Instance Launch Confirmation](/images/2-preparation/2.2-launch-ec2/2.2.9.png?featherlight=false&width=90pc)

**⚠️ Warning**: After launching, your instance may take a few minutes to initialize. Wait until the instance state shows as "running" and status checks have passed before proceeding to the next step.
