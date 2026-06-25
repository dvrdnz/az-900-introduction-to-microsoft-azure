# Build a Simple Website Endpoint with Azure Functions

## Overview

Deploy a serverless HTTP endpoint using Azure Functions, monitor its activity with Application Insights, and secure access using function keys.

---

## Prerequisites

* Azure Subscription
* Resource Group
* Required permissions to create Azure resources

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

## Configure the Function App

Azure Functions provides a serverless compute platform that executes code on demand. The Flex Consumption plan charges only for actual execution time.

1. Navigate to **Function App**.
2. Select **+ Create**.
3. Select **Flex Consumption** as the hosting option and choose **Select**.
4. On the **Basics** tab, select the resource group created earlier.
5. Enter a **Function App name**.
6. Leave **Secure unique default host name** enabled.
7. Select a region.
8. Choose **Node.js** as the **Runtime stack**.
9. Choose **22 LTS** as the **Version**.
10. Set **2048 MB** as the **Instance Size**.
11. Open the **Monitoring** tab.
12. Confirm that **Application Insights** is enabled.
13. Select **Review + Create**, then **Create**.
14. Leave **Zone Redundancy** disabled.

---

## Verify the Deployment

Confirm that the Function App deployed successfully.

1. When deployment completes, select **Go to resource**.
2. Verify that the Function App status is **Running**.

![WebApp](/Screenshots/img_01.png)

3. In the left-hand menu, under **Monitoring**, select **Application Insights**.
4. Confirm that Application Insights is enabled and connected to a resource.

![ApplicationInsights](/Screenshots/img_02.png)

---

## Open Cloud Shell

Launch Azure Cloud Shell to create and deploy a function from the command line.

1. Select the **Cloud Shell** icon in the top toolbar.
2. If prompted, select **Bash**.
3. If Cloud Shell requires storage, select **Create storage** and wait for initialization.

![MOUNT\_STORAGE-1](/Screenshots/img_03.png)

![MOUNT\_STORAGE-2](/Screenshots/img_04.png)

---

## Create the Function Project

Use Azure Functions Core Tools to create a new HTTP-triggered function.

1. Create a project folder and switch to it:

```bash
mkdir func-gp-endpoint && cd func-gp-endpoint
```

2. Initialize a new Functions project:

```bash
func init --worker-runtime node --language javascript --model V4
```

3. Create an HTTP-triggered function:

```bash
func new --name GetStatus --template "HTTP trigger" --authlevel anonymous
```

> The `anonymous` authorization level allows public access without authentication. This is useful for testing but should not be used for sensitive workloads.

4. Verify the function was created:

```bash
ls src/functions/
```

Expected output:

```text
GetStatus.js
```

---

## Deploy the Function to Azure

Publish the project to the Function App.

1. Store the Function App name in a variable:

```bash
FUNC_APP_NAME=$(az functionapp list --resource-group rg-gp-functions-endpoint --query "[0].name" -o tsv)
echo $FUNC_APP_NAME
```

2. Deploy the function:

```bash
func azure functionapp publish $FUNC_APP_NAME
```

3. Wait for deployment to complete.

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

5. Copy the URL and test it in a private browser window.

Verify that the endpoint responds without requiring authentication.

---

## Verify the Function in the Azure Portal

Confirm that the deployed function appears in the Function App.

1. Navigate to the Function App.
2. Open the Function App created earlier.
3. On the **Overview** page, verify that **GetStatus** appears under **Functions** with an **HTTP Trigger**.

![GetStatus\_HTTP](/Screenshots/img_06.png)

---

## Restrict Access to the Function

Change the authorization level so requests require a function key.

1. Open **Cloud Shell**.
2. Navigate to the project folder:

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

6. Wait for deployment to complete.

---

## Test Restricted Access

Verify that unauthenticated requests are blocked.

1. Refresh the function URL in your browser.
2. Confirm that a **401 Unauthorized** response is returned.
3. In the Azure portal, navigate to the Function App.
4. Select **GetStatus**.
5. Open **Function Keys**.
6. Copy the default key.
7. Append the key to the URL:

```text
https://<function-app>.azurewebsites.net/api/getstatus?code=<function-key>
```

8. Open the URL.

Expected response:

```text
Hello, world!
```

---

## Review Invocation Logs

Use Application Insights to review function telemetry.

1. Navigate to **Application Insights**.
2. Open the resource connected to your Function App.
3. Under **Investigate**, select **Search**.
4. Select **See all data in the last 24 hours**.

![Monitoring](/Screenshots/img_07.png)

5. Review successful requests with status code **200**.
6. Select an entry to view details such as duration, timestamp, and status code.
7. Under **Monitoring**, select **Logs**.
8. Switch from **Simple Mode** to **KQL Mode**.
9. Run the following query:

```kusto
requests
| order by timestamp desc
```

![QueryResults](/Screenshots/img_08.png)

---

## Clean Up

### Delete the Resource Group

Deleting the Resource Group removes the Function App and associated resources.

1. Navigate to **Resource Groups**.
2. Select **rg-gp-functions-endpoint**.
3. Select **Delete resource group**.
4. Enter the resource group name for confirmation.
5. Select **Delete**.
6. Wait for the deletion to complete.

### Remove Cloud Shell Files

Delete the local project files stored in Cloud Shell.

1. Open **Cloud Shell**.
2. Run:

```bash
cd ~ && rm -rf func-gp-endpoint
```

