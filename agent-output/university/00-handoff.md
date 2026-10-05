# university — Handoff (Step 1 complete)

Updated: 2026-10-05T15:52:00Z | IaC: bicep | Branch: not committed (workspace only)

## Completed Steps

- [x] Step 1 → agent-output/university/01-requirements.md (Gate 1 approved by owner; review: 0 must-fix open)

## Key Decisions

- Region: hub-derived; approved `swedencentral` (primary) + `germanywestcentral`; block otherwise
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

Step 2 must verify P0v3 regional VNet integration, price all pinned SKUs in both approved regions (SQL MI with AHB and licence-included), and carry forward the preflight checks (capabilities 14–18), the ACR import and image-pull post-deploy tests, and the App Insights Entra-ingestion dependency.

Deferred for Step 3.5 (f42fd8e0): persist `cost_monitoring_mode=deferred` and its exception (rationale, expiry 2027-01-03) in `04-governance-constraints.json`; if live governance disallows deferral, stop for owner reconciliation.

## Skill Context

- .github/skills/apex-azure-defaults/SKILL.md
- .github/skills/apex-azure-artifacts/SKILL.md
- .github/instructions/sku-manifest.instructions.md
- Decisions: `apex-recall show university --json`

## Artifacts

- agent-output/university/01-requirements.md
- agent-output/university/README.md
- agent-output/university/sku-manifest.json
- agent-output/university/sku-manifest.md
- agent-output/university/challenge-findings-requirements.json
- agent-output/university/challenge-findings-requirements-decisions.json
- agent-output/university/04-governance-constraints.{md,json} (pre-existing stand-in, not authored at Step 1)
