> # Lab 5: Security Groups & Network ACLs

**Author:** V Vier

**Level:** Beginner

**Goal:** To understand the difference between Security Groups and Network ACLs and how to use them to secure your AWS resources at the instance and subnet level.

---

## Table of Contents

1.  [Introduction](#introduction)
2.  [Prerequisites](#prerequisites)
3.  [Step 1: Understanding Security Groups](#step-1-understanding-security-groups)
4.  [Step 2: Understanding Network ACLs](#step-2-understanding-network-acls)
5.  [Step 3: Practical Scenario](#step-3-practical-scenario)
6.  [Conclusion](#conclusion)
7.  [References](#references)

---

## Introduction

Security is a top priority in the AWS cloud. Two fundamental services that help you secure your resources are Security Groups and Network Access Control Lists (ACLs). Security Groups act as a virtual firewall for your instances to control inbound and outbound traffic. Network ACLs act as a firewall for associated subnets, controlling both inbound and outbound traffic at the subnet level. [1]

## Prerequisites

*   An AWS account.
*   An administrative IAM user.
*   An existing VPC with at least one subnet and an EC2 instance (from previous labs).

## Step 1: Understanding Security Groups

*   **Stateful:** If you allow inbound traffic, the outbound return traffic is automatically allowed, regardless of outbound rules.
*   **Instance Level:** They are associated with EC2 instances.
*   **Allow Rules Only:** You can only specify `allow` rules, not `deny` rules.
*   **Default:** The default security group allows all outbound traffic and all traffic from other instances in the same security group.

### AWS Console Example

1.  Navigate to the **EC2** service.
2.  Under **Network & Security**, click **Security Groups**.
3.  Select the security group associated with your EC2 instance.
4.  Review the **Inbound rules** and **Outbound rules** tabs.

### AWS CLI Example

```bash
# Describe a security group
aws ec2 describe-security-groups --group-ids <your-security-group-id>
```

## Step 2: Understanding Network ACLs

*   **Stateless:** Return traffic must be explicitly allowed by outbound rules.
*   **Subnet Level:** They are associated with a VPC subnet.
*   **Allow and Deny Rules:** You can specify both `allow` and `deny` rules.
*   **Default:** The default network ACL allows all inbound and outbound traffic.

### AWS Console Example

1.  Navigate to the **VPC** service.
2.  Under **Security**, click **Network ACLs**.
3.  Select the network ACL associated with your subnet.
4.  Review the **Inbound Rules** and **Outbound Rules**.

### AWS CLI Example

```bash
# Describe a network ACL
aws ec2 describe-network-acls --network-acl-ids <your-network-acl-id>
```

## Step 3: Practical Scenario

Let's create a scenario to illustrate the difference. We will block a specific IP address from accessing our EC2 instance using a Network ACL.

1.  **Identify your IP address:** Use a service like [whatismyip.com](https://whatismyip.com).
2.  **Add a Deny Rule to the Network ACL:**
    *   Navigate to the **Network ACLs** page in the VPC console.
    *   Select the ACL for your subnet.
    *   Under **Inbound Rules**, click **Edit inbound rules**.
    *   Add a new rule with a low rule number (e.g., 10) to deny SSH (port 22) traffic from your IP address.
3.  **Attempt to connect to your EC2 instance:** Your SSH connection should fail.
4.  **Remove the Deny Rule:** Remove the rule you just created. Your SSH connection should now succeed.

This demonstrates that Network ACLs can be used to explicitly deny traffic, providing a layer of security at the subnet level before traffic even reaches the instance's security group.

## Conclusion

In this lab, you learned about the key differences between Security Groups and Network ACLs and how they work together to provide a layered security model for your VPC. Security Groups are stateful and operate at the instance level, while Network ACLs are stateless and operate at the subnet level. Understanding both is crucial for designing secure and robust AWS architectures.

---

## References

[1] Amazon VPC User Guide. (2023). *Security Groups and Network ACLs*. AWS. [https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Security.html](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Security.html)

