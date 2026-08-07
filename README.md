# AZ-900T00-A Introduction to Microsoft Azure

Hands-on Labs, Challenge Labs, and personal notes created during the **AZ-900T00-A: Introduction to Microsoft Azure** course.

These labs are delivered through [Skillable](https://www.skillable.com), a third-party platform hosting Microsoft's proprietary AZ-900 lab content. Labs 01–06 below were completed and documented. **Lab 07** could not be completed: a platform policy misconfiguration blocked the required resource creation, and the issue was never resolved by Skillable. The diagnosis is documented under [Findings](#findings).

## Status

This repository is archived. Labs 01–06 are complete; Lab 07 remains unresolved due to an unfixed platform-side policy bug (see Findings). No further labs will be added.

## Course Labs

### Lab 01 - Deploy a Static Website with Azure Blob Storage

Learn how to host a static website using Azure Storage and Blob Containers.

### Lab 02 - Organize and Protect Resources with Tags and Locks

Use Azure Tags for resource organization and Azure Locks to prevent accidental modifications or deletions.

### Lab 03 - Build a Simple Website Endpoint with Azure Functions

Create and deploy a serverless endpoint using Azure Functions.

### Lab 04 - Set Up New Employee Access with Entra ID and RBAC

Manage identities and permissions using Microsoft Entra ID and Azure Role-Based Access Control.

### Lab 05 - Share Files Securely

Configure Azure Storage services to securely share files.

### Lab 06 - Set Up Cost Guardrails in Azure

Use Azure Cost Management tools such as Budgets and Alerts to monitor spending.

---

## Repository Structure

```text
├── Labs/
│ ├── 01-Deploy-a-Static-Website-with-Azure-Blob-Storage.md
│ ├── 02-Organize-and-Protect-Resources-with-Tags-and-Locks.md
| ├── 03-Build-a-Simple-Website-Endpoint-with-Azure-Functions.md
| ├── 04-Set-Up-New-Employee-Access-Entra-ID-and-RBAC.md
│ ├── 05-Share-Files-Securely.md
│ └── 06-Set-Up-Cost-Guardrails-in-Azure.md
├── Findings/
│ ├── policy-deny-resource-group.md
│ ├── policy-deny-diagnosis.md
│ └── screenshot-lab07-policy-deny.png
├── Screenshots/
└── README.md
```

---

## Learning Objectives

* Understand cloud concepts and service models
* Explore Azure architecture and core services
* Gain practical experience with the Azure Portal

---

## Findings

Platform-level issues encountered while working through the labs, documented independently of the lab guides themselves.

### Finding 01 — Policy-Deny Blocking Resource Group Creation (Lab 04)

A policy scoped to Storage Accounts (`Microsoft.Storage/storageAccounts`) incorrectly evaluated against resource group creation (`Microsoft.Resources/subscriptions/resourceGroups`), blocking a legitimate step in Lab 04. Reported to Skillable support (Ticket #4077645); resolved by their engineering team on June 26, 2026.

See [`Findings/policy-deny-resource-group.md`](Findings/policy-deny-resource-group.md).

No independent root-cause diagnosis was performed here — the fix came from Skillable's engineering team, not from analysis on my end. Lab 04 was completed once the fix was applied.

### Finding 02 — Opaque Policy-Deny Error (Lab 07)

A platform policy blocked legitimate resource creation (a service health alert rule) without indicating which condition inside the policy actually failed. The generic error only referenced policy IDs. Diagnosed by pulling the policy definition directly and identifying a missing allowlist branch.

See [`Findings/policy-deny-diagnosis.md`](Findings/policy-deny-diagnosis.md).

Unlike Finding 01, this was diagnosed independently rather than resolved by Skillable — the issue was never corrected on their end, which is also why Lab 07 has no corresponding entry under `Labs/`.

---

## Disclaimer

This repository contains personal lab documentation and notes created while preparing for the Microsoft Azure Fundamentals (AZ-900) certification.
