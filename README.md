<!-- markdownlint-disable MD033 MD041 -->

# APEX Factory CoE

<div align="center">
  <img
   src="https://capsule-render.vercel.app/api?type=waving&height=180&color=0:0A66C2,50:0078D4,110:00B7C3&text=APEX%20Factory%20CoE&fontSize=40&fontColor=FFFFFF&fontAlignY=34&desc=CoE%20archetype%20for%20the%20Partner%20Modernization%20Factory%20hackathon&descAlignY=56"
   alt="APEX Factory CoE banner" />
</div>

> The Center of Excellence (CoE) workspace for the
> [Partner Modernization Factory hackathon](https://github.com/jonathan-vella/apex-factory-hackathon).
> The CoE uses [APEX](https://apexops.pro/) here to design and build the **CoE archetype** that every
> hackathon member later deploys into their vended spoke.

[![Azure](https://img.shields.io/badge/Azure-0078D4?logo=microsoft-azure&logoColor=white)](https://azure.microsoft.com)
[![Bicep](https://img.shields.io/badge/Bicep-0078D4?logo=azure-pipelines&logoColor=white)](https://github.com/Azure/bicep)
[![Copilot](https://img.shields.io/badge/GitHub_Copilot-000000?logo=github-copilot&logoColor=white)](https://github.com/features/copilot)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## What this repo is for

This repo was created from the [`apex-accelerator`](https://github.com/jonathan-vella/apex-accelerator)
template. It is where the CoE runs APEX steps 1–5 for the hackathon's landing-zone workload, passing every
human gate and challenger review. It is not the hackathon kit and attendees don't work in it.

```mermaid
flowchart LR
    BRIEF["Kit: archetype/BRIEF.md"] --> COE["This repo: APEX steps 1–5<br/>(Requirements → Bicep)"]
    COE --> KIT["Kit: archetype/<br/>(artifacts, state, Bicep)"]
    KIT --> MEMBER["Member's APEX repo<br/>Deploy (step 6) + As-Built (step 7)"]
```

1. **Input.** [`archetype/BRIEF.md`](https://github.com/jonathan-vella/apex-factory-hackathon/issues/9) in the
   hackathon kit is the workload brief pasted into Step 1 (Requirements).
2. **Build.** APEX runs Requirements → Architecture → Design → Governance → IaC Plan → Bicep code here.
3. **Package.** The finished project (`agent-output/university/`, `infra/bicep/university/`) is copied into the
   kit's `archetype/` folder, pinned to this repo's commit.
4. **Deploy.** At the event, each member copies `archetype/` into their own APEX repo and runs only APEX Deploy
   and As-Built, supplying a tenant ID, subscription ID and unique suffix.

## The archetype

| Item           | Value                                                                                 |
| -------------- | ------------------------------------------------------------------------------------- |
| APEX project   | `university` (Contoso University)                                                     |
| IaC            | Bicep only, AVM modules where available                                               |
| Target         | The member's existing vended Corp spoke under ALZ-lite (no new VNet, subnets or DNS)  |
| Services       | App Service for Linux (containers), ACR Premium, SQL Managed Instance General Purpose, Blob Storage, Service Bus Premium, Key Vault, Application Insights |
| Region         | Derived from the hub (`swedencentral` by default)                                     |
| Governance     | Must pass every ALZ-lite deny policy; private endpoints register in the central DNS zones by policy |

Scope and decisions live in the kit's [PRD](https://github.com/jonathan-vella/apex-factory-hackathon/blob/main/docs/prd.md)
and backlog item [B09](https://github.com/jonathan-vella/apex-factory-hackathon/issues/9).

## Repo layout

```text
agent-output/university/   # APEX artifacts, challenger reviews and workflow state
infra/bicep/university/    # Bicep produced by APEX step 5
.github/                   # APEX agents, skills, instructions (upstream-managed)
tools/                     # APEX scripts and MCP servers (upstream-managed)
```

## Setup

You need VS Code, GitHub Copilot, Docker Desktop (or Codespaces) and access to the hackathon build tenant.

1. Open the repo in the dev container (**Dev Containers: Reopen in Container**).
2. Initialize once, then commit the result:

   ```bash
   npm install
   npm run init
   npm run sync:workflows
   ```

3. Sign in with `az login` and run `npm run setup` if you want the governance baseline workflow. Otherwise
   APEX's Governance step (3.5) discovers policy from your signed-in subscription.

See the [APEX docs](https://apexops.pro/) for the full setup and the
[prompt guide](https://apexops.pro/guides/prompt-guide/) for running each step.

## Running the workflow

1. Select the **02-Requirements** agent (Step 1) and paste the whole of `archetype/BRIEF.md`. Answer every
   SKU question with the brief's pinned values, and confirm the existing vended spoke for networking.
2. Continue through Steps 2–5, passing every human gate and challenger review.
3. Commit, then hand the repo URL and commit SHA to the hackathon kit's B09 owner for packaging.

Don't run APEX Deploy (step 6) here. Deployment is validated from the kit's `archetype/` copy.

## Staying in sync with APEX

`README.md`, `agent-output/`, `infra/bicep/` and `.github/workflows/` are yours and survive the weekly
upstream sync. Everything else follows [APEX](https://github.com/jonathan-vella/apex). Before re-packaging the
archetype after a sync, re-run the affected steps and record the new commit in the kit's `versions.md`.

```bash
npm run validate:all
bicep lint infra/bicep/university/main.bicep
```

## License

[MIT](LICENSE)
