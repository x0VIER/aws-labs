> # Lab 7: RDS Database Setup

**Author:** V Vier

**Level:** Beginner

**Goal:** To launch a new Amazon RDS database instance (MySQL or PostgreSQL) and connect to it using a database client.

---

## Table of Contents

1.  [Introduction](#introduction)
2.  [Prerequisites](#prerequisites)
3.  [Step 1: Create an RDS Subnet Group](#step-1-create-an-rds-subnet-group)
4.  [Step 2: Launch an RDS Database Instance](#step-2-launch-an-rds-database-instance)
5.  [Step 3: Connect to the RDS Database](#step-3-connect-to-the-rds-database)
6.  [Step 4: Terminate the RDS Instance](#step-4-terminate-the-rds-instance)
7.  [Conclusion](#conclusion)
8.  [References](#references)

---

## Introduction

Amazon Relational Database Service (RDS) is a managed database service that makes it easy to set up, operate, and scale a relational database in the cloud. It provides cost-efficient and resizable capacity while automating time-consuming administration tasks such as hardware provisioning, database setup, patching, and backups. This lab will guide you through launching and connecting to an RDS database. [1]

## Prerequisites

*   An AWS account.
*   An administrative IAM user.
*   A VPC with public and private subnets (from Lab 6).
*   A database client (e.g., MySQL Workbench, DBeaver, or the `mysql` command-line client).

## Step 1: Create an RDS Subnet Group

A DB subnet group is a collection of subnets that you create in a VPC and that you then designate for your DB instances.

### AWS Console Steps

1.  Sign in to the AWS Management Console.
2.  Navigate to the **RDS** service.
3.  In the left navigation pane, click **Subnet groups**.
4.  Click **Create DB subnet group**.
5.  Enter a **Name** (e.g., `my-rds-subnet-group`).
6.  Select your `My-Custom-VPC`.
7.  Add your private subnets to the group.
8.  Click **Create**.

### AWS CLI Steps

```bash
# Replace with your subnet IDs
aws rds create-db-subnet-group --db-subnet-group-name my-rds-subnet-group --db-subnet-group-description "My RDS subnet group" --subnet-ids subnet-xxxxxxxx subnet-yyyyyyyy
```

## Step 2: Launch an RDS Database Instance

Now, you will launch a new database instance.

### AWS Console Steps

1.  In the RDS console, click **Databases** and then **Create database**.
2.  Choose **Standard create** and select **MySQL** or **PostgreSQL**.
3.  For the **Template**, choose **Free tier**.
4.  Set a **DB instance identifier** (e.g., `my-database`).
5.  Set a **Master username** and **Master password**.
6.  For **VPC**, select your `My-Custom-VPC`.
7.  For **DB subnet group**, select the one you just created.
8.  Set **Public access** to **No**.
9.  For **VPC security group**, create a new one that allows traffic on the appropriate port (3306 for MySQL, 5432 for PostgreSQL) from the security group of your EC2 instance (or your IP).
10. Click **Create database**.

### AWS CLI Steps

```bash
# Replace with your VPC security group ID and password
aws rds create-db-instance --db-instance-identifier my-database --db-instance-class db.t2.micro --engine mysql --master-username admin --master-user-password <your-password> --allocated-storage 20 --db-subnet-group-name my-rds-subnet-group --vpc-security-group-ids <your-sg-id>
```

## Step 3: Connect to the RDS Database

Once the database is available, you can connect to it from an EC2 instance in the same VPC or from your local machine if you configured public access and security group rules accordingly.

1.  Select your database in the RDS console.
2.  Note the **Endpoint** from the **Connectivity & security** tab.
3.  From an EC2 instance in your VPC, use the MySQL client to connect:

    ```bash
    mysql -h <your-rds-endpoint> -P 3306 -u admin -p
    ```

4.  Enter the master password when prompted.

## Step 4: Terminate the RDS Instance

To avoid incurring charges, terminate the RDS instance when you are finished.

### AWS Console Steps

1.  Select the database in the RDS console.
2.  Click **Actions** and select **Delete**.
3.  You will be asked if you want to create a final snapshot. For this lab, you can skip it.
4.  Confirm the deletion.

### AWS CLI Steps

```bash
aws rds delete-db-instance --db-instance-identifier my-database --skip-final-snapshot
```

## Conclusion

In this lab, you have learned how to launch a managed relational database using Amazon RDS. You created a subnet group, launched a database instance within your custom VPC, and connected to it. RDS simplifies database management, allowing you to focus on your applications.

---

## References

[1] Amazon RDS User Guide. (2023). *What is Amazon RDS?*. AWS. [https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html)

