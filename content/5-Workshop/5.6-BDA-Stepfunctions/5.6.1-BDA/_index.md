---
title: "Create a Blueprint in Bedrock Data Automation"
date: 2026-07-15
weight: 1
chapter: false
pre: " <b> 5.6.1 </b> "
---

#### Grant IAM permissions to use Bedrock for an IAM user

In this lab, you will create a Blueprint in Bedrock Data Automation (BDA) to extract key fields for use in a Lambda function.

1. First, open the **IAM Console** to grant Bedrock permissions to the user.
+ In the Dashboard, choose **IAM users**.
![console](images/user.png)
+ Select the user with the same name as your IAM login. In this example, the user is **khangtran**.
2. Under **Permissions**, choose **Add permission**.
![permission](images/permissions.png)
+ Select **Attach policies directly**.
+ Search for **AmazonBedrockFullAccess** and check the box.
 ![add](images/add_permission.png)
+ Choose **Next**.
3. In **Review and create**, choose **Add permissions**.
![review](images/user.png)

> **Warning**
>
> This guide applies only to accounts that sign in with an **IAM user**. If you sign in with the **Root user**, the required permissions are already available, so you can skip these steps.

## Create a Blueprint in Bedrock Data Automation

1. Open the **Bedrock Console** and navigate to **Data Automation**.
+ Scroll down and choose **Manage Blueprints**.
![bedrock](images/manage.png)
2. Create a new extraction blueprint.
+ Choose **Create blueprint**.
![console](images/console.png)
3. In the **Create blueprint** page:
+ Expand **Select/Upload files**.
+ Choose **Upload from computer** to upload a sample invoice.
![create](images/upload.png)
+ In the **Blueprint prompt** panel on the right, choose **Generate blueprint**. Bedrock will automatically generate a schema based on the uploaded file.
4. After the blueprint is generated, the extracted fields will appear in the right panel. You can customize them to fit your requirements.
![complete](images/complete.png)
5. You can also download the **JSON file** for future use.
6. Create Project in BDA
+ In Projects console ,choose **Create Project**
![create](images/project.png)
7. Open **Edit** mode , choose **Select from blueprint list**
![list](images/blueprint.png)
+ Choose **HOA-statement** blueprint
+ Then **Save changes**