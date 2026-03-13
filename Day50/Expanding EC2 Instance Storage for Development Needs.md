# AWS EBS Volume Expansion for EC2 Instance (xfusion-ec2)

## 📌 Project Overview

The Nautilus DevOps team identified that the EC2 instance **xfusion-ec2** was running out of storage space. The root volume attached to the instance had a capacity of **8 GiB**, which was insufficient for the growing development data.

To resolve this issue, the volume size was expanded to **12 GiB** and the root partition inside the instance was resized so the operating system could utilize the newly allocated storage **without downtime**.

---

## 🧰 Technologies Used

* **Amazon EC2**
* **Amazon EBS**
* **AWS Console / AWS CLI**
* **Linux File System Tools**

  * `lsblk`
  * `growpart`
  * `resize2fs` / `xfs_growfs`
* **SSH**

---

## 🎯 Objectives

1. Identify the **EBS volume** attached to the EC2 instance `xfusion-ec2`.
2. Expand the volume size from **8 GiB → 12 GiB**.
3. Reflect the updated size in the **root (`/`) partition** of the instance.
4. Ensure the additional storage is **immediately usable**.

---

## 🔐 Access Details

### AWS Console

* **Console URL:**
  https://621551581842.signin.aws.amazon.com/console?region=us-east-1

* **Username:**
  `kk_labs_user_400486`

* **Password:**
  `cxfF%G4lgL%t`

---

### Retrieve AWS Credentials

Run the following command on the **aws-client host**:

```bash
showcreds
```

---

### SSH Access to EC2 Instance

Use the provided key pair:

```bash
ssh -i /root/xfusion-keypair.pem ubuntu@<EC2-PUBLIC-IP>
```

---

# 🛠 Implementation Steps

## Step 1: Identify the Attached Volume

Navigate to:

```
AWS Console → EC2 → Instances
```

Locate the instance:

```
xfusion-ec2
```

Open the **Storage tab** to identify the attached **EBS volume ID**.

---

## Step 2: Expand the EBS Volume

Navigate to:

```
EC2 → Elastic Block Store → Volumes
```

1. Select the volume attached to `xfusion-ec2`.
2. Click **Actions → Modify Volume**.
3. Change size:

```
8 GiB → 12 GiB
```

4. Click **Modify → Confirm**.

AWS expands the volume **without stopping the instance**.

---

## Step 3: Verify Volume Expansion Inside EC2

SSH into the instance:

```bash
ssh -i /root/xfusion-keypair.pem ubuntu@<EC2-IP>
```

Check disk layout:

```bash
lsblk
```

Example output before resizing:

```
xvda    8G
└─xvda1 8G  /
```

---

## Step 4: Expand the Root Partition

Resize the partition:

```bash
sudo growpart /dev/xvda 1
```

---

## Step 5: Resize the Filesystem

### If filesystem is EXT4

```bash
sudo resize2fs /dev/xvda1
```

### If filesystem is XFS

```bash
sudo xfs_growfs /
```

---

## Step 6: Verify Updated Storage

Check disk space:

```bash
df -h
```

Expected output:

```
Filesystem      Size  Used Avail Use%
/dev/xvda1       12G
```

The root partition now reflects the **expanded storage capacity**.

---

# ✅ Result

* The EBS volume size was successfully increased from **8 GiB to 12 GiB**.
* The root filesystem was expanded **without downtime**.
* The additional storage is immediately available to the development team.

---

# 💡 Key Learning

This task demonstrates how **AWS EBS volumes can be resized dynamically** and highlights an important DevOps practice:

* **Elastic infrastructure scaling without service disruption**

Key commands used:

```bash
lsblk
growpart
resize2fs
xfs_growfs
df -h
```

---

# 🚀 Real-World DevOps Use Case

In production environments, disk expansion is commonly required when:

* Log files grow rapidly.
* Application data increases.
* Databases require additional storage.

AWS allows **live volume resizing**, enabling DevOps teams to scale storage **without stopping servers or impacting application availability**.

---
