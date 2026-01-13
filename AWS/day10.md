# Day 10: Attaching Elastic IP to Instance

## 📝 Task Details
The Nautilus DevOps team needs to assign a static IP address to their datacenter instance to ensure consistent access.
* **Task:** Associate an Elastic IP to an EC2 Instance.
* **Elastic IP Name:** `datacenter-ec2-eip`
* **Instance Name:** `datacenter-ec2`
* **Region:** `us-east-1`

---

## 📚 Theory: IP Association
* **Elastic IP Association:** The process of linking a static IPv4 address (EIP) to a running instance.
* **Effect:** The instance loses its previous dynamic public IP and immediately adopts the EIP. This address stays with the instance even through stop/start cycles.

---

## ⚙️ Solution

### Method 1: AWS Console (GUI)
1.  Navigated to **EC2 Dashboard** > **Elastic IPs**.
2.  Identified and selected the EIP `datacenter-ec2-eip`.
3.  Clicked **Actions** > **Associate Elastic IP address**.
4.  Selected the instance `datacenter-ec2`.
5.  Clicked **Associate**.

### Method 2: AWS CLI
```bash
# Fetch IDs and Associate
eip_id=$(aws ec2 describe-addresses --filters "Name=tag:Name,Values=datacenter-ec2-eip" --query "Addresses[0].AllocationId" --output text)
instance_id=$(aws ec2 describe-instances --filters "Name=tag:Name,Values=datacenter-ec2" --query "Reservations[0].Instances[0].InstanceId" --output text)

aws ec2 associate-address --allocation-id $eip_id --instance-id $instance_id
```
## ✅ Verification
I verified the association by checking the instance's public IP.

### 1. Verification Command:

``` Bash

aws ec2 describe-instances \
    --filters "Name=tag:Name,Values=datacenter-ec2" \
    --query "Reservations[*].Instances[*].[InstanceId, PublicIpAddress]"
```
### 2. Output Confirmation: 
The Public IP listed matched the Elastic IP address.