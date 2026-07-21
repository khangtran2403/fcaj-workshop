---
title : "Tạo Blueprint trong Bedrock Data Automation"
date : 2026-07-15
weight : 6
chapter : false
pre : " <b> 5.6.1 </b> "
---

#### Gán quyền IAM để sử dụng Bedrock cho IAM user
Trong lab này, bạn sẽ tạo Blueprint trong Bedrock Data Automation (BDA) để trích xuất các trường quan trọng dùng để đưa vào hàm Lambda.

1. Trước tiên chúng ta vào **IAM console** để cấp quyền bedrock cho User
+ trong Dashboard,chọn mục IAM users
![console](/images/5-Workshop/5.6-BDA-Stepfunctions/user.png)
+ Chọn user với tên giống với tên đăng nhập IAM user.Trong ví dụ này là **khangtran**
2. Trong **Permissions**,chọn **AddPermission**
![permission](/images/5-Workshop/5.6-BDA-Stepfunctions/permissions.png)
+ Chọn **Attach policies directly**
+ Tìm **AmazonBedrockFullAccess**, tick vào ô như hình
![add](/images/5-Workshop/5.6-BDA-Stepfunctions/add_permission.png)
+ Chọn **Next**
3. Phần Review & Create chọn **Add Permissions**
![review](/images/5-Workshop/5.6-BDA-Stepfunctions/user.png)

{{% notice warning %}}
⚠️ **Lưu ý:** Hướng dẫn trên chỉ áp dụng cho tài khoản đăng nhập bằng **IAM user**,đối với tài khoản đăng nhập bằng **Root user** thì đã có đầy đủ quyền,không cần làm theo hướng dẫn
{{% /notice %}}

## Tạo Blueprint trong Bedrock Data Automation
1. Truy cập Bedrock Console, tìm phần **Data Automation**.
+ Kéo xuống chọn **Manage Blueprints**
![bedrock](/images/5-Workshop/5.6-BDA-Stepfunctions/manage.png)

2. Tạo một Blueprint (Kịch bản trích xuất).
+ Chọn **Create blueprint**
![console](/images/5-Workshop/5.6-BDA-Stepfunctions/console.png)
3. Trong **Create blueprint console**
+ Mở mục **Select/Upload files**
+ chọn **Upload from computer** để tải một hóa đơn mẫu
![create](/images/5-Workshop/5.6-BDA-Stepfunctions/upload.png)
+ Mục **Blueprint prompt** bên phải,chọn **Generate blueprint** để Bedrock tự động tạo schema dựa trên file vừa tải lên
4. Sau khi tạo xong các trường được trích xuất sẽ xuất hiện ở bên phải,bạn có thể tùy chỉnh theo nhu cầu của mình 
![complete](/images/5-Workshop/5.6-BDA-Stepfunctions/complete.png)
5. Có thể chọn tải **file JSON** để sử dụng sau này.
6. Tạo Project trong BDA
+ Trong Projects console ,chọn **Create Project**
![create](/images/5-Workshop/5.6-BDA-Stepfunctions/project.png)
7. Mở chế độ **Edit** , chọn **Select from blueprint list**
![list](/images/5-Workshop/5.6-BDA-Stepfunctions/list.png)
+ Chọn **HOA-statement** blueprint
![blueprint](/images/5-Workshop/5.6-BDA-Stepfunctions/complete.png)
+ Sau đó **Save changes**











