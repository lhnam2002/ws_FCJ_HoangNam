+++
title = "Create DynamoDB Table"
date = 2022
weight = 6
chapter = false
pre = "<b>6. </b>"
+++

## Step 4: Create DynamoDB Table

Amazon DynamoDB is used to store metadata for each pipeline execution.

---

## 1. Access DynamoDB Console

- Go to AWS Console
- Search for **DynamoDB** → select **Tables** → click **Create table**

---

## 2. Configure the Table

- **Table name**: `ProcessingMetadata`
- **Partition key**: `ExecutionId` *(Type: String)*
- **Sort key**: *(leave empty)*
- **Table settings**: Select **On-demand (Pay-per-request)** for automatic scaling
- Click **Create table**

---

## 3. Verify the Table

- Go to the **Tables** section
- Ensure the `ProcessingMetadata` table has been created successfully
