---
title : "Tạo Lambda Funtion"
date : 2025-06-06 
weight : 5 
chapter : false
pre : " <b> 5. </b> "
---

## Bước 3: Tạo Lambda Functions

Tạo 4 hàm Lambda để xử lý các bước trong pipeline xử lý dữ liệu serverless.

---

## 1. Truy cập Lambda Console

- Vào AWS Console: [https://console.aws.amazon.com/lambda](https://console.aws.amazon.com/lambda)
- Tìm **Lambda** → chọn **Functions** → nhấn **Create function**

---

## 2. Tạo `ValidateDataFunction`

- **Function name**: `ValidateDataFunction`
- **Runtime**: Python 3.9 (hoặc mới hơn)
- **Architecture**: `x86_64`
- **Permissions**: Chọn **Use an existing role** → chọn `LambdaDataProcessingRole`
- Nhấn **Create function**

### ✅ Code (tab **Code > Code source**):

```python
import json
import boto3
import csv
import io

def lambda_handler(event, context):
    s3 = boto3.client('s3')
    try:
        bucket = event['Records'][0]['s3']['bucket']['name']
        key = event['Records'][0]['s3']['object']['key']
        response = s3.get_object(Bucket=bucket, Key=key)
        data = response['Body'].read().decode('utf-8')
        csv_reader = csv.reader(io.StringIO(data))
        headers = next(csv_reader, None)
        if not headers or len(headers) < 2:
            raise Exception("Invalid CSV: Missing or insufficient headers")
        first_row = next(csv_reader, None)
        if not first_row:
            raise Exception("Invalid CSV: No data rows")
        return {
            'statusCode': 200,
            'body': json.dumps({
                'bucket': bucket,
                'key': key,
                'valid': True,
                'headers': headers
            })
        }
    except Exception as e:
        raise Exception(f"Validation failed: {str(e)}")
```
- Nhấn Deploy

#### Configuration:
- Timeout: 30 giây
- Memory: 256 MB
- Environment variables:
- INPUT_BUCKET = data-processing-input-123456789012

## 3.Tạo `ProcessDataFunction`

- Tạo function tương tự, đặt tên: `ProcessDataFunction`

```python
import json

def lambda_handler(event, context):
    try:
        data = json.loads(event['body'])
        bucket = data['bucket']
        key = data['key']
        processed_data = {
            'result': f"Processed file {key} from {bucket}",
            'transformed': True
        }
        return {
            'statusCode': 200,
            'body': json.dumps(processed_data)
        }
    except Exception as e:
        raise Exception(f"Processing failed: {str(e)}")
```
- Nhấn Deploy

#### Configuration:
- Timeout: 30 giây
- Memory: 256 MB

## 4. Tạo `AggregateDataFunction`
- Tạo function tương tự, đặt tên: `AggregateDataFunction`

```py
import json

def lambda_handler(event, context):
    try:
        results = [json.loads(item['body']) for item in event]
        aggregated = {
            'aggregated_result': [r['result'] for r in results],
            'total_branches': len(results)
        }
        return {
            'statusCode': 200,
            'body': json.dumps(aggregated)
        }
    except Exception as e:
        raise Exception(f"Aggregation failed: {str(e)}")

```
- Nhấn Deploy

#### Configuration:
- Timeout: 30 giây
- Memory: 256 MB

## 5. Tạo `StoreResultsFunction`
Tạo function tương tự, đặt tên:`StoreResultsFunction`
```py
import json
import boto3
import datetime

def lambda_handler(event, context):
    s3 = boto3.client('s3')
    dynamodb = boto3.resource('dynamodb')
    table = dynamodb.Table('ProcessingMetadata')
    try:
        data = json.loads(event['body'])
        output_bucket = 'data-processing-output-123456789012'  # Thay bằng Bucket của bạn
        output_key = f"results/processed_{datetime.datetime.now().isoformat()}.json"
        s3.put_object(Bucket=output_bucket, Key=output_key, Body=json.dumps(data))
        table.put_item(Item={
            'ExecutionId': context.aws_request_id,
            'Timestamp': datetime.datetime.now().isoformat(),
            'OutputKey': output_key,
            'Status': 'Success'
        })
        return {
            'statusCode': 200,
            'body': json.dumps({'status': 'Stored successfully', 'output_key': output_key})
        }
    except Exception as e:
        raise Exception(f"Storage failed: {str(e)}")

```
- Nhấn Deploy
- Thay data-processing-output-123456789012 bằng tên output bucket thực tế của bạn

#### Configuration:
- Timeout: 1 phút
- Memory: 512 MB

## 6. Kiểm Tra Lambda Functions
- Vào từng Lambda function → chọn tab Test → nhấn Create test event
- Dán nội dung JSON sau:
```json
{
  "Records": [
    {
      "s3": {
        "bucket": {"name": "data-processing-input-123456789012"},
        "object": {"key": "test.csv"}
      }
    }
  ]
}
```
- Nhấn Test, sau đó kiểm tra log ở tab Monitor > Logs
