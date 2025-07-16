---
title : "Test the Pipeline"
date : 2025-06-06 
weight : 11 
chapter : false
pre : " <b> 11. </b> "
---

## Step 9: Test the Pipeline

Perform a full test of the serverless data pipeline after deployment.

---

##  1. Upload CSV File to S3

- Go to the **S3 Console**
- Navigate to the bucket: `data-processing-input-123456789012`
- Click **Upload**, select the file `test.csv`
- Click **Upload**

---

## 2. Monitor Step Functions Execution

- Open the **Step Functions Console**
- Select the state machine: `DataProcessingWorkflow`
- Go to the **Executions** tab
- Click on the latest execution to inspect the flow
- Review individual steps: in the ***Visual workflow***.
  - `ValidateData`
  - `ParallelProcess`
  - `AggregateData`
  - `StoreResults`
  - `NotifySuccess`

---

## 3. Check Output

- Go to the output S3 bucket: `data-processing-output-123456789012`
- Locate the folder `results/` and verify the result file (e.g., `processed_<timestamp>.json`)
- Open the **DynamoDB Console**
  - Navigate to table `ProcessingMetadata`
  - Use **Explore table items** to inspect execution metadata

---

## 4. Verify SNS Notifications

- Check your email inbox
- Look for the notification from the `PipelineNotifications` SNS topic

---

## ❗ 5. Error Handling (if any)

- Open the **CloudWatch Console**
  - Go to **Log groups** → check logs for relevant Lambda functions or Step Functions
- Revisit **Step Functions** → `Executions` to review failure state
- Identify the failed step → update Lambda code or state machine definition as needed
