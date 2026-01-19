# Day 17: Creating an IAM Group

## 📝 Task Details
The Nautilus DevOps team is structuring their access control by grouping users with similar roles.
* **Task:** Create an IAM Group.
* **Group Name:** `iamgroup_james`
* **Purpose:** To manage permissions for multiple users collectively rather than individually.

---

## 📚 Theory: IAM Groups
* **IAM Group:** A collection of IAM users. Groups let you specify permissions for multiple users, which can make it easier to manage the permissions for those users.
* **Inheritance:** Users added to the group automatically inherit the permissions attached to that group.
* **Best Practice:** Always attach policies to Groups, not individual Users. This makes onboarding and offboarding employees much safer and faster.

---

## ⚙️ Solution

### Method 1: AWS Console (GUI)
1.  Navigated to **IAM Dashboard** > **User groups**.
2.  Clicked **Create group**.
3.  **Settings:**
    * **Name:** `iamgroup_james`
    * **Users/Policies:** Left blank as per current requirements.
4.  Clicked **Create group**.

### Method 2: AWS CLI
```bash
aws iam create-group --group-name iamgroup_james
```
## ✅ Verification
I verified the group creation using the CLI.

### 1. Verification Command:

``` Bash

aws iam get-group --group-name iamgroup_james
```
### 2. Output Confirmation: 
The CLI returned the group details.

```JSON

{
    "Group": {
        "Path": "/",
        "GroupName": "iamgroup_james",
        "GroupId": "AGPAXAMPLEGROUPID",
        "Arn": "arn:aws:iam::123456789012:group/iamgroup_james",
        "CreateDate": "2023-10-27T10:05:00+00:00"
    }
}
```
