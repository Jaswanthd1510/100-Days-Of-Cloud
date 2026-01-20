# Day 20: Creating an IAM Role for EC2

## 📝 Task Details
The Nautilus DevOps team needs to grant their application servers secure access to other AWS resources without hardcoding credentials.
* **Task:** Create an IAM Role.
* **Role Name:** `iamrole_rose`
* **Trusted Entity:** AWS Service (EC2).
* **Attached Policy:** `iampolicy_rose`.

---

## 📚 Theory: IAM Roles
* **IAM Role:** An identity with specific permissions that is assumable by anyone (or anything) deemed "trusted". Unlike users, roles do not have long-term passwords or access keys.
* **Use Case:** The most common use case is assigning a role to an EC2 instance. This allows applications running on that instance to use the AWS SDK to perform actions (like reading S3 buckets) using temporary security credentials provided automatically by the role.
* **Trust Policy:** A JSON document that defines *who* (Principal) is allowed to assume the role. In this case, the Principal is `ec2.amazonaws.com`.

---

## ⚙️ Solution

### Method 1: AWS Console (GUI)
1.  Navigated to **IAM** > **Roles**.
2.  Clicked **Create role**.
3.  **Trusted Entity:** Selected **AWS Service** > **EC2**.
4.  **Permissions:** Searched for and selected `iampolicy_rose`.
5.  **Review:** Named the role `iamrole_rose` and created it.

### Method 2: AWS CLI
```bash
# 1. Create Trust Policy
# (See trust-policy.json in solution steps)

# 2. Create Role
aws iam create-role --role-name iamrole_rose --assume-role-policy-document file://trust-policy.json

# 3. Attach Policy
aws iam attach-role-policy --role-name iamrole_rose --policy-arn <ARN_OF_POLICY>
```
## ✅ Verification
I verified the role creation and the attached policy using the CLI.

### 1. Verification Command:

``` Bash

aws iam list-attached-role-policies --role-name iamrole_rose
```
### 2. Output Confirmation: 
The CLI returned the attached policy name.

```JSON

{
    "AttachedPolicies": [
        {
            "PolicyName": "iampolicy_rose",
            "PolicyArn": "arn:aws:iam::123456789012:policy/iampolicy_rose"
        }
    ]
}
```