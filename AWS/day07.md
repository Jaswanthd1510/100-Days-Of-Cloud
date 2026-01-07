# Day 7: Resizing an EC2 Instance

## 📝 Task Details
The Nautilus DevOps team identified an underutilized server and needs to downgrade its capacity to save costs.
* **Task:** Change Instance Type (Resize).
* **Instance Name:** `xfusion-ec2`
* **Current Type:** `t2.micro`
* **Target Type:** `t2.nano`
* **Requirement:** Ensure instance is in `running` state after the change.

---

## 📚 Theory: Vertical Scaling
* **Vertical Scaling:** The process of increasing (scaling up) or decreasing (scaling down) the compute power (CPU/RAM) of an existing server.
* **Stop-Change-Start:** AWS instances (EBS-backed) generally must be **Stopped** before their instance type can be changed. You are effectively moving the virtual hard drive to a different physical motherboard.
* **Instance Types:**
    * **t2.micro:** 1 vCPU, 1 GiB RAM.
    * **t2.nano:** 1 vCPU, 0.5 GiB RAM (Cheaper, for lighter workloads).

---

## ⚙️ Solution

### Method 1: AWS Console (GUI)
1.  **Verification:** Checked that `xfusion-ec2` status checks were "2/2 passed".
2.  **Stop:** Selected the instance > **Instance State** > **Stop Instance**. Waited for state to become `Stopped`.
3.  **Modify:**
    * Went to **Actions** > **Instance Settings** > **Change Instance Type**.
    * Changed type from `t2.micro` to `t2.nano`.
    * Applied changes.
4.  **Start:** Selected the instance > **Instance State** > **Start Instance**.

### Method 2: AWS CLI (Command Line)
```bash
# 1. Stop the instance
aws ec2 stop-instances --instance-ids i-0xxx...

# 2. Modify the attribute (after instance is fully stopped)
aws ec2 modify-instance-attribute \
    --instance-id i-0xxx... \
    --instance-type "{\"Value\": \"t2.nano\"}"

# 3. Start the instance
aws ec2 start-instances --instance-ids i-0xxx...
```
## ✅ Verification
I verified the change using the CLI to confirm the new type and running state.

### 1. Verification Command:

```Bash

aws ec2 describe-instances \
    --filters "Name=tag:Name,Values=xfusion-ec2" \
    --query "Reservations[*].Instances[*].[InstanceId, InstanceType, State.Name]"
```
### 2. Output Confirmation: 
The CLI returned the updated configuration showing t2.nano and running.

```JSON

[
    [
        "i-0123456789abcdef0",
        "t2.nano",
        "running"
    ]
]
```