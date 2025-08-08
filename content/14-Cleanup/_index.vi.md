---
title : "Dọn dẹp tài nguyên"
date : 2025-06-06 
weight : 14 
chapter : false
pre : " <b> 14. </b> "
---

## Bước 12: Dọn dẹp tài nguyên

Sau khi hoàn thành kiểm thử và đánh giá pipeline, cần xóa các tài nguyên để tránh phát sinh chi phí không cần thiết.

## 1. S3
- Xóa bucket:
  - `data-processing-input-123456789012`
  - `data-processing-output-123456789012`

## 2. Lambda
- Xóa các function:
  - `ValidateDataFunction`
  - `ProcessDataFunction`
  - `StoreResultsFunction`
  - `AggregateDataFunction`

## 3. Step Functions
- Xóa state machine:
  - `DataProcessingWorkflow`

## 4. EventBridge
- Xóa rule:
  - `S3TriggerRule`

## 5. DynamoDB
- Xóa bảng:
  - `ProcessingMetadata`

## 6. SNS
- Xóa topic:
  - `PipelineNotifications`

## 7. IAM
- Xóa role:
  - `LambdaDataProcessingRole`
  - `StepFunctionsDataProcessingRole`
