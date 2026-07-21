---
title : "Configure CloudFront and Set Up AWS WAF"
date : 2026-07-15
weight : 5
chapter : false
pre : " <b> 5.7.1 </b> "
---

1. Open the [CloudFront console](https://us-east-1.console.aws.amazon.com/cloudfront/v4/home?region=ap-southeast-1)

2. In the console, choose **Create distribution**.
![console](/images/5-Workshop/5.7-Frontend/console.png)
+ Under **Choose a plan**, select **Free**.
![free](/images/5-Workshop/5.7-Frontend/free_plan.png)
+ Under **Get Started**, set the Distribution name to **MyWorkshop**.
+ Distribution type: **Single website or app**.
![get](/images/5-Workshop/5.7-Frontend/get_started.png)
+ Under **Specify Origin**, choose the Amazon S3 frontend bucket as the origin.
+ For **S3 origin**, choose **Browse S3** and select the **Workshop-frontend-hosting** bucket.
![origin](/images/5-Workshop/5.7-Frontend/origin.png)
+ Under **Enable security**, AWS WAF is enabled by default, so choose **Next** without changing any settings.
+ Finally, under **Review & Create**, review the configuration and choose **Create distribution**.
![create](/images/5-Workshop/5.7-Frontend/create.png)
![success](/images/5-Workshop/5.7-Frontend/completed.png)
3. Open the [AWS WAF console](https://ap-southeast-1.console.aws.amazon.com/wafv2-pro/home?region=ap-southeast-1)
+ CloudFront automatically creates a Web ACL. This Web ACL protects the CloudFront distribution using AWS Managed Rules, including the Core Rule Set, SQL injection (SQLi) protection, and IP Reputation rules.
![webacl](/images/5-Workshop/5.7-Frontend/waf.png)




