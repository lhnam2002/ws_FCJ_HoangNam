---
title : "Kiểm Tra Khả Năng Mở Rộng"
date : 2025-06-06 
weight : 13 
chapter : false
pre : " <b> 13. </b> "
---

## Bước 11: Kiểm Tra Khả Năng Mở Rộng

## 1. Tạo Nhanh 100 File CSV

- Tạo file python với tên là `create_csv.py`


```py
import csv
import os
import random

def create_csv_files(num_files=100, folder_name="csv_files"):
    # In đường dẫn hiện tại
    current_path = os.getcwd()
    print(f"Đang chạy từ: {current_path}")
    
    # Tạo đường dẫn đầy đủ
    full_path = os.path.join(current_path, folder_name)
    print(f"Sẽ tạo file tại: {full_path}")
    
    # Tạo thư mục nếu chưa có
    if not os.path.exists(folder_name):
        os.makedirs(folder_name)
        print(f"Đã tạo thư mục: {folder_name}")
    else:
        print(f"Thư mục {folder_name} đã tồn tại")
    
    # Danh sách tên ngẫu nhiên
    names = [
        "John", "Jane", "Alice", "Bob", "Charlie", "Diana", "Eve", "Frank",
        "Grace", "Henry", "Ivy", "Jack", "Kate", "Leo", "Mia", "Nick",
        "Olivia", "Paul", "Quinn", "Rose", "Sam", "Tina", "Uma", "Victor",
        "Wendy", "Xavier", "Yara", "Zoe", "Anna", "Ben", "Chloe", "David"
    ]
    
    print(f"Bắt đầu tạo {num_files} file CSV...")
    
    for i in range(2, num_files + 2):
        filename = f"{folder_name}/test{i}.csv"
        
        with open(filename, 'w', newline='', encoding='utf-8') as csvfile:
            writer = csv.writer(csvfile)
            
            # Header
            writer.writerow(['id', 'name', 'amount'])
            
            # Tạo dữ liệu ngẫu nhiên (3-4 dòng mỗi file)
            rows = random.randint(3, 4)
            for j in range(1, rows + 1):
                row = [
                    j,
                    random.choice(names),
                    random.randint(50, 9999)  # Amount từ 50 đến 9999
                ]
                writer.writerow(row)
        
        # In progress mỗi 10 file
        if i % 10 == 0:
            print(f"Đã tạo {i-1} file...")
    
    print(f"✅ HOÀN THÀNH! Đã tạo {num_files} file CSV trong thư mục '{folder_name}'")
    print(f"📁 Đường dẫn đầy đủ: {full_path}")
    print(f"📄 File từ test2.csv đến test{num_files + 1}.csv")
    
    # Liệt kê 5 file đầu tiên để kiểm tra
    print("\n🔍 Kiểm tra một số file đã tạo:")
    for i in range(2, 7):  # test2.csv đến test6.csv
        filepath = os.path.join(folder_name, f"test{i}.csv")
        if os.path.exists(filepath):
            print(f"  ✓ {filepath}")
        else:
            print(f"  ✗ {filepath} - KHÔNG TÌM THẤY")

# Chạy hàm
if __name__ == "__main__":
    create_csv_files(100)
```
- Nhấn Run để chạy chương trình

## 2. Tăng Tải

- Tạo 100 tệp CSV mẫu.
- Sử dụng AWS CLI để tải lên hàng loạt:
  ```bash
  aws s3 cp ./test-files/ s3://data-processing-input-123456789012/ --recursive
  ```
![Connect](/ws_FCJ_HoangNam/images/11.ScalabilityTesting/B11.png)
- Hoặc tải lên thủ công thông qua **S3 Console**:
  - Truy cập bucket `data-processing-input-123456789012`
  - Nhấn **Upload** → Chọn các tệp `.csv` → Nhấn **Upload**

## 3. Kiểm Tra Hiệu Suất

### A. Kiểm Tra Số Lượng Executions (Step Functions)

- Truy cập **AWS Step Functions Console**
- Chọn state machine `DataProcessingWorkflow`
- Nhấn vào tab **Executions**
- Mỗi file `.csv` tải lên thành công sẽ khởi tạo 1 execution

📌 Nếu bạn upload 100 file → bạn nên thấy khoảng **100 executions**

**Lưu ý:**
- Nếu thấy ít hơn số file upload:
  - Có thể EventBridge chưa kích hoạt đúng
  - Lambda trigger lỗi hoặc timeout

---

### B. Giám Sát Hiệu Suất Với CloudWatch

#### 1. Mở Metrics Cho Lambda:

- Truy cập **CloudWatch Console**
- Chọn tab **Metrics** → nhấn **All metrics**
- Chọn: **Lambda → By Function Name**
- Chọn các hàm:
  - `ValidateDataFunction`
  - `ProcessDataFunction`
  - `AggregateDataFunction`
  - `StoreResultsFunction`

| Metric       | Ý Nghĩa |
|--------------|--------|
| Duration     | Thời gian trung bình chạy mỗi hàm (ms). Nếu >1000ms → nên tối ưu lại |
| Invocations  | Số lần được gọi. Có đúng bằng số file đã upload không? |
| Errors       | Số lỗi xảy ra trong Lambda |
| Throttles    | Có bị giới hạn do thiếu tài nguyên không (concurrency thấp?) |

![Connect](/ws_FCJ_HoangNam/images/11.ScalabilityTesting/B11_3.png)
![Connect](/ws_FCJ_HoangNam/images/11.ScalabilityTesting/B11_4.png)
![Connect](/ws_FCJ_HoangNam/images/11.ScalabilityTesting/B11_5.png)
![Connect](/ws_FCJ_HoangNam/images/11.ScalabilityTesting/B11_6.png)

---

### C. Tối Ưu Lambda Nếu Pipeline Chậm

- Vào **Lambda Console** → chọn hàm muốn tối ưu
- Tab **Configuration → General configuration**
- Nhấn **Edit** để chỉnh:
  - **Tăng Memory size** từ 256MB → 512MB hoặc 1024MB
  - RAM tăng sẽ đồng thời tăng CPU → giúp chạy nhanh hơn
- Nhấn **Save**

📌 **Gợi ý cấu hình:**
- `ProcessDataFunction`, `AggregateDataFunction`: **512–1024MB**
- `StoreResultsFunction`: **256MB**

---

## 4. Sử Dụng Distributed Map Cho Dữ Liệu Lớn

- Chỉnh sửa định nghĩa state machine:
  - Thay trạng thái **Parallel** bằng **Map**
```js
    "MapProcess": {
  "Type": "Map",
  "ItemsPath": "$.items",
  "MaxConcurrency": 100,
  "Iterator": {
    "StartAt": "ProcessData",
    "States": {
      "ProcessData": {
        "Type": "Task",
        "Resource": "arn:aws:lambda:ap-southeast-1:123456789012:function:ProcessDataFunction",
        "End": true
      }
    }
  },
  "Next": "AggregateData"
}

```  
- Yêu cầu:
  - Input cho `Map` phải là **mảng JSON**
  - Cần sửa `ValidateDataFunction` để trả về mảng JSON phù hợp

