# AWS Lambda Deployment using CloudFormation

## Project Overview

This project demonstrates how to deploy an **AWS Lambda function using Infrastructure as Code (IaC) with AWS CloudFormation**. The CloudFormation template automatically provisions the required AWS resources including an **IAM role** and a **Lambda function written in Python**.

The Lambda function returns a simple HTTP-style response containing a welcome message.

This project highlights how DevOps engineers can automate serverless deployments and manage cloud infrastructure efficiently using **AWS CLI and CloudFormation templates**.

---

# Architecture

The deployment uses the following AWS services:

* **AWS CloudFormation** – Infrastructure as Code to deploy resources.
* **AWS Lambda** – Serverless compute service to run the Python function.
* **AWS IAM** – Role that allows Lambda to execute and write logs.

### Architecture Flow

```
CloudFormation Stack
       │
       ├── IAM Role (lambda_execution_role)
       │
       └── Lambda Function (xfusion-lambda)
               │
               └── Python Runtime
                       │
                       └── Returns Response
                           Welcome to KKE AWS Labs!
```

---

# Technologies Used

* AWS Lambda
* AWS CloudFormation
* AWS IAM
* Python
* AWS CLI

---

# CloudFormation Template

The CloudFormation template defines the infrastructure required to deploy the Lambda function.

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: CloudFormation template to create Lambda function xfusion-lambda

Resources:

  LambdaExecutionRole:
    Type: AWS::IAM::Role
    Properties:
      RoleName: lambda_execution_role
      AssumeRolePolicyDocument:
        Version: "2012-10-17"
        Statement:
          - Effect: Allow
            Principal:
              Service:
                - lambda.amazonaws.com
            Action:
              - sts:AssumeRole
      ManagedPolicyArns:
        - arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole

  XfusionLambdaFunction:
    Type: AWS::Lambda::Function
    Properties:
      FunctionName: xfusion-lambda
      Runtime: python3.9
      Handler: index.lambda_handler
      Role: !GetAtt LambdaExecutionRole.Arn
      Code:
        ZipFile: |
          def lambda_handler(event, context):
              return {
                  "statusCode": 200,
                  "body": "Welcome to KKE AWS Labs!"
              }
```

---

# Deployment Steps

## 1. Create the CloudFormation Template

Create the file:

```
/root/xfusion-lambda.yml
```

---

## 2. Deploy the Stack

Use the AWS CLI to create the CloudFormation stack.

```bash
aws cloudformation create-stack \
--stack-name xfusion-lambda-app \
--template-body file:///root/xfusion-lambda.yml \
--capabilities CAPABILITY_NAMED_IAM
```

---

## 3. Check Stack Status

Verify the deployment status:

```bash
aws cloudformation describe-stacks \
--stack-name xfusion-lambda-app \
--query "Stacks[0].StackStatus"
```

Expected output:

```
CREATE_COMPLETE
```

---

# Testing the Lambda Function

Invoke the Lambda function using AWS CLI.

```bash
aws lambda invoke \
--function-name xfusion-lambda \
--payload '{}' \
output.json
```

Check the response:

```bash
cat output.json
```

Expected response:

```json
{
  "statusCode": 200,
  "body": "Welcome to KKE AWS Labs!"
}
```

---

# Key Features

* Infrastructure as Code using CloudFormation
* Automated serverless deployment
* IAM role integration with Lambda
* Simple Python-based Lambda function
* CLI-based AWS resource management

---

# Learning Outcomes

Through this project I learned:

* How to deploy **AWS Lambda using CloudFormation**
* Creating and attaching **IAM roles for Lambda execution**
* Managing AWS infrastructure using **AWS CLI**
* Writing **serverless Python functions**
* Automating deployments using **Infrastructure as Code**

---

# Author

**Chandan Kumar**

DevOps | Cloud | Full Stack Developer

---

# Future Improvements

* Add API Gateway to expose Lambda as a REST API
* Implement CloudWatch monitoring and alarms
* Add CI/CD pipeline using GitHub Actions
* Deploy Lambda using Terraform for comparison
