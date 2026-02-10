# Day 22 – Configuring Secure SSH Access to an EC2 Instance

## 📌 Objective

Provision a new EC2 instance named **xfusion-ec2** in the **us-east-1** region and configure secure, passwordless SSH access for the **root user** from the `aws-client` landing host.

---

## 🧾 Requirements

- Instance type: `t2.micro`
- Instance name: `xfusion-ec2`
- Region: `us-east-1`
- SSH key `id_rsa` must exist under:
  ```
  /root/.ssh/
  ```
- Public key must be added to:
  ```
  /root/.ssh/authorized_keys
  ```
- Final verification:
  ```
  ssh root@<public-ip>
  ```

---

## 🔐 Retrieve AWS Credentials

Run on `aws-client`:

```
showcreds
```

Login to AWS Console in **us-east-1** region.

---

# 🚀 Implementation Steps

---

## Step 1: Create SSH Key on aws-client

```
ssh-keygen -t rsa
```

Press Enter for default path:

```
/root/.ssh/id_rsa
```

Verify:

```
ls -l /root/.ssh/
```

---

## Step 2: Create Temporary Key for Initial EC2 Login

```
ssh-keygen -t rsa -f ~/ec2-temp
```

This generates:

```
~/ec2-temp
~/ec2-temp.pub
```

---

## Step 3: Import Temporary Public Key to AWS

1. Go to **EC2 → Key Pairs**
2. Click **Import Key Pair**
3. Name: `ec2-temp-key`
4. Paste output of:

   ```
   cat ~/ec2-temp.pub
   ```

5. Click **Import**

---

## Step 4: Launch EC2 Instance

Go to **EC2 → Launch Instance**

Configuration:

- Name: `xfusion-ec2`
- AMI: Ubuntu
- Instance Type: `t2.micro`
- Key Pair: `ec2-temp-key`
- Region: `us-east-1`

Launch instance.

---

## Step 5: SSH into EC2 Instance

From `aws-client`:

```
ssh -i ~/ec2-temp ubuntu@<public-ip>
```

---

## Step 6: Switch to Root User

Inside EC2:

```
sudo -i
```

Ensure SSH directory exists:

```
mkdir -p /root/.ssh
chmod 700 /root/.ssh
```

---

## Step 7: Add aws-client Public Key to Root

On aws-client:

```
cat /root/.ssh/id_rsa.pub
```

Copy the entire key.

On EC2 (as root):

```
nano /root/.ssh/authorized_keys
```

Paste the copied key.

Set permissions:

```
chmod 600 /root/.ssh/authorized_keys
```

---

## Step 8: Exit EC2

```
exit
exit
```

---

## Step 9: Verify Passwordless Root Login

From aws-client:

```
ssh root@<public-ip>
```

Login should succeed without password prompt.

---

# 🔒 Permission Summary

| Path | Permission |
|------|------------|
| /root/.ssh | 700 |
| authorized_keys | 600 |
| id_rsa | 600 |
| id_rsa.pub | 644 |

---

# ✅ Validation Checklist

- [ ] Instance created in `us-east-1`
- [ ] Name is `xfusion-ec2`
- [ ] Type is `t2.micro`
- [ ] `/root/.ssh/id_rsa` exists on aws-client
- [ ] Public key added to root authorized_keys
- [ ] `ssh root@<public-ip>` works without password

---

# 🎯 Result

Secure EC2 instance configured with passwordless root SSH access from the aws-client host.
