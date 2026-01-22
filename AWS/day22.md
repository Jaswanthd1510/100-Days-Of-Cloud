# Day 22: Bootstrapping Root Access with User Data

## 📝 Task Details
The Nautilus DevOps team requires a specialized instance with direct root access for management purposes.
* **Task:** Launch EC2 and configure passwordless SSH for `root`.
* **Instance Name:** `devops-ec2`
* **Key Location:** `/root/.ssh/` on `aws-client`.
* **Mechanism:** Use EC2 User Data to inject the key and modify SSH config.

---

## 📚 Theory: User Data & Cloud-Init
* **User Data:** Metadata passed to the instance at launch. Linux instances use `cloud-init` to execute this data. It is the standard way to "bootstrap" or pre-configure servers (installing packages, setting users) without manually logging in.
* **Root Login Security:** By default, `PermitRootLogin` is set to `no` or `prohibit-password` in `/etc/ssh/sshd_config`. To allow root access, this configuration must be changed, and the SSH service restarted.

---

## ⚙️ Solution

### 1. Key Generation (on Client)
Generated the RSA key pair on the client host:
```bash
ssh-keygen -t rsa -f /root/.ssh/id_rsa -N ""
cat /root/.ssh/id_rsa.pub
# Copied the output for the User Data script
```
### 2. Launch Instance (Console)
Launched devops-ec2 (t2.micro, Ubuntu) using the AWS Console. Under Advanced details, I pasted the following User Data script:

```Bash

#!/bin/bash
# 1. Create .ssh directory for root
mkdir -p /root/.ssh

# 2. Inject the specific public key
echo "ssh-rsa AAAAB3NzaC1yc2E..." >> /root/.ssh/authorized_keys
chmod 600 /root/.ssh/authorized_keys

# 3. Configure SSH to allow Root Login
sed -i 's/^#PermitRootLogin.*/PermitRootLogin yes/' /etc/ssh/sshd_config
sed -i 's/^PermitRootLogin.*/PermitRootLogin yes/' /etc/ssh/sshd_config
sed -i 's/^PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config

# 4. Restart SSH
service sshd restart || service ssh restart
```
### 3. Security Group Fix (CLI)
The default security group blocked SSH. I used the CLI to force port 22 open:

```Bash

aws ec2 authorize-security-group-ingress \
    --group-id sg-02cbee1cdc43fa36d \
    --protocol tcp \
    --port 22 \
    --cidr 0.0.0.0/0
```
## ✅ Verification
I verified access by SSHing directly as root from the client machine without a password.

### 1. Verification Command:

```Bash

ssh root@<PUBLIC_IP>
```
### 2. Success Criteria: Logged in successfully as root@ip-172-x-x-x.

