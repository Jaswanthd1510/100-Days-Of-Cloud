# Day 11: Attaching a Network Interface (ENI)

## 📝 Task Details
The Nautilus DevOps team needs to add a secondary network interface to a running instance to support a new network configuration.
* **Task:** Attach an Elastic Network Interface (ENI) to an EC2 Instance.
* **Instance Name:** `xfusion-ec2`
* **ENI Name:** `xfusion-eni`
* **Region:** `us-east-1`
* **Condition:** Wait for instance initialization and verify status is "attached".

---

## 📚 Theory: Elastic Network Interfaces (ENI)
* **ENI:** A logical networking component in a VPC that represents a virtual network card.
* **Attributes:** An ENI has a primary private IPv4 address, one or more secondary private IPv4 addresses, a MAC address, and security groups.
* **Use Cases:**
    * Creating management networks.
    * Using network and security appliances in your VPC.
    * Moving an IP/MAC address between instances for failover.

---

## ⚙️ Solution

### Method 1: AWS Console (GUI)
1.  Verified `xfusion-ec2` was running and initialized.
2.  Navigated to **EC2** > **Network Interfaces**.
3.  Selected the interface `xfusion-eni`.
4.  Clicked **Actions** > **Attach**.
5.  Selected instance `xfusion-ec2` and confirmed attachment.

### Method 2: AWS CLI
```bash
# Fetch IDs
instance_id=$(aws ec2 describe-instances --filters "Name=tag:Name,Values=xfusion-ec2" --query "Reservations[0].Instances[0].InstanceId" --output text)
eni_id=$(aws ec2 describe-network-interfaces --filters "Name=tag:Name,Values=xfusion-eni" --query "NetworkInterfaces[0].NetworkInterfaceId" --output text)

# Attach (Index 1 is the secondary slot)
aws ec2 attach-network-interface \
    --network-interface-id $eni_id \
    --instance-id $instance_id \
    --device-index 1
```
## ✅ Verification
I verified the attachment by checking the status of the ENI.

### 1. Verification Command:

```Bash

aws ec2 describe-network-interfaces \
    --network-interface-ids $eni_id \
    --query "NetworkInterfaces[*].[NetworkInterfaceId, Status, Attachment.InstanceId]"
```
### 2. Output Confirmation: 
The status returned as in-use and linked to the correct instance ID.

```JSON

[
    [
        "eni-0abcd1234efgh5678",
        "in-use",
        "i-0123456789abcdef0"
    ]
]
```