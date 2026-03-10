# 📋 AWS DevOps Project: To-Do Application Backend using Amazon DynamoDB

## 📌 Project Overview

This project demonstrates how to build the **backend data storage for a simple To-Do application using Amazon DynamoDB**. The goal is to store and manage tasks efficiently using a **NoSQL database**.

In this project, a DynamoDB table is created with a **primary key (`taskId`)**, and tasks are inserted with attributes such as **description** and **status**. The data is then verified to ensure correct storage and retrieval.

This setup simulates how a lightweight application backend can manage task records using a scalable, serverless database.

---

# 🏗 Architecture

```
User / Application
        │
        ▼
AWS Console / API
        │
        ▼
Amazon DynamoDB
(Table: datacenter-tasks)
        │
        ├── Task 1
        │    taskId: 1
        │    description: Learn DynamoDB
        │    status: completed
        │
        └── Task 2
             taskId: 2
             description: Build To-Do App
             status: in-progress
```

### Workflow

1. A DynamoDB table is created with a **primary key**.
2. Tasks are inserted as **items** into the table.
3. Each task contains:

   * Unique `taskId`
   * `description`
   * `status`
4. The stored items are verified through DynamoDB queries.

---

# ⚙️ Technologies Used

* **Amazon DynamoDB**
* **AWS Console**
* **NoSQL Database Concepts**
* **Cloud Data Storage**

---

# 🎯 Project Objectives

* Create a DynamoDB table for storing tasks
* Define a **primary key (`taskId`)**
* Insert tasks into the table
* Verify the stored data
* Understand how DynamoDB stores and retrieves items

---

# 🧩 Step 1: Create DynamoDB Table

Navigate to:

```
AWS Console → DynamoDB → Create Table
```

Enter the following configuration:

| Field         | Value            |
| ------------- | ---------------- |
| Table Name    | datacenter-tasks |
| Partition Key | taskId           |
| Key Type      | String           |
| Region        | us-east-1        |

Leave other settings as **default**.

Click **Create Table** and wait until the table status becomes **ACTIVE**.

---

# 🧩 Step 2: Insert Task 1

Open the table:

```
datacenter-tasks
```

Navigate to:

```
Explore table items → Create Item
```

Switch to **JSON View** and insert the following item:

```json
{
  "taskId": { "S": "1" },
  "description": { "S": "Learn DynamoDB" },
  "status": { "S": "completed" }
}
```

Click **Create Item**.

---

# 🧩 Step 3: Insert Task 2

Again click **Create Item** and insert:

```json
{
  "taskId": { "S": "2" },
  "description": { "S": "Build To-Do App" },
  "status": { "S": "in-progress" }
}
```

Click **Create Item**.

---

# 🧩 Step 4: Verify Stored Items

Go to:

```
Explore Table Items
```

You should see the following records:

| taskId | description     | status      |
| ------ | --------------- | ----------- |
| 1      | Learn DynamoDB  | completed   |
| 2      | Build To-Do App | in-progress |

Select each item to confirm the stored attributes.

---

# 📊 DynamoDB Table Structure

| Attribute   | Type   | Description                       |
| ----------- | ------ | --------------------------------- |
| taskId      | String | Primary key identifying each task |
| description | String | Task description                  |
| status      | String | Task progress status              |

---

# 🔐 Benefits of Using DynamoDB

Amazon DynamoDB provides several advantages:

* **Serverless architecture**
* **Automatic scaling**
* **Low latency performance**
* **Highly available NoSQL database**
* **Managed infrastructure**

This makes DynamoDB ideal for modern cloud applications such as **task managers, IoT systems, and real-time applications**.

---

# 📷 Project Screenshots

Below are the screenshots from the Day42 lab, in order of execution:


![EKS Cluster Creation](images/Screenshot%202026-03-10%20115650.png)

![Node Group Setup](images/Screenshot%202026-03-10%20115814.png)
 ![Kubernetes Dashboard](images/Screenshot%202026-03-10%20120037.png)

 ![Scaling Nodes](images/Screenshot%202026-03-10%20120144.png)

---

# 🎓 Learning Outcomes

Through this project, I learned:

* How to create and configure **DynamoDB tables**
* How DynamoDB uses **primary keys to identify items**
* How to insert and manage **NoSQL data**
* How to verify stored items using AWS Console
* Basic data modeling for a **task management system**

---

# 🚀 Future Improvements

Possible enhancements to this project:

* Add a **sort key for task categories**
* Build a **REST API using AWS Lambda**
* Connect the table with a **React/Next.js To-Do app**
* Implement **task filtering and updates**
* Add **CloudWatch monitoring**

---

# 👨‍💻 Author

**Chandan Kumar**

DevOps | Cloud | Backend Developer


