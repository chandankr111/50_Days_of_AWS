# Day 25 – EC2 Monitoring with CloudWatch Alarm & SNS Notification

## 📌 Objective

Launch an EC2 instance and configure a CloudWatch alarm to monitor CPU utilization.

If CPU usage exceeds **90% for one consecutive 5-minute period**, an alert should be sent via an existing SNS topic.

---

## 🏗 Task Requirements

- Launch EC2 instance: `nautilus-ec2`
- Ubuntu AMI
- Create CloudWatch alarm: `nautilus-alarm`
- Metric: CPUUtilization
- Statistic: Average
- Threshold: >= 90%
- Period: 5 minutes
- Evaluation: 1 consecutive period
- Alarm action: Notify SNS topic `nautilus-sns-topic`
- Region: `us-east-1`

---

# 🧭 Step 1: Launch EC2 Instance

Go to:

EC2 → Launch Instance

### Configuration:

- Name: `nautilus-ec2`
- AMI: Ubuntu (latest LTS)
- Instance type: t2.micro (or allowed type)
- Key pair: Select existing or create new
- VPC: Default VPC
- Auto-assign public IP: Enabled

Click **Launch Instance**

---

## 📸 Screenshot 1 – EC2 Instance Details

![EC2 Instance Details](./images/Screenshot%202026-02-11%20153908.png)

---

# 📊 Step 2: Create CloudWatch Alarm

Go to:

CloudWatch → Alarms → Create Alarm

### Select Metric:

- EC2 → Per-Instance Metrics
- Select instance: `nautilus-ec2`
- Choose metric: **CPUUtilization**

Click **Select metric**

---

## Configure Metric & Conditions

### Metric Settings:

- Statistic: **Average**
- Period: **5 minutes**

### Conditions:

- Threshold type: Static
- Whenever CPUUtilization is:
  >= 90
- Datapoints to alarm: 1 out of 1
- Period: 5 minutes

---

# 🔔 Step 3: Configure Alarm Action

Under Notifications:

- Select existing SNS topic
- Choose: `nautilus-sns-topic`

Alarm name:

```
nautilus-alarm
```

Click **Create Alarm**

---

## 📸 Screenshot 2 – CloudWatch Alarm Configuration

![Alarm Configuration](./images/Screenshot%202026-02-11%20154012.png)

---

# ✅ Step 4: Verify Alarm Creation

Go to:

CloudWatch → Alarms

Confirm:

- Alarm name: `nautilus-alarm`
- State: OK (initially)

---

## 📸 Screenshot 3 – CloudWatch Alarm Dashboard

![Alarm Dashboard](./images/Screenshot%202026-02-11%20154207.png)

---

# 🧪 Optional: Test Alarm (Simulate High CPU)

SSH into EC2:

```bash
sudo apt update
sudo apt install stress -y
stress --cpu 2 --timeout 300
```

Wait 5 minutes.

Expected Result:

- Alarm state changes to: **ALARM**
- Notification sent to SNS topic

---

# 🔎 Final Validation Checklist

- [ ] EC2 instance launched (nautilus-ec2)
- [ ] Instance in us-east-1
- [ ] CloudWatch alarm created
- [ ] CPUUtilization metric selected
- [ ] Threshold set to >= 90%
- [ ] Period set to 5 minutes
- [ ] Evaluation set to 1 datapoint
- [ ] SNS topic attached
- [ ] Alarm visible in CloudWatch dashboard

---

# 🏁 Result

Successfully configured monitoring for EC2 instance using CloudWatch.

System now:

EC2 → CloudWatch Metric → Alarm → SNS Notification

This ensures proactive monitoring of high CPU usage and improves reliability.

---

# 🧠 DevOps Learning Outcome

- EC2 metrics monitoring
- CloudWatch alarm configuration
- SNS integration for alerting
- Infrastructure observability basics
- Production-level monitoring mindset

---

## 🚀 100 Days of AWS – Day 25 Complete
