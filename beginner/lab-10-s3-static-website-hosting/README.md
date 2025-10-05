> # Lab 10: S3 Static Website Hosting

**Author:** V Vier

**Level:** Beginner

**Goal:** To host a simple static website on Amazon S3.

---

## Table of Contents

1.  [Introduction](#introduction)
2.  [Prerequisites](#prerequisites)
3.  [Step 1: Create an S3 Bucket for Your Website](#step-1-create-an-s3-bucket-for-your-website)
4.  [Step 2: Upload Your Website Content](#step-2-upload-your-website-content)
5.  [Step 3: Configure the Bucket for Website Hosting](#step-3-configure-the-bucket-for-website-hosting)
6.  [Step 4: Set Bucket Permissions](#step-4-set-bucket-permissions)
7.  [Step 5: Test Your Website](#step-5-test-your-website)
8.  [Conclusion](#conclusion)
9.  [References](#references)

---

## Introduction

You can host a static website on Amazon S3. On a static website, individual webpages include static content. They might also contain client-side scripts. This lab will guide you through the process of hosting a simple HTML website on S3. [1]

## Prerequisites

*   An AWS account.
*   An administrative IAM user.
*   A simple HTML file (e.g., `index.html`).

## Step 1: Create an S3 Bucket for Your Website

Your bucket name must be globally unique and should be DNS-compliant (e.g., `my-static-website-12345`).

### AWS Console Steps

1.  Navigate to the **S3** service.
2.  Click **Create bucket**.
3.  Enter a unique bucket name.
4.  **Important:** Uncheck **Block all public access** and acknowledge the warning.
5.  Click **Create bucket**.

### AWS CLI Steps

```bash
aws s3api create-bucket --bucket my-static-website-12345 --region us-east-1
aws s3api put-public-access-block --bucket my-static-website-12345 --public-access-block-configuration "BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=false,RestrictPublicBuckets=false"
```

## Step 2: Upload Your Website Content

Create a simple `index.html` file:

```html
<!DOCTYPE html>
<html>
<head>
  <title>My S3 Website</title>
</head>
<body>
  <h1>Welcome to my website hosted on S3!</h1>
</body>
</html>
```

### AWS Console Steps

1.  Select your bucket and click **Upload**.
2.  Upload your `index.html` file.

### AWS CLI Steps

```bash
aws s3 cp index.html s3://my-static-website-12345/
```

## Step 3: Configure the Bucket for Website Hosting

### AWS Console Steps

1.  Go to the **Properties** tab of your bucket.
2.  Scroll down to **Static website hosting** and click **Edit**.
3.  Select **Enable**.
4.  Enter `index.html` as the **Index document**.
5.  Click **Save changes**.

### AWS CLI Steps

```bash
aws s3 website s3://my-static-website-12345/ --index-document index.html
```

## Step 4: Set Bucket Permissions

You need to add a bucket policy that grants public read access to the objects in your bucket.

### AWS Console Steps

1.  Go to the **Permissions** tab of your bucket.
2.  Under **Bucket policy**, click **Edit**.
3.  Paste the following policy, replacing `my-static-website-12345` with your bucket name:

    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "PublicReadGetObject",
                "Effect": "Allow",
                "Principal": "*",
                "Action": "s3:GetObject",
                "Resource": "arn:aws:s3:::my-static-website-12345/*"
            }
        ]
    }
    ```

4.  Click **Save changes**.

## Step 5: Test Your Website

1.  Go back to the **Properties** tab and find the **Static website hosting** section.
2.  Click on the **Bucket website endpoint** URL.
3.  Your website should now be visible in your browser.

## Conclusion

You have successfully hosted a static website on Amazon S3. This is a cost-effective and highly scalable solution for hosting websites that do not require server-side processing.

---

## References

[1] Amazon S3 User Guide. (2023). *Hosting a static website using Amazon S3*. AWS. [https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html)

