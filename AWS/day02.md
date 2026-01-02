# Day 2: AWS Security Groups

## 📝 Task Details
The Nautilus DevOps team requires a network security layer for their future application servers.
* **Task:** Create a Security Group (SG).
* **Resource Name:** `xfusion-sg`
* **Description:** "Security group for Nautilus App Servers"
* **VPC:** Default VPC
* **Inbound Rules:**
    * **HTTP (80):** From `0.0.0.0/0` (Anywhere)
    * **SSH (22):** From `0.0.0.0/0` (Anywhere)
* **Region:** `us-east-1`

---

## 📚 Theory: Security Groups
* **What is it?** A Security Group acts as a **virtual firewall** for EC2 instances to control incoming and outgoing traffic.
* **Stateful:** Security Groups are stateful. If you allow an incoming request (Inbound), the response (Outbound) is automatically allowed, regardless of outbound rules.
* **CIDR `0.0.0.0/0`:** This notation represents "Any IP address," effectively allowing access from the entire internet.
* **Ports:**
    * **Port 80 (HTTP):** Standard port for unencrypted web traffic.
    * **Port 22 (SSH):** Standard port for secure remote login.

---

## ⚙️ Solution

### Method 1: AWS Console (GUI)
1.  Navigate to **EC2 Dashboard** > **Security Groups**.
2.  Click **Create security group**.
3.  **Basic Details:**
    * Name: `xfusion-sg`
    * Description: "Security group for Nautilus App Servers"
    * VPC: Selected the default VPC.
4.  **Inbound Rules:**
    * Added Rule 1: Type `HTTP`, Source `Anywhere-IPv4` (`0.0.0.0/0`).
    * Added Rule 2: Type `SSH`, Source `Anywhere-IPv4` (`0.0.0.0/0`).
5.  Clicked **Create security group**.

### Method 2: AWS CLI (Command Line)
*Commands to create the group and add rules programmatically.*

**Step 1: Create the Group**
```bash
aws ec2 create-security-group \
    --group-name xfusion-sg \
    --description "Security group for Nautilus App Servers" \
    --region us-east-1
```

**Step 2: Add Inbound Rules**
``` bash
# Allow HTTP (Port 80)
aws ec2 authorize-security-group-ingress \
    --group-name xfusion-sg \
    --protocol tcp --port 80 --cidr 0.0.0.0/0 \
    --region us-east-1

# Allow SSH (Port 22)
aws ec2 authorize-security-group-ingress \
    --group-name xfusion-sg \
    --protocol tcp --port 22 --cidr 0.0.0.0/0 \
    --region us-east-1

```
## ✅ Verification
I verified the Security Group configuration using the AWS CLI to ensure the rules were applied correctly.

**1. Verification Command:**

``` Bash

aws ec2 describe-security-groups --group-names xfusion-sg --region us-east-1
```

**2. Output Confirmation:**
 The CLI returned a JSON object listing the IpPermissions (Inbound Rules), confirming that ports 80 and 22 are open to 0.0.0.0/0.

``` JSON

{
    "SecurityGroups": [
        {
            "GroupName": "xfusion-sg",
            "IpPermissions": [
                {
                    "FromPort": 80,
                    "IpProtocol": "tcp",
                    "IpRanges": [{"CidrIp": "0.0.0.0/0"}],
                    "ToPort": 80
                },
                {
                    "FromPort": 22,
                    "IpProtocol": "tcp",
                    "IpRanges": [{"CidrIp": "0.0.0.0/0"}],
                    "ToPort": 22
                }
            ]
        }
    ]
}
```