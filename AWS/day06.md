# Day 6: Launching an EC2 Instance

## 📝 Task Details
The Nautilus DevOps team needs a basic compute instance for testing their application deployment scripts.
* **Task:** Launch an EC2 Instance.
* **Name:** `nautilus-ec2`
* **AMI:** Amazon Linux
* **Type:** `t2.micro`
* **Key Pair:** Create new `nautilus-kp` (RSA)
* **Security Group:** Default

---

## 📚 Theory: EC2 Fundamentals
* **AMI (Amazon Machine Image):** The blueprint for the instance, containing the OS and pre-installed software. Amazon Linux is a downstream version of Fedora/RHEL, optimized for AWS.
* **Instance Type (`t2.micro`):** Defines the hardware capacity. `t2` instances are "burstable," meaning they earn CPU credits during idle times and spend them when high performance is needed.
* **Key Pairs:** AWS uses public-key cryptography to secure the login information for instances. AWS stores the public key, and you store the private key (`.pem`).

---

## ⚙️ Solution

### Method 1: AWS Console (GUI)
1.  **Launch Instance:** Navigated to EC2 > Instances > Launch Instance.
2.  **Config:**
    * **Name:** `nautilus-ec2`
    * **AMI:** Amazon Linux 2023 AMI
    * **Instance Type:** `t2.micro`
3.  **Key Pair:**
    * Selected "Create new key pair".
    * Name: `nautilus-kp`, Type: RSA, Format: .pem.
4.  **Network:**
    * Selected "Select existing security group".
    * Chose the `default` security group.
5.  Clicked **Launch instance**.

### Method 2: AWS CLI (Command Line)
*Step 1: Create the Key Pair*
```bash
aws ec2 create-key-pair --key-name nautilus-kp --query 'KeyMaterial' --output text > nautilus-kp.pem
chmod 400 nautilus-kp.pem
```
### Step 2: Launch the Instance

```Bash

# Note: AMI ID varies by region/time. This example uses a common AL2023 AMI for us-east-1.
aws ec2 run-instances \
    --image-id ami-01816d07b1128cd2d \
    --count 1 \
    --instance-type t2.micro \
    --key-name nautilus-kp \
    --security-groups default \
    --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=nautilus-ec2}]' \
    --region us-east-1
```
## ✅ Verification
I verified the instance state and attributes using the AWS CLI.

### 1. Verification Command:

```Bash

aws ec2 describe-instances --filters "Name=tag:Name,Values=nautilus-ec2" --region us-east-1
```
### 2. Output Confirmation: 
The CLI returned the JSON object confirming the instance ID, t2.micro type, and running state.

```JSON

{
    "Reservations": [
        {
            "Instances": [
                {
                    "InstanceId": "i-0123456789abcdef0",
                    "InstanceType": "t2.micro",
                    "KeyName": "nautilus-kp",
                    "State": { "Name": "running" },
                    "Tags": [{ "Key": "Name", "Value": "nautilus-ec2" }]
                }
            ]
        }
    ]
}
```