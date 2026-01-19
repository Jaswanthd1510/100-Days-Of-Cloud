# Day 15: Creating an EBS Snapshot

## 📝 Task Details
The Nautilus DevOps team is implementing a backup strategy for their critical data volumes.
* **Task:** Create a Snapshot of an existing EBS Volume.
* **Source Volume:** `nautilus-vol`
* **Snapshot Name:** `nautilus-vol-ss`
* **Description:** `nautilus Snapshot`
* **Requirement:** Wait for snapshot state to be `completed`.

---

## 📚 Theory: EBS Snapshots
* **Snapshot:** A point-in-time backup of an EBS volume stored in Amazon S3.
* **Incremental Nature:** Snapshots are incremental backups, meaning only the blocks on the device that have changed after your most recent snapshot are saved. This minimizes storage costs and time.
* **Durability:** Snapshots are region-specific but AZ-independent. A snapshot taken in `us-east-1a` can be used to restore a volume in `us-east-1b`.

---

## ⚙️ Solution

### Method 1: AWS Console (GUI)
1.  Navigated to **EC2** > **Volumes** and selected `nautilus-vol`.
2.  Clicked **Actions** > **Create snapshot**.
3.  **Settings:**
    * **Description:** `nautilus Snapshot`
    * **Tags:** Key=`Name`, Value=`nautilus-vol-ss`
4.  Clicked **Create snapshot**.
5.  Monitored the **Snapshots** dashboard until the status reached `completed`.

### Method 2: AWS CLI
```bash
# Create Snapshot with Description and Name Tag
aws ec2 create-snapshot \
    --volume-id vol-0abcd1234efgh5678 \
    --description "nautilus Snapshot" \
    --tag-specifications 'ResourceType=snapshot,Tags=[{Key=Name,Value=nautilus-vol-ss}]'
```
## ✅ Verification
I verified the snapshot status using the CLI.

### 1. Verification Command:

```Bash

aws ec2 describe-snapshots \
    --filters "Name=tag:Name,Values=nautilus-vol-ss" \
    --query "Snapshots[*].[SnapshotId, State, Progress]"
```
### 2. Output Confirmation:

```JSON

[
    [
        "snap-0123456789abcdef0",
        "completed",
        "100%"
    ]
]
```