# Day 4: AWS Elastic IP Allocation

## 📝 Task Details
The Nautilus DevOps team needs a static entry point for their public-facing application to ensure the IP address remains consistent even if the underlying server changes.
* **Task:** Allocate an Elastic IP (EIP).
* **Resource Name:** `nautilus-eip`
* **Region:** `us-east-1`

---

## 📚 Theory: Elastic IPs
* **Dynamic vs. Static:** Standard EC2 Public IPs are dynamic; they change every time you stop/start the instance. An Elastic IP is static and never changes until you release it.
* **Remapping:** EIPs allow you to mask instance failures by programmatically remapping your public IP address to a new instance.
* **Billing/Cost:** An EIP is free *only* if it is attached to a running instance. If you allocate an EIP and leave it unattached (or attached to a stopped instance), AWS charges an hourly fee to discourage hoarding IPv4 addresses.
* **Naming via Tags:** EIPs do not have a native "Name" property. To name one, you must assign a **Tag** with the Key `Name` and the desired Value.

---

## ⚙️ Solution

### Method 1: AWS Console (GUI)
1.  Navigate to **EC2 Dashboard** > **Network & Security** > **Elastic IPs**.
2.  Clicked **Allocate Elastic IP address**.
3.  **Region:** `us-east-1`.
4.  **Tags:**
    * Added a tag to satisfy the naming requirement.
    * **Key:** `Name`
    * **Value:** `nautilus-eip`
5.  Clicked **Allocate**.

### Method 2: AWS CLI (Command Line)
*Allocating the IP and applying the Name tag in a single command.*

```bash
aws ec2 allocate-address \
    --domain vpc \
    --tag-specifications 'ResourceType=elastic-ip,Tags=[{Key=Name,Value=nautilus-eip}]' \
    --region us-east-1
```
## ✅ Verification
I verified the allocation and the tag using the AWS CLI.

### 1. Verification Command:

```Bash

aws ec2 describe-addresses --filters "Name=tag:Name,Values=nautilus-eip" --region us-east-1
```
### 2. Output Confirmation: 
The CLI returned the JSON details for the IP, confirming the PublicIp and the Tags list containing the name nautilus-eip.

``` JSON

{
    "Addresses": [
        {
            "PublicIp": "54.123.45.67",
            "AllocationId": "eipalloc-0abcd1234efgh5678",
            "Domain": "vpc",
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "nautilus-eip"
                }
            ],
            "PublicIpv4Pool": "amazon"
        }
    ]
}
```