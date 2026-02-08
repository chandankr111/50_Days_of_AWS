# Day 14 - Terminating an EC2 Instance (AWS Console)

## 📌 Objective

During the cloud migration process, some resources became obsolete.  
The task is to delete an EC2 instance that is no longer in use.

### Requirements:

1. Delete the EC2 instance named **`xfusion-ec2`**
2. Region: **us-east-1**
3. Ensure the instance reaches **Terminated** state before submission

---

## 🌎 AWS Environment Details

- **Region:** us-east-1
- Service: Amazon EC2
- Target Instance: `xfusion-ec2`

---

## 🚀 Steps Performed (Using AWS Console)

### Step 1: Login to AWS Console

- Open the provided console URL
- Login using given credentials
- Confirm region is set to **us-east-1**

⚠️ Always verify region before performing operations.

---

### Step 2: Locate the EC2 Instance

- Navigate to **EC2 Dashboard**
- Click on **Instances**
- Search for: `xfusion-ec2`
- Select the instance

---

### Step 3: Terminate the Instance

- Click **Instance state**
- Select **Terminate instance**
- Confirm termination

This action permanently deletes the EC2 instance.

---

### Step 4: Verify Terminated State

- Refresh the Instances page
- Ensure filter includes:
  - `All states`
- Confirm the instance state shows:

