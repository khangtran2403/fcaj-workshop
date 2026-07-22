---
title : "Cấu hình Amazon Cognito"
date : 2026-07-14
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

#### Cấu hình Amazon Cognito
1. Truy cập [Amazon Cognito Console](https://ap-southeast-1.console.aws.amazon.com/cognito/v2/home?region=ap-southeast-1#)
![endpoint](images/cognito.png)

2. Trong trang chính,chọn **Create User Pool**

![userpool](images/userpool.png)
+ Tạo một User Pool,đặt tên **MyApp**
+ Cấu hình đăng nhập bằng Email/Password.
![Create](images/userpool_name.png)
+ Chọn **Create User Pool**
3. click vào URL Cognito tạo ra cho chúng ta khi tạo thành công để tạo một **App Client**
+ Tạo một test user để lát nữa đăng nhập.
+ Nhập email,mật khẩu
+ Chọn **Sign up**  
![Sign up](images/signup.png)
+ Sau khi đăng ký thành công sẽ có thông báo như màn hình như hình và email thông báo 
![Sign up completed](images/signup_complete.png)




