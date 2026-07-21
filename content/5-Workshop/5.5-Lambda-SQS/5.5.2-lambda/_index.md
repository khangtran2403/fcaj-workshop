---
title : "Create a Lambda Function to Process and Store Data"
date : 2026-07-15
weight : 5
chapter : false
pre : " <b> 5.5.2 </b> "
---

Open the [Lambda console](https://ap-southeast-1.console.aws.amazon.com/lambda/home?region=ap-southeast-1#)

1. In the console, choose **Create function**.
2. In the **Create function** page:
+ Function name: **ProcessInvoiceData**
+ Runtime: **Node.js**
![create](/images/5-Workshop/5.5-SQS-Lambda/create_function.png)
3. Assign IAM permissions to this function: read from the S3 output bucket, write to DynamoDB, and send messages to Amazon SQS.
+ Scroll down to **Additional settings**, enable **Custom Execution Role**, choose **Create role**, then choose **Add a resource**.
+ Service: **S3**
+ Resource type: **Object**
+ Resource ARN: Copy the ARN of the **invoice_output** bucket from Amazon S3.
+ Grant the IAM permission **s3:GetObject** to this Lambda function.
![rule](/images/5-Workshop/5.5-SQS-Lambda/iam_rule.png)
4. Repeat the same process for the DynamoDB and Amazon SQS permissions.
+ For DynamoDB, grant the **PutItem** permission.
+ For Amazon SQS, grant the **SendMessage** permission.
![role](/images/5-Workshop/5.5-SQS-Lambda/create_role.png)

5. Choose **Create Role**.
+ The IAM role is created successfully.
![complete](/images/5-Workshop/5.5-SQS-Lambda/success.png)
6. Choose **Save**.
7. Choose **Create Function** to create the Lambda function.
![success](/images/5-Workshop/5.5-SQS-Lambda/create_complete.png)





