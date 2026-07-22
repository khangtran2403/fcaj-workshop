---
title : "Create a DynamoDB Table"
date : 2026-07-14 
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

#### Create the Invoices Table

1. Open the Amazon DynamoDB Console and create a table to store the normalized data.
![endpoint](images/dynamo.png)
2. In the console, choose **Create table**.

3. In the **Create table** page
+ Set the table name to **Invoices** 
+ Partition key: **invoice_id** (String)
![Table name](images/create_table.png)


+ Keep the default values for all other fields.
+ Scroll down and choose **Create table**.
+ The **Invoices** table has been created successfully.

![Success](images/create_completed.png)

#### Summary

Congratulations! You have completed this section. In this section, you created three Amazon S3 buckets and one Amazon DynamoDB table. These resources form the application's storage layer, allowing invoice data to be stored before and after processing, as well as the website's static assets.