---
title : "Serverless Data Processing Pipeline"
date : 2025-06-06 
weight : 1 
chapter : false
---
# Building a Serverless Data Processing Pipeline with AWS Step Functions and EventBridge

## 📌 Overview

In today’s technological landscape, the demand for efficient, flexible, and cost-effective data processing is becoming increasingly critical. Organizations require scalable and easily manageable solutions that eliminate the burden of maintaining complex infrastructure.

**Serverless technology** on the cloud has emerged as an optimal approach, allowing the development of powerful data processing systems without the need to manage physical servers. This reduces operational costs, accelerates deployment, and enables high automation.

### 🧩 Pipeline Overview

This serverless pipeline processes data from CSV files uploaded to **Amazon S3**. **Amazon EventBridge** listens to events from S3, filters for `.csv` files, and triggers **AWS Step Functions** to orchestrate the processing workflow. **AWS Lambda** handles tasks such as validation, parallel processing, aggregation, and result storage. **Amazon DynamoDB** stores metadata, **Amazon CloudWatch** monitors performance and errors, and **Amazon SNS** sends email notifications about the pipeline status.  
The entire solution is deployed in the **Singapore region (ap-southeast-1)**.

---

##  Key Components

1. **Amazon S3**  
   - `Input Bucket`: Stores the input CSV files  
     `data-processing-input-<your-account-id>`  
   - `Output Bucket`: Stores processed JSON results  
     `data-processing-output-<your-account-id>`

2. **Amazon EventBridge**  
   - Filters `ObjectCreated` events from S3 for `.csv` files and triggers Step Functions

3. **AWS Step Functions** *(State Machine: DataProcessingWorkflow)*  
   - `ValidateData`: Validates the uploaded CSV file  
   - `ParallelProcess`: Executes two parallel branches (`ProcessData1`, `ProcessData2`)  
   - `AggregateData`: Aggregates results from both parallel branches  
   - `StoreResults`: Stores the final results to S3 and metadata to DynamoDB  
   - `NotifySuccess`: Sends a success notification via SNS  
   - `ErrorHandler`: Sends an error notification via SNS

4. **AWS Lambda**  
   - `ValidateDataFunction`: Validates the CSV format  
   - `ProcessDataFunction`: Processes data (used in each parallel branch)  
   - `AggregateDataFunction`: Aggregates intermediate results  
   - `StoreResultsFunction`: Saves final results and metadata

5. **Amazon DynamoDB**  
   - `ProcessingMetadata` table stores information such as `ExecutionId`, `Timestamp`, and `Status`

6. **Amazon CloudWatch**  
   - Logs events and sets alarms for failures or anomalies

7. **Amazon SNS**  
   - `PipelineNotifications` topic sends email alerts about pipeline status

8. **IAM Roles**  
   - `LambdaDataProcessingRole`: Grants Lambda permission to access S3, DynamoDB, and SNS  
   - `StepFunctionsDataProcessingRole`: Grants Step Functions permission to invoke Lambda and SNS

---

## 🔁 Data Flow

1. A user uploads a CSV file (e.g., `test.csv`) to the **S3 Input Bucket**
2. S3 triggers an `ObjectCreated` event, which is sent to **EventBridge**
3. **EventBridge Rule** (`S3TriggerRule`) filters `.csv` files and triggers the **Step Functions** workflow (`DataProcessingWorkflow`)
4. **Step Functions** orchestrates the following steps:
   - `ValidateData`: Calls `ValidateDataFunction` to validate the CSV file
   - `ParallelProcess`: Executes two parallel branches, each invoking `ProcessDataFunction`
   - `AggregateData`: Calls `AggregateDataFunction` to merge results
   - `StoreResults`: Invokes `StoreResultsFunction` to save outputs to S3 and metadata to DynamoDB
   - `NotifySuccess`: Sends a success notification via SNS
   - `ErrorHandler`: Sends an error notification via SNS if any step fails

5. **CloudWatch** logs execution and triggers alerts in case of errors
6. **SNS** sends email notifications about the final pipeline status (success or failure)

![ConnectPrivate](/ws_FCJ_HoangNam/images/SoDo.drawio.png) 

