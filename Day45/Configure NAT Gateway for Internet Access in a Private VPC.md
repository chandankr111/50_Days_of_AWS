# AWS NAT Gateway Setup for Private EC2 Internet Access

## Project Overview
This project demonstrates how to enable **internet access for an EC2 instance running in a private subnet** using a **NAT Gateway**. The private EC2 instance uploads a test file to an S3 bucket after outbound internet connectivity is established.

This setup reflects a **common production cloud architecture** where backend servers remain private but still access external services.

---

## Services Used

- AWS VPC
- Amazon EC2
- Amazon S3
- NAT Gateway
- Internet Gateway
- Route Tables
- Elastic IP

---

## Architecture

```
                Internet
                    │
            Internet Gateway
                    │
          Public Subnet (xfusion-pub-subnet)
                    │
                NAT Gateway
                    │
      Route Table (0.0.0.0/0 → NAT Gateway)
                    │
         Private Subnet (xfusion-priv-subnet)
                    │
           EC2 Instance (xfusion-priv-ec2)
                    │
                AWS S3 Bucket
```

The **NAT Gateway** allows the private EC2 instance to initiate outbound internet connections without exposing it to inbound internet traffic.

---

## Existing Infrastructure

The following resources were already present in the environment:

| Resource | Name |
|--------|------|
| VPC | xfusion-priv-vpc |
| Private Subnet | xfusion-priv-subnet |
| EC2 Instance | xfusion-priv-ec2 |
| S3 Bucket | xfusion-nat-9789 |

The EC2 instance had a **cron job configured** to upload a test file to the S3 bucket once internet connectivity became available.

---

## Resources Created

| Resource | Name |
|--------|------|
| Public Subnet | xfusion-pub-subnet |
| Internet Gateway | xfusion-igw |
| Public Route Table | xfusion-pub-rt |
| NAT Gateway | xfusion-natgw |
| Elastic IP | Allocated for NAT Gateway |

---

## Implementation Steps

### 1. Create Public Subnet

```
Name: xfusion-pub-subnet
CIDR: 10.0.2.0/24
VPC: xfusion-priv-vpc
```

---

### 2. Create Internet Gateway

```
Name: xfusion-igw
Attach to VPC: xfusion-priv-vpc
```

---

### 3. Create Public Route Table

```
Route Table Name: xfusion-pub-rt
```

Add Route:

```
Destination: 0.0.0.0/0
Target: Internet Gateway
```

Associate subnet:

```
xfusion-pub-subnet
```

---

### 4. Allocate Elastic IP

An Elastic IP was allocated to provide a **static public IP** for the NAT Gateway.

---

### 5. Create NAT Gateway

```
Name: xfusion-natgw
Subnet: xfusion-pub-subnet
Elastic IP: Allocated EIP
```

---

### 6. Update Private Route Table

Add the following route to the private subnet route table:

```
Destination: 0.0.0.0/0
Target: NAT Gateway
```

This allows private instances to access the internet via the NAT Gateway.

---

## Verification

After completing the configuration:

1. Wait for the NAT Gateway status to become **Available**.
2. Wait 2–3 minutes for the EC2 cron job to run.
3. Navigate to the S3 bucket:

```
xfusion-nat-9789
```

A **test file appears in the bucket**, confirming that the private EC2 instance successfully accessed the internet.

---

## Key DevOps Concepts Demonstrated

- AWS VPC Networking
- Public vs Private Subnets
- NAT Gateway Architecture
- Route Table Configuration
- Secure Internet Access for Private Instances
- Cloud Infrastructure Design

---

## Outcome

Successfully configured **outbound internet access for a private EC2 instance using a NAT Gateway**, allowing the instance to upload a test file to an S3 bucket without exposing it to the public internet.

---

## Author

Chandan Kumar  
DevOps & Cloud Enthusiast

---

## 📸 Portfolio Screenshots

![Screenshot 2026-03-11 183025](images/Screenshot%202026-03-11%20183025.png)
![Screenshot 2026-03-11 183037](images/Screenshot%202026-03-11%20183037.png)
![Screenshot 2026-03-11 183136](images/Screenshot%202026-03-11%20183136.png)
![Screenshot 2026-03-11 183202](images/Screenshot%202026-03-11%20183202.png)
![Screenshot 2026-03-11 183304](images/Screenshot%202026-03-11%20183304.png)
![Screenshot 2026-03-11 183814](images/Screenshot%202026-03-11%20183814.png)
![Screenshot 2026-03-11 183905](images/Screenshot%202026-03-11%20183905.png)
![Screenshot 2026-03-11 184024](images/Screenshot%202026-03-11%20184024.png)
![Screenshot 2026-03-11 184056](images/Screenshot%202026-03-11%20184056.png)
![Screenshot 2026-03-11 184442](images/Screenshot%202026-03-11%20184442.png)
![Screenshot 2026-03-11 184833](images/Screenshot%202026-03-11%20184833.png)
![Screenshot 2026-03-11 184942](images/Screenshot%202026-03-11%20184942.png)