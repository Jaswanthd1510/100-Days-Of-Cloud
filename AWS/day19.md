# Day 19: Attaching IAM Policy to User

## 📝 Task Details
The Nautilus DevOps team is configuring access controls by assigning specific permission sets to existing users.
* **Task:** Attach an existing IAM Policy to an existing IAM User.
* **User Name:** `iamuser_jim`
* **Policy Name:** `iampolicy_jim`

---

## 📚 Theory: Policy Attachment
* **Authorization:** The process of granting permissions to an authenticated identity.
* **Direct Attachment:** Binding a managed policy directly to an IAM User. While effective, AWS recommends attaching policies to Groups or Roles for better scalability.
* **Effective Permissions:** Once attached, the permissions apply immediately. If the user is currently running a script, the very next API call will succeed (or fail) based on this new policy.

---

## ⚙️ Solution

### Method 1: AWS Console (GUI)
1.  Navigated to **IAM** > **Users** and selected `iamuser_jim`.
2.  Clicked **Add permissions** > **Attach policies directly**.
3.  Searched for `iampolicy_jim`.
4.  Selected the policy and clicked **Add permissions**.

### Method 2: AWS CLI
```bash
# Get ARN and Attach
policy_arn=$(aws iam list-policies --query "Policies[?PolicyName=='iampolicy_jim'].Arn" --output text)

aws iam attach-user-policy \
    --user-name iamuser_jim \
    --policy-arn $policy_arn
```
## ✅ Verification
I verified the attachment by listing the policies attached to the user.

### 1. Verification Command:

```Bash

aws iam list-attached-user-policies --user-name iamuser_jim
```
### 2. Output Confirmation: 
The CLI returned the list containing iampolicy_jim.

```JSON

{
    "AttachedPolicies": [
        {
            "PolicyName": "iampolicy_jim",
            "PolicyArn": "arn:aws:iam::123456789012:policy/iampolicy_jim"
        }
    ]
}
```