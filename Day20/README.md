# Day 20 - Creating IAM Role for EC2 and Attaching Policy (AWS Console)

## 📌 Objective

Create an IAM Role for EC2 and attach an existing policy to it.

### Requirements

- Role Name: `iamrole_james`
- Trusted Entity Type: AWS Service
- Use Case: EC2
- Attach Policy: `iampolicy_james`
- Region Context: us-east-1

---

## 🌍 AWS Environment Details

- Service Used: IAM (Identity and Access Management)
- Trusted Service: Amazon EC2
- Region Context: us-east-1

> Note: IAM is a global service, but always verify region context before performing tasks.

---

## 🚀 Implementation Steps (Using AWS Console)

### Step 1: Login to AWS Console

- Open provided Console URL
- Login using given credentials
- Ensure region is set to **us-east-1**

---

### Step 2: Navigate to IAM

1. Search for **IAM**
2. Open **IAM Dashboard**
3. Click **Roles**
4. Click **Create role**

---

### Step 3: Select Trusted Entity

1. Under **Trusted entity type**, select:
