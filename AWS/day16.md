# Day 16: Creating an IAM User

## 📝 Task Details
The Nautilus DevOps team is setting up their security infrastructure by provisioning individual user identities.
* **Task:** Create an IAM User.
* **User Name:** `iamuser_anita`
* **Scope:** Global (IAM is a global service).

---

## 📚 Theory: IAM Fundamentals
* **IAM (Identity and Access Management):** The service that controls access to AWS resources.
* **IAM User:** An identity with long-term credentials (password or access keys) used by a person or application to interact with AWS.
* **Principle of Least Privilege:** A security best practice where a user is granted only the minimum permissions necessary to perform their job, and nothing more.

---

## ⚙️ Solution

### Method 1: AWS Console (GUI)
1.  Navigated to the **IAM Dashboard** > **Users**.
2.  Clicked **Create user**.
3.  Entered Username: `iamuser_anita`.
4.  Left "Console access" unchecked (as not specified in requirements).
5.  Skipped permission assignment (user has no permissions by default).
6.  Clicked **Create user**.

### Method 2: AWS CLI
```bash
aws iam create-user --user-name iamuser_anita
```
## ✅ Verification
I verified the user creation using the CLI.

### 1. Verification Command:

```Bash

aws iam get-user --user-name iamuser_anita
```
### 2. Output Confirmation: 
The CLI returned the user details, confirming existence (ARN and UserID).

```JSON

{
    "User": {
        "Path": "/",
        "UserName": "iamuser_anita",
        "UserId": "AIDAXAMPLEUSERID",
        "Arn": "arn:aws:iam::123456789012:user/iamuser_anita",
        "CreateDate": "2023-10-27T10:00:00+00:00"
    }
}
```