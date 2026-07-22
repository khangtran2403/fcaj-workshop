---
title : "Tạo hàm Lambda Presigned URL"
date : 2026-07-14
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

Trong phần này, bạn sẽ tạo 1 hàm trong Lambda sinh ra một URL upload tạm thời có hiệu lực trong 5 phút trả về cho Frontend.

1. Vào [Lambda console](https://ap-southeast-1.console.aws.amazon.com/lambda/home?region=ap-southeast-1#)
![endpoint](images/lambda.png)
+ Trong lambda console ,chọn **Create Function**
![endpoint](images/lambda_console.png)
2. Trong Create function console:
+ Đặt tên function **GeneratePresignedUrl**
+ **Runtime NodeJs** 

![name](images/create_function.png)

3. Kéo xuống trong phần Additional Setting.Enable **Custom Execution Role** rồi chọn **Create role**,sau đó chọn **Add a resource** 
+ Service : S3
+ Resource type : object
+ Resource ARN : copy ARN từ bucket **invoice_input** trong S3

![service](images/add_resource.png)
4. Gán quyền (IAM Role) cho Lambda này: tìm **s3:PutObject**
![rule](images/putobject.png)
+ Chọn **Create Role**
+ Tạo role thành công 
![success](images/add_completed.png)
5. Chọn **Save**
6. Chọn **Create Function** để tạo hàm
