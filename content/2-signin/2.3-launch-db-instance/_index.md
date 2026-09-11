---
title: "Launch a Database Instance with RDS"
date: "2024-01-01"
weight: 3
chapter: false
pre: "<strong>2.3. </strong>"
---

#### Creating a DB Subnet Group

**ℹ️ Information**: A DB subnet group is a collection of subnets that you designate for your RDS database instances within a VPC. This configuration ensures high availability by allowing Amazon RDS to deploy instances across multiple Availability Zones.

Access the AWS Management Console:

- In the search bar, find and select **Aurora and RDS**

![RDS Console Navigation](/images/2-preparation/2.3-rds/2.3.1.png?featherlight=false&width=90pc)

In the Aurora and RDS console:

- Select **Subnet groups** from the left navigation panel
- Click **Create DB subnet group**

![Create DB Subnet Group](/images/2-preparation/2.3-rds/2.3.2.png?featherlight=false&width=90pc)

Configure the DB subnet group details:

- For **Name**, enter **`FCJ-Management-Subnet-Group`**
- For **Description**, enter **`Subnet Group for FCJ Management`**
- Select the VPC you created earlier (**AutoScaling-Lab**)

![DB Subnet Group Details](/images/2-preparation/2.3-rds/2.3.3.png?featherlight=false&width=90pc)

Configure the subnet selection:

- Select multiple Availability Zones for redundancy
- Choose the **private subnets** for enhanced security

**🔒 Security Note**: Always place your database instances in private subnets to prevent direct internet access, reducing your attack surface.

![Subnet Selection](/images/2-preparation/2.3-rds/2.3.4.png?featherlight=false&width=90pc)

Complete the creation:

- Click **Create**

![Create Button](/images/2-preparation/2.3-rds/2.3.5.png?featherlight=false&width=90pc)

Verify the DB subnet group has been created successfully with multiple AZs:

![Subnet Group Created](/images/2-preparation/2.3-rds/2.3.6.png?featherlight=false&width=90pc)

![Subnet Group Details](/images/2-preparation/2.3-rds/2.3.7.png?featherlight=false&width=90pc)

#### Launching an Amazon RDS Database Instance

**ℹ️ Information**: Amazon RDS makes it easy to set up, operate, and scale a relational database in the cloud. It provides cost-efficient and resizable capacity while automating time-consuming administration tasks.

Navigate to the RDS console:

- Select **Databases** from the left navigation panel
- Click **Create database**

![Create Database](/images/2-preparation/2.3-rds/2.3.8.png?featherlight=false&width=90pc)

Select the database creation method:

- Choose **Standard create** for full configuration options

![Standard Create Option](/images/2-preparation/2.3-rds/2.3.9.png?featherlight=false&width=90pc)

Select the database engine:

- Choose **MySQL**

![MySQL Engine Selection](/images/2-preparation/2.3-rds/2.3.10.png?featherlight=false&width=90pc)

Configure the deployment template:

- Select **Production** template
- Choose **Multi-AZ DB instance** for high availability

**💡 Pro Tip**: Multi-AZ deployments enhance availability by automatically provisioning and maintaining a synchronous standby replica in a different Availability Zone.

![Production Template](/images/2-preparation/2.3-rds/2.3.11.png?featherlight=false&width=90pc)

Configure instance details:

- For **DB instance identifier**, enter **`fcj-management-db-instance`**
- For **Master username**, enter **`admin`**
- Select **Self managed** for credential management

![Instance Configuration](/images/2-preparation/2.3-rds/2.3.12.png?featherlight=false&width=90pc)

Set the database password:

- For **Master password**, enter a strong password (for this lab: **`123Vodanhphai`**)
- Confirm the password

**🔒 Security Note**: In production environments, always use complex passwords and consider using AWS Secrets Manager to automatically rotate credentials.

![Password Configuration](/images/2-preparation/2.3-rds/2.3.13.png?featherlight=false&width=90pc)

Configure instance specifications:

- Select **`db.m5d.large`** for the instance class
- Choose **General Purpose SSD (gp3)** for storage type
- Set **Allocated storage** to **`20`** GiB

![Instance Specifications](/images/2-preparation/2.3-rds/2.3.14.png?featherlight=false&width=90pc)

Configure connectivity settings:

- Select **Don't connect to an EC2 compute resource**
- For **VPC**, select your created VPC (**`AutoScaling-Lab`**)
- For **Subnet group**, choose the subnet group you created earlier

![Connectivity Configuration](/images/2-preparation/2.3-rds/2.3.15.png?featherlight=false&width=90pc)

Configure security settings:

- For **VPC security group**, select **Choose existing**
- For **Security Group**, select **FCJ-Management-DB-SG**

**🔒 Security Note**: Using dedicated security groups for your database tier helps maintain proper network segmentation and access control.

![Security Configuration](/images/2-preparation/2.3-rds/2.3.16.png?featherlight=false&width=90pc)

Configure initial database settings:

- Set the initial database name to **`awsfcjuer`**
- Leave other settings at their default values

![Initial Database Configuration](/images/2-preparation/2.3-rds/2.3.17.png?featherlight=false&width=90pc)

Complete the database creation:

- Click **Create database**

![Create Database Button](/images/2-preparation/2.3-rds/2.3.18.png?featherlight=false&width=90pc)

Verify successful database creation:

![Database Created Successfully](/images/2-preparation/2.3-rds/2.3.19.png?featherlight=false&width=90pc)

Note the database endpoint and port for future reference:

**💡 Pro Tip**: You'll need this endpoint information when configuring your application to connect to the database.

![Database Endpoint Information](/images/2-preparation/2.3-rds/2.3.20.png?featherlight=false&width=90pc)
