---
title : "Giới thiệu"
date : 2025-06-06 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---
## Bối cảnh và Động lực

Trong thời đại dữ liệu bùng nổ, việc xử lý và phân tích dữ liệu một cách **hiệu quả**, **tự động**, và có **khả năng mở rộng linh hoạt** là yếu tố then chốt giúp các tổ chức hiện đại đưa ra quyết định kịp thời và tối ưu quy trình kinh doanh.

Đề tài **“Xây dựng Serverless Data Processing Pipeline với AWS Step Functions và Amazon EventBridge”** trình bày một giải pháp **xử lý dữ liệu không máy chủ (serverless)**, tận dụng sức mạnh của các dịch vụ **Amazon Web Services (AWS)** nhằm đáp ứng các yêu cầu về hiệu suất, độ linh hoạt và tối ưu chi phí.

Pipeline được thiết kế để **tự động xử lý các tệp CSV** được tải lên **Amazon S3**, sử dụng **Amazon EventBridge** để kích hoạt quy trình khi phát hiện tệp mới, và **AWS Step Functions** để điều phối từng bước xử lý — bao gồm kiểm tra định dạng, xử lý song song, tổng hợp kết quả và lưu trữ.  
Các tác vụ xử lý được thực hiện bởi các hàm **AWS Lambda**. Metadata được lưu vào **Amazon DynamoDB**, thông báo trạng thái được gửi qua **Amazon SNS**, và toàn bộ hệ thống được **giám sát bởi Amazon CloudWatch** để đảm bảo hiệu suất và phát hiện lỗi kịp thời.

---

## Ưu điểm của Giải pháp Serverless với Step Functions và EventBridge

Hệ thống sử dụng các dịch vụ chủ lực như **Amazon S3**, **EventBridge**, **Step Functions**, **Lambda**, **DynamoDB**, **CloudWatch**, và **SNS**, đem lại nhiều lợi ích nổi bật:

- **Tự động hóa hoàn toàn**:  
  Quy trình được kích hoạt và điều phối tự động dựa trên sự kiện.

- **Khả năng mở rộng linh hoạt**:  
  Tự động điều chỉnh theo khối lượng công việc, hỗ trợ xử lý song song.

- **Tối ưu chi phí vận hành**:  
  Mô hình trả phí theo mức sử dụng, không cần duy trì hạ tầng vật lý.

- **Xử lý lỗi thông minh**:  
  Logic retry tự động, thông báo lỗi qua SNS và theo dõi lỗi qua CloudWatch.

- **Linh hoạt & dễ bảo trì**:  
  Workflow dễ dàng cập nhật, tách biệt các chức năng bằng Lambda.

- **Giám sát hiệu quả**:  
  CloudWatch cung cấp log và cảnh báo theo thời gian thực.

- **Xác thực dữ liệu đầu vào**:  
  Đảm bảo dữ liệu hợp lệ trước khi xử lý.

- **Vận hành đơn giản**:  
  Dễ dàng quản lý vòng đời tài nguyên: tạo, cập nhật, xóa.

- **Phù hợp thực tiễn**:  
  Áp dụng tốt cho nhiều tình huống như ETL, phân tích dữ liệu theo lô, hệ thống phân tích sự kiện thời gian thực.
