> # Lab 9: Lambda Hello World

**Author:** V Vier

**Level:** Beginner

**Goal:** To create and invoke your first AWS Lambda function, a serverless compute service that runs your code in response to events.

---

## Table of Contents

1.  [Introduction](#introduction)
2.  [Prerequisites](#prerequisites)
3.  [Step 1: Create a Lambda Function](#step-1-create-a-lambda-function)
4.  [Step 2: Test the Lambda Function](#step-2-test-the-lambda-function)
5.  [Step 3: Review the Logs](#step-3-review-the-logs)
6.  [Conclusion](#conclusion)
7.  [References](#references)

---

## Introduction

AWS Lambda is a serverless compute service that lets you run code without provisioning or managing servers. You can run code for virtually any type of application or backend service - all with zero administration. This lab will guide you through creating and testing a simple "Hello, World!" Lambda function. [1]

## Prerequisites

*   An AWS account.
*   An administrative IAM user.

## Step 1: Create a Lambda Function

### AWS Console Steps

1.  Sign in to the AWS Management Console.
2.  Navigate to the **Lambda** service.
3.  Click **Create function**.
4.  Select **Author from scratch**.
5.  Enter a **Function name** (e.g., `HelloWorld`).
6.  For **Runtime**, select **Python 3.9** or another supported version.
7.  Keep the default execution role settings.
8.  Click **Create function**.

### AWS CLI Steps

```bash
# Create a zip file with your function code
echo 'def lambda_handler(event, context): return {"statusCode": 200, "body": "Hello, World!"}' > index.py
zip function.zip index.py

# Create an execution role
aws iam create-role --role-name lambda-ex --assume-role-policy-document '{\"Version\": \"2012-10-17\",\"Statement\":[{\"Effect\":\"Allow\",\"Principal\":{\"Service\":\"lambda.amazonaws.com\"},\"Action\":\"sts:AssumeRole\"}]}'
aws iam attach-role-policy --role-name lambda-ex --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole

# Create the function
aws lambda create-function --function-name HelloWorld --runtime python3.9 --role <your-role-arn> --handler index.lambda_handler --zip-file fileb://function.zip
```

## Step 2: Test the Lambda Function

### AWS Console Steps

1.  In the Lambda function editor, click the **Test** tab.
2.  Create a new test event. You can use the default "hello-world" template.
3.  Give the event a name (e.g., `MyTestEvent`).
4.  Click **Test**.
5.  You should see the execution result, which will include the "Hello, World!" message in the body.

### AWS CLI Steps

```bash
aws lambda invoke --function-name HelloWorld response.json
cat response.json
```

## Step 3: Review the Logs

CloudWatch Logs automatically captures the output from your Lambda function.

1.  Click the **Monitor** tab in your Lambda function.
2.  Click **View logs in CloudWatch**.
3.  You will be taken to the log group for your function, where you can see the execution logs.

## Conclusion

Congratulations! You have successfully created, invoked, and monitored your first AWS Lambda function. This is a fundamental step in building serverless applications on AWS. You can now trigger this function from various event sources, such as API Gateway, S3, or DynamoDB.

---

## References

[1] AWS Lambda Developer Guide. (2023). *What is AWS Lambda?*. AWS. [https://docs.aws.amazon.com/lambda/latest/dg/welcome.html](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)

