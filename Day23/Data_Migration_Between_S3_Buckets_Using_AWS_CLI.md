# Day 23 – S3 Data Migration and Verification Using AWS CLI

## 📌 Objective

Migrate all data from an existing S3 bucket:

```
datacenter-s3-18704
```

To a new private S3 bucket:

```
datacenter-sync-5444
```

Ensure:
- Complete data transfer
- No data loss
- Data consistency between both buckets
- All resources created in `us-east-1` region

---

## 🧾 Requirements

- Create new private S3 bucket
- Migrate full data
- Verify data consistency
- Use AWS CLI only
- Region: `us-east-1`

---

## 🔐 Step 1: Configure AWS CLI

On aws-client:

```
showcreds
```

Then configure:

```
aws configure
```

Enter:
- AWS Access Key
- AWS Secret Key
- Region: `us-east-1`
- Output format: json

Verify region:

```
aws configure get region
```

Expected output:
```
us-east-1
```

---

# 🚀 Implementation Steps

---

## Step 2: Create New Private S3 Bucket

```
aws s3api create-bucket \
  --bucket datacenter-sync-5444 \
  --region us-east-1
```

Verify bucket creation:

```
aws s3 ls
```

---

## Step 3: Verify Existing Bucket Data

Check source bucket contents:

```
aws s3 ls s3://datacenter-s3-18704 --recursive
```

Count total objects:

```
aws s3 ls s3://datacenter-s3-18704 --recursive | wc -l
```

---

## Step 4: Perform Data Migration (Recommended Method: sync)

Use sync for accurate and efficient transfer:

```
aws s3 sync s3://datacenter-s3-18704 s3://datacenter-sync-5444
```

This:
- Copies all objects
- Preserves structure
- Avoids duplicate transfers
- Handles large datasets efficiently

---

## Step 5: Verify Data in Destination Bucket

List destination bucket:

```
aws s3 ls s3://datacenter-sync-5444 --recursive
```

Count objects:

```
aws s3 ls s3://datacenter-sync-5444 --recursive | wc -l
```

Compare object count with source bucket.

Counts must match.

---

# 🔍 Advanced Data Verification (Recommended)

### Compare Object Size Totals

Source:

```
aws s3 ls s3://datacenter-s3-18704 --recursive --human-readable --summarize
```

Destination:

```
aws s3 ls s3://datacenter-sync-5444 --recursive --human-readable --summarize
```

Check:
- Total Objects
- Total Size

Both must be identical.

---

# 🔒 Ensure Bucket is Private

Check public access block:

```
aws s3api get-public-access-block \
  --bucket datacenter-sync-5444
```

If not enabled, enforce:

```
aws s3api put-public-access-block \
  --bucket datacenter-sync-5444 \
  --public-access-block-configuration \
  BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true
```

---

# ✅ Final Validation Checklist

- [ ] Region is `us-east-1`
- [ ] Bucket `datacenter-sync-5444` created
- [ ] Data migrated successfully
- [ ] Object counts match
- [ ] Total size matches
- [ ] Bucket is private

---

# 🎯 Result

Successfully migrated all data from:

```
datacenter-s3-18704
```

To:

```
datacenter-sync-5444
```

With complete data integrity and verification using AWS CLI.

---

# 🧠 DevOps Learning Outcome

- S3 bucket creation via CLI
- Efficient bulk data migration using `aws s3 sync`
- Data verification strategies
- Ensuring secure bucket configuration
- Working with AWS in production-like environments
