---
title : "Create an Amazon SQS Queue (Human Review)"
date : 2026-07-15
weight : 1
chapter : false
pre : " <b> 5.5.1 </b> "
---


In this section, you will create an Amazon SQS queue (Human Review Queue) that allows administrators to manually review invoices with a confidence score below 0.9.

Open the [Amazon SQS console](https://ap-southeast-1.console.aws.amazon.com/sqs/v3/home?region=ap-southeast-1#/queues)

1. In the console, choose **Create Queue**.
2. In the **Create Queue** page:
+ Type: **Standard**
+ Queue name: **Invoice-Human-Review-Queue**
+ Keep the remaining configuration settings as their default values.
![create](images/create_queue.png)
![config](images/config.png)

3. Choose **Create queue**.
![success](images/succes.png)
+ After the queue is created successfully, you will see a screen similar to the following image.




