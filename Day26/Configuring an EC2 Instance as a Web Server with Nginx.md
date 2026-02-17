## Day 26 – Configuring an EC2 Instance as a Web Server with Nginx

This project shows how I configured an **EC2 instance** as a public-facing **Nginx web server** using an automated **User Data** script.  
It is part of my **“50 Days of AWS”** series and demonstrates practical skills in EC2 provisioning, bootstrapping, and basic web server setup.

---

### 1. Project Overview

- **Task**: Configure and deploy a web server for the Nautilus application.  
- **Role**: DevOps Engineer  
- **Instance Name**: `datacenter-ec2`  
- **Goal**: Automatically install and start Nginx on first boot and expose it over HTTP.

---

### 2. Infrastructure Specifications

| Component          | Specification                    |
| :---------------- | :-------------------------------- |
| **Instance Name** | `datacenter-ec2`                 |
| **AMI (OS)**      | Ubuntu (critical requirement)    |
| **Web Server**    | Nginx                            |
| **Port Open**     | Port 80 (HTTP)                   |
| **Region**        | `us-east-1`                      |

---

### 3. Deployment Steps

#### Step 1 – Launch EC2 Instance with Ubuntu

1. Navigate to **EC2 → Launch Instances**.  
2. Set the **Name** to `datacenter-ec2`.  
3. Choose an **Ubuntu Server** AMI (latest LTS).  
4. Select an appropriate instance type (e.g., `t2.micro`).  

**Screenshot – Launch configuration summary**  

![Launch configuration summary](./images/Screenshot%202026-02-11%20161509.png)

---

#### Step 2 – Configure User Data to Install Nginx

In **Advanced details → User data**, I used the following script so that Nginx is installed and started automatically when the instance boots:

```bash
#!/bin/bash
sudo apt update -y
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```

**Screenshot – User Data script configuration**  

![User data script configuration](./images/Screenshot%202026-02-11%20161351.png)

---

#### Step 3 – Configure Security Group for HTTP Access

To make the Nginx web server reachable from the internet, I configured the instance security group to allow inbound HTTP traffic:

- **Type**: HTTP  
- **Port**: `80`  
- **Source**: `0.0.0.0/0` (for demo; in production, restrict further)  

**Screenshot – Security group inbound rules**  

![Security group inbound rules](./images/Screenshot%202026-02-11%20161138.png)

---

#### Step 4 – Verify Nginx is Running

After the instance reached the **running** state, I:

1. Copied the **Public IPv4 address** of the instance.  
2. Opened it in a browser using `http://<PUBLIC-IP>`.  
3. Verified that the default **“Welcome to nginx!”** page was displayed.  

**Screenshot – Nginx default page in browser**  

![Nginx default welcome page](./images/Screenshot%202026-02-11%20160718.png)

---

#### Step 5 – Console Overview

The AWS console shows the instance as running and reachable, with networking and security correctly configured for web access.

**Screenshot – EC2 instance overview**  

![EC2 instance overview](./images/Screenshot%202026-02-11%20155235.png)

---

### 4. Final Validation Checklist

- [x] EC2 instance `datacenter-ec2` launched in `us-east-1`.  
- [x] Ubuntu AMI selected as required.  
- [x] User Data script installs and starts Nginx on boot.  
- [x] Security group allows HTTP (port 80) from the internet.  
- [x] Nginx welcome page accessible via public IP.  

---

### 5. DevOps Learning Outcome

- Automated server provisioning using **EC2 User Data**.  
- Basic **Nginx web server** setup on Ubuntu.  
- Securely exposing a web server over **port 80** using security groups.  
- Translating a simple requirement into a repeatable infrastructure pattern suitable for real projects.  

This day strengthens my foundation for future tasks like using **ALBs**, **autoscaling groups**, and **infrastructure as code** (Terraform/CloudFormation).