# Set Up New Employee Access with Entra ID and RBAC

## Overview

In this lab, you create a new user and security group in Microsoft Entra ID, assign Azure RBAC permissions, and verify the principle of least privilege by testing access with the new account.

### Objectives

* Create a Resource Group and Storage Account
* Create a Microsoft Entra ID user and security group
* Assign the **Reader** RBAC role
* Verify inherited permissions
* Test user access using a Temporary Access Pass (TAP)
* Clean up all created resources

---

## Prepare the Environment

### Create a Resource Group

A Resource Group is a logical container for Azure resources.

1. Navigate to **Resource Groups**.
2. Select **+ Create**.
3. Enter a resource group name.
4. Select a region.
5. Select **Review + Create**.
6. Select **Create**.


---

### Create a Storage Account

A Storage Account provides access to Azure Storage services and is used later to verify RBAC permissions.

1. Navigate to **Storage Accounts**.
2. Select **+ Create**.
3. Select the previously created Resource Group.
4. Enter a globally unique storage account name.
5. Select the same region.
6. Choose **Standard** performance.
7. Select **Locally Redundant Storage (LRS)**.
8. Select **Review + Create**.
9. Select **Create**.


---

## Create a Security Group

Security groups simplify permission management by assigning roles to groups instead of individual users.

1. Navigate to **Microsoft Entra ID**.
2. Select **Groups**.
3. Select **+ New group**.
4. Set **Group type** to **Security**.
5. Enter a group name.
6. (Optional) Add a description.
7. Leave **Membership type** as **Assigned**.
8. Select **Create**.

---

## Create a User Account

Create a user that will inherit permissions from the security group.

1. Navigate to **Microsoft Entra ID**.
2. Select **Users**.
3. Select **+ New user**.
4. Enter a unique **User principal name**.
5. Enter a **Display name**.
6. Select **Review + Create**.
7. Select **Create**.
8. Refresh the user list if necessary.

> Record the **User Principal Name (UPN)**. It is required later when signing in.

---

## Add the User to the Security Group

1. Open **Microsoft Entra ID**.
2. Select **Users**.
3. Open the created user.
4. Select **Groups**.
5. Select **+ Add memberships**.
6. Select the previously created security group.
7. Confirm the selection.


---

# Assign RBAC Permissions

## Assign the Reader Role

Assign the **Reader** role at the Resource Group scope. Every member of the security group inherits this permission.

1. Open **Resource Groups**.
2. Select the created Resource Group.
3. Select **Access control (IAM)**.
4. Select **+ Add → Add role assignment**.
5. Select the **Reader** role.
6. Select **Next**.
7. Under **Assign access to**, keep **User, group, or service principal** selected.
8. Select **+ Select members**.
9. Select the created security group.
10. Select **Review + Assign**.



---

## Verify Permissions

Verify that the user inherits the Reader role through the security group.

1. Open the Resource Group.
2. Select **Access control (IAM)**.
3. Select **Check access**.
4. Search for the created user.
5. Confirm the **Reader** role is inherited from the security group.



---

## Review the Activity Log

Review the audit trail of the RBAC assignment.

1. Open the Resource Group.
2. Select **Activity log**.
3. Locate the **Create role assignment** event.
4. Review the assignment details and initiating user.

![ActivityLog](/Screenshots/img_09.png)

![ActivityLog_](/Screenshots/img_10.png)

---

# Temporary Access Pass (TAP)

## Enable Temporary Access Pass

Enable TAP to allow the test user to sign in without configuring MFA.

1. Open **Microsoft Entra Authentication Methods**.
2. Select **Policies**.
3. Open **Temporary Access Pass**.
4. Set **Enable** to **Yes**.
5. Apply the policy to **All users** or the created security group.
6. Select **Save**.


---

## Generate a Temporary Access Pass

1. Open **Microsoft Entra ID**.
2. Select **Users**.
3. Open the created user.
4. Select **Authentication methods**.
5. Select **+ Add authentication method**.
6. Choose **Temporary Access Pass**.
7. Keep the default settings.
8. Select **Add**.
9. Copy the generated TAP code.

> The TAP code is displayed only once.



---

# Test User Access

Sign in using the newly created account.

1. Go to **https://portal.azure.com**.
2. Sign in with the recorded **User Principal Name**.
3. Select **Use a Temporary Access Pass** if prompted.
4. Enter the TAP code.
5. Set a new password.
6. Open **Resource Groups**.
7. Verify that the created Resource Group is visible.
8. Attempt to create a new Storage Account.

The operation should fail because the account only has the **Reader** role.



---

# Clean Up

## Delete Azure Resources

1. Open **Resource Groups**.
2. Select the created Resource Group.
3. Select **Delete resource group**.
4. Confirm the Resource Group name.
5. Select **Delete**.



---

## Delete the User and Security Group

### Delete the User

1. Open **Microsoft Entra ID**.
2. Select **Users**.
3. Select the created user.
4. Select **Delete**.

### Delete the Security Group

1. Open **Microsoft Entra ID**.
2. Select **Groups**.
3. Select the created security group.
4. Select **Delete**.



---

## Disable Temporary Access Pass (Optional)

Only complete this step if TAP was enabled specifically for this lab.

1. Open **Microsoft Entra Authentication Methods**.
2. Select **Policies**.
3. Open **Temporary Access Pass**.
4. Set **Enable** to **Off**.
5. Select **Save**.



---

## Verify Cleanup

* Confirm the Resource Group no longer exists.
* Confirm the test user has been deleted.
* Confirm the security group has been deleted.
* Confirm TAP has been disabled (if applicable).

