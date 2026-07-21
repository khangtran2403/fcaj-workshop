---
title : "Create the Lambda Function for Presigned URLs"
date : 2026-07-14
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

In this section, you will create an AWS Lambda function that generates a temporary upload URL valid for 5 minutes and returns it to the frontend.

1. Open the [Lambda console](https://ap-southeast-1.console.aws.amazon.com/lambda/home?region=ap-southeast-1#)
![endpoint](/images/5-Workshop/5.4-Auth-PresignedURL/lambda.png)
+ In the Lambda console, choose **Create Function**
![endpoint](/images/5-Workshop/5.4-Auth-PresignedURL/lambda_console.png)
2. In the **Create function** page:
+ Set the function name to **GeneratePresignedUrl**
+ **Runtime: Node.js** 

![name](/images/5-Workshop/5.4-Auth-PresignedURL/create_function.png)

3. Scroll down to **Additional settings**. Enable **Custom Execution Role**, choose **Create role**, then choose **Add a resource**. 
+ Service: **S3**
+ Resource type: **Object**
+ Resource ARN: Copy the ARN of the **invoice_input** bucket from Amazon S3.

![service](/images/5-Workshop/5.4-Auth-PresignedURL/add_resource.png)
4. Assign permissions (IAM role) to this Lambda function by locating **s3:PutObject**.
![rule](/images/5-Workshop/5.4-Auth-PresignedURL/putobject.png)
+ Choose **Create Role**
+ The role is created successfully. 
![success](/images/5-Workshop/5.4-Auth-PresignedURL/add_completed.png)
5. Choose **Save**.
6. Choose **Create Function** to create the Lambda function.
