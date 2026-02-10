# Day 24 – Deploying Application Load Balancer in Front of EC2 (Nginx)

## 📌 Objective

Set up an Application Load Balancer (ALB) in front of an EC2 instance running Nginx.

---

## 🏗 Architecture Overview

Client (Internet)  
        ↓  
Application Load Balancer (devops-alb)  
        ↓  
Target Group (devops-tg)  
        ↓  
EC2 Instance (devops-ec2 running Nginx)  

---

## 🔐 Step 1: Login to AWS Console

Ensure region is set to:

```
us-east-1 (N. Virginia)
```

---

## 🔐 Step 2: Create Security Group for ALB

EC2 → Security Groups → Create Security Group

- Name: `devops-sg`
- Open port 80 (HTTP) to `0.0.0.0/0`

---

## 📸 Portfolio Image 1

```
src/Screenshot-security-group.png
```

---

## 🎯 Step 3: Create Target Group

EC2 → Target Groups → Create Target Group

- Name: `devops-tg`
- Protocol: HTTP
- Port: 80
- Target type: Instance
- Register: `devops-ec2`
- Health Check Path: `/`

---

## 📸 Portfolio Image 2

```
src/Screenshot-target-group-healthy.png
```

---

## 🌐 Step 4: Create Application Load Balancer

EC2 → Load Balancers → Create ALB

- Name: `devops-alb`
- Internet-facing
- HTTP : 80
- Attach `devops-sg`
- Forward to `devops-tg`

---

## 📸 Portfolio Image 3

```
src/Screenshot-alb-details.png
```

---

## 🔄 Step 5: Update EC2 Security Group

Allow HTTP traffic from:

- Source: `devops-sg` (Recommended)

---

## 📸 Portfolio Image 4

```
src/Screenshot-ec2-sg-rule.png
```

---

## ✅ Step 6: Verify Target Health

EC2 → Target Groups → Targets

Status must be:

```
healthy
```

---

## 🧪 Step 7: Test the Application

Copy ALB DNS name and open:

```
http://<ALB-DNS-NAME>
```

Expected:

```
Welcome to nginx!
```

Or test using CLI:

```
curl http://<ALB-DNS-NAME>
```

---

# 🔎 Final Validation Checklist

- [ ] devops-sg created
- [ ] devops-tg created
- [ ] devops-ec2 registered
- [ ] ALB devops-alb created
- [ ] Listener configured
- [ ] Target health = healthy
- [ ] ALB DNS returns Nginx page

---

# 🧠 DevOps Learning Outcome

- Application Load Balancer setup
- Target group health checks
- Security group chaining (ALB → EC2)
- Layer 7 routing
- Production-grade cloud networking

---

# 🖼 Final Output Screenshot

Below is the final verification of the ALB successfully serving the Nginx page:

![Final ALB Output](src/final-output.png)

> Replace `final-output.png` with your actual screenshot filename  
> (example: Screenshot 2026-02-10 191517.png)

---

# 🎯 Result

Successfully deployed an Application Load Balancer in front of an EC2 instance.

Traffic flow:

Internet → ALB → Target Group → EC2

Application is now scalable and production-ready.
