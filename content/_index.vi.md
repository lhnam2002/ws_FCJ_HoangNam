---
title : "Serverless Data Processing Pipeline"
date : 2025-06-06 
weight : 1 
chapter : false
---
# Xây dựng Pipeline Xử lý Dữ liệu Serverless với AWS Step Functions và EventBridge

## 📌 Tổng quan

Trong bối cảnh công nghệ hiện đại, nhu cầu xử lý dữ liệu lớn một cách hiệu quả, linh hoạt và tiết kiệm chi phí ngày càng trở nên cấp thiết. Các tổ chức và doanh nghiệp cần những giải pháp có khả năng mở rộng linh hoạt, dễ quản lý, và loại bỏ gánh nặng vận hành hạ tầng phức tạp.

Công nghệ **serverless** trên nền tảng đám mây đã nổi lên như một hướng đi tối ưu, cho phép xây dựng các hệ thống xử lý dữ liệu mạnh mẽ mà không cần quản lý máy chủ vật lý, từ đó giảm thiểu chi phí vận hành, tăng tốc độ triển khai và khả năng tự động hóa cao.

### 🧩 Mô tả tổng quan pipeline

Pipeline serverless này xử lý dữ liệu từ các tệp CSV được tải lên **Amazon S3**. **Amazon EventBridge** nhận sự kiện từ S3, lọc các tệp `.csv`, và kích hoạt **AWS Step Functions** để điều phối các bước xử lý. **AWS Lambda** thực hiện các tác vụ như xác thực, xử lý song song, tổng hợp, và lưu kết quả. **Amazon DynamoDB** lưu metadata, **Amazon CloudWatch** giám sát hiệu suất và lỗi, **Amazon SNS** gửi thông báo email về trạng thái pipeline.  
Tất cả được triển khai tại **Region Singapore (ap-southeast-1)**.

---

## 🧱 Các thành phần chính

1. **Amazon S3**  
   - `Input Bucket`: Lưu trữ tệp CSV đầu vào  
     `data-processing-input-<your-account-id>`  
   - `Output Bucket`: Lưu trữ kết quả JSON  
     `data-processing-output-<your-account-id>`

2. **Amazon EventBridge**  
   - Lọc sự kiện `ObjectCreated` từ S3 cho các tệp `.csv` và kích hoạt Step Functions

3. **AWS Step Functions** *(State Machine: DataProcessingWorkflow)*  
   - `ValidateData`: Kiểm tra tính hợp lệ của tệp CSV  
   - `ParallelProcess`: Xử lý song song hai nhánh (`ProcessData1`, `ProcessData2`)  
   - `AggregateData`: Tổng hợp kết quả từ các nhánh song song  
   - `StoreResults`: Lưu kết quả vào S3 và metadata vào DynamoDB  
   - `NotifySuccess`: Gửi thông báo thành công qua SNS  
   - `ErrorHandler`: Gửi thông báo lỗi qua SNS

4. **AWS Lambda**  
   - `ValidateDataFunction`: Kiểm tra định dạng CSV  
   - `ProcessDataFunction`: Xử lý dữ liệu (gọi trong các nhánh song song)  
   - `AggregateDataFunction`: Tổng hợp kết quả  
   - `StoreResultsFunction`: Lưu kết quả và metadata

5. **Amazon DynamoDB**  
   - Bảng `ProcessingMetadata` lưu thông tin như: `ExecutionId`, `Timestamp`, `Status`

6. **Amazon CloudWatch**  
   - Ghi log và thiết lập cảnh báo khi có lỗi

7. **Amazon SNS**  
   - `Topic: PipelineNotifications` gửi email thông báo trạng thái

8. **IAM Roles**  
   - `LambdaDataProcessingRole`: Quyền cho Lambda truy cập S3, DynamoDB, SNS  
   - `StepFunctionsDataProcessingRole`: Quyền cho Step Functions gọi Lambda và SNS

---

## 🔁 Luồng dữ liệu

1. Người dùng tải tệp CSV (ví dụ: `test.csv`) lên **S3 Input Bucket**
2. S3 tạo sự kiện `ObjectCreated`, gửi đến **EventBridge**
3. **EventBridge Rule** (`S3TriggerRule`) lọc tệp `.csv` và kích hoạt **Step Functions** (`DataProcessingWorkflow`)
4. **Step Functions** điều phối:
   - `ValidateData`: Gọi `ValidateDataFunction` để kiểm tra CSV
   - `ParallelProcess`: Chạy hai nhánh song song, mỗi nhánh gọi `ProcessDataFunction`
   - `AggregateData`: Gọi `AggregateDataFunction` để tổng hợp kết quả
   - `StoreResults`: Gọi `StoreResultsFunction` để lưu vào S3 Output Bucket và DynamoDB
   - `NotifySuccess`: Gửi thông báo thành công qua SNS
   - `ErrorHandler`: Gửi thông báo lỗi qua SNS nếu có lỗi ở bất kỳ bước nào

5. **CloudWatch** ghi log và kích hoạt cảnh báo nếu có lỗi
6. **SNS** gửi email thông báo trạng thái pipeline (thành công hoặc lỗi)

![ConnectPrivate](/images/SoDo.drawio.png) 

### Nội dung

 1. [Giới thiệu](1-introduce/)
 2. [Các bước chuẩn bị](2-Prerequiste/)
 3. [Tạo kết nối đến máy chủ EC2](3-Accessibilitytoinstance/)
 4. [Quản lý session logs](4-s3log/)
 5. [Port Forwarding](5-Portfwd/)
 6. [Dọn dẹp tài nguyên](6-cleanup/)
