# Day 3: AWS Subnet Creation

## 📝 Task Details
The Nautilus DevOps team is segmenting their network infrastructure to prepare for isolated workloads.
* **Task:** Create a Subnet inside the existing Default VPC.
* **Resource Name:** `devops-subnet`
* **VPC:** Default VPC (typically `172.31.0.0/16`)
* **Region/AZ:** `us-east-1a` (as per specific lab requirement)

---

## 📚 Theory: VPC & Subnets
* **VPC (Virtual Private Cloud):** A logically isolated virtual network in the AWS cloud. It acts as the "container" for all your network resources.
* **Subnet:** A segment of a VPC's IP address range where you can place groups of isolated resources.
    * **Availability Zone (AZ) Dependency:** A subnet resides entirely within one Availability Zone and cannot span zones. This ensures high availability planning.
* **CIDR (Classless Inter-Domain Routing):** The notation used to define IP ranges (e.g., `/24`).
    * **No Overlap Rule:** A new subnet's CIDR block must not overlap with any existing subnets in the same VPC. Since the Default VPC often uses the lower ranges (e.g., `172.31.0.0/20`), we must calculate a "safe" upper range (e.g., `172.31.100.0/24`) to avoid conflicts.

---

## ⚙️ Solution

### AWS Console (GUI)
1.  Navigate to the **VPC Dashboard** > **Subnets**.
2.  Click **Create subnet**.
3.  **VPC ID:** Selected the **Default VPC**.
4.  **Subnet Settings:**
    * **Subnet Name:** `devops-subnet`
    * **Availability Zone:** `us-east-1a`
    * **IPv4 CIDR Block:** `172.31.100.0/24`
    *(Note: Selected `100.0` to avoid overlapping with existing default subnets).*
5.  Clicked **Create subnet**.

## ✅ Verification
I verified the subnet creation and its association with the correct VPC and AZ using the AWS CLI.

### 1. Verification Command:

```Bash

aws ec2 describe-subnets --filters "Name=tag:Name,Values=devops-subnet" --region us-east-1
```
### 2. Output Confirmation: 
The CLI returned a JSON object confirming the SubnetId, CidrBlock (172.31.100.0/24), and AvailabilityZone (us-east-1a).

```JSON

{
    "Subnets": [
        {
            "AvailabilityZone": "us-east-1a",
            "CidrBlock": "172.31.100.0/24",
            "State": "available",
            "SubnetId": "subnet-0abcd1234efgh5678",
            "VpcId": "vpc-12345abcde",
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "devops-subnet"
                }
            ]
        }
    ]
}
```