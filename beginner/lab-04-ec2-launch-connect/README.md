> # Lab 4: EC2 Launch & Connect

**Author:** V Vier

**Level:** Beginner

**Goal:** To launch a new Amazon EC2 instance, connect to it using SSH, and understand the basic concepts of EC2, such as instance types, AMIs, and key pairs.

---

## Table of Contents

1.  [Introduction](#introduction)
2.  [Prerequisites](#prerequisites)
3.  [Step 1: Create an EC2 Key Pair](#step-1-create-an-ec2-key-pair)
4.  [Step 2: Launch an EC2 Instance](#step-2-launch-an-ec2-instance)
5.  [Step 3: Connect to the EC2 Instance](#step-3-connect-to-the-ec2-instance)
6.  [Step 4: Terminate the EC2 Instance](#step-4-terminate-the-ec2-instance)
7.  [Conclusion](#conclusion)
8.  [References](#references)

---

## Introduction

Amazon Elastic Compute Cloud (EC2) provides scalable computing capacity in the Amazon Web Services (AWS) cloud. Using Amazon EC2 eliminates your need to invest in hardware up front, so you can develop and deploy applications faster. This lab will guide you through launching and connecting to your first EC2 instance. [1]

## Prerequisites

*   An AWS account.
*   An administrative IAM user.
*   An SSH client (e.g., Terminal on macOS/Linux, PuTTY on Windows).

## Step 1: Create an EC2 Key Pair

A key pair, consisting of a public key and a private key, is a set of security credentials that you use to prove your identity when connecting to an EC2 instance.

### AWS Console Steps

1.  Sign in to the AWS Management Console.
2.  Navigate to the **EC2** service.
3.  In the left navigation pane, under **Network & Security**, click **Key Pairs**.
4.  Click **Create key pair**.
5.  Enter a **Name** for your key pair (e.g., `my-ec2-key`).
6.  Choose the **.pem** file format for use with OpenSSH.
7.  Click **Create key pair**. The private key file will be automatically downloaded. **Store this file in a secure location.**

### AWS CLI Steps

```bash
aws ec2 create-key-pair --key-name my-ec2-key --query 'KeyMaterial' --output text > my-ec2-key.pem
chmod 400 my-ec2-key.pem
```

## Step 2: Launch an EC2 Instance

Now you will launch a virtual server, known as an EC2 instance.

### AWS Console Steps

1.  In the EC2 console, click **Instances** and then **Launch instances**.
2.  Give your instance a name (e.g., `My-First-Instance`).
3.  Under **Application and OS Images (Amazon Machine Image)**, select **Amazon Linux 2 AMI**.
4.  For **Instance type**, choose `t2.micro` (which is eligible for the AWS Free Tier).
5.  For **Key pair (login)**, select the key pair you created in the previous step.
6.  In the **Network settings**, create a new security group that allows SSH traffic (port 22) from your IP address.
7.  Keep the default storage settings.
8.  Click **Launch instance**.

### AWS CLI Steps

```bash
# Get the latest Amazon Linux 2 AMI ID
AMI_ID=$(aws ssm get-parameters --names /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2 --query 'Parameters[0].Value' --output text)

# Get your VPC and Subnet IDs
VPC_ID=$(aws ec2 describe-vpcs --query 'Vpcs[?IsDefault].VpcId' --output text)
SUBNET_ID=$(aws ec2 describe-subnets --filters "Name=vpc-id,Values=$VPC_ID" --query 'Subnets[0].SubnetId' --output text)

# Create a security group and allow SSH
SG_ID=$(aws ec2 create-security-group --group-name my-sg --description "My security group" --vpc-id $VPC_ID --output text)
aws ec2 authorize-security-group-ingress --group-id $SG_ID --protocol tcp --port 22 --cidr 0.0.0.0/0

# Launch the instance
aws ec2 run-instances --image-id $AMI_ID --count 1 --instance-type t2.micro --key-name my-ec2-key --security-group-ids $SG_ID --subnet-id $SUBNET_ID
```

## Step 3: Connect to the EC2 Instance

Once the instance is running, you can connect to it using your SSH client and the private key file.

### AWS Console Steps

1.  Select your instance in the EC2 console.
2.  Click **Connect**.
3.  Go to the **SSH client** tab.
4.  Follow the instructions provided, which will look similar to this:

    ```bash
    ssh -i "my-ec2-key.pem" ec2-user@<public-dns-name>
    ```

5.  Open your terminal, navigate to the directory where you saved your `.pem` file, and run the command.

## Step 4: Terminate the EC2 Instance

To avoid incurring charges, it is important to terminate the instance when you are finished with it.

### AWS Console Steps

1.  Select the instance in the EC2 console.
2.  Click **Instance state** and select **Terminate instance**.
3.  Confirm the termination.

### AWS CLI Steps

```bash
# Replace with your instance ID
aws ec2 terminate-instances --instance-ids i-0123456789abcdef0
```

## Conclusion

In this lab, you have learned how to launch and connect to an Amazon EC2 instance. You have also been introduced to key concepts such as AMIs, instance types, key pairs, and security groups. EC2 is a fundamental building block for many AWS services and applications.

---

## References

[1] Amazon EC2 User Guide for Linux Instances. (2023). *What is Amazon EC2?*. AWS. [https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)

