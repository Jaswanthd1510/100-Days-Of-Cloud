# Day 5: AWS EBS Volume Creation

## 📝 Task Details
The Nautilus DevOps team needs a persistent storage unit for their data processing tasks.
* **Task:** Create an EBS Volume.
* **Resource Name:** `nautilus-volume`
* **Type:** `gp3` (General Purpose SSD)
* **Size:** `2 GiB`
* **Region:** `us-east-1`

---

## 📚 Theory: EBS (Elastic Block Store)
* **What is it?** EBS is block-level storage that acts like a network-attached hard drive for your EC2 instances. Unlike "Instance Store" (ephemeral) storage, EBS data persists even if the instance is stopped or terminated (depending on configuration).
* **gp3 vs gp2:**
    * **gp2:** Performance (IOPS) is tied to volume size. Bigger drive = Faster drive.
    * **gp3:** The modern standard. It decouples performance from storage capacity. You get a baseline of 3,000 IOPS and 125 MiB/s throughput regardless of size, making it cost-effective for small volumes.
* **Zone Lock:** EBS volumes are **Availability Zone (AZ) specific**. A volume created in `us-east-1a` cannot be attached to an instance in `us-east-1b`.

---

## ⚙️ Solution

### Method 1: AWS Console (GUI)
1.  Navigate to **EC2 Dashboard** > **Elastic Block Store** > **Volumes**.
2.  Click **Create volume**.
3.  **Settings Used:**
    * **Volume Type:** `General Purpose SSD (gp3)`
    * **Size:** `2 GiB`
    * **IOPS/Throughput:** Left at defaults (3000 IOPS, 125 MiB/s).
    * **Availability Zone:** `us-east-1a`
4.  **Tags:**
    * Added tag Key: `Name`, Value: `nautilus-volume`.
5.  Clicked **Create volume**.

### Method 2: AWS CLI (Command Line)
*Creating the volume with specific size, type, and tag.*

```bash
aws ec2 create-volume \
    --volume-type gp3 \
    --size 2 \
    --availability-zone us-east-1a \
    --tag-specifications 'ResourceType=volume,Tags=[{Key=Name,Value=nautilus-volume}]' \
    --region us-east-1
```
## ✅ Verification
I verified the volume creation and its attributes using the AWS CLI.

### 1. Verification Command:

```Bash

aws ec2 describe-volumes --filters "Name=tag:Name,Values=nautilus-volume" --region us-east-1
```
### 2. Output Confirmation: 
The CLI returned the JSON object confirming the VolumeType is gp3, Size is 2, and the State is available.

```JSON

{
    "Volumes": [
        {
            "AvailabilityZone": "us-east-1a",
            "Size": 2,
            "State": "available",
            "VolumeId": "vol-0abcd1234efgh5678",
            "VolumeType": "gp3",
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "nautilus-volume"
                }
            ]
        }
    ]
}
```