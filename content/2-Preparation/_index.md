---
title : "Preparation"
date : 2025-06-06
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

## Preparation Steps

## 1. Create IAM Components

### Step 1: Create an IAM User Group

- Go to AWS Console: [https://console.aws.amazon.com/iam](https://console.aws.amazon.com/iam)
- Select **User groups** → **Create group**
![Connect](/ws_FCJ_HoangNam/images/2.preparation/bs1.png)
- Group name: `devGr`
- Attach permissions:
  - Choose the existing policy: `AdministratorAccess`
- Click **Create group**

![Connect](/ws_FCJ_HoangNam/images/2.preparation/bs2.png)
---

### Step 2: Create an IAM User

- Go to **Users** → click **Add users**

![Connect](/ws_FCJ_HoangNam/images/2.preparation/bs3.png)
- Enter user name: `dev-user`
- Choose Access Types:
  - **Programmatic access** *(for AWS CLI usage)*
  - **AWS Management Console access** *(for web console login)*
- Set a password or let AWS auto-generate one
- Add the user to group: `devGr`
- Click **Create user**
![Connect](/ws_FCJ_HoangNam/images/2.preparation/bs4.png)
![Connect](/ws_FCJ_HoangNam/images/2.preparation/bs5.png)
![Connect](/ws_FCJ_HoangNam/images/2.preparation/bs6.png)
---

### Step 3: Create an AWS Access Key

- In the user list, click on `dev-user`
- Navigate to the **Security credentials** tab
- Under **Access keys**, click **Create access key**
![Connect](/ws_FCJ_HoangNam/images/2.preparation/bs7.png)
- Choose usage purpose: **Command Line Interface (CLI)**
![Connect](/ws_FCJ_HoangNam/images/2.preparation/bs8.png)
![Connect](/ws_FCJ_HoangNam/images/2.preparation/bs9.png)
- Once created, you will receive:
  - `Access key ID`
  - `Secret access key`
- ⚠️ **Save it immediately**, the `Secret access key` is **shown only once**
![Connect](/ws_FCJ_HoangNam/images/2.preparation/bs10.png)
---

## 2. Install and Configure AWS CLI

### 🔹 Install AWS CLI on Windows

- Open the **Run** dialog (Windows + R), paste the following command:

```bash
msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi
```
- Verify the CLI Version
```bash
aws --version
```
- Configure AWS CLI
```bash
aws configure
```

- Provide the following information when prompted:

  -- Access Key ID

  -- Secret Access Key

  -- Region (ap-southeast-1)

  -- Output format (json)

![Connect](/ws_FCJ_HoangNam/images/2.preparation/bs11.png)

## 3. Create a Sample CSV File

### Create a CSV File Using Notepad

- Press Windows + R, type notepad, and hit Enter

- Paste the following content:
  ```txt
    id,name,amount
    1,John,100
    2,Jane,200
  ```
- Go to File > Save As

- File name: test.csv

- Save as type: All Files (*.*)

- Encoding: UTF-8

✅ Make sure the file is not saved as test.csv.txt


