---
title : "Optimization and Cost Analysis"
date : 2025-06-06 
weight : 12 
chapter : false
pre : " <b> 12. </b> "
---

## Step 10: Optimization and Cost Analysis

### 1. Parallel Optimization

In the `ParallelProcess` state of Step Functions, there are currently two branches. To add a new branch:
- Go to **Step Functions Console**, select `DataProcessingWorkflow`, and click **Edit**.
- Add a new branch inside the `Branches` section of `ParallelProcess`.
```js
    {
  "StartAt": "ProcessData3",
  "States": {
    "ProcessData3": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:ap-southeast-1:123456789012:function:ProcessDataFunction",
      "End": true
    }
  }
}
```
![Connect](/ws_FCJ_HoangNam/images/10.OptimizationandCostAnalysis/B10_1.png)
- Click **Save** to apply the changes.

### 2. Error Handling

- **Retry logic**: Each step (`ValidateData`, `AggregateData`, `StoreResults`) retries up to 3 times with a 3-second delay and a backoff rate of 2.0.
- **ErrorHandler**: If a step fails, the state transitions to `ErrorHandler`, which sends a notification via SNS.
- **Advanced option**: You can optionally add a new state to log the error into DynamoDB for traceability.
```js
   "ErrorHandler": {
  "Type": "Task",
  "Resource": "arn:aws:lambda:ap-southeast-1:123456789012:function:StoreResultsFunction",
  "Parameters": {
    "error.$": "$.error"
  },
  "Next": "NotifyError"
},
"NotifyError": {
  "Type": "Task",
  "Resource": "arn:aws:states:::sns:publish",
  "Parameters": {
    "Message.$": "States.JsonToString($.error)",
    "Subject": "Pipeline Error",
    "TopicArn": "arn:aws:sns:ap-southeast-1:123456789012:PipelineNotifications"
  },
  "End": true
}
```
![Connect](/ws_FCJ_HoangNam/images/10.OptimizationandCostAnalysis/B10_2.png)
### 3. Cost Breakdown

- **S3**: ~$0.023/GB/month (Standard storage, ap-southeast-1).
- **Lambda**: ~$0.20 per 1M requests + $0.0000167/GB-s.
- **Step Functions**: ~$0.025 per 1000 state transitions.
- **EventBridge**: ~$1.00 per 1M events.
- **DynamoDB**: ~$1.25 per 1M write requests (On-demand mode).
- **SNS**: ~$0.50 per 1M notifications.
- Use **AWS Cost Explorer** (search in AWS Console) to monitor daily/monthly usage.

### 4. Cost Optimization

- Use **S3 Intelligent-Tiering** to reduce storage cost automatically.
- Reduce Lambda memory if not needed (256 MB is often sufficient).
- Use **DynamoDB Provisioned Capacity** instead of On-demand mode if traffic is predictable.
