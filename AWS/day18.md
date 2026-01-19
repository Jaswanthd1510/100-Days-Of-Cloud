# Day 18: Creating a Custom IAM Policy

## 📝 Task Details
The Nautilus DevOps team needs a specific permission set that allows users to audit EC2 resources without the risk of accidental modification.
* **Task:** Create a Customer Managed IAM Policy.
* **Policy Name:** `iampolicy_rose`
* **Permissions:** Read-only access to EC2 (Instances, AMIs, Snapshots).

---

## 📚 Theory: IAM JSON Policies
* **Policy Document:** Permissions in AWS are defined using JSON (JavaScript Object Notation).
* **Key Elements:**
    * **Effect:** `Allow` or `Deny`.
    * **Action:** The specific API call (e.g., `ec2:StartInstances`). Using a wildcard like `ec2:Describe*` grants all actions that start with "Describe".
    * **Resource:** The specific object the action applies to. `*` means all resources.
* **Read-Only:** In the AWS API, "Read" actions often start with `Describe`, `List`, or `Get`. "Write" actions start with `Create`, `Delete`, `Modify`, or `Run`.

---

## ⚙️ Solution

### Method 1: AWS Console (GUI)
1.  Navigated to **IAM Dashboard** > **Policies**.
2.  Clicked **Create policy** > **JSON**.
3.  **JSON definition used:**
    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": "ec2:Describe*",
                "Resource": "*"
            }
        ]
    }
    ```
4.  **Name:** `iampolicy_rose`.
5.  Clicked **Create policy**.

### Method 2: AWS CLI
```bash
aws iam create-policy \
    --policy-name iampolicy_rose \
    --policy-document '{"Version":"2012-10-17","Statement":[{"Effect":"Allow","Action":"ec2:Describe*","Resource":"*"}]}'
```
## ✅ Verification
I verified the policy creation and the permissions using the CLI.

### 1. Verification Command:

```Bash

aws iam get-policy --policy-arn arn:aws:iam::$(aws sts get-caller-identity --query Account --output text):policy/iampolicy_rose
```
### 2. Output Confirmation: 
The CLI returned the policy metadata, confirming it exists.

```JSON

{
    "Policy": {
        "PolicyName": "iampolicy_rose",
        "PolicyId": "ANPAXAMPLEPOLICYID",
        "Arn": "arn:aws:iam::123456789012:policy/iampolicy_rose",
        ...
    }
}
```