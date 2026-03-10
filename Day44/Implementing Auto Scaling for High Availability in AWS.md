# 🚀 AWS Auto Scaling & Application Load Balancer Setup

This project demonstrates how to deploy a **highly available and scalable web application on AWS** using Auto Scaling Groups and an Application Load Balancer. The infrastructure automatically launches EC2 instances with Nginx installed and distributes traffic across them.

---

## 📌 Project Overview

The goal of this project is to build a fault-tolerant architecture that:

* Automatically launches EC2 instances using a Launch Template
* Installs and runs **Nginx web server** on instance startup
* Uses an **Application Load Balancer (ALB)** to distribute incoming traffic
* Uses an **Auto Scaling Group (ASG)** to scale instances based on CPU utilization
* Ensures only **healthy instances receive traffic**

---

## 🧱 Architecture

```
                Internet
                    │
                    ▼
        Application Load Balancer (ALB)
                    │
                    ▼
            Target Group (HTTP : 80)
                    │
                    ▼
           Auto Scaling Group (ASG)
             │                │
             ▼                ▼
        EC2 Instance      EC2 Instance
        (Nginx Server)    (Nginx Server)
```

---

## ⚙️ AWS Services Used

* Amazon EC2
* Launch Templates
* Auto Scaling Groups
* Application Load Balancer
* Target Groups
* Security Groups
* Amazon Linux 2

---

## 🖥️ Launch Template Configuration

**Launch Template Name**

```
nautilus-launch-template
```

**Configuration**

* AMI: Amazon Linux 2
* Instance Type: t2.micro
* Security Group: Allow HTTP (Port 80)
* Key configuration for automated server provisioning.

---

## 📜 User Data Script (Install Nginx Automatically)

```bash
#!/bin/bash
yum update -y
yum install nginx -y
systemctl start nginx
systemctl enable nginx
```

This script ensures that whenever a new EC2 instance launches, **Nginx is installed and running automatically**.

---

## 🔐 Security Group Configuration

| Type | Protocol | Port | Source    |
| ---- | -------- | ---- | --------- |
| HTTP | TCP      | 80   | 0.0.0.0/0 |

This allows web traffic from anywhere to reach the servers.

---

## ⚡ Auto Scaling Group Configuration

| Setting                 | Value           |
| ----------------------- | --------------- |
| Auto Scaling Group Name | nautilus-asg    |
| Minimum Instances       | 1               |
| Desired Instances       | 1               |
| Maximum Instances       | 2               |
| Scaling Policy          | Target Tracking |
| CPU Utilization Target  | 50%             |

The ASG automatically **adds or removes instances based on CPU load**.

---

## 🌐 Application Load Balancer

| Setting            | Value        |
| ------------------ | ------------ |
| Load Balancer Name | nautilus-alb |
| Listener           | HTTP : 80    |
| Target Group       | nautilus-tg  |

The ALB distributes traffic across healthy EC2 instances.

---

## ❤️ Health Checks

The target group performs health checks to ensure only healthy instances receive traffic.

| Setting  | Value |
| -------- | ----- |
| Protocol | HTTP  |
| Port     | 80    |
| Path     | /     |

---

## 🧪 Testing the Deployment

1. Obtain the **ALB DNS Name**
2. Open it in a browser

Example:

```
http://nautilus-alb-xxxxxxxx.us-east-1.elb.amazonaws.com
```

If everything is configured correctly, the **default Nginx page** will appear.

---

## 📊 Key DevOps Concepts Demonstrated

* Infrastructure as Code concepts
* High availability architecture
* Horizontal scaling
* Load balancing
* Automated server provisioning
* Cloud-native architecture

---



## 🏁 Conclusion

This project demonstrates how to build a **scalable and resilient web infrastructure on AWS** using industry best practices. The system automatically scales based on demand while ensuring high availability through load balancing.


### Screenshots

![Screenshot 2026-03-10 184709](images/Screenshot%202026-03-10%20184709.png)
![Screenshot 2026-03-10 184753](images/Screenshot%202026-03-10%20184753.png)
![Screenshot 2026-03-10 185600](images/Screenshot%202026-03-10%20185600.png)
![Screenshot 2026-03-10 185749](images/Screenshot%202026-03-10%20185749.png)
![Screenshot 2026-03-10 185837](images/Screenshot%202026-03-10%20185837.png)
![Screenshot 2026-03-10 185937](images/Screenshot%202026-03-10%20185937.png)
![Screenshot 2026-03-10 190259](images/Screenshot%202026-03-10%20190259.png)
![Screenshot 2026-03-10 190349](images/Screenshot%202026-03-10%20190349.png)
![Screenshot 2026-03-10 190413](images/Screenshot%202026-03-10%20190413.png)
![Screenshot 2026-03-10 190456](images/Screenshot%202026-03-10%20190456.png)
![Screenshot 2026-03-10 190515](images/Screenshot%202026-03-10%20190515.png)
![Screenshot 2026-03-10 190735](images/Screenshot%202026-03-10%20190735.png)
![Screenshot 2026-03-10 190812](images/Screenshot%202026-03-10%20190812.png)
![Screenshot 2026-03-10 193026](images/Screenshot%202026-03-10%20193026.png)
![Screenshot 2026-03-10 193101](images/Screenshot%202026-03-10%20193101.png)
