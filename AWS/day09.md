# Day 9: Enabling EC2 Termination Protection

## 📝 Task Details
The Nautilus DevOps team identified a critical instance that was missing safety guardrails.
* **Task:** Enable Termination Protection.
* **Instance Name:** `devops-ec2`
* **Region:** `us-east-1`
* **Objective:** Ensure the instance cannot be permanently deleted (terminated) via the AWS Console or API without first disabling this setting.

---

## 📚 Theory: Termination Protection
* **Termination:** The process of permanently deleting an EC2 instance. By default, this also deletes the root EBS volume, resulting in total data loss.
* **Termination Protection (`DisableApiTermination`):** A boolean attribute that locks the instance against termination.
    * **Use Case:** Always recommended for production databases, stateful servers, and "Pet" instances (servers that are manually configured and hard to replace).
    * **Difference from Stop Protection:** Stop protection keeps the server *running*. Termination protection keeps the server *existing*.

---

## ⚙️ Solution

### Method 1: AWS Console (GUI)
1.  Navigate to **EC2 Dashboard** > **Instances**.
2.  Selected the `devops-ec2` instance.
3.  Went to **Actions** > **Instance settings** > **Change termination protection**.
4.  Checked **Enable** and clicked **Save**.

### Method 2: AWS CLI (Command Line)
```bash
aws ec2 modify-instance-attribute \
    --instance-id i-0123456789abcdef0 \
    --disable-api-termination
```
## ✅ Verification
I verified the protection was active by querying the instance attribute.

### 1. Verification Command:

```Bash

aws ec2 describe-instance-attribute \
    --instance-id i-0123456789abcdef0 \
    --attribute disableApiTermination
```
### 2. Output Confirmation: 
The CLI returned "True", confirming termination protection is enabled.

```JSON

{
    "InstanceId": "i-0123456789abcdef0",
    "DisableApiTermination": {
        "Value": true
    }
}
```