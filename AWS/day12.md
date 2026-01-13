# Day 12: Attaching EBS Volume to Instance

## 📝 Task Details
The Nautilus DevOps team needs to mount additional storage to their datacenter server for data processing.
* **Task:** Attach an existing EBS Volume to an EC2 Instance.
* **Instance Name:** `datacenter-ec2`
* **Volume Name:** `datacenter-volume`
* **Device Name:** `/dev/sdb`
* **Region:** `us-east-1`

---

## 📚 Theory: EBS Attachment
* **Attachment:** Linking an EBS volume to an EC2 instance. This allows the OS to see the volume as a raw block device.
* **Device Naming:** AWS requires a device name (e.g., `/dev/sdh`, `/dev/xvdh`) to identify the volume during attachment.
    * While AWS uses these names for the API, the internal OS kernel might map them differently (e.g., `/dev/sdb` might show up as `/dev/xvdb` in Amazon Linux).
* **Hot Attach:** EBS volumes can be attached while the instance is running.

---

## ⚙️ Solution

### Method 1: AWS Console (GUI)
1.  Navigated to **EC2** > **Volumes**.
2.  Selected `datacenter-volume`.
3.  Clicked **Actions** > **Attach volume**.
4.  Selected instance `datacenter-ec2`.
5.  **Important:** Changed the **Device name** field to `/dev/sdb`.
6.  Clicked **Attach volume**.

### Method 2: AWS CLI
```bash
# Fetch IDs
instance_id=$(aws ec2 describe-instances --filters "Name=tag:Name,Values=datacenter-ec2" --query "Reservations[0].Instances[0].InstanceId" --output text)
volume_id=$(aws ec2 describe-volumes --filters "Name=tag:Name,Values=datacenter-volume" --query "Volumes[0].VolumeId" --output text)

# Attach with specific device name
aws ec2 attach-volume \
    --volume-id $volume_id \
    --instance-id $instance_id \
    --device /dev/sdb
```
## ✅ Verification
I verified the volume state changed to in-use and is attached to the correct instance.

### 1. Verification Command:

```Bash

aws ec2 describe-volumes \
    --volume-ids $volume_id \
    --query "Volumes[*].Attachments[*].[InstanceId, Device, State]"
```
### 2. Output Confirmation:

```JSON

[
    [
        [
            "i-0123456789abcdef0",
            "/dev/sdb",
            "attached"
        ]
    ]
]
```