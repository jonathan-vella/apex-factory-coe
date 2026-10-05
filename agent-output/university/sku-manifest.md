# 📦 SKU Manifest - university

![Artifact](https://img.shields.io/badge/Artifact-SKU%20Manifest-blue?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Draft-orange?style=for-the-badge)
![Schema](https://img.shields.io/badge/Schema-sku--manifest--v1-purple?style=for-the-badge)

<details open>
<summary><strong>📑 Manifest Contents</strong></summary>

- [Overview](#overview)
- [Environments](#environments)
- [Services](#services)
- [Revision History](#revision-history)
- [Open Substitutions](#open-substitutions)

</details>

> Rendered from `sku-manifest.json` (rev 1) by `tools/scripts/render-sku-manifest-md.mjs`.
>
> **Do not hand-edit this file.** Mutate `sku-manifest.json` and re-run
> the renderer (wired into lefthook + CI). Authoring rules:
> [`.github/instructions/sku-manifest.instructions.md`](../../.github/instructions/sku-manifest.instructions.md).

## Overview

| Field            | Value                                                      |
| ---------------- | ---------------------------------------------------------- |
| Project          | `university`                                        |
| Default region   | `swedencentral` (per-service `regions[]` inherits this) |
| Schema version   | `sku-manifest-v1`                                          |
| Current revision | `1`                               |
| Last updated     | `2026-10-05T15:00:00Z`                                     |
| Environments     | `dev` (comma-separated)                              |
| Service count    | `5`                                        |

**Scope**: creative SKU decisions only — App Service plans, VMs/VMSS, SQL,
Cosmos, AKS pools, Redis, APIM, App Gateway, Storage replication tiers.

**Out of scope** (do not add to `services[]`): bandwidth, Log Analytics,
vnet, subnet, NSG, route table, public IP, diagnostics. See
[`.github/instructions/sku-manifest.instructions.md`](../../.github/instructions/sku-manifest.instructions.md).

## Environments

| Environment | In scope | Notes |
| ----------- | -------- | ----- |
| `dev` | ✅ | — |

## Services

> Rendered from `sku-manifest.json` `services[]`. Per-environment values
> reflect `environment_overrides` on top of the base entry.

| `id` | Service | Size (base) | Capacity | Zonal | Regions | SLA target / achieved | Commitment | Source | Rev |
| ---- | ------- | ----------- | -------- | ----- | ------- | --------------------- | ---------- | ------ | --- |
| `app-service-plan` | App Service Plan | `P0v3` | `fixed (default 1)` | ❌ | `swedencentral`, `germanywestcentral` | `best-effort (no target)` / `—` | `on-demand` | `user-pin` | `1` |
| `container-registry` | Container Registry | `Premium` | `fixed (default 1)` | ❌ | `swedencentral`, `germanywestcentral` | `best-effort (no target)` / `—` | `on-demand` | `user-pin` | `1` |
| `service-bus` | Service Bus Namespace | `Premium` | `fixed (default 1)` | ❌ | `swedencentral`, `germanywestcentral` | `best-effort (no target)` / `—` | `on-demand` | `user-pin` | `1` |
| `sql-managed-instance` | SQL Managed Instance | `GP_Gen5` | `fixed (default 1)` | ❌ | `swedencentral`, `germanywestcentral` | `best-effort (no target)` / `—` | `on-demand` | `user-pin` | `1` |
| `storage-account` | Storage Account | `Standard_LRS` | `fixed (default 1)` | ❌ | `swedencentral`, `germanywestcentral` | `best-effort (no target)` / `—` | `on-demand` | `user-pin` | `1` |

### Per-environment overrides

_No services declare environment overrides._

### Feature requirements

| `id` | `requires[]` | Verified at Step 4 |
| ---- | ------------ | ------------------ |
| `app-service-plan` | `vnet-integration`, `managed-identity` | ✅ / ❌ |
| `container-registry` | `private-endpoints`, `managed-identity` | ✅ / ❌ |
| `service-bus` | `private-endpoints`, `managed-identity` | ✅ / ❌ |
| `sql-managed-instance` | `managed-identity` | ✅ / ❌ |
| `storage-account` | `private-endpoints` | ✅ / ❌ |

### Cost estimate (USD/month)

> Populated by `cost-estimate-subagent` via `manifest_writeback[]` —
> Architect never types prices from parametric knowledge.

| `id` | `cost_estimate_monthly_usd` | Confidence |
| ---- | --------------------------- | ---------- |
| _none priced yet_ | — | — |

## Revision History

> Append-only. Each row is metadata about a git commit / apex-recall checkpoint.

| `rev` | Step | Agent | Created (UTC) | Summary | Changed `id`s | Commit | Checkpoint |
| ----- | ---- | ----- | ------------- | ------- | ------------- | ------ | ---------- |
| `1` | `1` | `02-Requirements` | `2026-10-05T15:00:00Z` | User pins from the B09 archetype brief for every service class; on-demand only, single dev environment, no zones; approved regions swedencentral + germanywestcentral. | `app-service-plan`, `container-registry`, `sql-managed-instance`, `storage-account`, `service-bus` | — | `university:1:phase_5_artifact` |

## Open Substitutions

> Captured at Step 6 (Deploy) when a planned SKU is unavailable due to
> quota / region capacity. Mirrors `decisions.sku_overrides[]` in
> `00-session-state.json`.

> **None open** — all SKUs deployed as planned.

---

## References

- Schema: [`tools/schemas/sku-manifest.schema.json`](../../tools/schemas/sku-manifest.schema.json)
- Authoring rules: [`.github/instructions/sku-manifest.instructions.md`](../../.github/instructions/sku-manifest.instructions.md)
- Renderer: `node tools/scripts/render-sku-manifest-md.mjs <project>`
- Validators: `npm run validate:sku-manifest` + `npm run validate:sku-iac-coverage`
