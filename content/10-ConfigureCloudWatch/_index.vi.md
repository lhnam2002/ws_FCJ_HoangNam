---
title : "Thiết lập CloudWatch"
date : 2025-06-06 
weight : 10 
chapter : false
pre : " <b> 10. </b> "
---

## Bước 8: Thiết lập CloudWatch Monitoring

Amazon CloudWatch sẽ giúp bạn giám sát hiệu suất và lỗi trong hệ thống Pipeline.

---

## 1. Truy cập CloudWatch Console

- Vào **AWS Console**
- Tìm kiếm **CloudWatch** và mở dịch vụ

---

## 2. Tạo Alarm cho Step Functions

- Chọn **Alarms** → Nhấn **Create alarm**
- **Select metric**:
  - Chọn `States` → `Execution Metrics` → `ExecutionsFailed`
  - Chọn workflow: `DataProcessingWorkflow`
- **Conditions**:
  - Threshold: `Greater than or equal to 1`
  - Period: `5 minutes`
- **Actions**:
  - Chọn: `Send notification to SNS topic`
  - SNS Topic: `PipelineNotifications`
- **Alarm name**: `StepFunctionsFailureAlarm`
- Nhấn **Create**

---

## 3. Tạo Alarms cho Lambda Functions

Lặp lại quy trình tạo alarm cho từng hàm Lambda với metric `Errors`.

### Danh sách hàm Lambda:
- `ValidateDataFunction`
- `ProcessDataFunction`
- `AggregateDataFunction`
- `StoreResultsFunction`

### Các bước thực hiện:
- Vào **Alarms** → Chọn **Create alarm**
- **Select metric**:
  - Chọn `Lambda` → `By Function Name`
  - Tìm hàm → chọn metric `Errors`
- **Conditions**:
  - **Threshold type**: Static
  - **Condition**: Greater than or equal to 1
  - **Period**: 5 minutes
  - **Datapoints to alarm**: 1 out of 1
- **Notification**:
  - Alarm state trigger: `In alarm`
  - SNS Topic: `PipelineNotifications`
- **Alarm name** (đặt tên rõ ràng):
  - `LambdaValidateDataErrorAlarm`
  - `LambdaProcessDataErrorAlarm`
  - `LambdaAggregateDataErrorAlarm`
  - `LambdaStoreResultsErrorAlarm`
- Nhấn **Create**

---

## 4. Kiểm tra Logs

- Vào mục **Log groups**
- Kiểm tra các nhóm log sau:

### Lambda:
- `/aws/lambda/ValidateDataFunction`
- `/aws/lambda/ProcessDataFunction`
- `/aws/lambda/AggregateDataFunction`
- `/aws/lambda/StoreResultsFunction`

### Step Functions:
- `/aws/vendedlogs/states/DataProcessingWorkflow`

