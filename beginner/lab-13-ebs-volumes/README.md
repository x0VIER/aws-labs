> # Lab 13: EBS Volumes

**Author:** V Vier

**Level:** Beginner

**Goal:** To learn how to create, attach, use, and create snapshots of Amazon Elastic Block Store (EBS) volumes.

---

## Table of Contents

1.  [Introduction](#introduction)
2.  [Prerequisites](#prerequisites)
3.  [Step 1: Create an EBS Volume](#step-1-create-an-ebs-volume)
4.  [Step 2: Attach the EBS Volume to an EC2 Instance](#step-2-attach-the-ebs-volume-to-an-ec2-instance)
5.  [Step 3: Format and Mount the EBS Volume](#step-3-format-and-mount-the-ebs-volume)
6.  [Step 4: Create an EBS Snapshot](#step-4-create-an-ebs-snapshot)
7.  [Conclusion](#conclusion)
8.  [References](#references)

---

## Introduction

Amazon Elastic Block Store (EBS) provides persistent block storage volumes for use with Amazon EC2 instances. Each EBS volume is automatically replicated within its Availability Zone to protect you from component failure, offering high availability and durability. This lab will cover the lifecycle of an EBS volume. [1]

## Prerequisites

*   An AWS account.
*   An administrative IAM user.
*   An EC2 instance running in a public subnet.

## Step 1: Create an EBS Volume

### AWS Console Steps

1.  Navigate to the **EC2** service.
2.  Under **Elastic Block Store**, click **Volumes**.
3.  Click **Create volume**.
4.  Set the **Size** (e.g., 1 GiB).
5.  **Important:** Select the same **Availability Zone** as your EC2 instance.
6.  Click **Create volume**.

### AWS CLI Steps

```bash
# Replace with the AZ of your EC2 instance
aws ec2 create-volume --size 1 --availability-zone us-east-1a
```

## Step 2: Attach the EBS Volume to an EC2 Instance

### AWS Console Steps

1.  Select the newly created volume.
2.  Click **Actions** and select **Attach volume**.
3.  Select your EC2 instance from the list.
4.  Click **Attach volume**.

### AWS CLI Steps

```bash
# Replace with your volume ID and instance ID
aws ec2 attach-volume --volume-id vol-0123456789abcdef0 --instance-id i-0123456789abcdef0 --device /dev/sdf
```

## Step 3: Format and Mount the EBS Volume

1.  SSH into your EC2 instance.
2.  Check if the volume is attached:

    ```bash
    lsblk
    ```

3.  Format the volume with a file system:

    ```bash
    sudo mkfs -t ext4 /dev/xvdf
    ```

4.  Create a mount point and mount the volume:

    ```bash
    sudo mkdir /data
    sudo mount /dev/xvdf /data
    ```

5.  You can now read and write data to the `/data` directory.

## Step 4: Create an EBS Snapshot

Snapshots are a point-in-time backup of your EBS volume, stored in S3.

### AWS Console Steps

1.  In the **Volumes** screen, select your volume.
2.  Click **Actions** and select **Create snapshot**.
3.  Add a description and click **Create snapshot**.

### AWS CLI Steps

```bash
aws ec2 create-snapshot --volume-id vol-0123456789abcdef0 --description "My first snapshot"
```

## Conclusion

You have learned how to manage EBS volumes. You created a volume, attached it to an EC2 instance, formatted and mounted it, and created a snapshot for backup. EBS volumes are essential for persistent storage requirements for your EC2 instances.

---

## References

[1] Amazon EBS User Guide. (2023). *Amazon Elastic Block Store (Amazon EBS)*. AWS. [https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonEBS.html](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonEBS.html)

