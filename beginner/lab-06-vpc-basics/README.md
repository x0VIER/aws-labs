> # Lab 6: VPC Basics - Create VPC, Subnets, and Internet Gateway

**Author:** V Vier

**Level:** Beginner

**Goal:** To create a custom Virtual Private Cloud (VPC), add public and private subnets, and configure an Internet Gateway to allow internet access for resources in the public subnet.

---

## Table of Contents

1.  [Introduction](#introduction)
2.  [Prerequisites](#prerequisites)
3.  [Step 1: Create a VPC](#step-1-create-a-vpc)
4.  [Step 2: Create Subnets](#step-2-create-subnets)
5.  [Step 3: Create an Internet Gateway](#step-3-create-an-internet-gateway)
6.  [Step 4: Create a Custom Route Table](#step-4-create-a-custom-route-table)
7.  [Step 5: Launch an EC2 Instance in the Public Subnet](#step-5-launch-an-ec2-instance-in-the-public-subnet)
8.  [Conclusion](#conclusion)
9.  [References](#references)

---

## Introduction

Amazon Virtual Private Cloud (VPC) lets you provision a logically isolated section of the AWS Cloud where you can launch AWS resources in a virtual network that you define. You have complete control over your virtual networking environment, including selection of your own IP address range, creation of subnets, and configuration of route tables and network gateways. [1]

## Prerequisites

*   An AWS account.
*   An administrative IAM user.

## Step 1: Create a VPC

First, you will create a new VPC.

### AWS Console Steps

1.  Sign in to the AWS Management Console.
2.  Navigate to the **VPC** service.
3.  Click **Your VPCs** and then **Create VPC**.
4.  Enter a **Name tag** (e.g., `My-Custom-VPC`).
5.  For the **IPv4 CIDR block**, enter `10.0.0.0/16`.
6.  Click **Create VPC**.

### AWS CLI Steps

```bash
aws ec2 create-vpc --cidr-block 10.0.0.0/16 --tag-specifications 'ResourceType=vpc,Tags=[{Key=Name,Value=My-Custom-VPC}]'
```

## Step 2: Create Subnets

Next, you will create a public and a private subnet within your VPC.

### AWS Console Steps

1.  In the VPC console, click **Subnets** and then **Create subnet**.
2.  Select your `My-Custom-VPC`.
3.  For the **Public Subnet**:
    *   **Name tag:** `Public-Subnet`
    *   **Availability Zone:** Select one (e.g., `us-east-1a`)
    *   **IPv4 CIDR block:** `10.0.1.0/24`
4.  Click **Create subnet**.
5.  Repeat the process for the **Private Subnet**:
    *   **Name tag:** `Private-Subnet`
    *   **Availability Zone:** Select a different one (e.g., `us-east-1b`)
    *   **IPv4 CIDR block:** `10.0.2.0/24`

### AWS CLI Steps

```bash
# Replace with your VPC ID and desired AZs
VPC_ID=$(aws ec2 describe-vpcs --filters "Name=tag:Name,Values=My-Custom-VPC" --query "Vpcs[0].VpcId" --output text)

aws ec2 create-subnet --vpc-id $VPC_ID --cidr-block 10.0.1.0/24 --availability-zone us-east-1a --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=Public-Subnet}]'
aws ec2 create-subnet --vpc-id $VPC_ID --cidr-block 10.0.2.0/24 --availability-zone us-east-1b --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=Private-Subnet}]'
```

## Step 3: Create an Internet Gateway

An Internet Gateway allows communication between your VPC and the internet.

### AWS Console Steps

1.  In the VPC console, click **Internet Gateways** and then **Create internet gateway**.
2.  Enter a **Name tag** (e.g., `My-IGW`).
3.  Click **Create internet gateway**.
4.  Select the newly created gateway, click **Actions**, and choose **Attach to VPC**.
5.  Select your `My-Custom-VPC` and click **Attach internet gateway**.

### AWS CLI Steps

```bash
IGW_ID=$(aws ec2 create-internet-gateway --tag-specifications 'ResourceType=internet-gateway,Tags=[{Key=Name,Value=My-IGW}]' --query 'InternetGateway.InternetGatewayId' --output text)
aws ec2 attach-internet-gateway --vpc-id $VPC_ID --internet-gateway-id $IGW_ID
```

## Step 4: Create a Custom Route Table

You need to create a route table and a route that directs internet-bound traffic to the Internet Gateway.

### AWS Console Steps

1.  In the VPC console, click **Route Tables** and then **Create route table**.
2.  Enter a **Name tag** (e.g., `Public-Route-Table`).
3.  Select your `My-Custom-VPC`.
4.  Click **Create route table**.
5.  Select the new route table, go to the **Routes** tab, and click **Edit routes**.
6.  Click **Add route**, enter `0.0.0.0/0` for the **Destination**, and select your `My-IGW` for the **Target**.
7.  Click **Save changes**.
8.  Go to the **Subnet Associations** tab, click **Edit subnet associations**, and select your `Public-Subnet`.

## Step 5: Launch an EC2 Instance in the Public Subnet

Finally, launch an EC2 instance in the public subnet to test connectivity.

1.  Launch a new `t2.micro` EC2 instance.
2.  On the **Configure Instance Details** page, select your `My-Custom-VPC` and `Public-Subnet`.
3.  Enable **Auto-assign Public IP**.
4.  Configure a security group to allow SSH access.
5.  Launch the instance and try to connect to it via SSH.

## Conclusion

In this lab, you have built a custom VPC from scratch. You created a VPC, public and private subnets, an Internet Gateway, and a custom route table. This foundational knowledge is critical for designing and building secure and scalable applications in the AWS cloud.

---

## References

[1] Amazon VPC User Guide. (2023). *What is Amazon VPC?*. AWS. [https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)

