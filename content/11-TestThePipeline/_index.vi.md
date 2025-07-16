---
title : "Tạo EventBridge Rule"
date : 2025-06-06 
weight : 11 
chapter : false
pre : " <b> 11. </b> "
---

## Bước 9: Kiểm Tra Pipeline

Thực hiện kiểm tra toàn bộ quy trình xử lý dữ liệu sau khi hoàn tất triển khai.

---

## 1. Tải Tệp CSV lên S3

- Truy cập **S3 Console**
- Vào bucket: `data-processing-input-123456789012`
- Nhấn **Upload**, chọn tệp `test.csv` đã tạo
- Nhấn **Upload**

---

## 2. Kiểm Tra Step Functions

- Truy cập **Step Functions Console**
- Chọn state machine: `DataProcessingWorkflow`
- Chuyển đến tab **Executions**
- Nhấp vào execution mới nhất để theo dõi quá trình thực thi
- Kiểm tra từng bước: trong ***Visual workflow***.
  - `ValidateData`
  - `ParallelProcess`
  - `AggregateData`
  - `StoreResults`
  - `NotifySuccess`

---

## 3. Kiểm Tra Output

- Vào S3 bucket `data-processing-output-123456789012`
- Tìm thư mục `results/` và mở file JSON kết quả (ví dụ: `processed_<timestamp>.json`)
- Truy cập **DynamoDB Console**
  - Mở bảng `ProcessingMetadata`
  - Chọn **Explore table items** để xem thông tin metadata

---

## 4. Kiểm Tra SNS Notifications

- Mở hộp thư email của bạn
- Kiểm tra email từ SNS topic `PipelineNotifications`

---

## 5. Xử Lý Lỗi (nếu có)

- Truy cập **CloudWatch Console**
  - Mở **Log groups** → kiểm tra log của các Lambda function hoặc Step Functions
- Truy cập lại **Step Functions** → `Executions` để xem trạng thái thất bại
- Xác định bước lỗi → sửa code Lambda hoặc JSON của state machine nếu cần
