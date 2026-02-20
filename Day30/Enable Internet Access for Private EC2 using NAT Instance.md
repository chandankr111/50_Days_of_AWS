# NAT Instance Setup for Private EC2 Internet Access

![Architecture Diagram](images/image.png)

## 📌 Problem Statement

The Nautilus DevOps team needs to enable internet access for an EC2 instance running in a private subnet.  
The private EC2 instance must upload a test file (`xfusion-test.txt`) to a public S3 bucket (`xfusion-nat-14836`).

To minimize costs, a **NAT Instance** is used instead of a NAT Gateway.

---

## 🏗️ Existing Infrastructure

- VPC: `xfusion-priv-vpc`
- Private Subnet: `xfusion-priv-subnet`
- Private EC2: `xfusion-priv-ec2`
- S3 Bucket: `xfusion-nat-14836`
- Cron job on private EC2 uploads file every minute

---

## 🎯 Objective

Enable internet access for the private EC2 instance using a NAT Instance so that `xfusion-test.txt` appears in the S3 bucket.

---

## 🧩 Architecture Overview

```
AWS Cloud
│
├── VPC (xfusion-priv-vpc)
│   │
│   ├── Public Subnet (xfusion-pub-subnet)
│   │   └── NAT Instance (xfusion-nat-instance)
│   │
│   └── Private Subnet (xfusion-priv-subnet)
│       └── EC2 (xfusion-priv-ec2)
│
└── Amazon S3 (xfusion-nat-14836)
```

### Traffic Flow

```
Private EC2
→ NAT Instance
→ Internet Gateway
→ Internet
→ Amazon S3
```

---

## 🚀 Implementation Steps

### 1️⃣ Create Public Subnet

- Name: `xfusion-pub-subnet`
- VPC: `xfusion-priv-vpc`
- CIDR: Non-overlapping range (e.g., `10.0.2.0/24`)

---

### 2️⃣ Create and Attach Internet Gateway

- Create IGW: `xfusion-igw`
- Attach to: `xfusion-priv-vpc`

---

### 3️⃣ Configure Public Route Table

Add route:

| Destination | Target |
|-------------|--------|
| `0.0.0.0/0` | Internet Gateway |

Associate with: `xfusion-pub-subnet`

---

### 4️⃣ Launch NAT Instance

- Name: `xfusion-nat-instance`
- AMI: Amazon Linux 2
- Subnet: `xfusion-pub-subnet`
- Auto-assign Public IP: **Enabled**
- Security Group: Custom (see Step 5)

---

### 5️⃣ Configure Security Group for NAT

#### Inbound Rules

| Type | Source |
|------|--------|
| SSH (22) | My IP |
| All Traffic | VPC CIDR (e.g., `10.0.0.0/16`) |

#### Outbound Rules

| Type | Destination |
|------|-------------|
| All Traffic | `0.0.0.0/0` |

---

### 6️⃣ Disable Source/Destination Check

Navigate to: **EC2 → Actions → Networking → Change Source/Destination Check**  
Set to: **Disabled**

---

### 7️⃣ Configure NAT Instance

SSH into the NAT instance and run the following commands:

```bash
# Enable IP forwarding
sudo sysctl -w net.ipv4.ip_forward=1

# Add NAT masquerade rule
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

# Install and persist iptables rules
sudo yum install iptables-services -y
sudo service iptables save
```

---

### 8️⃣ Update Private Subnet Route Table

Add a route in the **private subnet's route table** to direct internet-bound traffic through the NAT instance:

| Destination | Target |
|-------------|--------|
| `0.0.0.0/0` | NAT Instance (instance ID) |

---

### 9️⃣ Verify S3 Upload

Wait for the cron job to run (fires every minute) and verify that `xfusion-test.txt` appears in the S3 bucket `xfusion-nat-14836`.

---

## ✅ Result

The private EC2 instance can now reach the internet through the NAT Instance, and `xfusion-test.txt` is successfully uploaded to the S3 bucket.

---

## 🖼️ Screenshots

![](images/Screenshot%202026-02-20%20235435.png)

![](images/Screenshot%202026-02-20%20235453.png)

![](images/Screenshot%202026-02-20%20235514.png)

![](images/Screenshot%202026-02-20%20235528.png)

![](images/Screenshot%202026-02-20%20235558.png)

![](images/Screenshot%202026-02-20%20235613.png)

![](images/Screenshot%202026-02-20%20235624.png)

![](images/Screenshot%202026-02-20%20235638.png)

![](images/Screenshot%202026-02-20%20235811.png)

![](images/Screenshot%202026-02-20%20235836.png)

---

## 📝 Notes

- A **NAT Instance** is more cost-effective than a NAT Gateway but requires manual configuration and does not scale automatically.
- Remember to **disable source/destination check** on the NAT instance — this is essential for traffic forwarding to work.
- IP forwarding and iptables rules must be configured on the NAT instance OS level.
- For production workloads, consider using a **NAT Gateway** for higher availability and managed scaling.