+++
title = "Tạo DynamoDB Table"
date = 2021
weight = 6
chapter = false
pre = "<b>6. </b>"
+++

## Bước 4: Tạo DynamoDB Table

DynamoDB sẽ được sử dụng để lưu trữ metadata cho từng lần thực thi pipeline.

---

## 1. Truy cập DynamoDB Console

- Mở AWS Console
- Tìm **DynamoDB** → chọn **Tables** → nhấn **Create table**

---

## 2. Cấu hình bảng

- **Table name**: `ProcessingMetadata`
- **Partition key**: `ExecutionId` *(Kiểu: String)*
- **Sort key**: *(để trống)*
- **Table settings**: Chọn **On-demand (Pay-per-request)** để tự động co giãn theo lưu lượng
- Nhấn **Create table**

---

## 3. Kiểm tra bảng

- Truy cập mục **Tables**
- Đảm bảo bảng `ProcessingMetadata` đã được tạo thành công


