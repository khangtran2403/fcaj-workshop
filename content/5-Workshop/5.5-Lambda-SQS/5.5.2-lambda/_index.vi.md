---
title : "Tạo Lambda xử lý và lưu dữ liệu"
date : 2026-07-15
weight : 5
chapter : false
pre : " <b> 5.5.2 </b> "
---

Truy cập [Lambda console](https://ap-southeast-1.console.aws.amazon.com/lambda/home?region=ap-southeast-1#)

1. Trong console,chọn **Create function**
2. Trong Create function console
+ Dặt tên : **ProcessInvoiceData**
+ Runtime : **Node.js**
![create](/images/5-Workshop/5.5-SQS-Lambda/create_function.png)
3. Gán quyền IAM cho function này: Đọc từ S3 output, Ghi vào DynamoDB, và Gửi message vào SQS
+ Kéo xuống trong phần Additional Setting.Enable **Custom Execution Role** rồi chọn **Create role**,sau đó chọn **Add a resource** 
+ Service : S3
+ Resource type : object
+ Resource ARN : copy ARN từ bucket **invoice_output** trong S3
+ Gán quyền (IAM Role) cho Lambda này: tìm **s3:GetObject**
![rule](/images/5-Workshop/5.5-SQS-Lambda/iam_rule.png)
4. Làm lại tương tự với role cho DynamoDB và SQS
+ Đối với DynamoDB : tìm service **PutItem**
+ Đối với SQS : tìm service **sendMessage**
![role](/images/5-Workshop/5.5-SQS-Lambda/create_role.png)

5. Chọn **Create Role**
+ Tạo role thành công 
![complete](/images/5-Workshop/5.5-SQS-Lambda/success.png)
5. Chọn **Save**
6. Chọn **Create Function** để tạo hàm
![success](/images/5-Workshop/5.5-SQS-Lambda/create_complete.png)





