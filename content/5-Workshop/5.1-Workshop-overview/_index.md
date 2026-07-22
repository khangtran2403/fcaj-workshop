---
title : "Introduction"
date : 2026-07-13 
weight : 1 
chapter : false
pre : " <b> 5.1. </b> "
---

#### Amazon Bedrock Data Automation

Amazon Bedrock Data Automation (BDA) is a fully managed Generative AI service. It enables the automated extraction of structured information from unstructured documents (such as PDFs and images) at scale.
Unlike traditional OCR models, BDA has the ability to understand document context natively based on a predefined schema (blueprint). This allows for highly accurate data extraction without the need to train custom AI models or build complex parsers.

#### Workshop overview

In this workshop, you will build a complete serverless architecture on AWS.

Interaction Layer (Frontend & API): Consists of a React application hosted on S3 and protected by CloudFront, communicating with the backend via Amazon API Gateway.

Event-driven Processing Layer: Simulates the automated invoice processing workflow. When a PDF file is uploaded to the S3 Input Bucket via a Presigned URL, it automatically triggers an AWS Step Functions workflow. This state machine acts as the orchestrator, invoking Amazon Bedrock to extract data, and then using AWS Lambda to validate the confidence score. High-confidence data is stored directly in DynamoDB, while low-confidence extractions are routed to an Amazon SQS queue for human review.
![overview](images/architecture.png)