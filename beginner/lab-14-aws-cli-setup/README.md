> # Lab 14: AWS CLI Setup

**Author:** V Vier

**Level:** Beginner

**Goal:** To install and configure the AWS Command Line Interface (CLI) to manage AWS services from your local terminal.

---

## Table of Contents

1.  [Introduction](#introduction)
2.  [Prerequisites](#prerequisites)
3.  [Step 1: Install the AWS CLI](#step-1-install-the-aws-cli)
4.  [Step 2: Create an IAM User for the CLI](#step-2-create-an-iam-user-for-the-cli)
5.  [Step 3: Configure the AWS CLI](#step-3-configure-the-aws-cli)
6.  [Step 4: Test the AWS CLI](#step-4-test-the-aws-cli)
7.  [Conclusion](#conclusion)
8.  [References](#references)

---

## Introduction

The AWS Command Line Interface (CLI) is a unified tool to manage your AWS services. With just one tool to download and configure, you can control multiple AWS services from the command line and automate them through scripts. This lab will show you how to set up the AWS CLI on your local machine. [1]

## Prerequisites

*   An AWS account.
*   An administrative IAM user.
*   Python and pip installed on your local machine.

## Step 1: Install the AWS CLI

The AWS CLI is available for Windows, macOS, and Linux.

### macOS/Linux

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

### Windows

Download and run the MSI installer from the [AWS CLI documentation](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-windows.html).

## Step 2: Create an IAM User for the CLI

It is a best practice to create a dedicated IAM user for programmatic access.

1.  In the IAM console, create a new user named `cli-user`.
2.  Select **Access key - Programmatic access**.
3.  Attach the `PowerUserAccess` policy.
4.  **Important:** Download or copy the **Access key ID** and **Secret access key**. You will not be able to see the secret key again.

## Step 3: Configure the AWS CLI

Run the `aws configure` command and enter the credentials you just created.

```bash
aws configure
AWS Access Key ID [None]: YOUR_ACCESS_KEY_ID
AWS Secret Access Key [None]: YOUR_SECRET_ACCESS_KEY
Default region name [None]: us-east-1
Default output format [None]: json
```

## Step 4: Test the AWS CLI

You can test that the CLI is configured correctly by running a simple command.

```bash
aws s3 ls
```

This command should list all the S3 buckets in your account.

## Conclusion

You have successfully installed and configured the AWS CLI. You can now manage your AWS resources directly from your terminal, which is essential for automation and scripting.

---

## References

[1] AWS Command Line Interface User Guide. (2023). *What is the AWS CLI?*. AWS. [https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html)

