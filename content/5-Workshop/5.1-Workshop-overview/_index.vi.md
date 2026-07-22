---
title : "Giới thiệu"
date : 2026-07-13 
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Amazon Bedrock Data Automation

Amazon Bedrock Data Automation (BDA) là một dịch vụ Generative AI được quản lý hoàn toàn. Nó cho phép trích xuất thông tin có cấu trúc một cách tự động từ các tài liệu phi cấu trúc (như PDF, hình ảnh) ở quy mô lớn.
Khác với các mô hình OCR truyền thống, BDA có khả năng tự hiểu ngữ cảnh của tài liệu dựa trên một lược đồ (blueprint) định nghĩa trước, giúp trích xuất chính xác dữ liệu mà không cần phải huấn luyện (train) mô hình AI riêng hay xây dựng các bộ phân tích (parser) phức tạp.

#### Tổng quan Workshop

Trong workshop này, bạn sẽ xây dựng một kiến trúc serverless hoàn chỉnh trên AWS.

Lớp Tương tác (Frontend & API): Bao gồm một ứng dụng React được lưu trữ trên S3 và bảo vệ bởi CloudFront, giao tiếp với backend thông qua Amazon API Gateway.

Lớp Xử lý Sự kiện (Event-driven Core): Mô phỏng quy trình xử lý hóa đơn tự động. Khi một file PDF được tải lên S3 Input Bucket thông qua Presigned URL, nó sẽ tự động kích hoạt một luồng AWS Step Functions. Luồng này đóng vai trò như một nhạc trưởng, điều phối việc gọi Amazon Bedrock để trích xuất dữ liệu, sau đó sử dụng AWS Lambda để kiểm tra độ tin cậy (confidence score). Dữ liệu tốt sẽ được lưu thẳng vào DynamoDB, trong khi dữ liệu có độ tin cậy thấp sẽ được chuyển vào Amazon SQS để con người xem xét lại.

![overview](images/architecture.png)
