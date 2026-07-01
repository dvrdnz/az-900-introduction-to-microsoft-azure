# Share Files Securely

## Overview

In this lab, you create a private Azure Blob Storage container, upload a file, and securely share it using a Shared Access Signature (SAS). You then manage access with a stored access policy, revoke shared access, and configure lifecycle management for automatic cleanup.

## Learning Objectives

* Create and configure Azure Blob Storage.
* Upload and securely share files using a SAS.
* Manage SAS permissions with a stored access policy.
* Verify and revoke temporary access.
* Configure lifecycle management to automatically delete old files.

## Prerequisites

* Azure Subscription
* Resource Group
* Storage Account

## Lab Exercises

* Create a storage account and upload a file.
* Create a stored access policy and generate a SAS.
* Verify secure access.
* Revoke shared access.
* Configure lifecycle management.

**Outcome:** A private Blob Storage container containing a securely shared file, with SAS revocation and automated cleanup configured.

---

# Prepare the Environment

Create the Azure resources required for this lab.

## Create a Resource Group

A Resource Group is a logical container used to organize and manage related Azure resources.

1. Navigate to **Resource Groups**.
2. Select **+ Create**.
3. Enter a resource group name.
4. Select a region.
5. Select **Review + Create**.
6. Select **Create**.

<!-- keep existing image -->

## Create a Storage Account

A Storage Account provides access to Azure Storage services such as Blob Storage, File Storage, Queues, and Tables.

1. Navigate to **Storage Accounts**.
2. Select **+ Create**.
3. Select the previously created Resource Group.
4. Enter a globally unique storage account name.
5. Select the same region.
6. Set **Performance** to **Standard**.
7. Set **Redundancy** to **Locally Redundant Storage (LRS)**.
8. Select **Review + Create**.
9. Select **Create**.

<!-- keep existing image -->

---

# Create a Private Container

Create a private Blob container to prevent anonymous access.

1. Open the storage account.
2. Under **Data storage**, select **Containers**.
3. Select **+ Container**.
4. Enter a container name.
5. Leave **Anonymous access level** set to **Private (no anonymous access)**.
6. Select **Create**.

<!-- keep existing image -->

---

# Upload the Report File

Create a local file named `monthly-report.txt` with the following content:

```text
Monthly Partner Report
Status: Complete
Items processed: 142
Storage tier: Standard
Compliance check: Passed
Next review: Scheduled
```

Upload the file:

1. Open the container.
2. Select **Upload**.
3. Select **Browse for files** and choose `monthly-report.txt`.
4. Select **Upload**.
5. Verify that the file appears in the container.

<!-- keep existing image -->

---

# Create a Stored Access Policy

A stored access policy centralizes SAS permissions and expiration, making access easier to manage.

1. Open the container.

2. Navigate to **Settings → Access policy**.

3. Select **+ Add policy**.

4. Configure the policy:

   * **Identifier:** `partner-read-policy`
   * **Permissions:** Read
   * **Start time:** Today
   * **Expiry time:** One hour from now

5. Select **OK**.

6. Select **Save**.

<!-- keep existing image -->

---

# Generate a SAS Token

Generate a Shared Access Signature (SAS) based on the stored access policy.

1. Open `monthly-report.txt`.
2. Select **Generate SAS**.
3. Under **Signing method**, select **Account key**.

> **Note**
>
> Stored access policies are supported only for a **Service SAS** signed with an **Account key**. If **User delegation key** is selected, the **Access policy** option is unavailable.

4. Select the stored access policy `partner-read-policy`.
5. Select **Generate SAS token and URL**.
6. Copy the **Blob SAS URL**.

<!-- keep existing image -->

---

# Verify Direct Access Is Blocked

Verify that the blob cannot be accessed without authentication.

1. Copy the blob URL **without** the SAS token.
2. Open a private or incognito browser window.
3. Paste the URL into the address bar.
4. Confirm that access is denied.

<!-- keep existing image -->

---

# Test SAS Access

Verify that the SAS URL grants temporary access.

1. Paste the **Blob SAS URL** into the same private or incognito browser window.
2. Open the URL.
3. Verify that the report is displayed.

The SAS token provides temporary access without requiring the user to sign in.

<!-- keep existing image -->

---

# Revoke Shared Access

Delete the stored access policy to invalidate all SAS tokens associated with it.

1. Return to the container.
2. Open **Settings → Access policy**.
3. Delete `partner-read-policy`.
4. Select **Save**.

<!-- keep existing image -->

## Verify SAS Revocation

1. Return to the private or incognito browser window.
2. Refresh the page or reopen the previously generated **Blob SAS URL**.
3. Confirm that access is denied.

The request should return an authentication error similar to:

```text
<Error>
  <Code>AuthenticationFailed</Code>
  <Message>Server failed to authenticate the request. Make sure the value of Authorization header is formed correctly including the signature.</Message>
  <AuthenticationErrorDetail>SAS identifier cannot be found for specified signed identifier</AuthenticationErrorDetail>
</Error>
```

> **Note**
>
> Deleting a stored access policy invalidates all SAS tokens that reference it. In most cases, access is revoked immediately, although short propagation delays may occur due to client or service caching.
>
> See [Microsoft Learn – Define a stored access policy](https://learn.microsoft.com/en-us/rest/api/storageservices/define-stored-access-policy).


<!-- keep existing image -->

---

# Configure Lifecycle Management

Create a lifecycle rule that automatically deletes files after **30 days**.

Revoking a SAS prevents access to a file, but the blob remains in storage. Lifecycle management automatically removes old files, helping reduce storage costs and keeping the container clean.

**Outcome:** A lifecycle rule that automatically deletes shared files after 30 days.

## Create a Lifecycle Rule

1. Open the storage account.

2. Navigate to **Data management → Lifecycle management**.

3. Select **+ Add a rule**.

4. Enter the rule name `delete-shared-files`.

5. Set **Rule scope** to **Limit blobs with filters**.

6. Leave **Block blobs** and **Base blobs** selected.

7. Select **Next**.

8. Set:

   * **Base blobs were:** Last modified
   * **More than (days ago):** `30`
   * **Then:** Delete the blob

9. Select **Next**.

10. Under **Prefix match**, enter the container name followed by `/` (for example, `<container-name>/`).

11. Select **Add**.

Verify that the rule appears in the lifecycle management list.

<!-- keep existing image -->

## Review the Rule

Open the `delete-shared-files` rule and verify:

* Scope is limited to the target container.
* Files older than **30 days** are selected.
* The configured action is **Delete the blob**.

<!-- keep existing image -->

---

# Clean Up Resources

## Delete the Resource Group

Deleting the resource group removes every Azure resource created during this lab.

1. Navigate to **Resource Groups**.
2. Select the resource group.
3. Select **Delete resource group**.
4. Enter the resource group name.
5. Select **Delete** twice to confirm.
6. Wait until the deletion is complete.

<!-- keep existing image -->

## Clean Up Local Artifacts

1. Delete `monthly-report.txt` from your local machine if it is no longer needed.
2. Remove copied SAS URLs from notes, clipboard history, or shared documents as a security best practice.

## Verify Cleanup

Open **Resource Groups** and confirm that the resource group no longer exists.
