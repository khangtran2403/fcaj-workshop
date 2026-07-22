---
title: "Resource Cleanup"
date: 2026-07-18
weight: 8
chapter: false
pre: " <b> 5.8 </b> "
---

# Resource Cleanup Guide
After completing the Automated Invoice Digitization project lab, you should delete all provisioned resources to avoid ongoing charges for storage or running services.

1. Delete the Distribution & Security Layer (CloudFront, WAF, API Gateway)
+ CloudFront: Open the **Console** -> Select the Frontend **Distribution** -> Click **Disable** (wait until the status changes to **Disabled**) -> Select it again and click **Delete**.
+ WAF: Open **WAF & Shield** -> **Web ACLs** -> Select the Web ACL you created -> **Delete**.
+ API Gateway: Select the project's API -> Click **Delete**.

2. Delete the Storage & Database Layer (S3, DynamoDB)
+ Open the S3 Console and select the three buckets: **invoice-input**, **invoice-output**, and **frontend-hosting**.
+ Click **Empty** to remove all objects.
+ After the bucket is empty, select it again and click **Delete**.
+ DynamoDB: Open **Tables** -> Select the **Invoices** table -> Click **Delete**.

3. Delete the Logic & Orchestration Layer (Step Functions, Lambda, EventBridge, SQS)
+ EventBridge: Open **Rules** -> Select the S3 trigger rule -> Click **Delete**.
+ Step Functions: Open **State machines** -> Select the project workflow -> Click **Delete**.
+ Lambda: Open **Functions** -> Select **GeneratePresignedUrl** and **ProcessInvoiceData** -> **Actions** -> **Delete**.
+ SQS: Open **Queues** -> Select **Invoice-Human-Review-Queue** -> Click **Delete**.

4. Delete Amazon Bedrock Resources
+ Open the Amazon Bedrock Console -> **Data Automation**.
+ Delete the invoice Blueprint you created to avoid storage charges for sample documents.

5. Delete the Authentication Layer (Cognito)
+ Open the Cognito Console -> **User Pools**.
+ Select the User Pool you created -> Click **Delete**. (AWS may require you to re-enter the User Pool name for confirmation.)

6. Clean Up Logs and IAM Roles
+ Although these resources typically incur minimal cost, removing logs and IAM roles keeps your AWS account organized.
+ CloudWatch Logs: Open **CloudWatch** -> **Log groups**. Find log groups beginning with **/aws/lambda/GeneratePresignedUrl** or **/aws/states/** -> Click **Delete**.
+ IAM Roles: Open **IAM** -> **Roles**. Find and delete the IAM roles created specifically for the Lambda functions and Step Functions used in this project.
