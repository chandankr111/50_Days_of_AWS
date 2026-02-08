# Day 18 - Creating IAM Read-Only EC2 Policy (AWS Console)

## 📌 Objective

As part of IAM configuration, create a custom policy that provides **read-only access to Amazon EC2 resources**.

### Requirements

- Policy Name: `iampolicy_kareem`
- Region: us-east-1
- Access Type: Read-only
- Must allow users to:
  - View EC2 Instances
  - View AMIs
  - View Snapshots

---

## 🌍 AWS Environment Details

- Service Used: IAM (Identity and Access Management)
- Target Service: Amazon EC2
- Region: us-east-1

> Note: IAM is a global service, but the EC2 permissions apply within us-east-1 context.

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
3. Click **Policies**
4. Click **Create policy**

---

### Step 3: Configure Policy (JSON Editor)

1. Select **JSON** tab
2. Replace existing content with:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeInstances",
        "ec2:DescribeImages",
        "ec2:DescribeSnapshots",
        "ec2:DescribeVolumes",
        "ec2:DescribeTags"
      ],
      "Resource": "*"
    }
  ]
}
