---
title : "Tạo EventBridge Rule"
date : 2025-06-06 
weight : 9 
chapter : false
pre : " <b> 9. </b> "
---

## Bước 7: Tạo EventBridge Rule

Amazon EventBridge sẽ lắng nghe sự kiện từ S3 và kích hoạt Step Functions khi tệp mới được tải lên.

---

## 1. Truy cập EventBridge Console

- Mở AWS Console
- Tìm **EventBridge**
- Chọn **Rules** → nhấn **Create rule**

---

## 2. Cấu hình Rule

- **Name**: `S3TriggerRule`
- **Event source**: chọn **Event Pattern**
- **Event pattern** (JSON):

```json
{
  "source": ["aws.s3"],
  "detail-type": ["Object Created"],
  "detail": {
    "bucket": {
      "name": ["data-processing-input-123456789012"]
    },
    "object": {
      "key": [{"suffix": ".csv"}]
    }
  }
}

```
- Thay data-processing-input-123456789012 bằng tên bucket.

## 3. Thiết lập Target
- Target type: Chọn Step Functions state machine
- State machine: chọn DataProcessingWorkflow
- Execution role: chọn Create a new role for this specific resource
- Nhấn Create

## 4. Kiểm tra Rule
- Quay lại Rules
- Đảm bảo Rule có tên S3TriggerRule đã được tạo và đang ở trạng thái Enabled





