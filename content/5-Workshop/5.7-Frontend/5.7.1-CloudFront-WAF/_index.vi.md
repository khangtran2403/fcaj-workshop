---
title : "Thiết lập CloudFront và cài đặt WAF"
date : 2026-07-17
weight : 1
chapter : false
pre : " <b> 5.7.1 </b> "
---

1. Truy cập [CloudFront console](https://us-east-1.console.aws.amazon.com/cloudfront/v4/home?region=ap-southeast-1)

2. Trong console,chọn **Create distribution**
![console](images/console.png)
+ Mục Choose a plan chọn **Free**
![free](images/free_plan.png)
+ Mục Get Started, đặt tên cho Distribution : **MyWorkshop**
+ Distribution type : **Single website or app**
![get](images/get_started.png)
+ Mục Specify Origin,chọn Origin trỏ về S3 Frontend bucket.
+ S3 origin ,chọn **Browse S3** và tìm bucket **Workshop-frontend-hosting**
![origin](images/origin.png)
+ Mục Enable security, chúng ta có WAf là mặc định nên chọn **Next**,không cấu hình gì thêm
+ Cuối cùng ở Review & Create chúng ta kiểm tra lại những gì đã cấu hình và chọn **Create distribution**
![create](images/create.png)
![success](images/completed.png)
3. Truy cập vào [WAF console](https://ap-southeast-1.console.aws.amazon.com/wafv2-pro/home?region=ap-southeast-1)
+ Chúng ta Cloudfront đã tạo cho chúng web ACL,web ACL này dùng để bảo vệ CLoudfront với các rule bảo vệ cơ bản: AWS Managed Rules (Core rule set, SQLi, IP Reputation).
![webacl](images/waf.png)




