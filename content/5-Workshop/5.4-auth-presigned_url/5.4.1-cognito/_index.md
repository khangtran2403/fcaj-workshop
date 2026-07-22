---
title : "Configure Amazon Cognito"
date : 2026-07-14
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

#### Configure Amazon Cognito
1. Open the [Amazon Cognito Console](https://ap-southeast-1.console.aws.amazon.com/cognito/v2/home?region=ap-southeast-1#)
![endpoint](images/cognito.png)

2. On the main page, choose **Create User Pool**

![userpool](images/userpool.png)
+ Create a User Pool and name it **MyApp**.
+ Configure sign-in using **Email/Password**.
![Create](images/userpool_name.png)
+ Choose **Create User Pool**
3. Click the Cognito URL generated after the User Pool is created successfully to create an **App Client**.
+ Create a test user for signing in later.
+ Enter an email address and password.
+ Choose **Sign up**  
![Sign up](images/signup.png)
+ After successful registration, a confirmation message will be displayed and a confirmation email will be sent. 
![Sign up completed](images/signup_complete.png)




