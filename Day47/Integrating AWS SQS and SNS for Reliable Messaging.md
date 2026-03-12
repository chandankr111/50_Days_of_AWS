# AWS Priority Queue System using SNS, SQS, and Lambda

## Project Overview

This project demonstrates how to implement a **priority-based message processing system** using AWS services. The architecture routes messages based on their priority level using **SNS message attributes and SQS filter policies**, and processes them using **AWS Lambda**.

The system ensures that **high-priority messages are handled separately from low-priority messages**, enabling scalable and efficient event-driven processing.

---

# Architecture

The system uses the following AWS services:

* **Amazon SNS** – Publishes messages with priority attributes.
* **Amazon SQS** – Stores messages in separate queues based on priority.
* **AWS Lambda** – Consumes and processes messages from the queues.
* **AWS CloudFormation** – Deploys the entire infrastructure as code.

### Architecture Flow

```
SNS Topic
   |
   |--- priority = high ---> High Priority SQS Queue
   |                               |
   |                               v
   |                           AWS Lambda
   |
   |--- priority = low ----> Low Priority SQS Queue
                                   |
                                   v
                               AWS Lambda
```

---

# Technologies Used

* AWS SNS
* AWS SQS
* AWS Lambda
* AWS CloudFormation
* AWS CLI
* Python

---

# Infrastructure Deployment

The infrastructure is deployed using **AWS CloudFormation**.

### Step 1 – Upload Lambda Code to S3

```
zip function-code.zip index.py
aws s3 cp function-code.zip s3://<YOUR_BUCKET_NAME>/
```

---

### Step 2 – Deploy CloudFormation Stack

```
aws cloudformation create-stack \
--stack-name devops-priority-stack \
--template-body file:///root/devops-priority-stack.yml \
--parameters ParameterKey=LambdaS3Bucket,ParameterValue=<YOUR_BUCKET_NAME> \
ParameterKey=LambdaS3Key,ParameterValue=function-code.zip \
--capabilities CAPABILITY_NAMED_IAM
```

Wait for the stack status:

```
CREATE_COMPLETE
```

---

# SNS Topic and Message Publishing

Messages are published to the SNS topic with a **priority attribute**.

### Get Topic ARN

```
topicarn=$(aws sns list-topics --query "Topics[?contains(TopicArn, 'devops-Priority-Queues-Topic')].TopicArn" --output text)
```

---

### Publish High Priority Messages

```
aws sns publish \
--topic-arn $topicarn \
--message 'High Priority message 1' \
--message-attributes '{"priority" : { "DataType":"String", "StringValue":"high"}}'

aws sns publish \
--topic-arn $topicarn \
--message 'High Priority message 2' \
--message-attributes '{"priority" : { "DataType":"String", "StringValue":"high"}}'
```

---

### Publish Low Priority Messages

```
aws sns publish \
--topic-arn $topicarn \
--message 'Low Priority message 1' \
--message-attributes '{"priority" : { "DataType":"String", "StringValue":"low"}}'

aws sns publish \
--topic-arn $topicarn \
--message 'Low Priority message 2' \
--message-attributes '{"priority" : { "DataType":"String", "StringValue":"low"}}'
```

---

# Queue Configuration

Two SQS queues are created:

* `devops-High-Priority-Queue`
* `devops-Low-Priority-Queue`

Each queue subscribes to the SNS topic using **filter policies**:

| Queue               | Filter Policy   |
| ------------------- | --------------- |
| High Priority Queue | priority = high |
| Low Priority Queue  | priority = low  |

---

# Lambda Configuration

The Lambda function processes messages from the queues.

Environment variables are configured
