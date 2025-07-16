---
title : "Tối Ưu Hóa và Phân Tích Chi Phí"
date : 2025-06-06 
weight : 12 
chapter : false
pre : " <b> 12. </b> "
---

## Bước 10: Tối Ưu Hóa và Phân Tích Chi Phí

### 1. Tối ưu hóa song song

Trong Step Functions, trạng thái `ParallelProcess` hiện có hai nhánh. Để thêm nhánh:
- Truy cập **Step Functions Console**, chọn `DataProcessingWorkflow`, nhấn **Edit**.
- Thêm một nhánh mới trong phần `Branches` của trạng thái `ParallelProcess`.
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
- Nhấn **Save** để lưu thay đổi.

### 2. Xử lý lỗi

- **Retry logic**: Mỗi bước như `ValidateData`, `AggregateData`, `StoreResults` có 3 lần thử lại với khoảng cách 3 giây, hệ số backoff 2.0.
- **ErrorHandler**: Khi lỗi xảy ra, trạng thái sẽ chuyển tới `ErrorHandler` và gửi cảnh báo qua SNS.
- **Tùy chọn nâng cao**: Bạn có thể bổ sung một bước ghi lỗi vào DynamoDB để lưu lại chi tiết lỗi.
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

### 3. Phân tích chi phí

- **S3**: ~$0.023/GB/tháng (Standard storage, ap-southeast-1).
- **Lambda**: ~$0.20/1M requests + $0.0000167/GB-s.
- **Step Functions**: ~$0.025/1000 state transitions.
- **EventBridge**: ~$1.00/1M events.
- **DynamoDB**: ~$1.25/1M ghi (On-demand mode).
- **SNS**: ~$0.50/1M notifications.
- Sử dụng **AWS Cost Explorer** (trong AWS Console) để theo dõi chi phí theo ngày/tháng.

### 4. Tối ưu chi phí

- Dùng **S3 Intelligent-Tiering** để tối ưu chi phí lưu trữ.
- Giảm bộ nhớ Lambda nếu không cần thiết (256 MB thường là đủ).
- Chuyển sang **DynamoDB Provisioned Capacity** nếu có thể ước lượng trước tải hệ thống.