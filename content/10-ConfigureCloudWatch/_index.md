---
title : "Configure CloudWatch"
date : 2025-06-06 
weight : 10 
chapter : false
pre : " <b> 10. </b> "
---

## Step 8: Configure CloudWatch Monitoring

Amazon CloudWatch helps monitor performance and detect failures in your data pipeline.

---

## 1. Access the CloudWatch Console

- Open **AWS Console**
- Search for **CloudWatch** and open the service

---

## 2. Create Alarm for Step Functions

- Go to **Alarms** → Click **Create alarm**
- **Select metric**:
  - Choose: `States` → `Execution Metrics` → `ExecutionsFailed`
  - Select: `DataProcessingWorkflow`
- **Conditions**:
  - Threshold: `Greater than or equal to 1`
  - Period: `5 minutes`
- **Actions**:
  - Select: `Send notification to SNS topic`
  - SNS Topic: `PipelineNotifications`
- **Alarm name**: `StepFunctionsFailureAlarm`
- Click **Create**

---

## 3. Create Alarms for Lambda Functions

Repeat the process for each Lambda function using the `Errors` metric.

### Lambda Functions:
- `ValidateDataFunction`
- `ProcessDataFunction`
- `AggregateDataFunction`
- `StoreResultsFunction`

### Steps:
- Go to **Alarms** → Click **Create alarm**
- **Select metric**:
  - Navigate to: `Lambda` → `By Function Name`
  - Choose the function → Select `Errors` metric
- **Conditions**:
  - **Threshold type**: Static
  - **Condition**: Greater than or equal to 1
  - **Period**: 5 minutes
  - **Datapoints to alarm**: 1 out of 1
- **Notifications**:
  - Alarm state trigger: `In alarm`
  - Send notification to: `PipelineNotifications` SNS topic
- **Alarm name** (clear naming):
  - `LambdaValidateDataErrorAlarm`
  - `LambdaProcessDataErrorAlarm`
  - `LambdaAggregateDataErrorAlarm`
  - `LambdaStoreResultsErrorAlarm`
- Click **Create**

---

## 4. Check Logs

- Go to **Log groups**
- Check the following log groups:

### Lambda:
- `/aws/lambda/ValidateDataFunction`
- `/aws/lambda/ProcessDataFunction`
- `/aws/lambda/AggregateDataFunction`
- `/aws/lambda/StoreResultsFunction`

### Step Functions:
- `/aws/vendedlogs/states/DataProcessingWorkflow`

