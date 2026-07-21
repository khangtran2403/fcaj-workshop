---
title : "Auth & Presigned URL"
date : 2026-07-14 
weight : 4 
chapter : false
pre : " <b> 5.4. </b> "
---

#### Overview

+ In this section, you will create an Amazon Cognito User Pool for user sign-up and sign-in authentication, configure Amazon API Gateway to receive user requests, and create an AWS Lambda function that generates a temporary upload URL valid for 5 minutes and returns it to the frontend.

+ Why use **Presigned_URL**:
    + Uploading a PDF through API Gateway to Lambda before storing it in Amazon S3 is a serious anti-pattern. API Gateway has a 10 MB payload limit, and AWS Lambda has a 6 MB synchronous payload limit. If the PDF invoice is large, the system can fail immediately.
    + Using the Amazon S3 Presigned URL model, the client calls API Gateway, which invokes Lambda to generate a temporary upload URL. The client then uses that URL to upload the PDF file directly to the Amazon S3 input bucket.
![Auth & Presigned_URL architecture](/images/5-Workshop/5.4-Auth-PresignedURL/architecture.png)




