> # Lab 12: CloudFormation Basics

**Author:** V Vier

**Level:** Beginner

**Goal:** To learn the basics of AWS CloudFormation by creating a simple stack from a YAML template to provision an S3 bucket.

---

## Table of Contents

1.  [Introduction](#introduction)
2.  [Prerequisites](#prerequisites)
3.  [Step 1: Write a CloudFormation Template](#step-1-write-a-cloudformation-template)
4.  [Step 2: Create a CloudFormation Stack](#step-2-create-a-cloudformation-stack)
5.  [Step 3: Delete the CloudFormation Stack](#step-3-delete-the-cloudformation-stack)
6.  [Conclusion](#conclusion)
7.  [References](#references)

---

## Introduction

AWS CloudFormation is a service that helps you model and set up your Amazon Web Services resources so that you can spend less time managing those resources and more time focusing on your applications that run in AWS. You create a template that describes all the AWS resources that you want (like Amazon EC2 instances or Amazon RDS DB instances), and CloudFormation takes care of provisioning and configuring those resources for you. [1]

## Prerequisites

*   An AWS account.
*   An administrative IAM user.

## Step 1: Write a CloudFormation Template

A CloudFormation template is a JSON or YAML file that defines your AWS infrastructure.

Create a file named `s3-bucket-template.yaml` with the following content:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: A simple CloudFormation template to create an S3 bucket.
Resources:
  MyS3Bucket:
    Type: 'AWS::S3::Bucket'
    Properties:
      BucketName: !Sub "cf-test-bucket-${AWS::AccountId}"
```

This template defines a single resource: an S3 bucket with a unique name.

## Step 2: Create a CloudFormation Stack

A stack is a collection of AWS resources that you can manage as a single unit.

### AWS Console Steps

1.  Navigate to the **CloudFormation** service.
2.  Click **Create stack** > **With new resources (standard)**.
3.  Select **Upload a template file** and choose your `s3-bucket-template.yaml` file.
4.  Click **Next**.
5.  Enter a **Stack name** (e.g., `my-s3-stack`).
6.  Click **Next** through the options pages, and then click **Create stack**.
7.  Wait for the stack status to change to `CREATE_COMPLETE`.

### AWS CLI Steps

```bash
aws cloudformation create-stack --stack-name my-s3-stack --template-body file://s3-bucket-template.yaml
```

## Step 3: Delete the CloudFormation Stack

When you delete a stack, you delete all the resources in that stack.

### AWS Console Steps

1.  Select your stack in the CloudFormation console.
2.  Click **Delete**.
3.  Confirm the deletion.

### AWS CLI Steps

```bash
aws cloudformation delete-stack --stack-name my-s3-stack
```

## Conclusion

You have learned the fundamentals of AWS CloudFormation. You created a template to define an S3 bucket and used it to create and delete a stack. CloudFormation is a powerful tool for automating your infrastructure deployment and management (Infrastructure as Code).

---

## References

[1] AWS CloudFormation User Guide. (2023). *What is AWS CloudFormation?*. AWS. [https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)

