---
title : "Tạo SNS Topic"
date : 2025-06-06 
weight : 7 
chapter : false
pre : " <b> 7. </b> "
---

## Bước 5: Tạo SNS Topic

Amazon SNS sẽ được dùng để gửi thông báo trạng thái pipeline qua email.

---

## 1. Truy cập SNS Console

- Mở AWS Console
- Tìm **SNS**
![Connect](/ws_FCJ_HoangNam/images/7.createSNSTopic/B5.png)
- Chọn **Topics** → nhấn **Create topic**
![Connect](/ws_FCJ_HoangNam/images/7.createSNSTopic/B5_1.png)

---

## 2. Cấu hình Topic

- **Type**: `Standard`
- **Name**: `PipelineNotifications`
- Nhấn **Create topic**
![Connect](/ws_FCJ_HoangNam/images/7.createSNSTopic/B5_2.png)
---

## 3. Tạo Subscription (Đăng ký nhận thông báo)

- Trong topic `PipelineNotifications`, chọn **Create subscription**
- **Protocol**: `Email`
- **Endpoint**: Nhập địa chỉ email bạn muốn nhận thông báo
- Nhấn **Create subscription**
- Mở email và **xác nhận đăng ký** bằng cách nhấn vào liên kết xác nhận
![Connect](/ws_FCJ_HoangNam/images/7.createSNSTopic/B5_3.png)
---

## 4. Lấy ARN của Topic

- Quay lại tab **Topics**
- Nhấn vào `PipelineNotifications`
- Ghi lại **Topic ARN**, ví dụ: (arn:aws:sns:ap-southeast-1:123456789012:PipelineNotifications)
![Connect](/ws_FCJ_HoangNam/images/7.createSNSTopic/B5_3_1.png)
  