---
title : "Configure API Gateway"
date : 2026-07-14
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

1. Open [API Gateway console](https://ap-southeast-1.console.aws.amazon.com/apigateway/main/apis?region=ap-southeast-1).
![endpoint](images/api_gateway.png)

2. In the API Gateway console, choose **Create API**
![endpoint](images/gateway_console.png)
3. In the **Create API** page, choose **Build** to create an **HTTP API** 
![create](images/create_httpapi.png)
4. In **API details** 
+ Set the API name to: **WorkshopAPI**
+ For **Integrations**, choose **Lambda** 
+ AWS Region: **ap-southeast-1**
+ Lambda function: Copy the ARN of the **GeneratePresignedUrl** function you created earlier to integrate with API Gateway.
+ Choose **Next**
![detail](images/api_details.png)
5. In **Configure Routes**
+ Method: **GET**
+ Resource path: **/upload-url**
+ Integration target: **GeneratePresignedURL** Lambda function
![configure](images/configure.png)
6. Keep the default settings for **defined stages**. In **Review and Create**, review your configuration, then choose **Create**.
![create](images/create.png)
7. Open the dashboard and choose **Authorization**
![auth](images/authorizer.png)
+ Under **Manage authorizers**, choose **Create**
8. In the **Create authorizer** page
+ Authorizer type: **JWT**
+ Name: **MyAppAuthorizer**
+ For **Issuer URL**, open Amazon Cognito and select the user pool created in the previous section.
![cognito_console](images/userpool_console.png)
+ Copy the **Token signing key URL** and paste it into the **Issuer URL** field.
![create_auth](images/create_authorizer.png)
+ Choose **Create**
9. Open the dashboard and go to **Routes** to attach the authorizer to the **/upload-url** route.
+ Choose **Attach Authorization**
![details](images/route_details.png)
+ In the **Attach Authorization** page, select **MyAppAuthorizer**, then choose **Attach authorizer**.
+ Choose **Save**
10. Next, integrate this route to trigger the **GeneratePresignedUrl** Lambda function.
+ Open the Lambda console and select the **GeneratePresignedURL** function.
+ Choose **Configuration → Triggers**, then select **API Gateway**.
![endpoint](images/attach_route.png)
+ Scroll to the **Security** section and select **MyAppAuthorizer** as the authorizer.
![trigger](images/trigger.png)

Current workflow: User signs in to the frontend → obtains a token → calls API Gateway (including the token) → Lambda returns an Amazon S3 presigned URL → the user uploads the file directly to Amazon S3 using that URL.




