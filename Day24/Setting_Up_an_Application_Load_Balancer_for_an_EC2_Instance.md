## Day 24 – Setting Up an Application Load Balancer for an EC2 Instance (Nginx)

This project demonstrates how to place an **Application Load Balancer (ALB)** in front of an **EC2 instance running Nginx**.  
It is part of my **“50 Days of AWS”** learning series and is designed to be **portfolio-ready**, showing end‑to‑end setup with clear steps and screenshots.

---

### Objective

- **Configure an EC2 instance** running Nginx.
- **Create and attach an Application Load Balancer** in front of that instance.
- **Use a Target Group** with health checks to ensure only healthy instances receive traffic.
- **Secure traffic flow** using Security Groups (ALB → EC2).

---

### Architecture Overview

Traffic flow:

```text
Client (Internet)
        ↓
Application Load Balancer (devops-alb)
        ↓
Target Group (devops-tg)
        ↓
EC2 Instance (devops-ec2 running Nginx)
```

---

### Prerequisites

- AWS Account with permissions to create EC2, Target Groups and Load Balancers.
- Region: `us-east-1 (N. Virginia)`
- One EC2 instance (e.g., Amazon Linux) with:
  - Nginx installed and running on port 80.
  - Appropriate key pair for SSH (for initial setup, not shown here).

---

### Step 1 – Log in to AWS Console

1. Sign in to the **AWS Management Console**.
2. Set the region to:

```text
us-east-1 (N. Virginia)
```

---

### Step 2 – Create Security Group for the ALB

Navigate to **EC2 → Security Groups → Create Security Group**:

- **Name**: `devops-sg`
- **Inbound rule**:
  - Type: HTTP
  - Port: `80`
  - Source: `0.0.0.0/0` (for demo; in production, tighten this)

**Screenshot – Security Group for ALB**

![Security group overview](src/Screenshot%202026-02-10%20183824.png)
![Security group inbound rules](src/Screenshot%202026-02-10%20184202.png)

---

### Step 3 – Create Target Group

Go to **EC2 → Target Groups → Create target group**:

- **Target type**: Instance  
- **Name**: `devops-tg`  
- **Protocol**: HTTP  
- **Port**: `80`  
- **Health check path**: `/`

After creating the target group:

- Register the EC2 instance running Nginx (`devops-ec2`) as a target.

**Screenshots – Target Group Configuration**

![Create target group](src/Screenshot%202026-02-10%20184518.png)
![Register EC2 instance as target](src/Screenshot%202026-02-10%20184532.png)

---

### Step 4 – Create the Application Load Balancer

Go to **EC2 → Load Balancers → Create Load Balancer → Application Load Balancer**:

- **Name**: `devops-alb`
- **Scheme**: Internet-facing
- **IP address type**: IPv4
- **Listeners**:
  - HTTP on port `80`
- **Availability Zones**: Select appropriate VPC and subnets.
- **Security Groups**: Attach `devops-sg`
- **Default action (listener rule)**:
  - Forward to target group `devops-tg`

**Screenshots – ALB Configuration**

![ALB basic configuration](src/Screenshot%202026-02-10%20184826.png)
![ALB listeners and routing](src/Screenshot%202026-02-10%20184959.png)
![ALB security group selection](src/Screenshot%202026-02-10%20185042.png)
![ALB summary before creation](src/Screenshot%202026-02-10%20185420.png)
![ALB successfully created](src/Screenshot%202026-02-10%20185502.png)

---

### Step 5 – Update EC2 Security Group

For a secure and controlled flow, the EC2 instance should only allow HTTP traffic **from the ALB security group**, not directly from the internet.

On the EC2 instance’s security group:

- Add an inbound rule:
  - Type: HTTP
  - Port: `80`
  - Source: `devops-sg` (the ALB security group)

This effectively creates a chain:

```text
Internet → ALB Security Group (devops-sg) → EC2 Security Group → EC2 instance
```

---

### Step 6 – Verify Target Health

Go to **EC2 → Target Groups → devops-tg → Targets** and confirm that the instance status is:

```text
healthy
```

**Screenshot – Healthy Target in Target Group**

![Target group health check status](src/Screenshot%202026-02-10%20191056.png)

---

### Step 7 – Test the Application via ALB

From the **Load Balancers** page:

1. Select `devops-alb`.
2. Copy the **DNS name** of the ALB (e.g., `devops-alb-1234567890.us-east-1.elb.amazonaws.com`).

Test in a browser:

```text
http://<ALB-DNS-NAME>
```

Or test from a terminal:

```bash
curl http://<ALB-DNS-NAME>
```

You should see the Nginx default page content:

```text
Welcome to nginx!
```

**Screenshots – Testing and DNS**

![ALB details and DNS name](src/Screenshot%202026-02-10%20191454.png)

**Final Output – Nginx Behind the ALB**

![Nginx welcome page served via ALB](src/Screenshot%202026-02-10%20191517.png)

---

### Additional Console Views

Below is another console view used during the setup process:

![Additional AWS console step](src/Screenshot%202026-02-10%20185502.png)

---

### Final Validation Checklist

- [x] Security group `devops-sg` created for ALB.
- [x] Target group `devops-tg` created.
- [x] EC2 instance (`devops-ec2`) registered in target group.
- [x] Application Load Balancer `devops-alb` created.
- [x] Listener on port 80 forwards to `devops-tg`.
- [x] Target health status is **healthy**.
- [x] ALB DNS serves the Nginx welcome page.

---

### What I Learned (Day 24)

- How to design and implement a **Layer 7 Application Load Balancer** on AWS.
- How **Target Groups** and **health checks** work to keep traffic going only to healthy instances.
- How to chain **Security Groups** so that only the ALB can talk to the EC2 instance on HTTP.
- How to expose a simple web application (Nginx) in a more **production-ready** way, rather than directly exposing the EC2 instance.

---

### Summary

On Day 24, I successfully:

- Deployed an **Application Load Balancer** in front of an **EC2 instance running Nginx**.
- Verified end‑to‑end traffic flow:

```text
Internet → ALB → Target Group → EC2 (Nginx)
```

This setup is a solid foundation for scaling to **multiple EC2 instances** behind the same ALB for high availability and improved resilience.

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
