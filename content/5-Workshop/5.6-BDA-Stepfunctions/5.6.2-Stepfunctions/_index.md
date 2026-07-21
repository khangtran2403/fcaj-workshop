---
title: "Build the Workflow"
date: 2026-07-16
weight: 6
chapter: false
pre: " <b> 5.6.2 </b> "
---

#### Build the State Machine (Step Functions)

1. Go to **Step Functions** and create a new State Machine with the following workflow:

![main](fcaj-workshop/images/5-Workshop/5.6-BDA-Stepfunctions/mainpage.png)

+ **Task 1 (Call BDA):** Invoke the `bedrock-data-automation:InvokeDataAutomationAsync` API, passing the URI of the PDF file uploaded to S3 (from the input event) and the Blueprint ID created in **Phase 3**. Configure the output to be written to the **workshop-invoice-output** bucket.
+ **Task 2 (Wait/Check Status):** Repeatedly check whether BDA has finished processing, since this is an asynchronous operation.
+ **Task 3 (Invoke Lambda):** Invoke the **ProcessInvoiceData** Lambda function created earlier.
+ **Choice State:** Use the Step Functions **Choice** state to determine whether the workflow branches to DynamoDB or SQS, as shown in the workflow diagram.
+ **Note:** The IAM role used by Step Functions must have permission to access Bedrock, S3, and Lambda.

Instead of building the workflow manually, we will import the workflow definition. The code is available through the **GitHub link** in the **References** section.

2. After importing the workflow, you will see the following diagram:

![diagram](/images/5-Workshop/5.6-BDA-Stepfunctions/diagram.png)

3. Open the **Config** section.

+ Enter a name for the workflow.
+ Select **Standard** as the **Type**.

![name](/images/5-Workshop/5.6-BDA-Stepfunctions/config.png)

4. Scroll down to the **Logging** section.

+ Select **ALL**. A CloudWatch Logs group will be created automatically to store the State Machine logs.

![log](/images/5-Workshop/5.6-BDA-Stepfunctions/logging.png)

5. Click **Create**.

![create](/images/5-Workshop/5.6-BDA-Stepfunctions/completed.png)

## Connect Using EventBridge

1. Go to **Amazon EventBridge** → **Rules** → **Create Rule**.

![main](/images/5-Workshop/5.6-BDA-Stepfunctions/main.png)

+ Configure the **Event pattern**:

![event](/images/5-Workshop/5.6-BDA-Stepfunctions/event_pattern.png)

+ **Service:** **S3**
+ **Event Type:** **Object Created**
+ **Specific bucket:** **workshop-invoice-input**

![s3](/images/5-Workshop/5.6-BDA-Stepfunctions/s3.png)

+ **Target:** Select the Step Functions State Machine you created earlier.

![sf](/images/5-Workshop/5.6-BDA-Stepfunctions/stepfunction.png)

2. In the **Configure** section:

+ Set the rule name to **TriggerRule**.
+ Leave the remaining settings unchanged.

![config](/images/5-Workshop/5.6-BDA-Stepfunctions/configure.png)

3. Click **Create**.

![create](/images/5-Workshop/5.6-BDA-Stepfunctions/triggerrule.png)