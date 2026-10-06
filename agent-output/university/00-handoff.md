# university — Step 3.5 governance gate

## Completed Steps
- agent-output/university/01-requirements.md
- agent-output/university/02-architecture-assessment.md
- agent-output/university/04-governance-constraints.md

## Key Decisions
- agent-output/university/04-governance-constraints.json
- agent-output/university/sku-manifest.json
- agent-output/university/sku-manifest.md
- decision: subscription_scope=workload

## Open Challenger Findings (must_fix only)
- none (pass 4: 0 must_fix; 2 should_fix deferred to Step 4)

## Required Challenger Review
- review: governance-reconciliation
- pass: 4 (complete; 0 must_fix, 2 should_fix deferred)
- owner: 10-Challenger
- artifact: agent-output/university/04-governance-constraints.md
- status: complete

## Context for Next Step
- next-owner: 05-IaC Planner (after owner approval of Step 3.5)
- gate: owner approval pending the location evidence below
- step 4: plan DINE rows as discovered assignments with remediation unverified; keep the post-deploy DNS (capability 15) and diagnostics readiness gates; owner states B08 grants the DINE identities their roles
- deferred 6dec76b5: the 3 filtered Defender for Cloud assignments were checked (11 policies, all DeployIfNotExists, none Deny): verified non-blocking; the Defender for SQL managed instances DINE applies to the archetype SQL MI as configuration, not a block
- deferred 1d29a697: both ALZ-lite location effects (Audit) were verified by the owner with az policy assignment show at mg-factory-corp on 2026-10-06, not from discovery; discovery does not itemize Audit effects or allowed locations; cite that command as the Step 4 evidence; command output still pending in the decisions log

## Skill Context
- .github/skills/apex-azure-governance-discovery/SKILL.md
- .github/skills/apex-azure-defaults/SKILL.md

## Artifacts
- agent-output/university/04-governance-constraints.md
- agent-output/university/04-governance-constraints.json
