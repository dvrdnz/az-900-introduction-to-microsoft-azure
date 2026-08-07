# Finding 01 — Policy-Deny Blocking Resource Group Creation (Lab 04)

> Written retrospectively, reconstructing a support case from June 2026 during Lab 04 on the Skillable platform.

## Context

Lab 04 requires creating a resource group (`rg-gp-access-model`). The action failed with:

```
Failed... Resource 'rg-gp-access-model' was disallowed by policy.
```

The full evaluation payload returned by Azure showed the mismatch driving the denial:

```json
{
  "result": "False",
  "expressionKind": "Field",
  "expression": "type",
  "path": "type",
  "expressionValue": "Microsoft.Resources/subscriptions/resourceGroups",
  "targetValue": "Microsoft.Storage/storageAccounts",
  "operator": "Equals"
}
```

The policy in question (`policy5336`, assignment display name `AZ-900T00-A: Lab 04`) appears intended to govern Storage Accounts, but was evaluated against a resource group creation instead — a type mismatch between the resource actually targeted and the resource type the policy expects.

## Handling

This was reported to Skillable support (Ticket #4077645). The initial exchange required back-and-forth to identify the affected lab (the environment's Support ID, found under the in-lab Help panel, was needed to locate the case). Skillable's Lab Developers investigated and confirmed a fix on June 26, 2026:

> "Our Lab Developers worked to resolve the issue you initially opened the ticket for due to the control policy error."

Lab 04 was completed once the fix was applied.

## Note on Attribution

Unlike Finding 01, no independent root-cause diagnosis was performed here. The mismatch between `expressionValue` and `targetValue` was visible directly in the returned error payload — the fix itself, and the underlying policy logic, came from Skillable's engineering team.
