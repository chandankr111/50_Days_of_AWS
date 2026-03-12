# 🚀 Day 46 – Automating File Management using AWS S3, Lambda, and DynamoDB

## 📌 Project Overview

This project demonstrates how to automate file management between two Amazon S3 buckets using AWS Lambda and DynamoDB.

When a file is uploaded to a **public S3 bucket**, an **AWS Lambda function** is automatically triggered. The Lambda function copies the uploaded file to a **private S3 bucket** and stores the operation details in a **DynamoDB table** for logging and tracking.

This architecture improves **automation, security, and observability** in cloud-based file workflows.

---

# 🏗️ Architecture

```
User Upload
     │
     ▼
Public S3 Bucket
(devops-public-31415)
     │
     ▼
AWS Lambda Function
(devops-copyfunction)
     │
     ├── Copy File
     │        ▼
     │   Private S3 Bucket
     │   (devops-private-11062)
     │
     └── Store Logs
              ▼
        DynamoDB Table
       (devops-S3CopyLogs)
```

---

# 🛠️ AWS Services Used

- Amazon S3
- AWS Lambda
- Amazon DynamoDB
- AWS IAM
- AWS CLI

---

# 📂 Resources Created

| Resource Type | Name |
|---------------|------|
| Public S3 Bucket | devops-public-31415 |
| Private S3 Bucket | devops-private-11062 |
| Lambda Function | devops-copyfunction |
| DynamoDB Table | devops-S3CopyLogs |
| IAM Role | lambda_execution_role |

---

# ⚙️ Implementation Using AWS CLI

## 1️⃣ Create Public S3 Bucket

```bash
aws s3api create-bucket \
--bucket devops-public-31415 \
--region us-east-1
```

Allow public access:

```bash
aws s3api put-public-access-block \
--bucket devops-public-31415 \
--public-access-block-configuration \
BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=false,RestrictPublicBuckets=false
```

---

## 2️⃣ Create Private S3 Bucket

```bash
aws s3api create-bucket \
--bucket devops-private-11062 \
--region us-east-1
```

---

## 3️⃣ Create DynamoDB Table

```bash
aws dynamodb create-table \
--table-name devops-S3CopyLogs \
--attribute-definitions AttributeName=LogID,AttributeType=S \
--key-schema AttributeName=LogID,KeyType=HASH \
--billing-mode PAY_PER_REQUEST
```

---

## 4️⃣ Create IAM Role for Lambda

Create trust policy:

```json
{
"Version": "2012-10-17",
"Statement": [
{
"Effect": "Allow",
"Principal": {
"Service": "lambda.amazonaws.com"
},
"Action": "sts:AssumeRole"
}
]
}
```

Create role:

```bash
aws iam create-role \
--role-name lambda_execution_role \
--assume-role-policy-document file://trust-policy.json
```

Attach required policies:

```bash
aws iam attach-role-policy \
--role-name lambda_execution_role \
--policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
```

```bash
aws iam attach-role-policy \
--role-name lambda_execution_role \
--policy-arn arn:aws:iam::aws:policy/AmazonDynamoDBFullAccess
```

```bash
aws iam attach-role-policy \
--role-name lambda_execution_role \
--policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
```

---

## 5️⃣ Prepare Lambda Function

Update the Lambda code values:

```python
DYNAMODB_TABLE = "devops-S3CopyLogs"
DEST_BUCKET = "devops-private-11062"
```

Zip the Lambda function:

```bash
zip function.zip lambda-function.py
```

---

## 6️⃣ Create Lambda Function

```bash
aws lambda create-function \
--function-name devops-copyfunction \
--runtime python3.9 \
--role arn:aws:iam::ACCOUNT_ID:role/lambda_execution_role \
--handler lambda-function.lambda_handler \
--zip-file fileb://function.zip
```

---

## 7️⃣ Add Permission for S3 to Invoke Lambda

```bash
aws lambda add-permission \
--function-name devops-copyfunction \
--principal s3.amazonaws.com \
--statement-id s3invoke \
--action "lambda:InvokeFunction" \
--source-arn arn:aws:s3:::devops-public-31415
```

---

## 8️⃣ Configure S3 Event Notification

Create notification configuration:

```json
{
"LambdaFunctionConfigurations": [
{
"LambdaFunctionArn": "arn:aws:lambda:us-east-1:ACCOUNT_ID:function:devops-copyfunction",
"Events": ["s3:ObjectCreated:*"]
}
]
}
```

Apply configuration:

```bash
aws s3api put-bucket-notification-configuration \
--bucket devops-public-31415 \
--notification-configuration file://notification.json
```

---

# 🧪 Testing

Upload a file to the public bucket:

```bash
aws s3 cp /root/sample.zip s3://devops-public-31415/
```

This upload triggers the Lambda function which:

- Copies the file to the private bucket
- Logs the operation in DynamoDB

---

# ✅ Verification

Check private bucket:

```bash
aws s3 ls s3://devops-private-11062
```

Check DynamoDB logs:

```bash
aws dynamodb scan --table-name devops-S3CopyLogs
```

---

# 📸 Screenshots

Add your screenshots here.

### Public Bucket Upload
![Public Bucket](screenshots/public-bucket.png)

### Lambda Trigger
![Lambda Function](screenshots/lambda.png)

### Private Bucket File Copy
![Private Bucket](screenshots/private-bucket.png)

### DynamoDB Logs
![DynamoDB](screenshots/dynamodb.png)

---

# 🎯 Key Learnings

- Event-driven automation using AWS S3 triggers
- Serverless computing with AWS Lambda
- Logging and monitoring using DynamoDB
- Managing permissions using IAM roles
- Infrastructure automation using AWS CLI

---

# 📅 Challenge

**100 Days of AWS – Day 46**

Building real-world cloud automation workflows using serverless AWS services.

---

⭐ If you like this project, feel free to star the repository.

---

# Portfolio Screenshots

The screenshots below are attached in top-to-bottom sequence for use in the portfolio.

![Screenshot 01](images/Screenshot%202026-03-12%20133731.png)
![Screenshot 02](images/Screenshot%202026-03-12%20133836.png)
![Screenshot 03](images/Screenshot%202026-03-12%20134329.png)
![Screenshot 04](images/Screenshot%202026-03-12%20134632.png)
![Screenshot 05](images/Screenshot%202026-03-12%20135006.png)
![Screenshot 06](images/Screenshot%202026-03-12%20135125.png)
![Screenshot 07](images/Screenshot%202026-03-12%20135232.png)
![Screenshot 08](images/Screenshot%202026-03-12%20135417.png)
![Screenshot 09](images/Screenshot%202026-03-12%20135711.png)
![Screenshot 10](images/Screenshot%202026-03-12%20135746.png)
![Screenshot 11](images/Screenshot%202026-03-12%20135856.png)
![Screenshot 12](images/Screenshot%202026-03-12%20135912.png)
![Screenshot 13](images/Screenshot%202026-03-12%20135958.png)
![Screenshot 14](images/Screenshot%202026-03-12%20140150.png)
