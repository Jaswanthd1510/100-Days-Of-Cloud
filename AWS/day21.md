# Day 21: Full Provisioning (Instance + EIP)

## 📝 Task Details
The Nautilus DevOps team requires a stable application server with a persistent entry point.
* **Task:** Launch an Instance and Associate an Elastic IP.
* **Instance Name:** `devops-ec2`
* **AMI:** Ubuntu Linux
* **Instance Type:** `t2.micro`
* **Elastic IP Name:** `devops-eip`

---

## 📚 Theory: Static Addressing
* **Elastic IP (EIP):** A static IPv4 address designed for dynamic cloud computing. An EIP mask the failure of an instance or software by allowing you to rapidly remap the address to another instance in your account.
* **Association:** The binding process that assigns the EIP to the network interface of an EC2 instance. This replaces the instance's default public IP.

---

## ⚙️ Solution

### Method 1: AWS Console (GUI)
1.  **Launch Instance:**
    * Launched an Ubuntu `t2.micro` instance named `devops-ec2`.
2.  **Allocate IP:**
    * Allocated a new Elastic IP.
    * Tagged it with Key: `Name`, Value: `devops-eip`.
3.  **Association:**
    * Selected the EIP and associated it with the running `devops-ec2` instance.

### Method 2: AWS CLI
```bash
# Launch
instance_id=$(aws ec2 run-instances --image-id ami-04b70fa74e45c3917 --instance-type t2.micro --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=devops-ec2}]' --output text --query 'Instances[0].InstanceId')

# Allocate
eip_id=$(aws ec2 allocate-address --tag-specifications 'ResourceType=elastic-ip,Tags=[{Key=Name,Value=devops-eip}]' --output text --query 'AllocationId')

# Associate
aws ec2 associate-address --instance-id $instance_id --allocation-id $eip_id
```
## ✅ Verification
I verified the association by checking the instance's public IP configuration.

### 1. Verification Command:

```Bash

aws ec2 describe-instances \
    --filters "Name=tag:Name,Values=devops-ec2" \
    --query "Reservations[*].Instances[*].[InstanceId, PublicIpAddress]"
```
### 2. Output Confirmation: The Public IP listed matches the allocated Elastic IP.

```JSON

[
    [
        "i-0123456789abcdef0",
        "54.123.45.67"
    ]
]
```