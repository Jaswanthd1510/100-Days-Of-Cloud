# Day 1: AWS Key Pair Creation

## 📝 Task Details
The Nautilus DevOps team requires a secure method to connect to their future infrastructure.
* **Task:** Create a Key Pair for EC2 authentication.
* **Resource Name:** `nautilus-kp`
* **Type:** RSA
* **Region:** `us-east-1` (North Virginia)

---

## 📚 Theory: AWS Key Pairs
* **What is it?** A Key Pair consists of a **Public Key** (stored by AWS on the server) and a **Private Key** (stored by you, the user). It replaces traditional passwords for SSH access.
* **RSA:** The standard encryption algorithm used to generate these keys.
* **Regionality:** Key Pairs are strictly regional resources. A key created in `us-east-1` does not exist in `us-west-2`. You must be in the correct region to use it.
* **Security:** The private key (`.pem` file) is shown/downloaded **only once** at the time of creation. If lost, it cannot be recovered.

---

## ⚙️ Solution

### Method 1: AWS Console (GUI)
1.  Logged into the AWS Management Console.
2.  Navigated to the **EC2 Dashboard**.
3.  In the left sidebar, under **Network & Security**, selected **Key Pairs**.
4.  Clicked **Create key pair**.
5.  **Settings Used:**
    * Name: `nautilus-kp`
    * Key pair type: `RSA`
    * Private key file format: `.pem`
6.  Clicked **Create key pair** and securely saved the downloaded file.

### Method 2: AWS CLI (Command Line)
*If creating via CLI, this command generates the key and saves the private key file locally.*
```bash
aws ec2 create-key-pair \
    --key-name nautilus-kp \
    --key-type rsa \
    --region us-east-1 \
    --query 'KeyMaterial' \
    --output text > nautilus-kp.pem
```
## AWS CLI (Verification)
Since the task requires the key to be created, I verified its existence using the command line to ensure it was registered correctly in the region.

**Command:**
```bash
aws ec2 describe-key-pairs --key-names nautilus-kp --region us-east-1
```
