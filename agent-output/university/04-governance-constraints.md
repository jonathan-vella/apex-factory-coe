# Governance Constraints — Partner Modernization Factory kit (mg-factory-corp)

## Discovery Source

18 policies discovered (precomputed, not a live `az policy` query). Source: `infra/foundation/alz-lite/modules/policies.bicep` in `jonathan-vella/apex-factory-hackathon`, scope `mg-factory-corp`, built-in policy definition IDs checked with `az policy definition list` on 2026-10-02T00:00:00Z.

This file is a stand-in for Step 3.5 (04g-Governance) live discovery, built from the hackathon kit's own ALZ-lite Bicep rather than an `az rest` query against a real subscription. Re-run the Governance agent for a live discovery once this attendee/team's actual subscription IDs are known — in particular to pick up any build-tenant MCAPS policies, which are **not** included below (see Advisories).

## Summary

| Metric | Count |
|---|---|
| Total assignments | 18 |
| Deny (blocker) | 7 |
| DeployIfNotExists (auto-remediate) | 11 |
| Scope | Management group `mg-factory-corp` (workload subscriptions only; the shared services subscription is out of scope for these) |
| Allowed locations | `swedencentral`, `germanywestcentral`, `global` |

## Deny policies (blockers — must be satisfied by generated IaC)

| Policy | Resource type | Required value |
|---|---|---|
| Allowed locations | `*` | `swedencentral`, `germanywestcentral`, `global` |
| Network interfaces should not have public IPs | `Microsoft.Network/networkInterfaces` | no `publicIPAddress` on any IP config |
| Storage accounts should disable public network access | `Microsoft.Storage/storageAccounts` | `properties.publicNetworkAccess = Disabled` |
| Azure Key Vault should disable public network access | `Microsoft.KeyVault/vaults` | `properties.publicNetworkAccess = Disabled` |
| Service Bus Namespaces should disable public network access | `Microsoft.ServiceBus/namespaces` | `properties.publicNetworkAccess = Disabled` |
| Public network access should be disabled for Container registries | `Microsoft.ContainerRegistry/registries` | `properties.publicNetworkAccess = Disabled` |
| Azure SQL Managed Instances should disable public network access | `Microsoft.Sql/managedInstances` | `properties.publicDataEndpointEnabled = false` |

Note: the web app's front end and Application Insights ingestion are the kit's only accepted public-network exceptions (documented separately in the kit's conventions); nothing above exempts them automatically — model that in the architecture/requirements, not by expecting these policies to allow it.

## DeployIfNotExists policies (auto-remediate — don't block, but their effect should be modeled so As-Built matches reality)

| Policy | Resource type | Effect |
|---|---|---|
| Configure a private DNS Zone ID for blob groupID | `Microsoft.Network/privateEndpoints` | registers in the central `privatelink.blob.core.windows.net` zone (shared services subscription, `rg-hub`) |
| Configure Service Bus namespaces to use private DNS zones | `Microsoft.Network/privateEndpoints` | registers in `privatelink.servicebus.windows.net` |
| Configure Container registries to use private DNS zones | `Microsoft.Network/privateEndpoints` | registers in `privatelink.azurecr.io` |
| Configure Azure Key Vaults to use private DNS zones | `Microsoft.Network/privateEndpoints` | registers in `privatelink.vaultcore.azure.net` |
| Enable logging by category group — App Service | `Microsoft.Web/sites` | diagnostic setting to `log-management` |
| Enable logging by category group — SQL managed instances | `Microsoft.Sql/managedInstances` | diagnostic setting to `log-management` |
| Enable logging by category group — Key vaults | `Microsoft.KeyVault/vaults` | diagnostic setting to `log-management` |
| Enable logging by category group — Service Bus Namespaces | `Microsoft.ServiceBus/namespaces` | diagnostic setting to `log-management` |
| Enable logging by category group — Container registries | `Microsoft.ContainerRegistry/registries` | diagnostic setting to `log-management` |
| Enable logging by category group — Application Insights | `Microsoft.Insights/components` | diagnostic setting to `log-management` |
| Configure diagnostic settings for Blob Services | `Microsoft.Storage/storageAccounts/blobServices` | diagnostic setting to `log-management` |

SQL Managed Instance has no private endpoint policy here: it registers in the spoke's own `snet-sqlmi` subnet (delegated, no DNS zone), not the central private-link zones.

## Advisories (not in this kit's Bicep — verify live)

- The build tenant applies its own MCAPS Modify policies forcing `publicNetworkAccess = Disabled` on Storage and Key Vault ahead of this kit's Deny policies. Their definition IDs are tenant-owned and not captured here. A live Governance Discovery run against the real subscription will surface them.
- Microsoft Defender for Cloud plans are set to Foundational CSPM only by this kit (`setDefenderFoundationalOnly = true` in `alz-lite/main.bicep`); Defender's own auto-assignments are not modeled above and should be filtered the same way `04g-Governance` normally filters them.

## How to use this file

Copy both `04-governance-constraints.json` and this `.md` into `agent-output/{project}/` in the APEX accelerator repo before running Step 3.5 (Governance), so the IaC Planner (Step 4) and CodeGen (Step 5) treat these constraints as already discovered. Confirm `constraints_ref` hashes in any downstream `04-policy-property-map.json` are regenerated against this file, not an older one.
