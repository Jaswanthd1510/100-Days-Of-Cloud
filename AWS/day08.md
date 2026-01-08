# Day 8: Enabling EC2 Stop Protection

## 📝 Task Details
The Nautilus DevOps team needs to prevent accidental shutdowns of a critical server instance.
* **Task:** Enable Stop Protection.
* **Instance Name:** `nautilus-ec2`
* **Region:** `us-east-1`
* **Objective:** Ensure the instance cannot be stopped via the AWS Console or API without first disabling this setting.

---

## 📚 Theory: Stop Protection
* **Stop vs. Terminate:**
    * **Stop:** Shuts down the instance (OS level), persists data on EBS.
    * **Terminate:** Deletes the instance and (usually) its data.
* **Stop Protection (`DisableApiStop`):** A security feature that rejects any API call or Console action attempting to stop the instance. This acts as a "guardrail" against accidental clicks or automated scripts shutting down critical production workloads.

---

## ⚙️ Solution

### Method 1: AWS Console (GUI)
1.  Navigate to **EC2 Dashboard** > **Instances**.
2.  Selected the `nautilus-ec2` instance.
3.  Went to **Actions** > **Instance settings** > **Change stop protection**.
4.  Checked **Enable** and clicked **Save**.

### Method 2: AWS CLI (Command Line)
```bash
aws ec2 modify-instance-attribute \
    --instance-id i-0123456789abcdef0 \
    --disable-api-stop
```
## ✅ Verification
I verified the protection was active by attempting to stop the instance via CLI (dry-run) or checking the attribute.

### 1. Verification Command:

```Bash

aws ec2 describe-instance-attribute \
    --instance-id i-0123456789abcdef0 \
    --attribute disableApiStop
```
### 2. Output Confirmation: 
The CLI returned "True", confirming stop protection is enabled.

```JSON

{
    "InstanceId": "i-0123456789abcdef0",
    "DisableApiStop": {
        "Value": true
    }
}
```