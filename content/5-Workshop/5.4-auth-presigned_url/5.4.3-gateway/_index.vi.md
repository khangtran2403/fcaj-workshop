---
title : "Cấu hình API Gateway"
date : 2026-07-14
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

1. Truy cập [API Gateway console](https://ap-southeast-1.console.aws.amazon.com/apigateway/main/apis?region=ap-southeast-1).
![endpoint](images/api_gateway.png)

2. Trong Gateway console,chọn **Create API**
![endpoint](images/gateway_console.png)
3. Trong Create API console,chọn **Build** để tạo một **HTTP API** 
![create](images/create_httpapi.png)
4. Trong API details 
+ Đặt tên API : **WorkshopAPI**
+ Phần **Integrations** chọn **Lambda** 
+ AWS region : **ap-southeast-1**
+ Lambda function : copy ARN của hàm **GeneratePresignedUrl** vừa tạo để liên kết với API Gateway
+ Chọn **Next**
![detail](images/api_details.png)
5. Trong Configure Routes
+ Method : **GET**
+ Resource path : **/upload-url**
+ Integration target : hàm lambda **GeneratePresignedURL**
![configure](images/configure.png)
6. Giữ nguyên ở phần **defined stages** ,phần **Review and Createe** xem lại những gì đã cấu hình rồi chọn **Create**
![create](images/create.png)
7. Mở dashboard,chọn Authorization
![auth](images/authorizer.png)
+ Phần Manage authorizers,chọn **Create**
8. Trong Create authorizer console
+ Authorizer type : chọn **JWT**
+ Đặt tên : **MyAppAuthorizer**
+ Phần Issuer URL chúng ta vào Amazon Cognito,chọn user pool đã tạo ở phần trước
![cognito_console](images/userpool_console.png)
+ Copy Token signing key URL  và dán vào phần Issuer URL
![create_auth](images/create_authorizer.png)
+ Chọn **Create**
9. Mở dashboard,vào **Routes** để gán authorizer cho route **/upload-url** vừa tạo
+ Chọn **Attach Authorization**
![details](images/route_details.png)
+ Trong Attach Authorization console , chọn **MyAppAuthorizer** rồi chọn **attach authorizer**
+ Chọn **Save**
10. Chúng ta sẽ tích hợp route này để trigger Lambda GeneratePresignedUrl
+ Mở lambda console,vào function **GeneratePresignedURL**
+ Chọn Configuration -> Triggers,chọn API gateway
![endpoint](images/attach_route.png)
+ Kéo xuống mục **Security** chọn authorizer là **MyAppauthorizer**
![trigger](images/trigger.png)

Luồng lúc này: User đăng nhập Frontend -> lấy Token -> Gọi API Gateway (kèm token) -> Lambda trả về S3 URL -> User dùng URL đó upload file trực tiếp lên S3.




