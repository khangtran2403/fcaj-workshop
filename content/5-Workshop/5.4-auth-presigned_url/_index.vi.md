---
title : "Lớp Xác thực và API"
date : 2026-07-14 
weight : 4 
chapter : false
pre : " <b> 5.4. </b> "
---

#### Tổng quan

+ Trong phần này, bạn sẽ tạo một User Pool trong Amazon Cognito xác thực đăng nhập,đăng ký cho người dùng,thiết lập API Gateway để tiếp nhận các yêu cầu của người dùng.Tạo 1 hàm trong Lambda sinh ra một URL upload tạm thời có hiệu lực trong 5 phút trả về cho Frontend.

+ Tại sao nên sử dụng **Presigned_URL**:
    + Đẩy trực tiếp file PDF qua API Gateway vào Lambda rồi mới lưu lên S3 là một Anti-pattern cực kỳ nghiêm trọng. API Gateway có giới hạn payload 10MB, Lambda có giới hạn 6MB (synchronous). Nếu hóa đơn PDF nặng, hệ thống sẽ sập ngay lập tức.
    + Mô hình S3 Presigned URL: Khách hàng gọi API Gateway -> Lambda để xin một URL tải lên tạm thời. Sau đó, Client dùng URL đó để upload file PDF trực tiếp vào S3 Input Bucket.
![Auth & Presigned_URL architecture](/images/5-Workshop/5.4-Auth-PresignedURL/architecture.png)




