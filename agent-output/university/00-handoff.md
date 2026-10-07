# university — Handoff (Step 4 complete, revised)

Updated: 2026-10-07T13:25:00Z | IaC: Bicep | Next owner: 06b-Bicep CodeGen (Step 5 update)

## Completed Steps

- [x] Step 1 → agent-output/university/01-requirements.md
- [x] Step 2 → agent-output/university/02-architecture-assessment.md
- [x] Step 3 → agent-output/university/03-des-adr-0001…0008 (8 ADRs) and 03-des diagrams, approved 2026-10-06
- [x] Step 3.5 → agent-output/university/04-governance-constraints.md
- [x] Step 4 → agent-output/university/04-implementation-plan.md, revised and APPROVED 2026-10-07 (sha256 `5a4b96e45528e883d5cc870886e892efc0fb7d29c5b6f22c7b5aef7e63171a00`)
- [ ] Step 5 → infra/bicep/university/ was built from plan `4a67a8d8`; update it for the Step 4 revisions

## Key Decisions

- One subscription-scope deployment; inputs: tenant ID, subscription ID, suffix; region swedencentral (derived from hub)
- deployment_strategy=single; identity_model=user-assigned-shared (`id-university-<suffix>`, web app only); public_edge_auth=none
- script_runtime_image=not-applicable; az_posture=single-zone-mvp (forced values recorded: Service Bus `zoneRedundant`, ACR `zoneRedundancy`)
- 10 AVM modules, all MCR-latest and frozen; raw Bicep: SQL MI and `startStopSchedules` `@2025-01-01`, diagnostic settings `@2016-09-01`
- Stable APIs apply to archetype-declared resources; AVM-internal preview versions listed for B09 `versions.md`
- Hooks: azd pre/postprovision in pwsh 7, `az` only, standalone `.ps1` reusable by `archetype/deploy.ps1`
- SKU manifest rev 3 locked; 57f85f8d (deployer grant scope) and 569fbf8e (shared `id-sqlmi-directory` attachable outside SQL MI) are owner-accepted risks
- ACR `azureADAuthenticationAsArmPolicyStatus: 'enabled'` (UAMI pull); 3fd577cf deferred (policy 42781ec6 reports Audit or Disabled, never Deny)
- SQL MI identity `SystemAssigned,UserAssigned`, primary = `id-sqlmi-directory` (derived by preflight, no new member input)
- Subscription-ID exception covers all of `04-governance-constraints.json`; ADR-0008 keeps "about 20 minutes"

## Open Challenger Findings (must_fix only)

- None. Completion selected `challenge-findings-plan-pass3.json` (comprehensive confirmation of plan `5a4b96e4`, 0 findings); 569fbf8e is owner-accepted, 92079f7c deferred (B08), 2c0780f6 applied.

## Context for Next Step

- CodeGen delta (contract `plan_ref` = `5a4b96e4`): Task 4 ACR `azureADAuthenticationAsArmPolicyStatus: 'enabled'`; Task 13 step 5 pre-switch check; Task 1 param `sqlMiDirectoryIdentityId`; Task 9 SQL MI `SystemAssigned,UserAssigned` + `primaryUserAssignedIdentityId`; Task 11 `SQLMI_DIRECTORY_IDENTITY_ID`; Task 12 steps 9–11 (atScope query kept, new identity check, outputs). Rebuild `05-iac-handoff.json` after the code changes.
- Earlier validate and what-if (before the revisions) passed; Private endpoint diagnostics `@2016-09-01` were accepted, so the `2021-05-01-preview` fallback was not used. No tenant or subscription IDs are in the tree.
- Run the security scanner as `npm run validate:iac-security-baseline -- --public-web-app infra/bicep/university/modules/web-app.bicep`; without the flag the approved public web app is reported.
- Preflight step 9 reads inherited policy assignments through the ARM `atScope()` filter, because `az policy assignment list` omitted the management-group ALZ-lite assignments in this tenant.
- Azure Hybrid Benefit assumption and the opt-out are in `infra/bicep/university/README.md` (5d241bcc closed).
- Owner-approved variance: Key Vault `tenantId` contract input is not wired (AVM 0.14.2 has no such parameter). Plan-heading warnings (`st-container`, `sbns-queue`, `sqlmi-schedule`) are left as cosmetic; the frozen plan and contract are unchanged.
- Budget warning: `budget-factory-workload` is 500; the 24×7 run rate is about $1,321/month, so `actual80` will fire.
- Telemetry readiness stays pending the app image (B06/B10); local auth stays off.
- Owner-stated, not in Step 1–3.5 artifacts: MI link `vm-app01` → hub firewall → `snet-sqlmi`, TCP 5022 and 11000–11999 (B07/B08).
- B08 dependency (92079f7c, deferred): ALZ-lite creates `id-sqlmi-directory` and only teardown removes it; recovery is to re-run ALZ-lite, the grant script and vending. Verified 2026-10-07: it exists and the deployer can assign it.

## Skill Context

- .github/skills/apex-azure-defaults/SKILL.md
- .github/skills/apex-azure-bicep-patterns/SKILL.md
- .github/skills/apex-iac-common/references/codegen-do-dont.md
- Security baseline: TLS 1.2, HTTPS-only, managed identity, no keys or secrets; AVM-first

## Artifacts

- agent-output/university/04-implementation-plan.md
- agent-output/university/04-iac-contract.json
- agent-output/university/04-policy-property-map.json
- agent-output/university/04-environment-manifest.json
- agent-output/university/04-dependency-diagram.{py,png,svg} and 04-runtime-diagram.{py,png,svg}
- agent-output/university/sku-manifest.{json,md} (rev 3, locked)
- agent-output/university/challenge-findings-plan.json, -pass2 to -pass7 (pass3 = selected confirmation), and challenge-findings-plan-decisions.json
