<!-- markdownlint-disable MD033 MD041 -->

<a id="readme-top"></a>

<div align="center">

![Status](https://img.shields.io/badge/Status-In%20Progress-yellow?style=for-the-badge)
![Step](https://img.shields.io/badge/Step-3.5%20of%207-blue?style=for-the-badge)

# 🏗️ university

**CoE archetype (B09): Azure platform for the Contoso University app in an existing Corp landing zone spoke**

[View Architecture](#-architecture) · [View Artifacts](#-generated-artifacts) · [View Progress](#-workflow-progress)

</div>

---

## 📋 Project Summary

| Property           | Value                                                          |
| ------------------ | -------------------------------------------------------------- |
| **Created**        | 2026-10-05                                                     |
| **Last Updated**   | 2026-10-06                                                     |
| **Region**         | swedencentral (derived from hub; alternate germanywestcentral) |
| **Environment**    | dev (training/demo)                                            |
| **Estimated Cost** | $1,320.95/month (≈ $1.81/hour), Step 2 verified pricing        |
| **AVM Coverage**   | Determined at Step 5                                           |

---

## ✅ Workflow Progress

```text
[██████░░░░░░░░░░░░░░] 29% Complete (2 of 7 required steps; optional Design skipped)
```

| Step | Phase          |                                    Status                                     | Artifact                                                           |
| :--: | -------------- | :---------------------------------------------------------------------------: | ------------------------------------------------------------------ |
|  1   | Requirements   |     ![Done](https://img.shields.io/badge/-Done-success?style=flat-square)     | [01-requirements.md](./01-requirements.md)                         |
|  2   | Architecture   |     ![Done](https://img.shields.io/badge/-Done-success?style=flat-square)     | [02-architecture-assessment.md](./02-architecture-assessment.md) · [03-des-cost-estimate.md](./03-des-cost-estimate.md) — approved 2026-10-06 |
|  3   | Design         |   ![Skip](https://img.shields.io/badge/-Skipped-blue?style=flat-square)   | Skipped by owner (As-Built covers diagrams) |
| 3.5  | Governance     | ![Pending](https://img.shields.io/badge/-Pending-lightgrey?style=flat-square) | [04-governance-constraints.md](./04-governance-constraints.md)     |
|  4   | Planning       | ![Pending](https://img.shields.io/badge/-Pending-lightgrey?style=flat-square) | [04-implementation-plan.md](./04-implementation-plan.md)           |
|  5   | Implementation | ![Pending](https://img.shields.io/badge/-Pending-lightgrey?style=flat-square) | [05-implementation-reference.md](./05-implementation-reference.md) |
|  6   | Deployment     | ![Pending](https://img.shields.io/badge/-Pending-lightgrey?style=flat-square) | [06-deployment-summary.md](./06-deployment-summary.md)             |
|  7   | Documentation  | ![Pending](https://img.shields.io/badge/-Pending-lightgrey?style=flat-square) | [07-documentation-index.md](./07-documentation-index.md)           |

> **Legend**:
> ![Done](https://img.shields.io/badge/-Done-success?style=flat-square) Complete
> | ![WIP](https://img.shields.io/badge/-WIP-yellow?style=flat-square) In Progress
> | ![Pending](https://img.shields.io/badge/-Pending-lightgrey?style=flat-square) Pending
> | ![Skip](https://img.shields.io/badge/-Skipped-blue?style=flat-square) Skipped

---

## 🏛️ Architecture

No diagram yet (produced at Step 3/4).

### Key Resources

| Resource                 | Type                                | SKU                     | Purpose                                 |
| ------------------------ | ----------------------------------- | ----------------------- | --------------------------------------- |
| App Service plan + app   | Microsoft.Web                       | P0v3 (Linux, container) | Hosts the Contoso University container  |
| Container registry       | Microsoft.ContainerRegistry         | Premium                 | Private image registry                  |
| SQL Managed Instance     | Microsoft.Sql/managedInstances      | GP Gen5, 4 vCores, 64 GB | Migration target database              |
| Storage account          | Microsoft.Storage                   | Standard_LRS (GPv2)     | `teaching-materials` blobs              |
| Service Bus namespace    | Microsoft.ServiceBus                | Premium, 1 MU           | `notifications` queue                   |
| Key Vault                | Microsoft.KeyVault                  | Standard                | Runtime secrets                         |
| Application Insights     | Microsoft.Insights                  | Workspace-based         | Telemetry to central `log-management`   |
| User-assigned identity   | Microsoft.ManagedIdentity           | —                       | Web app runtime identity                |

---

## 📄 Generated Artifacts

<details open>
<summary><strong>📁 Step 1-3: Requirements, Architecture & Design</strong></summary>

| File                                       | Description                        |                               Status                               | Created    |
| ------------------------------------------ | ---------------------------------- | :----------------------------------------------------------------: | ---------- |
| [01-requirements.md](./01-requirements.md) | Project requirements with NFRs     | ![Done](https://img.shields.io/badge/-Done-success?style=flat-square) | 2026-10-05 |
| [sku-manifest.json](./sku-manifest.json)   | SKU manifest rev 2 (pins verified, prices written back) | ![Done](https://img.shields.io/badge/-Done-success?style=flat-square) | 2026-10-06 |
| [sku-manifest.md](./sku-manifest.md)       | Rendered SKU manifest              | ![Done](https://img.shields.io/badge/-Done-success?style=flat-square) | 2026-10-05 |
| [challenge-findings-requirements.json](./challenge-findings-requirements.json) | Step 1 challenger review | ![Done](https://img.shields.io/badge/-Done-success?style=flat-square) | 2026-10-05 |
| [challenge-findings-requirements-decisions.json](./challenge-findings-requirements-decisions.json) | Per-finding decisions | ![Done](https://img.shields.io/badge/-Done-success?style=flat-square) | 2026-10-05 |
| [02-architecture-assessment.md](./02-architecture-assessment.md) | WAF assessment (S 8, R 4, P 6, C 7, O 7) | ![Done](https://img.shields.io/badge/-Done-success?style=flat-square) | 2026-10-06 |
| [03-des-cost-estimate.md](./03-des-cost-estimate.md) | Cost estimate ($1,320.95/month) | ![Done](https://img.shields.io/badge/-Done-success?style=flat-square) | 2026-10-06 |
| [02-waf-scores.png](./02-waf-scores.png) · [03-des-cost-distribution.png](./03-des-cost-distribution.png) · [03-des-cost-projection.png](./03-des-cost-projection.png) | Charts (`.py` + `.png` + `.svg`) | ![Done](https://img.shields.io/badge/-Done-success?style=flat-square) | 2026-10-06 |
| [02-cost-estimate.json](./02-cost-estimate.json) | Workload pricing (COMPLETE, ARM MCP) | ![Done](https://img.shields.io/badge/-Done-success?style=flat-square) | 2026-10-06 |
| [02-cost-estimate-sqlmi-options.json](./02-cost-estimate-sqlmi-options.json) · [02-cost-estimate-sqlmi-schedule.json](./02-cost-estimate-sqlmi-schedule.json) · [02-cost-estimate-shared-services.json](./02-cost-estimate-shared-services.json) · [02-cost-estimate-manifest.json](./02-cost-estimate-manifest.json) | Comparison, shared-services and manifest pricing (COMPLETE) | ![Done](https://img.shields.io/badge/-Done-success?style=flat-square) | 2026-10-06 |
| [challenge-findings-architecture.json](./challenge-findings-architecture.json) | Step 2 architecture review (APPROVED) | ![Done](https://img.shields.io/badge/-Done-success?style=flat-square) | 2026-10-06 |
| [challenge-findings-cost-estimate.json](./challenge-findings-cost-estimate.json) | Step 2 cost-feasibility review (APPROVED) | ![Done](https://img.shields.io/badge/-Done-success?style=flat-square) | 2026-10-06 |
| [challenge-findings-architecture-decisions.json](./challenge-findings-architecture-decisions.json) | Step 2 per-finding decisions | ![Done](https://img.shields.io/badge/-Done-success?style=flat-square) | 2026-10-06 |

</details>

---

## 🔗 Related Resources

| Resource            | Path                                                                           |
| ------------------- | ------------------------------------------------------------------------------ |
| **Bicep Templates** | [`infra/bicep/university/`](../../infra/bicep/university/) (created at Step 5) |
| **Workflow Docs**   | [Published workflow guide](https://apexops.pro/concepts/workflow/)             |
| **Troubleshooting** | [Published troubleshooting guide](https://apexops.pro/guides/troubleshooting/) |

---

<div align="center">

**Generated by [APEX](../../README.md)** · 02-Requirements agent | 2026-10-05

<a href="#readme-top">⬆️ Back to Top</a>

</div>
