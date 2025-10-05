> # Lab 1: Getting Started with the AWS Console

**Author:** V Vier

**Level:** Beginner

**Goal:** To create a new AWS account, set up a billing alarm to monitor costs, and create an IAM user with administrative privileges to follow security best practices.

---

## Table of Contents

1.  [Introduction](#introduction)
2.  [Prerequisites](#prerequisites)
3.  [Step 1: Create a New AWS Account](#step-1-create-a-new-aws-account)
4.  [Step 2: Set Up a Billing Alarm](#step-2-set-up-a-billing-alarm)
5.  [Step 3: Create an Administrative IAM User](#step-3-create-an-administrative-iam-user)
6.  [Step 4: Sign In as the IAM User](#step-4-sign-in-as-the-iam-user)
7.  [Conclusion](#conclusion)
8.  [References](#references)

---

## Introduction

Welcome to your first AWS lab! The AWS Management Console is a web-based interface for accessing and managing AWS services. This lab will guide you through the initial steps of setting up your AWS environment securely and cost-effectively. By the end of this lab, you will have a new AWS account, a billing alarm to prevent unexpected charges, and a separate administrative user for day-to-day tasks, which is a foundational security practice. [1]

## Prerequisites

*   A unique email address that has not been previously used with AWS.
*   A valid credit card for billing purposes.
*   A phone number for verification.

## Step 1: Create a New AWS Account

First, you need to create an AWS account. This account will be the root of your AWS environment.

1.  Navigate to the [AWS Free Tier](https://aws.amazon.com/free/) page and click **Create a Free Account**.
2.  Enter your email address, a strong password, and an AWS account name. Click **Continue**.
3.  Follow the on-screen instructions to provide your contact information and credit card details. AWS uses this for identity verification and for any usage that exceeds the Free Tier limits.
4.  Complete the phone verification step.
5.  Choose the **Basic Support - Free** plan.

Once completed, you will have access to the AWS Management Console. The user you are currently logged in as is the **root user**, which has unrestricted access to all resources in the account. For security reasons, you should avoid using the root user for everyday tasks. [2]

## Step 2: Set Up a Billing Alarm

To avoid unexpected costs, it is crucial to set up a billing alarm. This alarm will notify you via email when your estimated charges exceed a certain threshold.

### AWS Console Steps

1.  Sign in to the AWS Management Console as the root user.
2.  In the search bar, type `CloudWatch` and select it from the results.
3.  In the CloudWatch console, navigate to **Alarms** > **Billing**.
4.  Click **Create alarm**.
5.  Under **Metric**, ensure that the metric is `EstimatedCharges` and the currency is `USD`.
6.  Set the **Threshold** to a value you are comfortable with (e.g., `$10`).
7.  Under **Actions**, configure a new SNS topic to send an email notification. Enter your email address and click **Create topic**.
8.  Confirm the SNS topic subscription by clicking the link in the confirmation email you receive.
9.  Click **Next**, give your alarm a name (e.g., `Billing-Alarm-10USD`), and click **Create alarm**.

### AWS CLI Steps

```bash
# Replace with your AWS Account ID and email address
aws cloudwatch put-metric-alarm --alarm-name Billing-Alarm-10USD --alarm-description "Alarm when estimated charges exceed $10" --metric-name EstimatedCharges --namespace AWS/Billing --statistic Maximum --period 86400 --evaluation-periods 1 --threshold 10 --comparison-operator GreaterThanThreshold --dimensions Name=Currency,Value=USD --alarm-actions arn:aws:sns:us-east-1:ACCOUNT_ID:Billing-Alerts --unit None

aws sns subscribe --topic-arn arn:aws:sns:us-east-1:ACCOUNT_ID:Billing-Alerts --protocol email --notification-endpoint your-email@example.com
```

## Step 3: Create an Administrative IAM User

Next, you will create an IAM (Identity and Access Management) user with administrative permissions. This user will be used for all subsequent labs.

### AWS Console Steps

1.  In the search bar, type `IAM` and select it.
2.  In the IAM console, go to **Users** and click **Add users**.
3.  Enter a **User name** (e.g., `aws-admin`).
4.  Select **Provide user access to the AWS Management Console**.
5.  Choose **I want to create an IAM user**.
6.  Set a custom password and uncheck **User must create a new password at next sign-in**.
7.  Click **Next**.
8.  On the **Set permissions** page, select **Attach policies directly**.
9.  Search for and select the `AdministratorAccess` policy.
10. Click **Next**, review the details, and click **Create user**.
11. **Important:** Download the `.csv` file containing the user credentials and the console sign-in link. Store it in a secure location.

### AWS CLI Steps

```bash
# Create the IAM user
aws iam create-user --user-name aws-admin

# Create a login profile for the user
aws iam create-login-profile --user-name aws-admin --password "YourSecurePassword" --no-password-reset-required

# Attach the AdministratorAccess policy
aws iam attach-user-policy --user-name aws-admin --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
```

## Step 4: Sign In as the IAM User

Now, sign out of the root user account and sign back in using the newly created IAM user.

1.  In the top-right corner of the console, click on your account name and select **Sign Out**.
2.  Use the unique console sign-in link from the `.csv` file you downloaded earlier.
3.  Enter the IAM user name (`aws-admin`) and the password you set.

You are now logged in as an administrative user. You can verify this by checking the user name in the top-right corner.

## Conclusion

Congratulations! You have successfully set up your AWS account, created a billing alarm for cost management, and configured an administrative IAM user for secure access. These are the essential first steps for working with AWS. You are now ready to proceed with more advanced labs.

---

## References

[1] AWS Identity and Access Management User Guide. (2023). *IAM best practices*. AWS. [https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)

[2] AWS Billing and Cost Management User Guide. (2023). *Creating a billing alarm to monitor your estimated AWS charges*. AWS. [https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/monitor_estimated_charges_with_cloudwatch.html](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/monitor_estimated_charges_with_cloudwatch.html)

