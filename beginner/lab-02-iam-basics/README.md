> # Lab 2: IAM Basics - Users, Groups, Policies, and Roles

**Author:** V Vier

**Level:** Beginner

**Goal:** To understand and implement fundamental IAM concepts by creating user groups with specific permissions, assigning users to those groups, and exploring the purpose of IAM roles.

---

## Table of Contents

1.  [Introduction](#introduction)
2.  [Prerequisites](#prerequisites)
3.  [Step 1: Create IAM User Groups](#step-1-create-iam-user-groups)
4.  [Step 2: Create IAM Policies](#step-2-create-iam-policies)
5.  [Step 3: Create IAM Users and Assign to Groups](#step-3-create-iam-users-and-assign-to-groups)
6.  [Step 4: Test User Permissions](#step-4-test-user-permissions)
7.  [Step 5: Understand IAM Roles](#step-5-understand-iam-roles)
8.  [Conclusion](#conclusion)
9.  [References](#references)

---

## Introduction

AWS Identity and Access Management (IAM) is a web service that helps you securely control access to AWS resources. You use IAM to control who is authenticated (signed in) and authorized (has permissions) to use resources. This lab covers the core components of IAM: users, groups, policies, and roles. [1]

## Prerequisites

*   An AWS account.
*   An administrative IAM user (created in Lab 1).

## Step 1: Create IAM User Groups

User groups are a way to manage permissions for multiple users at once. Instead of attaching policies to individual users, you can attach them to a group. When a user is added to the group, they inherit the group's permissions.

### AWS Console Steps

1.  Sign in to the AWS Management Console with your `aws-admin` user.
2.  Navigate to the **IAM** service.
3.  In the left navigation pane, click **User groups**.
4.  Click **Create group**.
5.  Enter a **User group name**, such as `S3-Developers`.
6.  In the **Attach permissions policies** section, search for and select `AmazonS3FullAccess`.
7.  Click **Create group**.
8.  Repeat the process to create another group named `EC2-Support` with the `AmazonEC2ReadOnlyAccess` policy attached.

### AWS CLI Steps

```bash
# Create the S3-Developers group and attach the S3 full access policy
aws iam create-group --group-name S3-Developers
aws iam attach-group-policy --group-name S3-Developers --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess

# Create the EC2-Support group and attach the EC2 read-only policy
aws iam create-group --group-name EC2-Support
aws iam attach-group-policy --group-name EC2-Support --policy-arn arn:aws:iam::aws:policy/AmazonEC2ReadOnlyAccess
```

## Step 2: Create IAM Policies

While AWS managed policies are useful, you often need to create your own custom policies to grant specific permissions. In this step, we'll create a policy that allows a user to manage their own access keys.

### AWS Console Steps

1.  In the IAM console, go to **Policies** and click **Create policy**.
2.  Select the **JSON** tab and paste the following policy:

    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    "iam:CreateAccessKey",
                    "iam:DeleteAccessKey",
                    "iam:ListAccessKeys",
                    "iam:UpdateAccessKey"
                ],
                "Resource": "arn:aws:iam::*:user/${aws:username}"
            }
        ]
    }
    ```

3.  Click **Next: Tags**, then **Next: Review**.
4.  Give the policy a name, such as `ManageOwnAccessKeys`, and a description.
5.  Click **Create policy**.

### AWS CLI Steps

```bash
# Create a JSON file with the policy content
echo '{...}' > manage-keys-policy.json

# Create the policy
aws iam create-policy --policy-name ManageOwnAccessKeys --policy-document file://manage-keys-policy.json
```

## Step 3: Create IAM Users and Assign to Groups

Now, create new IAM users and add them to the groups you created.

### AWS Console Steps

1.  Go to **Users** and click **Add users**.
2.  Enter a user name, such as `s3-dev-1`.
3.  Select **Provide user access to the AWS Management Console** and set a password.
4.  Click **Next**.
5.  On the **Set permissions** page, select **Add user to group** and check the `S3-Developers` group.
6.  Click **Next**, review, and click **Create user**.
7.  Repeat the process to create a user named `ec2-support-1` and add them to the `EC2-Support` group.

### AWS CLI Steps

```bash
# Create the s3-dev-1 user and add to the S3-Developers group
aws iam create-user --user-name s3-dev-1
aws iam add-user-to-group --user-name s3-dev-1 --group-name S3-Developers

# Create the ec2-support-1 user and add to the EC2-Support group
aws iam create-user --user-name ec2-support-1
aws iam add-user-to-group --user-name ec2-support-1 --group-name EC2-Support
```

## Step 4: Test User Permissions

Sign out of your admin user and sign in as `s3-dev-1`. Try to access the S3 console. You should have full access. Now, try to access the EC2 console. You should see a message indicating you do not have permission. Repeat this test with the `ec2-support-1` user.

## Step 5: Understand IAM Roles

An IAM role is similar to a user in that it is an AWS identity with permission policies that determine what the identity can and cannot do in AWS. However, instead of being uniquely associated with one person, a role is intended to be assumable by anyone who needs it. Roles are often used to grant permissions to AWS services. For example, you can create a role that allows an EC2 instance to access an S3 bucket.

### AWS Console Steps

1.  In the IAM console, go to **Roles** and click **Create role**.
2.  For the trusted entity type, select **AWS service**.
3.  For the use case, choose **EC2**.
4.  Click **Next**.
5.  Attach the `AmazonS3ReadOnlyAccess` policy.
6.  Click **Next**, give the role a name like `EC2-S3-ReadOnly-Role`, and click **Create role**.

This role can now be attached to an EC2 instance, granting it read-only access to S3 without needing to store AWS credentials on the instance.

## Conclusion

In this lab, you learned how to manage access to AWS resources using IAM users, groups, and policies. You also explored the concept of IAM roles for granting permissions to AWS services. These are fundamental skills for maintaining a secure and well-organized AWS environment.

---

## References

[1] AWS Identity and Access Management User Guide. (2023). *What is IAM?*. AWS. [https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)

