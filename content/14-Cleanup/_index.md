---
title : "Clean Up"
date : 2025-06-06 
weight : 14 
chapter : false
pre : " <b> 14. </b> "
---

## Step 12: Clean Up Resources

After completing the testing and evaluation of the pipeline, you should delete unused resources to avoid incurring unnecessary costs.

## 1. S3
- Delete buckets:
  - `data-processing-input-123456789012`
  - `data-processing-output-123456789012`

## 2. Lambda
- Delete functions:
  - `ValidateDataFunction`
  - `ProcessDataFunction`
  - `StoreResultsFunction`
  - `AggregateDataFunction`

## 3. Step Functions
- Delete state machine:
  - `DataProcessingWorkflow`

## 4. EventBridge
- Delete rule:
  - `S3TriggerRule`

## 5. DynamoDB
- Delete table:
  - `ProcessingMetadata`

## 6. SNS
- Delete topic:
  - `PipelineNotifications`

## 7. IAM
- Delete roles:
  - `LambdaDataProcessingRole`
  - `StepFunctionsDataProcessingRole`

