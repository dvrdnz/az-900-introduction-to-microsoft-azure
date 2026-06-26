# Build a Simple Website Endpoint with Azure Functions

## Overview

Deploy a serverless HTTP endpoint using Azure Functions, monitor its execution with Application Insights, and secure access using function keys.

---

## Prerequisites

* Azure Subscription
* Resource Group
* Permissions to create Azure resources

---

## Create a Resource Group

A Resource Group is a logical container used to organize and manage related Azure resources.

1. Navigate to **Resource Groups**.
2. Select **+ Create**.
3. Enter a resource group name.
4. Choose a region.
5. Select **Review + Create**, then **Create**.

---

## Create the Function App

Azure Functions is a serverless compute service that runs code on demand. The Flex Consumption plan charges only for actual execution.

1. Navigate to **Function App**.
2. Select **+ Create**.
3. Select **Flex Consumption** and choose **Select**.
4. On the **Basics** tab:

   * Select the previously created Resource Group.
   * Enter a unique **Function App name**.
   * Keep **Secure unique default host name** enabled.
   * Select a region.
   * Set **Runtime stack** to **Node.js**.
   * Set **Version** to **22 LTS**.
   * Set **Instance Size** to **2048 MB**.
5. Open the **Monitoring** tab and verify that **Application Insights** is enabled.
6. Select **Review + Create**, then **Create**.
7. Leave **Zone Redundancy** disabled.

---

## Verify the Deployment

1. After deployment completes, select **Go to resource**.
2. Verify that the Function App status is **Running**.

![WebApp](/Screenshots/img_01.png)

3. Under **Monitoring**, open **Application Insights**.
4. Confirm that it is enabled and linked to the Function App.

![ApplicationInsights](/Screenshots/img_02.png)

---

## Open Azure Cloud Shell

Azure Cloud Shell provides a browser-based command-line environment with Azure CLI and Azure Functions Core Tools preinstalled.

1. Select the **Cloud Shell** icon.
2. Choose **Bash** if prompted.
3. If required, select **Create storage** and wait for initialization.

![MOUNT\_STORAGE-1](/Screenshots/img_03.png)

![MOUNT\_STORAGE-2](/Screenshots/img_04.png)

---

## Create the Function Project

1. Create a project directory:

```bash
mkdir func-gp-endpoint && cd func-gp-endpoint
```

2. Initialize a new Azure Functions project:

```bash
func init --worker-runtime node --language javascript --model V4
```

3. Create an HTTP-triggered function:

```bash
func new --name GetStatus --template "HTTP trigger" --authlevel anonymous
```

> **Note:** The `anonymous` authorization level allows unrestricted access and should only be used for testing.

4. Verify the function was created:

```bash
ls src/functions/
```

Expected output:

```text
GetStatus.js
```

---

## Deploy the Function

1. Store the Function App name in a variable:

```bash
FUNC_APP_NAME=$(az functionapp list --resource-group rg-gp-functions-endpoint --query "[0].name" -o tsv)
echo $FUNC_APP_NAME
```

2. Publish the function:

```bash
func azure functionapp publish $FUNC_APP_NAME
```

3. Wait for the deployment to complete.

Example output:

```text
Functions in <your-function-app-name>:
    GetStatus - [httpTrigger]
        Invoke url: https://<your-function-app-name>.azurewebsites.net/api/getstatus
```

![Invoke\_URL](/Screenshots/img_05.png)

4. Open the **Invoke URL**.

Expected response:

```text
Hello, world!
```

5. Open the URL in a private browser window and verify that it is publicly accessible.

---

## Verify the Function

1. Open the Function App in the Azure portal.
2. On the **Overview** page, verify that **GetStatus** appears under **Functions** with an **HTTP Trigger**.

![GetStatus\_HTTP](/Screenshots/img_06.png)

---

## Restrict Function Access

Change the authorization level to require a function key.

1. Open **Cloud Shell**.
2. Navigate to the project directory:

```bash
cd ~/func-gp-endpoint
```

3. Update the authorization level:

```bash
sed -i "s/authLevel: 'anonymous'/authLevel: 'function'/" src/functions/GetStatus.js
```

4. Verify the change:

```bash
grep authLevel src/functions/GetStatus.js
```

Expected output:

```text
authLevel: 'function',
```

5. Redeploy the function:

```bash
FUNC_APP_NAME=$(az functionapp list --resource-group rg-gp-functions-endpoint --query "[0].name" -o tsv)
func azure functionapp publish $FUNC_APP_NAME
```

---

## Verify Restricted Access

1. Refresh the Function URL.
2. Confirm that the response is **401 Unauthorized**.
3. In the Azure portal, open **GetStatus**.
4. Select **Function Keys**.
5. Copy the default key.
6. Append the key to the URL:

```text
https://<function-app>.azurewebsites.net/api/getstatus?code=<function-key>
```

7. Open the updated URL.

Expected response:

```text
Hello, world!
```

---

## Review Application Insights

1. Open the connected **Application Insights** resource.
2. Under **Investigate**, select **Search**.
3. Select **See all data in the last 24 hours**.

![Monitoring](/Screenshots/img_07.png)

4. Review successful requests with status code **200**.
5. Open **Logs** under **Monitoring**.
6. Switch to **KQL Mode**.
7. Execute the following query:

```kusto
requests
| order by timestamp desc
```

![QueryResults](/Screenshots/img_08.png)

Review timestamps, response codes, execution duration, and request details.

---

## Clean Up

### Delete the Resource Group

1. Navigate to **Resource Groups**.
2. Select **rg-gp-functions-endpoint**.
3. Select **Delete resource group**.
4. Enter the resource group name.
5. Select **Delete** and wait for completion.

### Remove Cloud Shell Files

Delete the local project files stored in Cloud Shell.

```bash
cd ~
rm -rf func-gp-endpoint
```
