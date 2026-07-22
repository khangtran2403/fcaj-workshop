---
title : "Tạo bảng DynamoDB"
date : 2026-07-14 
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

#### Tạo bảng Invoices

1. Vào DynamoDB Console, tạo bảng để lưu dữ liệu đã chuẩn hóa
![endpoint](images/dynamo.png)
2. Trong console, chọn **Create table**

3. Trong Create table console
+ Đặt tên table: Invoices 
+ Partition key : invoice_id (String)
![Table name](images/create_table.png)


+ Giữ nguyên giá trị của các fields khác (default)
+ Kéo chuột xuống và chọn **Create table**
+ Tạo thành công bảng Invoices

![Success](images/create_completed.png)


#### Tóm tắt

Chúc mừng bạn đã hoàn thành. Trong phần này, bạn đã tạo 3 buckets trong Amazon S3 và 1 bảng dữ liệu trong DynamoDB.Đây là lớp nền tảng của ứng dụng giúp chúng ta có thể lưu trữ dữ liệu hóa đơn để xử lý và sau khi xử lý cũng như tài nguyên tĩnh của website.