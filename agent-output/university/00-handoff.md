# university — Step 3 design complete, Step 4 in progress

## Status
- Step 3 reopened 2026-10-06 (`skip_design=false`) and approved by the owner on 2026-10-06. Step 3 stays complete.
- All 8 ADRs are approved with current reviews: 0 must_fix and 0 should_fix. The only open item is suggestion 3ea4e2b0 (ADR-0004).
- Step 4 was already marked in progress and was not reset or touched.
- Steps 2 and 3.5 are complete and unchanged.

## Completed Steps
- agent-output/university/01-requirements.md
- agent-output/university/02-architecture-assessment.md
- agent-output/university/03-des-diagram.py
- agent-output/university/03-des-diagram.png
- agent-output/university/03-des-diagram.svg
- agent-output/university/03-des-network-diagram.py
- agent-output/university/03-des-network-diagram.png
- agent-output/university/03-des-network-diagram.svg
- agent-output/university/03-des-adr-0001-linux-app-service-vs-aks.md
- agent-output/university/03-des-adr-0002-sql-mi-general-purpose-vs-azure-sql-database.md
- agent-output/university/03-des-adr-0003-service-bus-and-acr-premium-private-endpoints.md
- agent-output/university/03-des-adr-0004-acr-trusted-services-bypass-for-import.md
- agent-output/university/03-des-adr-0005-azure-hybrid-benefit-default-on.md
- agent-output/university/03-des-adr-0006-inherited-cost-monitoring-from-vending.md
- agent-output/university/03-des-adr-0007-no-zone-redundancy-region-derived-from-hub.md
- agent-output/university/03-des-adr-0008-native-sql-mi-start-stop-schedule.md
- agent-output/university/04-governance-constraints.md
- agent-output/university/04-governance-constraints.json

## Review Evidence
Pass 1 (comprehensive, `challenger-review-subagent`), supporting inputs 01, 02 and 04-governance-constraints.md:
- agent-output/university/challenge-findings-design-adr-0001.json to challenge-findings-design-adr-0008.json: 0 must_fix, 17 should_fix, 8 suggestions
- Owner dispositions: challenge-findings-design-adr-000N-decisions.json. 24 accepted and applied; 06e6a398 rejected (subscription_id allowed in 04-governance-constraints.json; governance preview.md deleted)

Pass 2 (confirmation, comprehensive) and pass 3 (ADR-0001), all passing `validate-challenger-findings --verify-cache` against the current ADR bytes:
- ADR-0002, ADR-0003: run by `10-Challenger`. Approved, 0 findings.
- ADR-0004 to ADR-0008: pass 2, ADRs unchanged since. 0 must_fix, 0 should_fix; ADR-0004 has suggestion 3ea4e2b0 (Learn quote not verified by the worker).
- ADR-0001: pass 2 found should_fix 3c8478ec (Performance arrow versus an unevaluated AKS design). The owner applied it (Performance set to Not assessed). `challenge-findings-design-adr-0001-pass3.json` (`10-Challenger`, comprehensive): APPROVED, 0 findings, 3c8478ec closed. It supersedes the stale `challenge-findings-design-adr-0001-pass2.json`, which is kept as history.

## Open Findings
- 3ea4e2b0 (ADR-0004, suggestion): Learn quotation not independently verified by the worker.
- Resolved: 3c8478ec (ADR-0001, should_fix), closed by `challenge-findings-design-adr-0001-pass3.json`; recorded as resolved in challenge-findings-design-adr-0001-decisions.json.

## Next Step
- next-owner: 05-IaC Planner (Step 4, already in progress)
- carry: the ADR set and the open suggestion above; the owner-stated MI-link topology below

## Owner decisions
- The subscription-ID exception covers all of `04-governance-constraints.json`, including its resource paths.
- ADR-0008 keeps "about 20 minutes" with its Microsoft Learn citation.

## Owner-stated facts not in Step 1-3.5 artifacts
- MI link topology (B07/B08): `vm-app01` (SQL Server 2022, datacenter) through the hub firewall to `snet-sqlmi`, TCP 5022 and 11000-11999. Labelled owner-stated in ADR-0002 and the diagrams.

## Skill Context
- .github/skills/apex-python-diagrams/SKILL.md
- .github/skills/apex-azure-adr/SKILL.md
