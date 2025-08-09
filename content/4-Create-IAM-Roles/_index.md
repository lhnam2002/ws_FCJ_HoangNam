---
title : "Create IAM Roles"
date : 2025-06-06 
weight : 4 
chapter : false
pre : " <b> 4. </b> "
---

## Step 2: Create IAM Roles

IAM Roles grant permissions to AWS services such as **Lambda** and **Step Functions** to access other AWS resources required during the data processing workflow.

---

## 1. Access the IAM Console

- Go to AWS Console: [https://console.aws.amazon.com/iam](https://console.aws.amazon.com/iam)
![Connect](/ws_FCJ_HoangNam/images/4.createIAMRole/B2_1.png)
- Search for **IAM** → go to **Roles** → click **Create role**
![Connect](/ws_FCJ_HoangNam/images/4.createIAMRole/B2_2.png)
---

## 2. Create Role for Lambda

- **Trusted entity type**: Select `AWS service` → choose `Lambda`
- Click **Next**
![Connect](/ws_FCJ_HoangNam/images/4.createIAMRole/B2_2_2.png)

### Attach Permissions:

- Click **Add permissions**
- Search and attach the following policies:

  - `AWSLambdaBasicExecutionRole` *(for writing logs to CloudWatch)*
  - `AmazonS3FullAccess` *(to access S3 buckets)*
  - `AmazonDynamoDBFullAccess` *(to access DynamoDB)*
  - `AmazonSNSFullAccess` *(to send notifications via SNS)*

> 🔒 **Security Note**: For production environments, consider creating **custom policies** with fine-grained access to only the necessary resources.

- Click **Next**
- Role name: `LambdaDataProcessingRole`
- Click **Create role**
![Connect](/ws_FCJ_HoangNam/images/4.createIAMRole/B2_2_3.png)

---

## 3. Create Role for Step Functions

- Return to IAM > Roles → click **Create role**
- **Trusted entity type**: Select `AWS service` → choose `Step Functions`
- Click **Next**
![Connect](/ws_FCJ_HoangNam/images/4.createIAMRole/B2_3_1.png)

### Attach Permissions:

- Attach the following policies:

  - `AWSLambdaFullAccess` *(allows Step Functions to invoke Lambda functions)*
  - `AmazonS3FullAccess`
  - `AmazonDynamoDBFullAccess`
  - `CloudWatchLogsFullAccess`
  - `AmazonSNSFullAccess`

- Click **Next**
- Role name: `StepFunctionsDataProcessingRole`
- Click **Create role**

---

## 4. Verify Roles

- Go to **IAM > Roles**
- Confirm that both roles are created:

  - `LambdaDataProcessingRole`
  - `StepFunctionsDataProcessingRole`
  ![Connect](/ws_FCJ_HoangNam/images/4.createIAMRole/B2_3_2.png)
