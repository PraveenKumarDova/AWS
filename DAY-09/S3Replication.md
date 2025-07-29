# Amazon S3 Cross-Region & Same-Region Replication – Complete Guide

This document provides a complete explanation of **Amazon S3 Data Replication** features including setup, use cases, configuration steps, and validation. Ideal for production planning or teaching sessions.

---

## What is Amazon S3 Replication?

**Amazon S3 Replication** automatically copies objects across S3 buckets, either within the same AWS region (SRR) or across regions (CRR).

### Types of Replication

| Type                               | Description                                  |
| ---------------------------------- | -------------------------------------------- |
| **CRR** (Cross-Region Replication) | Replicates objects to a different AWS region |
| **SRR** (Same-Region Replication)  | Replicates objects within the same region    |

---

## Key Use Cases

* **Disaster recovery**: Replication across regions protects data from region-wide outages
* **Data locality**: Store copies closer to users
* **Backup compliance**: Meet regulations by duplicating data
* **Log aggregation**: Consolidate logs from multiple buckets to a central bucket

---

## Prerequisites

1. Both **source and destination buckets must exist**
2. **Versioning must be enabled** on both source and destination
3. The source bucket **must have permission** to replicate to destination bucket (via IAM role)
4. Optional: **Bucket policies or KMS permissions** if encryption is used

---

## Step-by-Step: Configure Cross-Region Replication (CRR)

### Step 1: Create Destination Bucket (in another region)

1. Go to S3 → Create Bucket
2. Use a unique name (e.g., `my-bucket-replica`) and select **another region**
3. Enable **Versioning**
4. Block Public Access (recommended)

### Step 2: Enable Versioning on Source Bucket

1. Open the source bucket → **Properties** → **Versioning**
2. Click **Enable** → Save changes

### Step 3: Setup Replication Rule on Source Bucket

1. Open **Source bucket → Management → Replication Rules → Create Rule**
2. Enter Rule Name
3. Choose:

   * **Apply to all objects** or
   * Filter by prefix or tag
4. Destination:

   * Choose destination bucket
   * Choose destination region (for CRR)
   * **Enable replication of delete markers** (optional)
   * **Enable replica ownership override** (optional)
5. IAM role:

   * Choose existing or let S3 create a new one
6. Review and **Create Rule**

---

## Verifying Replication

1. Upload a test object into source bucket
2. Wait for replication (few seconds to minutes)
3. Check **Destination bucket**
4. You will see a **replica** of the object with the same key and metadata

> Note: Only new uploads after replication rule creation are replicated. Existing objects are not replicated retroactively.

---

## Monitoring Replication

* Enable **CloudWatch metrics** or **S3 Event Notifications**
* Use **Replication Status** metadata on object:

  * `PENDING` → waiting to be replicated
  * `COMPLETED` → successfully replicated
  * `FAILED` → error during replication

---

## Same-Region Replication (SRR)

Use the same steps as CRR, but destination bucket must be in the **same AWS region**.

Useful when:

* Aggregating logs into a central bucket
* Keeping backup copies for compliance


Notes & Best Practices

* Replication can only copy new/updated objects
* Delete operations can be replicated if configured
* Replication supports **Object Lock** (compliance mode)
* Lifecycle rules apply independently to source and destination
* Replicated objects show **replica** status in their metadata

---

## Summary Table

| Feature                 | CRR                           | SRR                      |
| ----------------------- | ----------------------------- | ------------------------ |
| Region                  | Across Regions                | Within Same Region       |
| Use Case                | DR, compliance, data locality | Log aggregation, backups |
| Versioning Required     | Yes                           | Yes                      |
| Default Replicates      | New objects only              | New objects only         |
| Encryption Supported    | Yes (SSE-S3, SSE-KMS)         | Yes                      |
| Retroactive Replication | No                            | No                       |
