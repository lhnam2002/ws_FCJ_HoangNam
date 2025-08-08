---
title : "Tạo Step Functions State Machine"
date : 2025-06-06 
weight : 8 
chapter : false
pre : " <b> 8. </b> "
---

## Bước 6: Tạo Step Functions State Machine

AWS Step Functions sẽ điều phối toàn bộ pipeline xử lý dữ liệu serverless.

---

## 1. Truy cập Step Functions Console

- Mở AWS Console
- Tìm **Step Functions**
- Chọn **State machines** → nhấn **Create state machine**
![Connect](/ws_FCJ_HoangNam/images/8.createStepFunctions/B6.png)

---

## 2. Chọn loại state machine

- **Author with code**
- **Type**: `Standard`

---

## 3. Định nghĩa State Machine

- Dán đoạn mã JSON định nghĩa workflow
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
- **Thay thế các giá trị sau**:
  - `123456789012` → bằng AWS Account ID của bạn
  - ARN của các Lambda function → copy từ **Lambda Console**
  - ARN của SNS topic → copy từ **SNS Console**

- Nhấn **Next**

---

## 4. Cấu hình State Machine

- **Name**: `DataProcessingWorkflow`
- **Permissions**:
  - Chọn **Choose an existing role**
  - Chọn role: `StepFunctionsDataProcessingRole`
  ![Connect](/ws_FCJ_HoangNam/images/8.createStepFunctions/B6_4.png)

- **Logging**:
  - Bật `Log to CloudWatch Logs`
  - Chọn mức log: `ALL`

- Nhấn **Create state machine**
![Connect](/ws_FCJ_HoangNam/images/8.createStepFunctions/B6_4_1.png)



---

## 5. Kiểm tra

- Quay lại danh sách **State machines**
- Đảm bảo thấy `DataProcessingWorkflow` trong danh sách
