---
title: "Dọn dẹp tài nguyên"
date: 2026-07-15
weight: 5
chapter: false
pre: " <b> 5.8 </b> "
---

# Hướng dẫn Dọn dẹp Tài nguyên
Sau khi hoàn tất thử nghiệm dự án Số hóa Hóa đơn tự động, việc dọn dẹp các tài nguyên là bắt buộc để đảm bảo bạn không bị trừ tiền cho các dịch vụ lưu trữ hoặc chạy ngầm.

1. Xóa Lớp Phân phối & Bảo mật (CloudFront, WAF, API Gateway)
+ CloudFront: Vào **Console** -> Chọn **Distribution** của Frontend -> Bấm **Disable** (phải đợi vài phút cho đến khi status thành Disabled) -> Chọn lại và bấm Delete.
+ WAF: Vào WAF & Shield -> **Web ACLs** -> Chọn Web ACL đã tạo -> **Delete**.
+ API Gateway: Chọn API của dự án -> Bấm **Delete**.

2. Xóa Lớp Lưu trữ & CSDL (S3, DynamoDB)
+ Vào S3 Console, chọn lần lượt 3 buckets **(invoice-input, invoice-output, frontend-hosting)**.
+ Bấm **Empty** để xóa hết file bên trong.
+ Sau khi Empty xong, chọn lại bucket và bấm **Delete**.
+ DynamoDB: Vào Tables -> Chọn bảng **Invoices** -> Bấm **Delete**.

3. Xóa Lớp Logic & Điều phối (Step Functions, Lambda, EventBridge, SQS)
+ EventBridge: Vào Rules -> Chọn rule trigger S3 -> Bấm **Delete**.
+ Step Functions: Vào State machines -> Chọn workflow của dự án -> Bấm **Delete**.
+ Lambda: Vào Functions -> Chọn các hàm **GeneratePresignedUrl** và **ProcessInvoiceData** -> Bấm Actions -> **Delete**.
+ SQS: Vào Queues -> Chọn **Invoice-Human-Review-Queue** -> Bấm **Delete**.

4. Xóa Amazon Bedrock
+ Truy cập Amazon Bedrock Console -> Data Automation.
+ Xóa Blueprint mà bạn đã tạo cho hóa đơn để tránh bị tính phí lưu trữ tài liệu mẫu.
5. Xóa Lớp Xác thực (Cognito)
+ Vào Cognito Console -> User Pools.
+ Chọn User Pool mà bạn đã tạo -> Bấm **Delete**. (AWS có thể yêu cầu gõ lại tên User Pool để xác nhận).

6. Dọn dẹp Log và Quyền 
+ Dù không tốn quá nhiều tiền, việc xóa Log và Role giúp tài khoản của bạn sạch sẽ hơn
+ CloudWatch Logs: Vào CloudWatch -> Log groups. Tìm các log group có tên bắt đầu bằng /aws/lambda/GeneratePresignedUrl hoặc /aws/states/ -> Bấm Delete.
+ IAM Roles: Vào IAM -> Roles. Tìm và xóa các role mà bạn đã tạo riêng cho Lambda và Step Functions trong dự án này.

