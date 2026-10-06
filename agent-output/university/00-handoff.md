# university — Handoff (Step 2 complete)

Updated: 2026-10-06T08:47:00Z | IaC: bicep | Branch: not committed (workspace only)

## Completed Steps

- [x] Step 1 → agent-output/university/01-requirements.md (Gate 1 approved by owner; review: 0 must-fix open)
- [x] Step 2 → agent-output/university/02-architecture-assessment.md + 03-des-cost-estimate.md (approved by owner 2026-10-06; architecture and cost-feasibility reviews APPROVED and hash-current)
- [ ] Step 3 Design → skipped by owner (`skip_design=true`; As-Built covers diagrams)

## Key Decisions

- Region: `swedencentral` (hub region, observed live); `germanywestcentral` approved alternate (all SKUs offered; +$0.10/month)
- SKUs: all five user pins kept (P0v3 Linux, ACR Premium, SQL MI GP_Gen5 4 vCores AHB, Standard_LRS, Service Bus Premium 1 MU); manifest rev 2, `sku_manifest_status=reviewed`, prices written back
- Architect-derived: Key Vault Standard; 4 private endpoints; App Insights workspace-based, `DisableLocalAuth: true`; SQL MI native `startStopSchedules@2025-01-01` (`default`, Mon–Fri 07:30–18:30, `timeZoneId` parameter)
- Stable API versions only for every resource; no environment identifiers in any artifact
- VNet: existing spoke used as-is (`vnet_mode=use-existing`); no NSG on `snet-app`/`snet-pe` is an accepted landing-zone decision (B08)
- Cost: $1,320.95/month ≈ $1.81/hour (1.1% under the ~$1.83/hour brief); shared services $6.06/month separate; licence-included +$291.90/month; schedule −$314.32/month after C7
- WAF: Security 8 · Reliability 4 · Performance 6 · Cost 7 · Operations 7
- Post-deploy readiness (856501fb): import the placeholder into ACR, switch the web app to the registry copy with the UAMI, expect HTTP 200; app stays on the registry copy
- Private-endpoint diagnostics (5c01d1e4): archetype-owned `AllMetrics` setting on each of the 4 endpoints; no DINE-only path
- Compliance: none beyond ALZ-lite deny policies at `mg-factory-corp` (training/demo, no production data)
- Budget: soft; ≈ $1.83/hour workload (brief estimate; Step 2 re-prices, SQL MI with and without AHB)
- IaC tool: bicep
- Pattern: Linux container App Service + SQL MI + Storage + Service Bus + Key Vault + App Insights in existing Corp spoke (no network/DNS resources created)
- SLA/RTO/RPO best-effort; env `dev`; on-demand only; AHB on (owner assumption)
- Cost monitoring: inherited from landing-zone budget `budget-factory-workload` (B08); `cost_monitoring_mode=deferred`, exception expiry 2027-01-03; `cost_alert_emails=[]`; archetype creates no budget/Action Group
- Placeholder image `mcr.microsoft.com/dotnet/samples:aspnetapp-10.0`, `WEBSITES_PORT=8080`; SQL MI schedule Mon–Fri 07:30–18:30 `W. Europe Standard Time`

## Open Challenger Findings (must_fix only)

None

## Context for Next Step

Step 3.5 (04g-Governance): run **live** policy discovery against the workload subscription (owner has set the CLI to it). Use the pre-staged `04-governance-constraints.{md,json}` only as a cross-check and record differences (expected: tenant-level MCAPS Modify policies on Storage/Key Vault, the effective tag-policy contract, any private-endpoint diagnostics assignment).

Deferred for Step 3.5 (f42fd8e0): persist `cost_monitoring_mode=deferred` and its exception (rationale "inherited from landing zone: vending (B08) creates `budget-factory-workload` with 80% e-mail alert; archetype creates no budget or Action Group", expiry 2027-01-03) in `04-governance-constraints.json`; if live governance disallows deferral, stop for owner reconciliation.

Also verify at Step 3.5: DINE private-DNS assignments (scope, parameters, identity permissions) for capability 15, and the allowed-locations set.

## Skill Context

- .github/skills/apex-azure-governance-discovery/SKILL.md
- .github/skills/apex-azure-defaults/SKILL.md
- .github/instructions/governance-discovery.instructions.md
- Decisions: `apex-recall show university --json`

## Artifacts

- agent-output/university/01-requirements.md
- agent-output/university/02-architecture-assessment.md
- agent-output/university/03-des-cost-estimate.md
- agent-output/university/02-waf-scores.{py,png,svg}, 03-des-cost-distribution.{py,png,svg}, 03-des-cost-projection.{py,png,svg}
- agent-output/university/02-cost-estimate.json (workload, COMPLETE) + 02-cost-estimate-{sqlmi-options,sqlmi-schedule,shared-services,manifest}.json
- agent-output/university/sku-manifest.{json,md} (rev 2)
- agent-output/university/challenge-findings-architecture.json, challenge-findings-cost-estimate.json, challenge-findings-architecture-decisions.json
- agent-output/university/challenge-findings-requirements.json, challenge-findings-requirements-decisions.json
- agent-output/university/04-governance-constraints.{md,json} (pre-staged stand-in; cross-check only)
- agent-output/university/README.md
