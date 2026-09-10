---
title: "Build App with Kiro Powers"
date: "2024-01-01"
weight: 3
chapter: false
pre: "<strong>3. </strong>"
---

#### Overview

**ℹ️ Information**: In this section, we'll create a Launch Template using an Amazon Machine Image (AMI) from our existing EC2 instance. This approach ensures consistent deployment of our application across multiple instances.

#### Understanding AMIs and Launch Templates

**ℹ️ Information**: Amazon Machine Images (AMIs) capture the complete state of an EC2 instance, including the operating system, applications, and configurations. Launch Templates define the complete instance configuration, including AMI, instance type, networking, and security settings, enabling consistent and repeatable deployments.

#### Creating an Amazon Machine Image (AMI)

To create an AMI from your existing EC2 instance:

1. Navigate to the **EC2** console
2. In the left navigation pane, select **Instances**
3. Select the **FCJ-Management** instance
4. Click **Actions** → **Image and templates** → **Create image**

![Creating an AMI](/images/3-create-launch-template/3.1.png?featherlight=false&width=90pc)

Configure the AMI with the following settings:

- **Image name**: `FCJ-Management-AMI`
- **Image description**: `AMI for FCJ-Management`
- Click **Create Image**

![AMI Configuration](/images/3-create-launch-template/3.2.png?featherlight=false&width=90pc)

Verify the AMI creation:

1. In the left navigation pane, select **AMIs**
2. Locate and select **FCJ-Management-AMI**

![AMI Verification](/images/3-create-launch-template/3.3.png?featherlight=false&width=90pc)

**⚠️ Warning**: The AMI creation process takes approximately 3 minutes to complete. Wait until the **Status** changes to **Available** before proceeding.

![AMI Available Status](/images/3-create-launch-template/3.4.png?featherlight=false&width=90pc)

#### Creating a Launch Template

Once your AMI is available, create a Launch Template:

1. In the EC2 console, select **Launch Templates** from the left navigation pane
2. Click **Create launch template**

![Create Launch Template](/images/3-create-launch-template/3.5.png?featherlight=false&width=90pc)

Configure the basic template information:

- **Launch template name**: `FCJ-Management-template`
- **Template version description**: `Template for FCJ Management`

![Template Basic Information](/images/3-create-launch-template/3.6.png?featherlight=false&width=90pc)

Configure the AMI and instance specifications:

1. Under **Application and OS Image**:
   - Select **My AMIs**
   - Choose **Owned by me**
   - Select the **FCJ-Management-AMI** you created earlier

![AMI Selection](/images/3-create-launch-template/3.7.png?featherlight=false&width=90pc)

2. Configure instance details:
   - **Instance type**: `t2.micro`
   - **Key pair**: `fcj-key`

![Instance Configuration](/images/3-create-launch-template/3.8.png?featherlight=false&width=90pc)

3. Configure network settings:
   - **Subnet**: `AutoScaling-Lab-public-ap-southeast-1a`
   - **Security group**: Select **Select existing security group** and choose **FCJ-Management-SG**
   - Click **Create launch template**

![Network Configuration](/images/3-create-launch-template/3.9.png?featherlight=false&width=90pc)

#### Verifying the Launch Template

After creation, verify your Launch Template:

1. Select **FCJ-Management-template** from the Launch Templates list

![Launch Template Selection](/images/3-create-launch-template/3.10.png?featherlight=false&width=90pc)

2. Review the template configuration details to ensure all settings are correct

![Launch Template Details](/images/3-create-launch-template/3.11.png?featherlight=false&width=90pc)

**💡 Pro Tip**: Launch Templates support versioning, allowing you to iterate on your configuration while maintaining the ability to roll back to previous versions if needed.

**🔒 Security Note**: When creating Launch Templates, always follow the principle of least privilege by assigning only the necessary permissions to your instances through security groups and IAM roles.
