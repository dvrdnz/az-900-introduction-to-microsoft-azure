# Share Files Securely

## Overview

In this lab, you create a private Azure Blob Storage container, upload a file, and securely share it using a Shared Access Signature (SAS). You then use a stored access policy to manage permissions and demonstrate how access can be revoked.

## Learning Objectives

* Create and configure Azure Blob Storage.
* Upload and securely share files using SAS.
* Manage SAS permissions with stored access policies.
* Verify and revoke temporary access.

## Prerequisites

* Azure Subscription
* Resource Group
* Storage Account

## Lab Exercises

* Create storage and upload a file.
* Create a stored access policy and generate a SAS.
* Test partner access.
* Revoke partner access.

**Outcome:** A private Blob Storage container containing a securely shared report file.

---

# Prepare the Environment

Create the Azure resources required for this lab.

## Create a Resource Group

A Resource Group is a logical container for managing related Azure resources.

1. Navigate to **Resource Groups**.
2. Select **+ Create**.
3. Enter a resource group name.
4. Select a region.
5. Select **Review + Create**.
6. Select **Create**.

## Create a Storage Account

A Storage Account provides access to Azure Storage services, including Blob Storage.

1. Navigate to **Storage Accounts**.
2. Select **+ Create**.
3. Select the Resource Group created earlier.
4. Enter a globally unique storage account name.
5. Select the same region.
6. Choose **Standard** performance.
7. Select **Locally Redundant Storage (LRS)**.
8. Select **Review + Create**.
9. Select **Create**.

---

# Create the Private Container

Create a private Blob container to prevent anonymous access.

1. Open the storage account.
2. Under **Data storage**, select **Containers**.
3. Select **+ Container**.
4. Enter a container name.
5. Leave **Anonymous access level** set to **Private (no anonymous access)**.
6. Select **Create**.

---

# Upload the Report File

Create and upload a sample report.

Create a local file named `monthly-report.txt` with the following content:

```text
Monthly Partner Report
Status: Complete
Items processed: 142
Storage tier: Standard
Compliance check: Passed
Next review: Scheduled
```

Then:

1. Open the container.
2. Select **Upload**.
3. Select **Browse for files** and choose `monthly-report.txt`.
4. Select **Upload**.
5. Verify that the file appears in the blob list.

---

# Create a Stored Access Policy

Create a stored access policy to centrally manage SAS permissions and expiration.

1. Open the **Storage Account**.
2. Navigate to **Containers**.
3. Open the created container.
4. Under **Settings**, select **Access policy**.
5. Select **+ Add policy**.
6. Configure the policy:

   * **Identifier:** `partner-read-policy`
   * **Permissions:** Read
   * **Start time:** Today
   * **Expiry time:** One hour from now
7. Select **OK**.
8. Select **Save**.

---

# Generate a SAS Token

Generate a Shared Access Signature (SAS) using the stored access policy.

1. Open the created container.
2. Select **monthly-report.txt**.
3. Select **Generate SAS**.
4. Under **Signing method**, select **Account key** (Stored access policies are only available for a Service SAS signed with the account key — the "Access policy" option will not appear if "User delegation key" is selected.).
5. Choose the stored access policy **partner-read-policy**
6. Select **Generate SAS token and URL**.
7. Copy the generated **Blob SAS URL**.

---

# Verify Direct Access Is Blocked

Verify that the blob cannot be accessed without authentication.

1. Open **monthly-report.txt**.
2. Copy the **URL** without the SAS token.
3. Open a private or incognito browser window.
4. Paste the URL into the address bar.
5. Verify that access is denied.

---

# Test SAS Access

Verify that the SAS URL provides temporary access.

1. In the same private browser window, paste the **Blob SAS URL**.
2. Open the URL.
3. Verify that the report is displayed.
4. Confirm that access is granted without signing in because the SAS token provides the required permissions.

---

# Delete the Stored Access Policy

Delete the stored access policy to revoke all SAS tokens associated with it.

1. Return to the container.
2. Open **Settings → Access policy**.
3. Delete **partner-read-policy**.
4. Select **Save**.

## Verify SAS Revocation

Confirm that the SAS token no longer grants access after deleting the stored access policy.

1. Return to the private or incognito browser window.
2. Refresh the page or reopen the previously generated **Blob SAS URL**.
3. Verify that access is denied.

The request should return an authentication error similar to the following:

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
