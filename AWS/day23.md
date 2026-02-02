# Day 23: S3 Bucket Migration

## 📝 Task Details
The Nautilus DevOps team needs to migrate data to a new storage location for better organization and redundancy.
* **Task:** Create a new bucket and sync data from an existing one.
* **Source Bucket:** `nautilus-s3-26103`
* **Destination Bucket:** `nautilus-sync-17993`
* **Tool:** AWS CLI

---

## 📚 Theory: AWS S3 Sync
* **`aws s3 sync`:** A powerful CLI command that recursively copies new and updated files from the source directory to the destination.
* **Differential Transfer:** unlike `cp` (copy), `sync` only transfers files that are missing or have a different file size/modified time in the destination. This makes it idempotent and ideal for migrations.
* **Verification:** Using `--recursive --summarize` allows for a quick audit of the total object count and size to ensure data integrity.

---

## ⚙️ Solution

### 1. Create Destination Bucket
```bash
aws s3 mb s3://nautilus-sync-17993
```
### 2. Perform Migration
```Bash

aws s3 sync s3://nautilus-s3-26103 s3://nautilus-sync-17993
```
### 3. Verification
Checked that the object count and total size matched in both locations.

### Source Check:

```Bash

aws s3 ls s3://nautilus-s3-26103 --recursive --summarize
```
### Destination Check:

```Bash

aws s3 ls s3://nautilus-sync-17993 --recursive --summarize
```