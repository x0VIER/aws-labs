> # Lab 3: S3 Fundamentals - Buckets, Objects, and Lifecycle Policies

**Author:** V Vier

**Level:** Beginner

**Goal:** To learn how to use Amazon S3 for object storage, including creating buckets, uploading and managing objects, and configuring lifecycle policies to automate storage tiering.

---

## Table of Contents

1.  [Introduction](#introduction)
2.  [Prerequisites](#prerequisites)
3.  [Step 1: Create an S3 Bucket](#step-1-create-an-s3-bucket)
4.  [Step 2: Upload and Manage Objects](#step-2-upload-and-manage-objects)
5.  [Step 3: Enable Bucket Versioning](#step-3-enable-bucket-versioning)
6.  [Step 4: Configure a Lifecycle Policy](#step-4-configure-a-lifecycle-policy)
7.  [Conclusion](#conclusion)
8.  [References](#references)

---

## Introduction

Amazon Simple Storage Service (S3) is an object storage service that offers industry-leading scalability, data availability, security, and performance. This lab will introduce you to the basic concepts of S3, including buckets, objects, versioning, and lifecycle policies. [1]

## Prerequisites

*   An AWS account.
*   An administrative IAM user.

## Step 1: Create an S3 Bucket

A bucket is a container for objects stored in S3. Every object is contained in a bucket, and bucket names must be globally unique.

### AWS Console Steps

1.  Sign in to the AWS Management Console.
2.  Navigate to the **S3** service.
3.  Click **Create bucket**.
4.  Enter a globally unique **Bucket name** (e.g., `your-name-test-bucket-123`).
5.  Select an **AWS Region**.
6.  Keep the default settings for **Block Public Access**.
7.  Click **Create bucket**.

### AWS CLI Steps

```bash
# Replace with your unique bucket name and desired region
aws s3api create-bucket --bucket your-name-test-bucket-123 --region us-east-1
```

## Step 2: Upload and Manage Objects

Objects are the fundamental entities stored in S3. They consist of data and metadata.

### AWS Console Steps

1.  Select the bucket you just created.
2.  Click **Upload**.
3.  Click **Add files** and select a file from your local machine.
4.  Click **Upload**.
5.  Once uploaded, you can click on the object to view its properties, such as the object URL and metadata.

### AWS CLI Steps

```bash
# Create a sample file
echo "Hello, S3!" > sample.txt

# Upload the file to your bucket
aws s3 cp sample.txt s3://your-name-test-bucket-123/
```

## Step 3: Enable Bucket Versioning

Versioning is a means of keeping multiple variants of an object in the same bucket. You can use versioning to preserve, retrieve, and restore every version of every object stored in your bucket.

### AWS Console Steps

1.  Go to the **Properties** tab of your bucket.
2.  Under **Bucket Versioning**, click **Edit**.
3.  Select **Enable** and click **Save changes**.
4.  Now, upload a new version of the same file. You will see a **Versions** toggle appear in the bucket view.

### AWS CLI Steps

```bash
aws s3api put-bucket-versioning --bucket your-name-test-bucket-123 --versioning-configuration Status=Enabled
```

## Step 4: Configure a Lifecycle Policy

Lifecycle policies allow you to automate the process of moving objects to different storage classes or deleting them after a certain period.

### AWS Console Steps

1.  Go to the **Management** tab of your bucket.
2.  Click **Create lifecycle rule**.
3.  Give the rule a name, such as `Move-to-Glacier-after-90-days`.
4.  Choose to apply the rule to **all objects in the bucket**.
5.  In the **Lifecycle rule actions**, select **Transition current versions of objects between storage classes**.
6.  Set the transition to **Standard-IA** after 30 days and to **Glacier Flexible Retrieval** after 90 days.
7.  Select **Expire current versions of objects** and set it to 365 days.
8.  Click **Create rule**.

### AWS CLI Steps

```bash
# Create a JSON file with the lifecycle configuration
echo '{...}' > lifecycle.json

# Apply the lifecycle policy to the bucket
aws s3api put-bucket-lifecycle-configuration --bucket your-name-test-bucket-123 --lifecycle-configuration file://lifecycle.json
```

## Conclusion

In this lab, you learned the fundamentals of Amazon S3. You created a bucket, uploaded objects, enabled versioning to protect against accidental deletion, and configured a lifecycle policy to optimize storage costs. S3 is a core AWS service, and understanding its features is essential for building scalable and cost-effective applications.

---

## References

[1] Amazon S3 User Guide. (2023). *What is Amazon S3?*. AWS. [https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)

