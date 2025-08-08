---
title : "Create SNS Topic"
date : 2025-06-06 
weight : 7 
chapter : false
pre : " <b> 7. </b> "
---

# 📬 Step 5: Create SNS Topic

Amazon SNS will be used to send pipeline status notifications via email.

---

## 1. Access SNS Console

- Go to AWS Console
- Search for **SNS**
![Connect](/ws_FCJ_HoangNam/images/7.createSNSTopic/B5.png)
- Choose **Topics** → click **Create topic**
![Connect](/ws_FCJ_HoangNam/images/7.createSNSTopic/B5_1.png)

---

## 2. Configure the Topic

- **Type**: `Standard`
- **Name**: `PipelineNotifications`
- Click **Create topic**
![Connect](/ws_FCJ_HoangNam/images/7.createSNSTopic/B5_2.png)

---

## 3. Create Subscription

- Inside the `PipelineNotifications` topic, click **Create subscription**
- **Protocol**: `Email`
- **Endpoint**: Enter your email address
- Click **Create subscription**
- Check your email and **confirm the subscription** via the link provided
![Connect](/ws_FCJ_HoangNam/images/7.createSNSTopic/B5_3.png)

---

## 4. Get the Topic ARN

- Go back to the **Topics** page
- Click on `PipelineNotifications`
- Copy the **Topic ARN**, e.g.: (arn:aws:sns:ap-southeast-1:123456789012:PipelineNotifications)
![Connect](/ws_FCJ_HoangNam/images/7.createSNSTopic/B5_3_1.png)

