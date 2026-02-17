# 🚀 Day 27 – AWS Public VPC Setup
## 27 Days of Claude on KodeKloud

![Day 27 - Public VPC setup](./image/Screenshot%202026-02-17%20154548.png)

---

## 📌 Lab Objective

The Nautilus DevOps Team requested a new **public VPC** to support public-facing applications.

The objective was to:

- Create a custom VPC
- Create a public subnet
- Enable automatic public IP assignment
- Attach an Internet Gateway
- Configure routing for internet access
- Create a Security Group allowing SSH
- Launch a public EC2 instance (t2.micro)
- Ensure SSH accessibility from the internet

Region used: **us-east-1**

---

## 🏗 Architecture Overview

                 Internet
                     │
            ┌────────────────┐
            │ Internet Gateway│
            └────────────────┘
                     │
             Route Table
       (0.0.0.0/0 → IGW)
                     │
            Public Subnet
            (10.0.1.0/24)
                     │
            EC2 Instance
           (t2.micro | SSH)

---

## 🛠 Step 1: Create VPC

- Name: `nautilus-pub-vpc`
- CIDR Block: `10.0.0.0/16`
- Tenancy: Default
- DNS Resolution: Enabled
- DNS Hostnames: Enabled

This creates the virtual private network for hosting resources.

---

## 🌐 Step 2: Create Public Subnet

- Name: `nautilus-pub-subnet`
- CIDR Block: `10.0.1.0/24`
- Availability Zone: `us-east-1a`
- Auto-assign Public IPv4: ✅ Enabled

Enabling auto-assign public IP ensures that EC2 instances launched here automatically receive a public IP.

---

## 🌍 Step 3: Create Internet Gateway

- Name: `nautilus-igw`
- Attached to: `nautilus-pub-vpc`

Internet Gateway enables communication between the VPC and the internet.

---

## 🛣 Step 4: Configure Route Table

- Name: `nautilus-rt`
- Associated Subnet: `nautilus-pub-subnet`

### Routes Configured:

| Destination   | Target              |
|--------------|--------------------|
| 10.0.0.0/16  | local              |
| 0.0.0.0/0    | Internet Gateway   |

The `0.0.0.0/0` route makes the subnet public.

---

## 🔐 Step 5: Create Security Group

- Name: `nautilus-sg`
- VPC: `nautilus-pub-vpc`

### Inbound Rules:

| Type | Port | Source |
|------|------|--------|
| SSH  | 22   | 0.0.0.0/0 |

Outbound: Default (Allow all traffic)

This allows SSH access from anywhere.

---

## 💻 Step 6: Launch EC2 Instance

- Name: `nautilus-pub-ec2`
- AMI: Amazon Linux 2
- Instance Type: `t2.micro`
- VPC: `nautilus-pub-vpc`
- Subnet: `nautilus-pub-subnet`
- Auto Public IP: Enabled
- Security Group: `nautilus-sg`
- Key Pair: Created and downloaded

---

## ✅ Verification Checklist

✔ Instance State: Running  
✔ Status Checks: 2/2 Passed  
✔ Public IPv4 Address Assigned  
✔ Security Group Allows SSH  
✔ Route Table Has Internet Route  
✔ Internet Gateway Attached  
✔ DNS Hostnames Enabled  

---

## 🔎 SSH Test

```bash
ssh -i testing.pem ec2-user@<public-ip>
```

Expected:

- Successful SSH connection to the EC2 instance (no timeout).

---

## 📸 Screenshots (Start → End)

Images added from the `image` folder in **starting to end** order:

![Screenshot 1](./image/Screenshot%202026-02-17%20154548.png)
![Screenshot 2](./image/Screenshot%202026-02-17%20154700.png)
![Screenshot 3](./image/Screenshot%202026-02-17%20154816.png)
![Screenshot 4](./image/Screenshot%202026-02-17%20154930.png)
![Screenshot 5](./image/Screenshot%202026-02-17%20154949.png)
![Screenshot 6](./image/Screenshot%202026-02-17%20155151.png)
![Screenshot 7](./image/Screenshot%202026-02-17%20155227.png)
![Screenshot 8](./image/Screenshot%202026-02-17%20155522.png)
![Screenshot 9](./image/Screenshot%202026-02-17%20160026.png)
![Screenshot 10](./image/Screenshot%202026-02-17%20160343.png)
![Screenshot 11](./image/Screenshot%202026-02-17%20161312.png)
![Screenshot 12](./image/Screenshot%202026-02-17%20161733.png)
