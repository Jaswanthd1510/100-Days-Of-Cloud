# Day 13: Creating an AMI (Amazon Machine Image)

## 📝 Task Details
The Nautilus DevOps team needs to capture the current state of their configured server to enable backups and future scaling.
* **Task:** Create an AMI from an existing EC2 instance.
* **Source Instance:** `nautilus-ec2`
* **AMI Name:** `nautilus-ec2-ami`
* **Requirement:** Wait for AMI state to be `available`.

---

## 📚 Theory: AMI Creation
* **AMI (Amazon Machine Image):** A template that contains the software configuration (operating system, application server, and applications) required to launch an instance.
* **Golden Image Strategy:** In DevOps, we often configure one server perfectly (the "Golden Image"), create an AMI from it, and then use that AMI to launch all future production servers.
* **Rebooting:** By default, AWS stops the instance, takes snapshots of attached volumes, creates and registers the AMI, and then reboots the instance. This ensures data consistency (file system integrity).

---

## ⚙️ Solution

### Method 1: AWS Console (GUI)
1.  Selected the running instance `nautilus-ec2`.
2.  Navigated to **Actions** > **Image and templates** > **Create image**.
3.  **Settings:**
    * **Name:** `nautilus-ec2-ami`
    * **Reboot:** Enabled (Default)
4.  Clicked **Create image**.
5.  Monitored the **AMIs** dashboard until status changed from `pending` to `available`.

### Method 2: AWS CLI
```bash
aws ec2 create-image \
    --instance-id i-0123456789abcdef0 \
    --name "nautilus-ec2-ami" \
    --description "Golden Image from nautilus-ec2"
```
## ✅ Verification
I verified the AMI exists and is available using the CLI.

### 1. Verification Command:

```Bash

aws ec2 describe-images \
    --filters "Name=name,Values=nautilus-ec2-ami" \
    --query "Images[*].[ImageId, State, Name]"
```
### 2. Output Confirmation: 
The state is confirmed as available.

```JSON

[
    [
        "ami-0abcd1234efgh5678",
        "available",
        "nautilus-ec2-ami"
    ]
]
```