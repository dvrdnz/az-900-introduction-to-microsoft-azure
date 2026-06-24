# Deploy a Static Website with Azure Blob Storage

## Overview

Azure Blob Storage can host static websites without requiring a virtual machine or web server. This service is cost-effective and easy to manage for simple web applications.

## Prerequisites

* Azure Subscription
* Resource Group

---

## Create a Resource Group

A Resource Group is a logical container used to organize and manage Azure resources.

1. Navigate to **Resource Groups**.
2. Select **+ Create**.
3. Enter a resource group name.
4. Select a region.
5. Select **Review + Create**.
6. Select **Create**.

---

## Create a Storage Account

A Storage Account provides access to Azure Storage services such as Blob Storage, File Storage, Queues, and Tables.

1. Navigate to **Storage Accounts**.

2. Select **+ Create**.

3. Select the Resource Group.

4. Enter a unique Storage Account name.

5. Select the same region as the Resource Group.

6. Select **Azure Blob Storage or Azure Data Lake Storage** as the preferred storage type.

7. Choose **Standard** performance.

8. Choose **Locally Redundant Storage (LRS)**.

9. Select **Review + Create**.

10. Select **Create**.

11. Wait until the deployment is complete and select **Go to resource**.

12. Navigate to **Data Management → Static Website**.

13. Select **Enable**.

14. Configure the following settings:

    * Index document name: `index.html`
    * Error document path: `404.html`

15. Select **Save**.

16. Copy the **Primary Endpoint** URL and open it in a new browser tab.

17. The following message is displayed:

```text
The requested content does not exist.
```

18. Leave the browser tab open and return to the Azure portal.

---

## Upload Website Files

1. Create the following files locally using PowerShell:

   * `index.html`
   * `404.html`

```powershell
@"
<!DOCTYPE html>
<html>
<head><title>Product Landing Page</title></head>
<body>
  <h1>Version 1 - Landing Page</h1>
  <p>Welcome to our product page. This is the initial published version.</p>
</body>
</html>
"@ | Set-Content -Path .\index.html

@"
<!DOCTYPE html>
<html>
<head><title>Page Not Found</title></head>
<body>
  <h1>404 - Page Not Found</h1>
  <p>The page you requested does not exist. Return to the <a href="/">home page</a>.</p>
</body>
</html>
"@ | Set-Content -Path .\404.html
```

2. Navigate to **Data Storage → Containers**.

3. Open the **$web** container.

4. Upload the following files:

   * `index.html`
   * `404.html`

5. Wait for the upload to complete.

6. Return to the browser tab containing the Primary Endpoint URL and refresh the page.

7. The following content is displayed:

```text
Version 1 - Landing Page

Welcome to our product page. This is the initial published version.
```

8. Modify the endpoint URL by appending `/fakesite` and press **Enter**.
9. The custom error page is displayed:

```text
404 - Page Not Found

The page you requested does not exist. Return to the home page.
```

---

## Verification

1. Open the Primary Endpoint URL.
2. Verify that the landing page is displayed successfully.
3. Verify that navigating to a non-existing path displays the custom `404.html` page.

---

## Result

A static website is now hosted directly from Azure Blob Storage.

## Key Takeaways

* Azure Blob Storage can host static websites.
* No virtual machine or web server is required.
* Static website hosting is simple and cost-effective.
* The `$web` container stores website content.
* Custom error pages can be configured using the Static Website feature.
