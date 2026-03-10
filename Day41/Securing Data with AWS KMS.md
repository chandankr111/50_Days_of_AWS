# 🔐 AWS DevOps Project: Data Encryption Using AWS KMS

## 📌 Project Overview

This project demonstrates how to secure sensitive data using **AWS Key Management Service (KMS)**.
The goal is to create a **symmetric KMS key** and use it to **encrypt and decrypt a sensitive file** using the AWS CLI.

The encryption ensures that sensitive information is protected at rest, while AWS KMS manages the cryptographic keys securely.

---

## 📷 Project Images (Day41)

Below are the screenshots from the Day41 lab, in order of execution:


        ![KMS Key Creation](images/Screenshot%202026-03-10%20110723.png)

        ![Sensitive File Verification](images/Screenshot%202026-03-10%20110735.png)
3
        ![Encryption Command Execution](images/Screenshot%202026-03-10%20110807.png)
4.
        ![Encrypted File Generated](images/Screenshot%202026-03-10%20111733.png)
5.
        ![Decryption Command](images/Screenshot%202026-03-10%20112341.png)
6.
        ![Verification of Decrypted File](images/Screenshot%202026-03-10%20112505.png)

---

In this project:

* A **KMS key** was created.
* A sensitive file was **encrypted using the KMS key**.
* The encrypted output was **base64 encoded and stored**.
* The encrypted data was **decrypted to verify integrity**.

---

# 🏗 Architecture

```text
SensitiveData.txt
        │
        ▼
AWS CLI Encryption Command
        │
        ▼
AWS KMS Key (datacenter-KMS-Key)
        │
        ▼
Encrypted Ciphertext
        │
        ▼
Base64 Encoding
        │
        ▼
EncryptedData.bin
```

### Decryption Flow

```text
EncryptedData.bin
        │
        ▼
Base64 Decode
        │
        ▼
AWS KMS Decrypt
        │
        ▼
Decrypted Plaintext
        │
        ▼
Verification with Original File
```

---

# ⚙️ Technologies Used

* AWS KMS (Key Management Service)
* AWS CLI
* Linux (Ubuntu)
* Base64 Encoding
* Encryption / Decryption Concepts

---

# 🎯 Project Objectives

* Create a **symmetric encryption key in AWS KMS**
* Encrypt a sensitive file using the key
* Store encrypted output safely
* Decrypt the file using the same KMS key
* Verify the decrypted data matches the original file

---

# 🧩 Step 1: Create KMS Key

Navigate to the AWS Console:

```
AWS Console → Key Management Service → Create Key
```

Configuration:

| Setting   | Value               |
| --------- | ------------------- |
| Key type  | Symmetric           |
| Key usage | Encrypt and Decrypt |
| Alias     | datacenter-KMS-Key  |
| Region    | us-east-1           |

The key will be used to perform encryption and decryption operations.

---

# 🧩 Step 2: Verify Sensitive File

The file already exists in the system.

```bash
ls /root/
```

Expected output:

```text
SensitiveData.txt
```

View file content:

```bash
cat /root/SensitiveData.txt
```

---

# 🧩 Step 3: Encrypt the File Using AWS KMS

Run the following command to encrypt the file.

```bash
aws kms encrypt \
--key-id alias/datacenter-KMS-Key \
--plaintext fileb:///root/SensitiveData.txt \
--output text \
--query CiphertextBlob | base64 --decode > /root/EncryptedData.bin
```

### Explanation

| Parameter           | Purpose                          |
| ------------------- | -------------------------------- |
| `--key-id`          | Specifies the KMS key            |
| `--plaintext`       | Input file to encrypt            |
| `CiphertextBlob`    | Encrypted output                 |
| `base64 --decode`   | Converts output to binary format |
| `EncryptedData.bin` | Final encrypted file             |

Verify encrypted file:

```bash
ls /root/
```

Expected output:

```text
EncryptedData.bin
SensitiveData.txt
```

---

# 🧩 Step 4: Decrypt the Encrypted File

Run the following command:

```bash
aws kms decrypt \
--ciphertext-blob fileb:///root/EncryptedData.bin \
--output text \
--query Plaintext | base64 --decode > /root/DecryptedData.txt
```

This decrypts the encrypted file using the same KMS key.

---

# 🧩 Step 5: Verify File Integrity

Compare original and decrypted files.

```bash
diff /root/SensitiveData.txt /root/DecryptedData.txt
```

If no output appears, the files are identical.

Alternatively:

```bash
cat /root/DecryptedData.txt
```

Output should match the original file content.

---

# 🔐 Security Benefits

Using AWS KMS provides:

* Centralized key management
* Secure cryptographic operations
* Automatic key rotation
* Integration with AWS services
* Fine-grained access control via IAM

---

# 📊 Key DevOps Concepts Demonstrated

* Data encryption using AWS KMS
* CLI-based cryptographic operations
* Base64 encoding/decoding
* Secure key management
* Encryption validation

---

# 📷 Project Screenshots

(Add screenshots from your lab)

### KMS Key Creation

```
images/kms-key-created.png
```

### Encryption Command Execution

```
images/encryption-command.png
```

### Encrypted File Generated

```
images/encrypted-file.png
```

### Decryption Command

```
images/decryption-command.png
```

### Verification of Decrypted File

```
images/verification.png
```

---

# 🎓 Learning Outcomes

Through this project I learned:

* How AWS KMS manages encryption keys
* How to encrypt files using AWS CLI
* How base64 encoding works with encrypted data
* How to securely store and decrypt sensitive files
* Best practices for protecting sensitive information

---


# 👨‍💻 Author

**Chandan Kumar**

DevOps | Cloud | Backend Developer
