---
title : "Create Step Functions State Machine"
date : 2025-06-06 
weight : 8 
chapter : false
pre : " <b> 8. </b> "
---

## Step 6: Create Step Functions State Machine

AWS Step Functions will orchestrate the entire serverless data processing pipeline.

---

## 1. Access the Step Functions Console

- Open AWS Console
- Search for **Step Functions**
- Select **State machines** → click **Create state machine**

---

## 2. Choose the State Machine Type

- **Author with code**
- **Type**: `Standard`

---

## 3. Define the State Machine

- Paste the JSON definition for the state machine:
```js
        {
  "Comment": "Serverless Data Processing Pipeline",
  "StartAt": "ValidateData",
  "States": {
    "ValidateData": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:ap-southeast-1:123456789012:function:ValidateDataFunction",
      "Next": "ParallelProcess",
      "Retry": [
        {
          "ErrorEquals": [
            "States.ALL"
          ],
          "IntervalSeconds": 3,
          "MaxAttempts": 3,
          "BackoffRate": 2
        }
      ],
      "Catch": [
        {
          "ErrorEquals": [
            "States.ALL"
          ],
          "ResultPath": "$.error",
          "Next": "ErrorHandler"
        }
      ]
    },
    "ParallelProcess": {
      "Type": "Parallel",
      "Next": "AggregateData",
      "Branches": [
        {
          "StartAt": "ProcessData1",
          "States": {
            "ProcessData1": {
              "Type": "Task",
              "Resource": "arn:aws:lambda:ap-southeast-1:123456789012:function:ProcessDataFunction",
              "End": true
            }
          }
        },
        {
          "StartAt": "ProcessData2",
          "States": {
            "ProcessData2": {
              "Type": "Task",
              "Resource": "arn:aws:lambda:ap-southeast-1:123456789012:function:ProcessDataFunction",
              "End": true
            }
          }
        }
      ],
      "Catch": [
        {
          "ErrorEquals": [
            "States.ALL"
          ],
          "ResultPath": "$.error",
          "Next": "ErrorHandler"
        }
      ]
    },
    "AggregateData": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:ap-southeast-1:123456789012:function:AggregateDataFunction",
      "Next": "StoreResults",
      "Retry": [
        {
          "ErrorEquals": [
            "States.ALL"
          ],
          "IntervalSeconds": 3,
          "MaxAttempts": 3,
          "BackoffRate": 2
        }
      ],
      "Catch": [
        {
          "ErrorEquals": [
            "States.ALL"
          ],
          "ResultPath": "$.error",
          "Next": "ErrorHandler"
        }
      ]
    },
    "StoreResults": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:ap-southeast-1:123456789012:function:StoreResultsFunction",
      "Next": "NotifySuccess",
      "Catch": [
        {
          "ErrorEquals": [
            "States.ALL"
          ],
          "ResultPath": "$.error",
          "Next": "ErrorHandler"
        }
      ]
    },
    "NotifySuccess": {
      "Type": "Task",
      "Resource": "arn:aws:states:::sns:publish",
      "Parameters": {
        "Message": "Pipeline completed successfully",
        "Subject": "Pipeline Status",
        "TopicArn": "arn:aws:sns:ap-southeast-1:123456789012:PipelineNotifications"
      },
      "End": true
    },
    "ErrorHandler": {
      "Type": "Task",
      "Resource": "arn:aws:states:::sns:publish",
      "Parameters": {
        "Message.$": "States.JsonToString($.error)",
        "Subject": "Pipeline Error",
        "TopicArn": "arn:aws:sns:ap-southeast-1:123456789012:PipelineNotifications"
      },
      "End": true
    }
  }
}
```
- **Replace the following**:
  - `123456789012` → with your AWS Account ID
  - Lambda function ARNs → copy from **Lambda Console**
  - SNS topic ARN → copy from **SNS Console**

- Click **Next**

---

## 4. Configure the State Machine

- **Name**: `DataProcessingWorkflow`
- **Permissions**:
  - Choose **Choose an existing role**
  - Select role: `StepFunctionsDataProcessingRole`
- **Logging**:
  - Enable `Log to CloudWatch Logs`
  - Set level: `ALL`

- Click **Create state machine**

---

## 5. Verify

- Go back to **State machines** list
- Ensure that `DataProcessingWorkflow` appears successfully
