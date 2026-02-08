# Day 13 - Creating AMI from Existing EC2 Instance (AWS Console)

## 📌 Objective
Create an Amazon Machine Image (AMI) from an existing EC2 instance named **`nautilus-ec2`**.

The AMI must:
- Be named **`nautilus-ec2-ami`**
- Reach the **Available** state successfully

---

## 🏗 Architecture Concept

This task demonstrates:
- Infrastructure backup strategy
- Immutable infrastructure concept
- Migration preparation step
- Instance snapshotting for scaling or recovery

AMI = Snapshot of EC2 instance (including OS, configuration, and attached volumes).

---

## 🚀 Steps Performed (Using AWS Console)

### Step 1: Login to AWS Console
- Navigate to **EC2 Dashboard**
- Ensure correct **Region** is selected

---

### Step 2: Locate EC2 Instance
- Go to **Instances**
- Search for: `nautilus-ec2`
- Select the instance

---

### Step 3: Create AMI
- Click **Actions**
- Select:  
  `Image and templates → Create image`
- Enter details:
  - **Image name:** `nautilus-ec2-ami`
- Click **Create image**

---

### Step 4: Verify AMI Creation
- Navigate to **AMIs**
- Set filter to **Owned by me**
- Search for: `nautilus-ec2-ami`
- Check **State**

Expected Result:

