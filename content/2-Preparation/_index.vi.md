---
title : "Chuẩn bị"
date : 2025-06-06 
weight : 2 
chapter : false
pre : " <b> 2. </b> "
---

## Các Bước Chuẩn Bị

## 1. Tạo Các Thành Phần IAM

### Bước 1: Tạo IAM User Group

- Truy cập AWS Console: [https://console.aws.amazon.com/iam](https://console.aws.amazon.com/iam)
- Chọn **User groups** → **Create group**
![Connect](/ws_FCJ_HoangNam/images/2.preparation/bs1.png)
- Đặt tên nhóm:  `devGr`
- Gán quyền:
  - Chọn policy có sẵn: `AdministratorAccess`
- Nhấn **Create group**
![Connect](/ws_FCJ_HoangNam/images/2.preparation/bs2.png)
---

### Bước 2: Tạo IAM User

- Truy cập mục **Users** → chọn **Add users**
![Connect](/ws_FCJ_HoangNam/images/2.preparation/bs3.png)
- Nhập tên: `dev-user`
- Chọn Access Type:
  - **Programmatic access** *(để dùng AWS CLI)*
  - **AWS Management Console access** *(để đăng nhập web)*
- Thiết lập mật khẩu hoặc để AWS tạo ngẫu nhiên
- Thêm user vào group: chọn `devGr`
- Nhấn **Create user**
![Connect](/ws_FCJ_HoangNam/images/2.preparation/bs4.png)
![Connect](/ws_FCJ_HoangNam/images/2.preparation/bs5.png)
![Connect](/ws_FCJ_HoangNam/images/2.preparation/bs6.png)
---

### Bước 3: Tạo AWS Access Key

- Trong danh sách users, click vào user `dev-user`
- Chuyển sang tab **Security credentials**
- Ở mục **Access keys**, nhấn **Create access key**
![Connect](/ws_FCJ_HoangNam/images/2.preparation/bs7.png)
- Chọn mục đích sử dụng: **CLI**
![Connect](/ws_FCJ_HoangNam/images/2.preparation/bs8.png)
![Connect](/ws_FCJ_HoangNam/images/2.preparation/bs9.png)
- Sau khi tạo thành công, bạn sẽ nhận:
  - `Access key ID`
  - `Secret access key`
- ⚠️ **Lưu lại ngay**, vì bạn chỉ thấy `Secret access key` duy nhất một lần!
![Connect](/ws_FCJ_HoangNam/images/2.preparation/bs10.png)

---

## 2. Cài Đặt và Cấu Hình AWS CLI

###  Cài Đặt AWS CLI trên Windows

- Mở hộp thoại Run (Windows + R), dán lệnh sau để tải và cài:

```bash
msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi
```
- Kiểm Tra Phiên Bản CLI
```bash
aws --version
```
- Cấu Hình CLI
```bash
aws configure
```
- Nhập các thông tin sau:

    -- Access Key ID

    -- Secret Access Key

    -- Region (ví dụ: ap-southeast-1)

    -- Output format (ví dụ: json)

    ![Connect](/ws_FCJ_HoangNam/images/2.preparation/bs11.png)

## 3. Tạo Tệp CSV

### Tạo File CSV
- Nhấn Windows + R → gõ notepad

- Dán nội dung sau:
```txt
id,name,amount
1,John,100
2,Jane,200
```
- Chọn Save As:

- Tên file: test.csv

- Save as type: All Files (*.*)

- Encoding: UTF-8

- Đảm bảo không lưu thành .txt