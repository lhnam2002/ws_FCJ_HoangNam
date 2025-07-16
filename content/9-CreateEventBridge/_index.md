---
title : "Create EventBridge Rule"
date : 2025-06-06 
weight : 9 
chapter : false
pre : " <b> 9. </b> "
---

## Step 7: Create EventBridge Rule

Amazon EventBridge will listen for S3 events and trigger the Step Functions workflow when a new file is uploaded.

---

## 1. Access the EventBridge Console

- Go to AWS Console
- Search for **EventBridge**
- Select **Rules** → click **Create rule**

---

## 2. Configure the Rule

- **Name**: `S3TriggerRule`
- **Event source**: select **Event Pattern**
- **Event pattern** (JSON):

```json
{
  "source": ["aws.s3"],
  "detail-type": ["Object Created"],
  "detail": {
    "bucket": {
      "name": ["data-processing-input-123456789012"]
    },
    "object": {
      "key": [{"suffix": ".csv"}]
    }
  }
}

```
- Note: Replace data-processing-input-123456789012 with your actual S3 bucket name.

## 3. Set Target
- Target type: Select Step Functions state machine
- State machine: choose DataProcessingWorkflow
- Execution role: select Create a new role for this specific resource
-Click Create

## 4. Verify the Rule
- Return to the Rules section
- Ensure the S3TriggerRule has been created and is Enabled

