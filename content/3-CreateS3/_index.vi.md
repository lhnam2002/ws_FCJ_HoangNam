---
title : "Tạo S3 Buckets"
date : 2025-06-06 
weight : 3 
chapter : false
pre : " <b> 3. </b> "
---
## ☁️ Bước 1: Tạo S3 Buckets

Amazon S3 được sử dụng để lưu trữ dữ liệu **đầu vào (input)** và **đầu ra (output)** cho pipeline xử lý dữ liệu.

---

## 1. Truy cập S3 Console

- Vào AWS Console: [https://console.aws.amazon.com/s3](https://console.aws.amazon.com/s3)
- Gõ **S3** vào thanh tìm kiếm và chọn **Amazon S3**

![Connect](/ws_FCJ_HoangNam/images/3.createS3/B1_1.png)

---

## 2. Tạo Bucket Đầu Vào (Input Bucket)

- Nhấn **Create bucket**
- Thiết lập thông tin:

  - **Bucket name**: `data-processing-input-123456789012`  
    *(thay `123456789012` bằng AWS Account ID của bạn)*

  - **Region**: Chọn `Asia Pacific (Singapore)` - `ap-southeast-1`

  - **Object Ownership**: Chọn `ACLs disabled`

  ![Connect](/ws_FCJ_HoangNam/images/3.createS3/B1_2_1.png)

  - **Block Public Access**: Giữ mặc định *(chặn tất cả truy cập công khai)*

  - **Encryption**: Bật `Enable` với tùy chọn  
    **Server-side encryption with Amazon S3-managed keys (SSE-S3)**
  
  ![Connect](/ws_FCJ_HoangNam/images/3.createS3/B1_2_2.png)

- Nhấn **Create bucket**

  ![Connect](/ws_FCJ_HoangNam/images/3.createS3/B1_2_3.png)

⚠️ *Nếu gặp lỗi `"Bucket name already exists"`, hãy thêm hậu tố ngẫu nhiên, ví dụ:*  
`data-processing-input-123456789012-v1`


---

## 3. Tạo Bucket Đầu Ra (Output Bucket)

- Lặp lại các bước trên để tạo bucket:
  - **Bucket name**: `data-processing-output-123456789012`     

---

## 4. Kiểm Tra Bucket

- Vào tab **Buckets**, xác nhận đã tạo được cả:
  - `data-processing-input-123456789012`
  - `data-processing-output-123456789012`

![Connect](/ws_FCJ_HoangNam/images/3.createS3/B1_3.png)
---

## Tùy Chọn: Tải file test.csv lên Input Bucket

- Vào bucket `data-processing-input-123456789012`
![Connect](/ws_FCJ_HoangNam/images/3.createS3/B1_4.png)
- Nhấn **Upload**
- Chọn file `test.csv` đã tạo trước đó
![Connect](/ws_FCJ_HoangNam/images/3.createS3/B1_4_2.png)
- Nhấn **Upload** để tải lên
![Connect](/ws_FCJ_HoangNam/images/3.createS3/B1_4_3.png)

---





