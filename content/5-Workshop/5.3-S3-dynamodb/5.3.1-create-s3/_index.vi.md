---
title : "Tạo S3 buckets"
date : 2026-07-13 
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

1. Mở [Amazon S3 console](https://ap-southeast-1.console.aws.amazon.com/s3/home?region=ap-southeast-1#)
2. Trong trang chính, click **Create bucket** để tạo bucket đầu tiên:

![endpoint](/images/5-Workshop/5.3-S3-DynamoDB/create-bucket.png)

3. Trong **Create bucket**:
+ Đặt tên cho bucket: **workshop-invoice-input** (Nhận file PDF gốc)
+ Block tất cả public access
+ Để cấu hình default
+ click **Create bucket** 

![endpoint](/images/5-Workshop/5.3-S3-DynamoDB/invoice_input.png)

4. Tạo bucket thứ hai: 
+ Quay về trang chủ,chọn **Create bucket**
+ Đặt tên cho bucket: **workshop-invoice-output** (Lưu file JSON kết quả từ BDA)
+ Block tất cả public access
+ Để cấu hình default
+ click **Create bucket**
![endpoint](/images/5-Workshop/5.3-S3-DynamoDB/invoice_output.png)
5. Tạo bucket thứ ba: 
+ Quay về trang chủ,chọn **Create bucket**
+ Đặt tên cho bucket: **workshop-frontend-hosting** (Chứa source code giao diện Web)
+ Block tất cả public access
+ Để cấu hình default
+ click **Create bucket**
![endpoint](/images/5-Workshop/5.3-S3-DynamoDB/frontend_hosting.png)


