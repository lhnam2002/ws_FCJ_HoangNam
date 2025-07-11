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
- Choose **Topics** → click **Create topic**

---

## 2. Configure the Topic

- **Type**: `Standard`
- **Name**: `PipelineNotifications`
- Click **Create topic**

---

## 3. Create Subscription

- Inside the `PipelineNotifications` topic, click **Create subscription**
- **Protocol**: `Email`
- **Endpoint**: Enter your email address
- Click **Create subscription**
- Check your email and **confirm the subscription** via the link provided

---

## 4. Get the Topic ARN

- Go back to the **Topics** page
- Click on `PipelineNotifications`
- Copy the **Topic ARN**, e.g.: (arn:aws:sns:ap-southeast-1:123456789012:PipelineNotifications)

