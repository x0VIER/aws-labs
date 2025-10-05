> # Lab 11: Route 53 DNS Setup

**Author:** V Vier

**Level:** Beginner

**Goal:** To learn how to use Amazon Route 53 to register a domain name and create a hosted zone to route traffic to your AWS resources.

---

## Table of Contents

1.  [Introduction](#introduction)
2.  [Prerequisites](#prerequisites)
3.  [Step 1: Register a Domain Name (Optional)](#step-1-register-a-domain-name-optional)
4.  [Step 2: Create a Hosted Zone](#step-2-create-a-hosted-zone)
5.  [Step 3: Create DNS Records](#step-3-create-dns-records)
6.  [Conclusion](#conclusion)
7.  [References](#references)

---

## Introduction

Amazon Route 53 is a highly available and scalable cloud Domain Name System (DNS) web service. It is designed to give developers and businesses an extremely reliable and cost-effective way to route end users to Internet applications by translating names like `www.example.com` into the numeric IP addresses like `192.0.2.1` that computers use to connect to each other. [1]

## Prerequisites

*   An AWS account.
*   An administrative IAM user.
*   An S3 static website bucket (from Lab 10) or another resource with a public IP/DNS name.

## Step 1: Register a Domain Name (Optional)

If you don't already have a domain name, you can register one with Route 53.

### AWS Console Steps

1.  Navigate to the **Route 53** service.
2.  In the left navigation pane, click **Registered domains**.
3.  Click **Register domain**, and follow the steps to find and purchase a domain.

## Step 2: Create a Hosted Zone

A hosted zone is a container for records, and records contain information about how you want to route traffic for a specific domain.

### AWS Console Steps

1.  In the Route 53 console, click **Hosted zones**.
2.  Click **Create hosted zone**.
3.  Enter your domain name.
4.  Select **Public hosted zone**.
5.  Click **Create hosted zone**.

### AWS CLI Steps

```bash
aws route53 create-hosted-zone --name example.com --caller-reference 2023-10-27-18:00
```

## Step 3: Create DNS Records

Now, you will create an `A` record to point your domain to the S3 website endpoint.

### AWS Console Steps

1.  Select your hosted zone.
2.  Click **Create record**.
3.  For **Record name**, you can leave it blank to configure the root domain.
4.  For **Record type**, select **A - Routes traffic to an IPv4 address and some AWS resources**.
5.  Enable the **Alias** toggle.
6.  For **Route traffic to**, select **Alias to S3 website endpoint**.
7.  Choose the region of your S3 bucket and then select your bucket from the list.
8.  Click **Create records**.

## Conclusion

You have learned how to use Amazon Route 53 to manage the DNS for your domain. You created a hosted zone and an `A` record to route traffic to your S3 static website. Route 53 is a critical component for any public-facing application on AWS.

---

## References

[1] Amazon Route 53 Developer Guide. (2023). *What is Amazon Route 53?*. AWS. [https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/Welcome.html](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/Welcome.html)

