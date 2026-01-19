# Day 14: Terminating an EC2 Instance

## 📝 Task Details
The Nautilus DevOps team is cleaning up obsolete resources to optimize costs and remove unused infrastructure.
* **Task:** Delete (Terminate) an EC2 Instance.
* **Instance Name:** `nautilus-ec2`
* **Region:** `us-east-1`
* **Requirement:** Ensure the instance reaches the `terminated` state.

---

## 📚 Theory: Instance Lifecycle
* **Termination:** The irreversible process of shutting down and deleting an EC2 instance.
* **Cleanup:**
    * **Root Volume:** By default, the root EBS volume is deleted upon termination.
    * **Elastic IPs:** Any attached Elastic IPs are disassociated but *not* released (they remain in your account and may incur costs if not released manually).
* **Termination Protection:** A safety feature that prevents API calls from terminating an instance. This must be disabled before a termination request can succeed.

---

## ⚙️ Solution

### Method 1: AWS Console (GUI)
1.  Selected `nautilus-ec2`.
2.  **Safety Check:** Went to **Actions** > **Instance settings** > **Change termination protection** and verified it was disabled.
3.  Selected **Instance State** > **Terminate Instance**.
4.  Confirmed the deletion prompt.
5.  Waited for the status to transition from `shutting-down` to `terminated`.

### Method 2: AWS CLI
```bash
# Disable protection and terminate
aws ec2 modify-instance-attribute --instance-id i-0123456789abcdef0 --no-disable-api-termination
aws ec2 terminate-instances --instance-ids i-0123456789abcdef0
```
## ✅ Verification
I verified the instance state is terminated.

### 1. Verification Command:

```Bash

aws ec2 describe-instances \
    --filters "Name=tag:Name,Values=nautilus-ec2" \
    --query "Reservations[*].Instances[*].[InstanceId, State.Name]"
```
### 2. Output Confirmation:

``` JSON

[
    [
        "i-0123456789abcdef0",
        "terminated"
    ]
]
```