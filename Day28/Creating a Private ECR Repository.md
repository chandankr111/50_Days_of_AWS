# 🚀 Containerized Application Deployment to AWS ECR

## 📌 Project Overview

This project demonstrates the complete end-to-end workflow of:

- Creating a **Private Amazon ECR Repository**
- Building a Docker image from a Dockerfile
- Tagging the image correctly
- Authenticating Docker with AWS
- Pushing the image to ECR
- Verifying the uploaded image

This simulates a real-world DevOps container deployment workflow used in production systems.

---

# 🏗 Architecture Workflow

Dockerfile
↓
Docker Build (Local Image)
↓
Tag Image with ECR URI
↓
Authenticate Docker to AWS ECR
↓
Push Image to Private Repository
↓
Image Stored in AWS Cloud


---

# 🛠 Technologies Used

- AWS (Elastic Container Registry - ECR)
- Docker
- AWS CLI
- Linux (aws-client host)

---

# 📂 Project Structure

/root/pyapp
├── Dockerfile
├── app.py
└── requirements.txt


---

# 🔹 Implementation Steps

---

## ✅ Step 1: Configure AWS CLI

Retrieve AWS credentials on the lab host:

```bash
showcreds
```

Configure AWS:

```bash
aws configure
```

Verify configuration:

```bash
aws sts get-caller-identity
```

---

## ✅ Step 2: Create Private ECR Repository

```bash
aws ecr create-repository \
  --repository-name devops-ecr \
  --region us-east-1
```

This creates a private ECR repository named **devops-ecr** in `us-east-1`.

---

## ✅ Step 3: Build Docker Image

Navigate to the application directory and build the Docker image:

```bash
cd /root/pyapp
docker build -t devops-ecr:latest .
docker images
```

The image `devops-ecr:latest` should now be available locally.

---

## ✅ Step 4: Authenticate Docker to ECR

```bash
aws ecr get-login-password --region us-east-1 | \
  docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com
```

Expected output:

```text
Login Succeeded
```

---

## ✅ Step 5: Tag Image for ECR

```bash
docker tag devops-ecr:latest \
  <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/devops-ecr:latest
```

This gives the image a fully qualified ECR URI so it can be pushed.

---

## ✅ Step 6: Push Image to ECR

```bash
docker push <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/devops-ecr:latest
```

Docker uploads the image layers and registers the `latest` tag in the private repository.

---

## ✅ Step 7: Verify Image in ECR

Using the AWS CLI:

```bash
aws ecr describe-images --repository-name devops-ecr
```

Or via the AWS Console:

- Navigate to **ECR → Repositories → devops-ecr → Images** and confirm that the `latest` tag exists.

---

## 🧠 Key Concepts Explained

### 🔹 Docker Image Structure

Docker image naming format:

```text
<registry>/<repository>:<tag>
```

Example:

```text
906292119996.dkr.ecr.us-east-1.amazonaws.com/devops-ecr:latest
```

- **Registry** → AWS ECR endpoint  
- **Repository** → `devops-ecr`  
- **Tag** → `latest`  

### 🔹 What `docker tag` Does

```bash
docker tag SOURCE_IMAGE TARGET_IMAGE
```

This creates an additional reference (name) for the same image ID.  
It **does not** duplicate or copy the image layers.

### 🔹 What `docker push` Does

```bash
docker push <ECR_URI>
```

This uploads the image layers (if not already present) and associates the specified tag in the remote registry.

---

## 📸 Screenshots

All screenshots for this lab are attached below, in order:

![Screenshot 1](./images/Screenshot%202026-02-17%20172045.png)
![Screenshot 2](./images/Screenshot%202026-02-17%20172057.png)
![Screenshot 3](./images/Screenshot%202026-02-17%20172545.png)
![Screenshot 4](./images/Screenshot%202026-02-17%20173307.png)
![Screenshot 5](./images/Screenshot%202026-02-17%20173315.png)
![Screenshot 6](./images/Screenshot%202026-02-17%20173339.png)