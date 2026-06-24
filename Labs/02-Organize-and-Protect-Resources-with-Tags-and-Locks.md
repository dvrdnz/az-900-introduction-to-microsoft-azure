# Organize and Protect Resources with Tags and Locks

## Overview

This lab demonstrates how to organize Azure resources using tags and protect them with resource locks. Tags improve resource management and cost tracking, while locks help prevent accidental changes or deletion.

---

## Prerequisites

* Azure Subscription
* Permissions to create and manage resources

---

## Create a Resource Group

A Resource Group is a logical container for Azure resources.

1. Navigate to **Resource Groups**.
2. Select **+ Create**.
3. Enter a resource group name.
4. Select a region.
5. Select **Review + Create**.
6. Select **Create**.

---

## Create a Storage Account

A Storage Account provides access to Azure Storage services such as Blob Storage, Files, Queues, and Tables.

1. Navigate to **Storage Accounts**.
2. Select **+ Create**.
3. Select the Resource Group.
4. Enter a unique storage account name.
5. Select the same region as the Resource Group.
6. Choose **Standard** performance.
7. Select **Locally Redundant Storage (LRS)**.
8. Select **Review + Create**.
9. Select **Create**.

---

## Tag the Resource Group

Tags are key-value pairs used to organize resources, simplify reporting, and support governance.

1. Navigate to **Resource Groups**.

2. Open the Resource Group.

3. Select **Tags**.

4. Add:

   | Name        | Value       |
   | ----------- | ----------- |
   | department  | development |
   | environment | test        |

5. Select **Apply**.

6. Verify both tags are present.

---

## Tag the Storage Account

Apply the same tags to the Storage Account for consistent classification.

1. Navigate to **Storage Accounts**.

2. Open the Storage Account.

3. Select **Tags**.

4. Add:

   | Name        | Value       |
   | ----------- | ----------- |
   | department  | development |
   | environment | test        |

5. Select **Apply**.

6. Verify both tags are present.

---

## Create a Second Storage Account

Create an additional Storage Account with different tag values to demonstrate filtering.

1. Navigate to **Storage Accounts**.
2. Select **+ Create**.
3. Select the Resource Group.
4. Enter a unique storage account name.
5. Select the same region.
6. Choose **Standard** performance.
7. Select **Locally Redundant Storage (LRS)**.
8. Select **Review + Create**.
9. Select **Create**.
10. Open the newly created Storage Account.
11. Select **Tags**.
12. Add:

| Name        | Value      |
| ----------- | ---------- |
| department  | operations |
| environment | test       |

13. Select **Apply**.
14. Verify both tags are present.

---

## Filter Resources by Tag

Use tags to locate resources belonging to a specific department.

1. Navigate to **Resource Groups**.

2. Open the Resource Group.

3. Select **Add filter**.

4. Choose:

   * **Filter:** department
   * **Operator:** Equals
   * **Value:** development

5. Select **Apply**.

6. Verify only the first Storage Account is displayed.

7. Remove the filter.

8. Repeat the process using the value **operations**.

9. Verify only the second Storage Account is displayed.

10. Remove the filter.

---

## Apply a Delete Lock to a Storage Account

A Delete lock prevents accidental deletion while still allowing modifications.

1. Open a Storage Account.

2. Navigate to **Settings → Locks**.

3. Select **+ Add**.

4. Configure:

   * **Lock name:** prevent-delete
   * **Lock type:** Delete

5. Add an optional note.

6. Select **OK**.

7. Verify the lock appears in the list.

---

## Apply a Read-Only Lock to the Resource Group

A Read-only lock prevents modifications to resources within the Resource Group.

1. Open the Resource Group.

2. Navigate to **Settings → Locks**.

3. Select **+ Add**.

4. Configure:

   * **Lock name:** read-only-rg
   * **Lock type:** Read-only

5. Add an optional note.

6. Select **OK**.

7. Verify the lock appears in the list.

---

## Test the Read-Only Lock

Verify that modifications are blocked.

1. Open the Resource Group.

2. Select **Tags**.

3. Add a tag:

   | Name     | Value   |
   | -------- | ------- |
   | test-tag | blocked |

4. Select **Apply**.

5. Verify the operation fails with a `ScopeLocked` error.

```text
Could not save the tags for the resource...
(Error code: ScopeLocked)
```

---

## Test the Delete Lock

Verify that deletion is blocked.

1. Open the locked Storage Account.
2. Select **Delete**.
3. Confirm the deletion request.
4. Verify the operation fails with a lock-related error.

```text
This resource can't be deleted because it has a delete lock.
Locks must be removed before the resource can be deleted.
```

---

## Remove the Locks

Restore normal operations by removing both locks.

### Remove the Read-Only Lock

1. Open the Resource Group.
2. Navigate to **Settings → Locks**.
3. Delete the **read-only-rg** lock.

### Remove the Delete Lock

1. Open the Storage Account.
2. Navigate to **Settings → Locks**.
3. Delete the **prevent-delete** lock.

### Verify Access

1. Return to the Resource Group.

2. Select **Tags**.

3. Add:

   | Name      | Value  |
   | --------- | ------ |
   | lock-test | passed |

4. Select **Apply**.

5. Verify the tag is successfully created.

6. Remove the tag again.

---

## Verification

* [ ] Resource Group created
* [ ] Two Storage Accounts created
* [ ] Tags applied successfully
* [ ] Resource filtering tested
* [ ] Delete lock verified
* [ ] Read-only lock verified
* [ ] Locks removed successfully

---

## Key Takeaways

* Tags help organize resources and improve cost visibility.
* Resource Groups can be filtered using tag values.
* Delete locks prevent accidental resource removal.
* Read-only locks prevent configuration changes.
* Locks can be applied at different scopes and inherited by child resources.

---

## References

* Azure Tags
* Azure Resource Locks
* Azure Governance
