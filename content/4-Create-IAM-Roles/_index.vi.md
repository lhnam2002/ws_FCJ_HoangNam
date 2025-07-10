---
title : "Tạo IAM Roles"
date : 2025-06-06 
weight : 4 
chapter : false
pre : " <b> 4. </b> "
---

##  Bước 2: Tạo IAM Roles

IAM Roles dùng để cấp quyền truy cập cho các dịch vụ **AWS Lambda** và **AWS Step Functions** trong quá trình xử lý dữ liệu.

---

## 1. Truy cập IAM Console

- Vào AWS Console: [https://console.aws.amazon.com/iam](https://console.aws.amazon.com/iam)
- Tìm **IAM** → chọn **Roles** → nhấn **Create role**

---

## 2. Tạo Role Cho Lambda

- **Trusted entity type**: Chọn `AWS service` → chọn `Lambda`
- Nhấn **Next**

### Gán Permissions:

- Nhấn **Add permissions**
- Tìm và chọn các policy sau:

  - `AWSLambdaBasicExecutionRole` *(ghi log lên CloudWatch)*
  - `AmazonS3FullAccess` *(quyền truy cập S3)*
  - `AmazonDynamoDBFullAccess` *(truy cập DynamoDB)*
  - `AmazonSNSFullAccess` *(gửi thông báo qua SNS)*

> 🔒 *Khuyến nghị bảo mật*: Thay vì dùng `FullAccess`, nên tạo **chính sách tuỳ chỉnh** giới hạn quyền theo tài nguyên cụ thể (nêu ở phần sau)

- Nhấn **Next** → đặt tên role: `LambdaDataProcessingRole`
- Nhấn **Create role**

---

## 3. Tạo Role Cho Step Functions

- Quay lại IAM > Roles → **Create role**
- **Trusted entity type**: Chọn `AWS service` → chọn `Step Functions`
- Nhấn **Next**

### Gán Permissions:

- Tìm và chọn các policy sau:

  - `AWSLambdaFullAccess` *(để Step Functions gọi Lambda)*
  - `AmazonS3FullAccess`
  - `AmazonDynamoDBFullAccess`
  - `CloudWatchLogsFullAccess`
  - `AmazonSNSFullAccess`

- Nhấn **Next** → đặt tên role: `StepFunctionsDataProcessingRole`
- Nhấn **Create role**

---

## 4. Kiểm Tra Roles

- Truy cập IAM > **Roles**
- Tìm và xác nhận đã tạo 2 role:

  - `LambdaDataProcessingRole`
  - `StepFunctionsDataProcessingRole`

