# 🚀 Establishing Secure Communication Between Public and Private VPCs via VPC Peering

---

## 📌 Project Overview
This project demonstrates the manual configuration of **AWS VPC Peering**. We establish a secure network bridge between a **Public EC2 Instance** (Default VPC) and a **Private EC2 Instance** (Custom VPC) to allow internal traffic without exposing the private instance to the open internet.

---

## 🏗️ Architecture Summary
* **Public EC2:** Default VPC (`172.31.0.0/16`)
* **Private EC2:** Custom VPC (`10.1.0.0/16`)
* **Connectivity:** VPC Peering Connection (`pcx-xxxx`)
* **Verification:** ICMP (Ping) and SSH Connectivity tests.

---

## 🛠️ Step-by-Step Implementation

### 1. VPC and Subnet Infrastructure
We start by defining the custom VPC and the private subnet where our isolated workload will reside.

![VPC Setup Initial](./images/Screenshot%202026-02-18%20213132.png)
![Subnet Configuration](./images/Screenshot%202026-02-18%20213429.png)

### 2. Creating the VPC Peering Connection
The Peering connection acts as the "bridge." We initiate the request from the Default VPC and accept it within the same account for the Custom VPC.

![Creating Peering Connection](./images/Screenshot%202026-02-18%20213521.png)
![Peering Request Status](./images/Screenshot%202026-02-18%20213628.png)
![Accepting the Peering Request](./images/Screenshot%202026-02-18%20213809.png)

### 3. Route Table Updates
Communication won't happen unless the routers know where to send the traffic. We add routes in both directions pointing to the Peering ID.

![Default VPC Route Table Update](./images/Screenshot%202026-02-18%20213818.png)
![Private VPC Route Table Update](./images/Screenshot%202026-02-18%20213922.png)

### 4. Provisioning EC2 Instances
We launch a Public EC2 (with a Public IP) to act as our jump host and a Private EC2 (Internal IP only) to be our secure target.

![Launching Public EC2](./images/Screenshot%202026-02-18%20214206.png)
![Launching Private EC2](./images/Screenshot%202026-02-18%20214236.png)
![Instance Dashboard Overview](./images/Screenshot%202026-02-18%20214643.png)

### 5. Security Group Configuration
To allow the "Ping" command and SSH traffic, we modify the Security Groups to permit ICMP and Port 22 from the opposing VPC's CIDR block.

![Security Group Inbound Rules](./images/Screenshot%202026-02-18%20215224.png)
![Security Group Verification](./images/Screenshot%202026-02-18%20220615.png)

### 6. Final Connectivity Testing
The final step is to SSH into the Public EC2 and attempt to ping the Private EC2 via its internal IP address.

![SSH Connectivity Test](./images/Screenshot%202026-02-18%20220944.png)
![Successful Ping via Peering](./images/Screenshot%202026-02-18%20221437.png)

---

## 🔑 Key Commands Used

### Check Available Keys
```bash
ls -l /root/.ssh