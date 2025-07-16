---
title : "Scalability Testing"
date : 2025-06-06 
weight : 13 
chapter : false
pre : " <b> 13. </b> "
---

## Step 11: Scalability Testing

## 1. Quickly Create 100 CSV Files

Create file python: `create_csv.py`


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

## 2. Load Testing

- Prepare 100 sample CSV files.
- Use AWS CLI to upload all files in bulk:  
  ```bash
  aws s3 cp ./test-files/ s3://data-processing-input-123456789012/ --recursive
  ```
![Connect](/ws_FCJ_HoangNam/images/11.ScalabilityTesting/B11.png)
- Or upload manually via the **S3 Console**:
  - Navigate to the `data-processing-input-123456789012` bucket
  - Click **Upload** → Select all CSV files → Click **Upload**

## 3. Performance Monitoring

### A. Check Number of Executions (in Step Functions)

- Go to **AWS Step Functions Console**
- Select the state machine: `DataProcessingWorkflow`
- Open the **Executions** tab
- You should see one execution per uploaded file

📌 If you upload 100 files → you should see around **100 executions**

**Note:**
- If the number is lower than expected:
  - EventBridge rule might not be triggering correctly
  - Or Lambda trigger might be failing silently

---

### B. Monitor Performance Using CloudWatch

#### 1. View Lambda Metrics:

- Open **CloudWatch Console**
- Navigate to the **Metrics** tab → Click **All metrics**
- Select: **Lambda → By Function Name**
- Choose your Lambda functions:
  - `ValidateDataFunction`
  - `ProcessDataFunction`
  - `AggregateDataFunction`
  - `StoreResultsFunction`

| Metric      | Meaning |
|-------------|---------|
| Duration    | Average execution time per function. If >1000ms → consider optimizing |
| Invocations | Number of times the function is triggered (should match number of files) |
| Errors      | Total number of Lambda errors |
| Throttles   | Indicates resource limitation or concurrency issues |

![Connect](/ws_FCJ_HoangNam/images/11.ScalabilityTesting/B11_3.png)
![Connect](/ws_FCJ_HoangNam/images/11.ScalabilityTesting/B11_4.png)
![Connect](/ws_FCJ_HoangNam/images/11.ScalabilityTesting/B11_5.png)
![Connect](/ws_FCJ_HoangNam/images/11.ScalabilityTesting/B11_6.png)


---

### C. Optimize Lambda Functions If Pipeline Is Slow

- Open **Lambda Console** → Select the function to optimize
- Go to **Configuration → General configuration**
- Click **Edit**
  - Increase **Memory size** (e.g., from 256MB → 512MB or 1024MB)
  - Increasing memory will also boost CPU power → better performance
- Click **Save**

📌 **Suggested Configuration:**
- For heavy functions (`ProcessDataFunction`, `AggregateDataFunction`): **512–1024MB**
- For light functions (`StoreResultsFunction`): **256MB**

---

## 4. Use Distributed Map (for Large-Scale Data)

- Edit the state machine definition:
  - Replace the `Parallel` state with a `Map` state
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
- Requirements:
  - The input to the `Map` state must be a **JSON array**
  - You will need to modify `ValidateDataFunction` to return a valid array as output

