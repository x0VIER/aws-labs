> # Lab 15: Cost Explorer & Budgets

**Author:** V Vier

**Level:** Beginner

**Goal:** To learn how to use AWS Cost Explorer to visualize your costs and to set up AWS Budgets to get alerted when you exceed your defined cost thresholds.

---

## Table of Contents

1.  [Introduction](#introduction)
2.  [Prerequisites](#prerequisites)
3.  [Step 1: Enable Cost Explorer](#step-1-enable-cost-explorer)
4.  [Step 2: Analyze Costs with Cost Explorer](#step-2-analyze-costs-with-cost-explorer)
5.  [Step 3: Create a Budget](#step-3-create-a-budget)
6.  [Conclusion](#conclusion)
7.  [References](#references)

---

## Introduction

AWS Cost Explorer has an easy-to-use interface that lets you visualize, understand, and manage your AWS costs and usage over time. AWS Budgets gives you the ability to set custom budgets that alert you when your costs or usage exceed (or are forecasted to exceed) your budgeted amount. [1, 2]

## Prerequisites

*   An AWS account with some usage (even a small amount is fine).
*   An administrative IAM user.

## Step 1: Enable Cost Explorer

If you have not used it before, you may need to enable Cost Explorer first.

1.  Navigate to **AWS Cost Management**.
2.  Click **Cost Explorer**.
3.  If it's your first time, you will see a welcome page. Click **Enable Cost Explorer**. It can take up to 24 hours for your cost and usage data to become available.

## Step 2: Analyze Costs with Cost Explorer

Once your data is available, you can explore your costs.

1.  In Cost Explorer, you will see a default report showing your costs for the last few months.
2.  Use the filters on the right to group your costs by **Service**, **Region**, or **Usage Type**.
3.  This allows you to identify which services are contributing the most to your bill.

## Step 3: Create a Budget

Now, you will create a budget to monitor your costs.

### AWS Console Steps

1.  In the AWS Cost Management console, click **Budgets**.
2.  Click **Create budget**.
3.  For **Budget type**, choose **Cost budget**.
4.  Give your budget a name (e.g., `Monthly-Website-Budget`).
5.  Set a **Period** (e.g., `Monthly`) and a **Budgeted amount** (e.g., `$5`).
6.  Under **Alerts**, configure an alert to be sent when your actual costs reach a certain percentage of your budget (e.g., 80%).
7.  Enter your email address for the notification.
8.  Click **Create budget**.

### AWS CLI Steps

```bash
# Replace with your account ID and email
aws budgets create-budget --account-id 123456789012 --budget '... (JSON structure for budget) ...'
```

## Conclusion

You have learned how to use AWS Cost Explorer to analyze your spending and AWS Budgets to proactively monitor your costs. These tools are essential for maintaining financial governance over your AWS account and avoiding unexpected charges.

---

## References

[1] AWS Cost Management User Guide. (2023). *AWS Cost Explorer*. AWS. [https://docs.aws.amazon.com/cost-management/latest/userguide/ce-what-is.html](https://docs.aws.amazon.com/cost-management/latest/userguide/ce-what-is.html)

[2] AWS Cost Management User Guide. (2023). *Managing Your Costs with AWS Budgets*. AWS. [https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html)

