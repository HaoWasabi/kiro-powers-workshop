---
title: "Prepare metric for Predictive scaling"
date: "2024-01-01"
weight: 6
chapter: false
pre: "<strong>2.6. </strong>"
---

### Preparing Data for Predictive Scaling

Since Predictive Scaling requires more than 2 days' worth of data to make predictions for the following days, and we don’t have this data available, we will need to simulate such an environment.

### Preparation Steps

First, create a new folder named `metric-preparation` and navigate into this directory.

```bash
mkdir metric-preparation && cd metric-preparation
```

Then, download the script to prepare the data.

```bash
curl -o prepare-metric-data.sh https://raw.githubusercontent.com/awslabs/ec2-spot-workshops/master/workshops/efficient-and-resilient-ec2-auto-scaling/prepare-metric-data.sh
```

![2.6.1.png](/images/2-preparation/2.6-prepare-metric-data/2.6.1.png)

After downloading, modify the script slightly.

```bash
vim prepare-metric-data.sh
```
Edit the time variable to:
```bash
time=$(date -d "$((5*i)) minutes ago")
```
![2.6.2.png](/images/2-preparation/2.6-prepare-metric-data/2.6.2.png)

Once the script is edited, proceed to download the raw data, which is why we need to prepare this data processing script first. Start with metrics for the CPU.

```bash
curl -o metric-cpu.json https://raw.githubusercontent.com/awslabs/ec2-spot-workshops/master/workshops/efficient-and-resilient-ec2-auto-scaling/metric-cpu.json
```

![2.6.3.png](/images/2-preparation/2.6-prepare-metric-data/2.6.3.png)

Next, download the data for Instance.

```bash
curl -o metric-instances.json https://raw.githubusercontent.com/awslabs/ec2-spot-workshops/master/workshops/efficient-and-resilient-ec2-auto-scaling/metric-instances.json
```

![2.6.4.png](/images/2-preparation/2.6-prepare-metric-data/2.6.4.png)

Edit the two types of data one by one, starting with CPU.

```bash
bash prepare-metric-data.sh metric-cpu.json FCJ-Management-ASG && cat metric-cpu.json
```

![2.6.5.png](/images/2-preparation/2.6-prepare-metric-data/2.6.5.png)

Then proceed with the instance data.

```bash
bash prepare-metric-data.sh metric-instances.json FCJ-Management-ASG && cat metric-instances.json
```

![2.6.6.png](/images/2-preparation/2.6-prepare-metric-data/2.6.6.png)

{{% notice note %}}
The parameter **FCJ-Management-ASG** that appears in the two commands above is the name of the Auto Scaling Group that we will create later. Therefore, you need to create an ASG with the same name afterward, or you should change it to a different name now.
{{% /notice %}}

### Upload Data to CloudWatch

In Amazon Linux 2023, if you are using the correct AMI, AWS CLI is pre-installed. At this point, you only need to configure the credentials. Remember that you must have an IAM User with sufficient permissions to upload data to CloudWatch or at least enough to complete this workshop.

Go to the IAM page, open the IAM User details, and get the Access Key Id and Secret Access Key. If you don't have one, create a new one.

```bash
aws configure
```

And configure the credentials.

![2.6.7.png](/images/2-preparation/2.6-prepare-metric-data/2.6.7.png)

After that, upload the two data files we prepared earlier to CloudWatch.

```bash
aws cloudwatch put-metric-data --namespace 'FCJ Management Custom Metrics' --metric-data file://metric-cpu.json
```

```bash
aws cloudwatch put-metric-data --namespace 'FCJ Management Custom Metrics' --metric-data file://metric-instances.json
```

![2.6.8.png](/images/2-preparation/2.6-prepare-metric-data/2.6.8.png)

### Verification

Finally, we will check the results in CloudWatch.

- Search for **CloudWatch**
- Click to enter the CloudWatch Console.

![2.6.9.png](/images/2-preparation/2.6-prepare-metric-data/2.6.9.png)

In the **CloudWatch** Console:

- Select **All metrics**
- Choose **FCJ Management Custom Metrics**

![2.6.10.png](/images/2-preparation/2.6-prepare-metric-data/2.6.10.png)

Next, select **AutoScalingGroupName**.

![2.6.11.png](/images/2-preparation/2.6-prepare-metric-data/2.6.11.png)

Then, select the two parameters as shown in the image, and wait for some time to receive the results.

![2.6.12.png](/images/2-preparation/2.6-prepare-metric-data/2.6.12.png)

{{% notice note %}}
We will have to wait about **30 minutes** or longer for CloudWatch to process the data. Instead of waiting, we can proceed with the next sections.
{{% /notice %}}
