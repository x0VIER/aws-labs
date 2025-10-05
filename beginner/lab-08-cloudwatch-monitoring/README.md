> # Lab 8: CloudWatch Monitoring

**Author:** V Vier

**Level:** Beginner

**Goal:** To learn how to use Amazon CloudWatch to monitor AWS resources, create alarms, and build a custom dashboard.

---

## Table of Contents

1.  [Introduction](#introduction)
2.  [Prerequisites](#prerequisites)
3.  [Step 1: Explore CloudWatch Metrics](#step-1-explore-cloudwatch-metrics)
4.  [Step 2: Create a CloudWatch Alarm](#step-2-create-a-cloudwatch-alarm)
5.  [Step 3: Create a CloudWatch Dashboard](#step-3-create-a-cloudwatch-dashboard)
6.  [Conclusion](#conclusion)
7.  [References](#references)

---

## Introduction

Amazon CloudWatch is a monitoring and observability service for AWS cloud resources and the applications you run on AWS. You can use CloudWatch to collect and track metrics, collect and monitor log files, set alarms, and automatically react to changes in your AWS resources. This lab will introduce you to the basics of CloudWatch monitoring. [1]

## Prerequisites

*   An AWS account.
*   An administrative IAM user.
*   An EC2 instance running (from Lab 4).

## Step 1: Explore CloudWatch Metrics

CloudWatch automatically collects metrics from many AWS services.

### AWS Console Steps

1.  Sign in to the AWS Management Console.
2.  Navigate to the **CloudWatch** service.
3.  In the left navigation pane, click **Metrics**.
4.  You will see a list of namespaces for different AWS services. Click on **EC2**.
5.  Select **Per-Instance Metrics**.
6.  Find your EC2 instance and select the `CPUUtilization` metric. This will graph the CPU usage of your instance over time.

## Step 2: Create a CloudWatch Alarm

You can create alarms that watch a single CloudWatch metric and perform an action when the metric breaches a threshold.

### AWS Console Steps

1.  In the CloudWatch console, click **Alarms** and then **Create alarm**.
2.  Click **Select metric**.
3.  Select the `CPUUtilization` metric for your EC2 instance.
4.  Set the **Threshold** to a value like `70` percent for 1 consecutive period of 5 minutes.
5.  Under **Actions**, you can configure an SNS topic to send an email notification (as you did for the billing alarm in Lab 1).
6.  Give your alarm a name (e.g., `High-CPU-Alarm`) and click **Create alarm**.

### AWS CLI Steps

```bash
# Replace with your instance ID and SNS topic ARN
aws cloudwatch put-metric-alarm --alarm-name High-CPU-Alarm --alarm-description "Alarm when CPU exceeds 70%" --metric-name CPUUtilization --namespace AWS/EC2 --statistic Average --period 300 --evaluation-periods 1 --threshold 70 --comparison-operator GreaterThanThreshold --dimensions Name=InstanceId,Value=<your-instance-id> --alarm-actions <your-sns-topic-arn>
```

## Step 3: Create a CloudWatch Dashboard

Dashboards are customizable home pages in the CloudWatch console that you can use to monitor your resources in a single view.

### AWS Console Steps

1.  In the CloudWatch console, click **Dashboards** and then **Create dashboard**.
2.  Give your dashboard a name (e.g., `My-EC2-Dashboard`).
3.  Click **Create dashboard**.
4.  You can add widgets to your dashboard. Choose **Line** graph and click **Next**.
5.  Select the `CPUUtilization` and `NetworkIn` metrics for your EC2 instance.
6.  Click **Create widget**.
7.  You can resize and rearrange the widgets on your dashboard.
8.  Click **Save dashboard**.

## Conclusion

In this lab, you have learned the basics of Amazon CloudWatch. You explored metrics, created an alarm to notify you of high CPU utilization, and built a custom dashboard to visualize key metrics for your EC2 instance. CloudWatch is an essential service for maintaining the health and performance of your AWS environment.

---

## References

[1] Amazon CloudWatch User Guide. (2023). *What is Amazon CloudWatch?*. AWS. [https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)

