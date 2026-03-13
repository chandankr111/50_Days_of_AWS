# AWS Log Aggregation Architecture (VPC Peering + S3 + Cron Jobs)

## Project Overview

This project demonstrates a **secure and scalable log aggregation architecture on AWS**. The goal was to collect logs from a **private EC2 instance inside a private VPC**, securely transfer them to a **public EC2 instance**, and finally store them in a **private S3 bucket**.

The infrastructure was designed to simulate a **real-world DevOps logging pipeline** using networking, IAM, and automation.

---

## Architecture

Private EC2 → Public EC2 → Amazon S3

* Logs are generated in the **private EC2 instance**
* Logs are transferred securely to a **public EC2 instance using SCP**
* The public EC2 pushes logs to **Amazon S3**
* Automation is implemented using **Cron Jobs**

---

## AWS Services Used

* Amazon VPC
* EC2
* S3
* IAM Roles
* VPC Peering
* Internet Gateway
* Route Tables
* Cron Jobs
* SCP / Secure File Transfer

---

## Infrastructure Setup

### 1. Private Environment (Pre-existing)

The following resources already existed:

* **VPC:** `devops-priv-vpc`
* **Subnet:** `devops-priv-subnet`
* **Route Table:** `devops-priv-rt`
* **EC2 Instance:** `devops-priv-ec2`
* **Key Pair:** `devops-key.pem`

This EC2 instance is used to generate and send logs.

---

### 2. Public Environment Setup

A new public infrastructure was created:

**VPC**

```
devops-pub-vpc
```

**Subnet**

```
devops-pub-subnet
```

**Route Table**

```
devops-pub-rt
```

**Internet Gateway**
Attached to the public VPC to enable internet connectivity.

---

### 3. Public EC2 Instance

A new EC2 instance was launched:

```
Name: devops-pub-ec2
Type: t2.micro
Subnet: devops-pub-subnet
Key Pair: devops-key
```

This instance receives logs from the private EC2 and uploads them to S3.

---

### 4. S3 Bucket Creation

A secure private S3 bucket was created:

```
devops-s3-logs-3398
```

Logs are stored inside the following path:

```
devops-priv-vpc/boot/boots.log
```

---

### 5. IAM Role Configuration

An IAM Role was created:

```
devops-s3-role
```

Permissions:

```
s3:PutObject
```

The role was attached to the **public EC2 instance** to allow it to upload logs to S3 securely without storing credentials.

---

### 6. VPC Peering

A **VPC Peering connection** was established:

```
devops-vpc-peering
```

This allows secure communication between:

```
devops-priv-vpc ↔ devops-pub-vpc
```

---

### 7. Route Table Updates

Both route tables were updated to allow communication through the peering connection.

| Route Table    | Destination      | Target      |
| -------------- | ---------------- | ----------- |
| devops-priv-rt | Public VPC CIDR  | VPC Peering |
| devops-pub-rt  | Private VPC CIDR | VPC Peering |

---

## Log Automation

### Private EC2 Cron Job

The private instance sends logs to the public instance.

```
/var/log/boots.log
```

Cron job example:

```
*/5 * * * * scp /var/log/boots.log ubuntu@PUBLIC-EC2-IP:/tmp/boots.log
```

---

### Public EC2 Cron Job

The public instance uploads logs to S3.

```
*/5 * * * * aws s3 cp /tmp/boots.log s3://devops-s3-logs-3398/devops-priv-vpc/boot/boots.log
```

---

## Log Flow

```
Private EC2
   │
   │ SCP Transfer
   ▼
Public EC2
   │
   │ AWS CLI Upload
   ▼
Amazon S3
(devops-s3-logs-3398/devops-priv-vpc/boot/boots.log)
```

---

## Key DevOps Concepts Demonstrated

* Secure network communication using **VPC Peering**
* Automated log transfer using **Cron Jobs**
* Secure file transfer using **SCP**
* IAM best practices using **Instance Roles**
* Cloud storage using **Amazon S3**
* AWS networking configuration

---

## Screenshots

Add screenshots of:

* VPC configuration
* VPC Peering
* EC2 instances
* S3 bucket
* Cron jobs
* Successful log upload

---

## Learning Outcome

Through this project, I gained practical experience with:

* AWS networking architecture
* Secure instance communication
* IAM role-based access
* Automated log pipelines
* Production-style DevOps infrastructure setup

---

## Author


**Chandan Kumar**

DevOps | Cloud | AWS | Linux

---

## Portfolio Screenshots

Below are the screenshots for Day 49, in order:

![Screenshot 2026-03-13 134916](images/Screenshot%202026-03-13%20134916.png)
![Screenshot 2026-03-13 135032](images/Screenshot%202026-03-13%20135032.png)
![Screenshot 2026-03-13 135046](images/Screenshot%202026-03-13%20135046.png)
![Screenshot 2026-03-13 135119](images/Screenshot%202026-03-13%20135119.png)
![Screenshot 2026-03-13 135206](images/Screenshot%202026-03-13%20135206.png)
![Screenshot 2026-03-13 135248](images/Screenshot%202026-03-13%20135248.png)
![Screenshot 2026-03-13 135524](images/Screenshot%202026-03-13%20135524.png)
![Screenshot 2026-03-13 135626](images/Screenshot%202026-03-13%20135626.png)
![Screenshot 2026-03-13 135737](images/Screenshot%202026-03-13%20135737.png)
![Screenshot 2026-03-13 140154](images/Screenshot%202026-03-13%20140154.png)
![Screenshot 2026-03-13 140212](images/Screenshot%202026-03-13%20140212.png)
![Screenshot 2026-03-13 140330](images/Screenshot%202026-03-13%20140330.png)
![Screenshot 2026-03-13 142302](images/Screenshot%202026-03-13%20142302.png)
![Screenshot 2026-03-13 142412](images/Screenshot%202026-03-13%20142412.png)
![Screenshot 2026-03-13 142616](images/Screenshot%202026-03-13%20142616.png)
![Screenshot 2026-03-13 142905](images/Screenshot%202026-03-13%20142905.png)
![Screenshot 2026-03-13 143119](images/Screenshot%202026-03-13%20143119.png)
![Screenshot 2026-03-13 143639](images/Screenshot%202026-03-13%20143639.png)
![Screenshot 2026-03-13 143803](images/Screenshot%202026-03-13%20143803.png)
![Screenshot 2026-03-13 144351](images/Screenshot%202026-03-13%20144351.png)
![Screenshot 2026-03-13 144642](images/Screenshot%202026-03-13%20144642.png)
![Screenshot 2026-03-13 144741](images/Screenshot%202026-03-13%20144741.png)
![Screenshot 2026-03-13 145004](images/Screenshot%202026-03-13%20145004.png)
![Screenshot 2026-03-13 145233](images/Screenshot%202026-03-13%20145233.png)
![Screenshot 2026-03-13 161118](images/Screenshot%202026-03-13%20161118.png)
![Screenshot 2026-03-13 161255](images/Screenshot%202026-03-13%20161255.png)
