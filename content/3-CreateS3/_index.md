---
title : "Create S3 Buckets"
date : 2025-06-06
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

## ☁️ Step 1: Create S3 Buckets

Amazon S3 is used to store **input** and **output** data for the serverless data processing pipeline.

---

## 1. Access the S3 Console

- Go to AWS Console: [https://console.aws.amazon.com/s3](https://console.aws.amazon.com/s3)
- In the search bar, type **S3** and select **Amazon S3**

![Connect](/images/3.createS3/B1_1.png)

---

## 2. Create Input Bucket

- Click **Create bucket**
- Configure the following:

  - **Bucket name**: `data-processing-input-123456789012`  
    *(replace `123456789012` with your actual AWS Account ID)*

  - **Region**: Select `Asia Pacific (Singapore)` - `ap-southeast-1`

  - **Object Ownership**: Choose `ACLs disabled`

  ![Connect](/images/3.createS3/B1_2_1.png)

  - **Block Public Access**: Leave default *(block all public access)*

  - **Encryption**: Enable  
    **Server-side encryption with Amazon S3-managed keys (SSE-S3)**

  ![Connect](/images/3.createS3/B1_2_2.png)

- Click **Create bucket**

  ![Connect](/images/3.createS3/B1_2_3.png)

⚠️ *If you receive the error "Bucket name already exists", add a suffix to make it unique, e.g.:*  
`data-processing-input-123456789012-v1`

---

## 3. Create Output Bucket

- Repeat the same steps as above
- Use the name: `data-processing-output-123456789012`  

---

## 4. Verify the Buckets

- Go to the **Buckets** list in the S3 console
- Confirm both buckets are created:
  - `data-processing-input-123456789012`
  - `data-processing-output-123456789012`

  ![Connect](/images/3.createS3/B1_3.png)

---

## Optional: Upload `test.csv` to Input Bucket

- Open the `data-processing-input-123456789012` bucket
![Connect](/images/3.createS3/B1_4.png)
- Click **Upload**
- Select the previously created `test.csv` file
![Connect](/images/3.createS3/B1_4_2.png)
- Click **Upload** to confirm
![Connect](/images/3.createS3/B1_4_3.png)

---
