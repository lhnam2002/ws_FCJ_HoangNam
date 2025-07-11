---
title : "Create Lambda Funtion"
date : 2025-06-06 
weight : 5 
chapter : false
pre : " <b> 5. </b> "
---

## Step 3: Create Lambda Functions

Create 4 Lambda functions to handle each step of the serverless data processing pipeline.

---

## 1. Access the Lambda Console

- Open AWS Console: [https://console.aws.amazon.com/lambda](https://console.aws.amazon.com/lambda)
- Search for **Lambda**, go to **Functions**, and click **Create function**

---

## 2. Create `ValidateDataFunction`

- **Function name**: `ValidateDataFunction`
- **Runtime**: Python 3.9 (or newer)
- **Architecture**: `x86_64`
- **Permissions**: Choose **Use an existing role** → select `LambdaDataProcessingRole`
- Click **Create function**

### ✅ Code (Tab: **Code > Code source**)

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
- Click Deploy

#### Configuration
- Timeout: 30 seconds
- Memory: 256 MB
- Environment variable:
- INPUT_BUCKET = data-processing-input-123456789012

## 3. Create `ProcessDataFunction`

- Repeat creation, name the function: `ProcessDataFunction`
```py
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
- Click Deploy

#### Configuration
- Timeout: 30 seconds
- Memory: 256 MB

## 4. Create `AggregateDataFunction`
- Repeat creation, name the function: `AggregateDataFunction`
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
- Click Deploy

#### Configuration
- Timeout: 30 seconds
- Memory: 256 MB

## 5. Create `StoreResultsFunction`
- Repeat creation, name the function:  `StoreResultsFunction`
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
        output_bucket = 'data-processing-output-123456789012'  # Replace with your actual bucket
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

- Replace data-processing-output-123456789012 with your actual S3 bucket name

#### Configuration
- Timeout: 1 minute
- Memory: 512 MB

## 6. Test Each Lambda Function
- In each Lambda function → go to the Test tab → click Create test event

- Use the following test payload:

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
- Click Test

- Check logs in Monitor > Logs




