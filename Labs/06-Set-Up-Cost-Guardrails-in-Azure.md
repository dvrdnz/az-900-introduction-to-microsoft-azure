# Set Up Cost Guardrails in Azure

## Overview

This lab demonstrates how to implement basic cost governance in Azure. You will organize resources using tags, configure budgets and alerts, and enforce governance by assigning Azure Policy.

## Learning Objectives

* Organize Azure resources using tags
* Monitor spending with Azure Cost Management budgets
* Enforce governance using Azure Policy

## Prerequisites

* Azure Subscription
* Azure Portal access

---

## Lab Exercises

This guided project consists of the following exercises:

1. Apply cost-tracking tags
2. Create budgets and alerts
3. Assign and test Azure Policy

---

# Exercise 1 - Apply Cost-Tracking Tags

In this exercise, you create a Resource Group and a Storage Account before applying organizational tags. Tags allow Azure resources to be categorized for cost reporting, filtering, and governance.

### Objectives

* Prepare the environment
* Create a test Storage Account
* Tag the Resource Group
* Tag the Storage Account

**Outcome**

Tagged resources ready for cost reporting and filtering.

---

## Prepare the Environment

### Create a Resource Group

A Resource Group is a logical container used to organize and manage related Azure resources.

1. Navigate to **Resource Groups**.
2. Select **+ Create**.
3. Enter a Resource Group name.
4. Select a region.
5. Select **Review + Create**.
6. Select **Create**.

---

### Create a Test Storage Account

A Storage Account provides access to Azure Storage services such as Blob Storage, File Storage, Queues, and Tables.

1. Navigate to **Storage Accounts**.
2. Select **+ Create**.
3. Select the previously created Resource Group.
4. Enter a globally unique Storage Account name.
5. Select the same region.
6. Set **Performance** to **Standard**.
7. Set **Redundancy** to **Locally Redundant Storage (LRS)**.
8. Select **Review + Create**.
9. Select **Create**.

---

### Tag the Resource Group

Tags are key-value pairs that help organize Azure resources, improve cost tracking, and simplify governance.

1. Navigate to **Resource Groups**.

2. Select **rg-gp-cost-guardrails**.

3. Open **Tags**.

4. Add the following tags:

   * `environment = pilot`
   * `owner = it-team`

5. Select **Apply**.

---

### Tag the Storage Account

Apply the same tags to the Storage Account to ensure consistent cost reporting.

1. Navigate to **Storage Accounts**.

2. Select your Storage Account.

3. Open **Tags**.

4. Add the following tags:

   * `environment = pilot`
   * `owner = it-team`

5. Select **Apply**.

---

# Exercise 2 - Create Budgets and Alerts

In this exercise, you create a monthly budget with alert thresholds that notify you before spending exceeds your target. Budget alerts provide early warnings, allowing you to investigate and adjust spending before costs become excessive.

### Objectives

* Open Cost Management
* Configure a budget
* Create alert thresholds

**Outcome**

Budget thresholds configured with actionable notifications.

---

## Open Cost Management

1. Search for **Cost Management**.
2. Open **Cost Management**.
3. Under **Monitoring**, select **Budgets**.
4. Select **+ Add**.

---

## Configure the Budget

1. For **Name**, enter:

   ```
   gp-pilot-budget
   ```

2. Set **Reset period** to **Monthly**.

3. Set **Amount** to a small test value, for example:

   ```
   10
   ```

4. Select **Next**.

5. Configure the first alert:

   * Type: **Actual Cost**
   * Threshold: **80%**

6. Configure the second alert:

   * Type: **Actual Cost**
   * Threshold: **100%**

7. Enter your email address as the alert recipient.

8. Select **Create**.

---

# Exercise 3 - Assign and Test Azure Policy

Azure Policy helps enforce organizational standards and prevent non-compliant deployments by automatically evaluating Azure resources.

### Objectives

* Assign the **Allowed locations** policy
* Validate policy enforcement
* Review policy compliance

**Outcome**

Azure Policy successfully enforces deployment restrictions.

---

## Assign the Policy

1. Search for **Policy**.

2. Select **Definitions**.

3. Search for **Allowed locations**.

4. Select the policy.

5. Select **Assign**.

6. Set the scope to:

   ```
   rg-gp-cost-guardrails
   ```

7. On the **Parameters** tab, allow only your preferred Azure region.

8. Select **Review + Create**.

9. Select **Create**.

---

## Validate Policy Enforcement

1. Navigate to **Storage Accounts**.
2. Select **+ Create**.
3. Select **rg-gp-cost-guardrails**.
4. Enter a temporary Storage Account name.
5. Choose a region that is **not** allowed by the policy.
6. Select **Review + Create**.
7. Verify deployment is denied.
8. Return to the previous page.
9. Change the region to the allowed location.
10. Select **Review + Create**.
11. Verify validation succeeds.
12. Select **Create**.

---

## Review Policy Compliance

1. Search for **Policy**.
2. Select **Compliance**.
3. Locate the **Allowed locations** assignment.
4. Review the compliance status.
5. Verify that compliant resources are listed correctly.

---

## Key Takeaways

* Tags organize Azure resources for cost reporting.
* Budgets provide proactive cost monitoring.
* Budget alerts help prevent unexpected spending.
* Azure Policy enforces governance automatically.
* Compliance reports provide visibility into policy enforcement.

---

## Cleanup

### Remove the Policy Assignment

Remove the policy assignment before deleting the Resource Group. Active policy assignments can prevent resource deletion.

1. Search for **Policy**.
2. Select **Assignments**.
3. Locate the **Allowed locations** assignment scoped to `rg-gp-cost-guardrails`.
4. Select the **...** (ellipsis) menu.
5. Select **Delete assignment**.
6. Select **Yes** to confirm.

---

### Delete the Budget

1. Search for **Cost Management + Billing**.
2. Select **Cost Management + Billing**.
3. Select **Budgets**.
4. Locate **gp-pilot-budget**.
5. Select the **...** (ellipsis) menu.
6. Select **Delete**.
7. Select **Yes** to confirm.

---

### Delete the Resource Group

Deleting the Resource Group removes all resources created during this lab.

1. Search for **Resource Groups**.

2. Select **rg-gp-cost-guardrails**.

3. Select **Delete resource group**.

4. Enter:

   ```
   rg-gp-cost-guardrails
   ```

5. Select **Delete**.

6. Confirm the deletion.

7. Wait until Azure confirms the Resource Group has been deleted.

---

### Verify Cleanup

Confirm that all lab resources have been removed.

* Verify **rg-gp-cost-guardrails** no longer appears under **Resource Groups**.
* Verify the **Allowed locations** assignment no longer appears under **Policy → Assignments**.
* Verify **gp-pilot-budget** no longer appears under **Cost Management → Budgets**.
