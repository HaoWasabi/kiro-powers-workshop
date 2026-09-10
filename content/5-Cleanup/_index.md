---
title: "Clean up Resources"
date: "2024-01-01"
weight: 5
chapter: false
pre: "<strong>5. </strong>"
---

#### Overview

**ℹ️ Information**: After completing the workshop, it's important to properly clean up all AWS resources to avoid ongoing charges. This section guides you through the systematic removal of all resources created during this workshop.

#### Delete Auto Scaling Group

1. Navigate to the EC2 management console
2. In the left navigation pane, scroll down and select **Auto Scaling Groups**
3. Select the Auto Scaling Group **FCJ-Management-ASG**
4. Click the **Actions** button at the top right of the screen
5. Choose **Delete**

![Delete Auto Scaling Group](/images/8-Cleanup/8.1.png?featherlight=false&width=90pc)

#### Delete Load Balancer

1. In the EC2 management console, on the left navigation pane, select **Load Balancers**
2. Select the Load Balancer **FCJ-Management-LB**
3. Click the **Actions** button
4. Choose **Delete load balancer**

![Delete Load Balancer](/images/8-Cleanup/8.2.png?featherlight=false&width=90pc)

#### Delete Target Group

1. In the EC2 management console, on the left navigation pane, select **Target Groups**
2. Select the Target Group **FCJ-Management-TG**
3. Click the **Actions** button
4. Choose **Delete**

![Delete Target Group](/images/8-Cleanup/8.3.png?featherlight=false&width=90pc)

#### Delete Launch Template

1. In the EC2 management console, on the left navigation pane, select **Launch Templates**
2. Select the Launch Template **FCJ-Management-TG**
3. Click the **Actions** button
4. Choose **Delete template**

![Delete Launch Template](/images/8-Cleanup/8.4.png?featherlight=false&width=90pc)

**⚠️ Warning**: Ensure you've terminated all instances using this launch template before deletion to avoid potential errors.

#### Deregister AMI

1. In the EC2 management console, on the left navigation pane, select **AMIs**
2. Select the AMI **FCJ-Management-AMI**
3. Click the **Actions** button
4. Choose **Deregister AMI**

![Deregister AMI](/images/8-Cleanup/8.5.png?featherlight=false&width=90pc)

**💡 Pro Tip**: After deregistering an AMI, remember that its associated EBS snapshots will still exist and may incur charges. Consider deleting these snapshots separately if they're no longer needed.

#### Terminate EC2 Instance

1. In the EC2 management console, on the left navigation pane, select **Instances**
2. Select the **FCJ-Management** instance
3. Click the **Instance state** button
4. Choose **Terminate (delete) instance**

![Terminate EC2 Instance](/images/8-Cleanup/8.6.png?featherlight=false&width=90pc)

**🔒 Security Note**: Terminating an instance permanently deletes its instance store volumes. If you have important data, ensure it's backed up before proceeding.

#### Delete RDS Database

1. Navigate to the **RDS** console
2. On the left navigation pane, select **Databases**
3. Select the database instance **fcj-management-db-instance**
4. Click **Modify**

![Modify RDS Database](/images/8-Cleanup/8.7.png?featherlight=false&width=90pc)

5. In the Modify DB Instance section, scroll down and uncheck **Enable deletion protection**
6. Click **Continue**

![Disable Deletion Protection](/images/8-Cleanup/8.8.png?featherlight=false&width=90pc)

7. In the Schedule modifications section:
   - Select **Apply immediately**
   - Click **Modify DB instance**

![Apply Modifications](/images/8-Cleanup/8.9.png?featherlight=false&width=90pc)

8. After the modification completes:
   - Select the database instance **fcj-management-db-instance**
   - Click the **Actions** button
   - Choose **Delete**

![Delete RDS Database](/images/8-Cleanup/8.10.png?featherlight=false&width=90pc)

9. In the confirmation dialog:
   - Select **I acknowledge that upon instance deletion, automated backups, including system snapshots and point-in-time recovery, will no longer be available**
   - Enter **delete me** in the confirmation field
   - Click **Delete**

![Confirm RDS Deletion](/images/8-Cleanup/8.11.png?featherlight=false&width=90pc)

**⚠️ Warning**: This action permanently deletes your database and all its data. If you need to preserve any information, create a final snapshot before deletion.

#### Delete Subnet Group

1. In the RDS console, select **Subnet groups** from the left navigation pane
2. Select the subnet group **fcj-management-subnet-group**
3. Click **Delete**

![Delete Subnet Group](/images/8-Cleanup/8.12.png?featherlight=false&width=90pc)

**ℹ️ Information**: After completing these steps, verify in the AWS Billing Dashboard that there are no unexpected charges related to resources from this workshop. Some resources like EBS snapshots or CloudWatch logs may persist unless explicitly deleted.
