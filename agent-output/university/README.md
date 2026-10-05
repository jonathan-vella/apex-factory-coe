<!-- markdownlint-disable MD033 MD041 -->

<a id="readme-top"></a>

<div align="center">

![Status](https://img.shields.io/badge/Status-In%20Progress-yellow?style=for-the-badge)
![Step](https://img.shields.io/badge/Step-2%20of%207-blue?style=for-the-badge)

# 🏗️ university

**CoE archetype (B09): Azure platform for the Contoso University app in an existing Corp landing zone spoke**

[View Architecture](#-architecture) · [View Artifacts](#-generated-artifacts) · [View Progress](#-workflow-progress)

</div>

---

## 📋 Project Summary

| Property           | Value                                                          |
| ------------------ | -------------------------------------------------------------- |
| **Created**        | 2026-10-05                                                     |
| **Last Updated**   | 2026-10-05                                                     |
| **Region**         | swedencentral (derived from hub; alternate germanywestcentral) |
| **Environment**    | dev (training/demo)                                            |
| **Estimated Cost** | ≈ $1.83/hour workload, brief estimate pending Step 2 pricing   |
| **AVM Coverage**   | Determined at Step 5                                           |

---

## ✅ Workflow Progress

```text
[███░░░░░░░░░░░░░░░░░] 14% Complete (Step 1 of 7 required steps)
```

| Step | Phase          |                                    Status                                     | Artifact                                                           |
| :--: | -------------- | :---------------------------------------------------------------------------: | ------------------------------------------------------------------ |
|  1   | Requirements   |     ![Done](https://img.shields.io/badge/-Done-success?style=flat-square)     | [01-requirements.md](./01-requirements.md)                         |
|  2   | Architecture   | ![Pending](https://img.shields.io/badge/-Pending-lightgrey?style=flat-square) | [02-architecture-assessment.md](./02-architecture-assessment.md)   |
|  3   | Design         | ![Pending](https://img.shields.io/badge/-Pending-lightgrey?style=flat-square) | [03-des-\*.md](.)                                                  |
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
| [sku-manifest.json](./sku-manifest.json)   | SKU manifest rev 1 (user pins)     | ![Done](https://img.shields.io/badge/-Done-success?style=flat-square) | 2026-10-05 |
| [sku-manifest.md](./sku-manifest.md)       | Rendered SKU manifest              | ![Done](https://img.shields.io/badge/-Done-success?style=flat-square) | 2026-10-05 |
| [challenge-findings-requirements.json](./challenge-findings-requirements.json) | Step 1 challenger review | ![Done](https://img.shields.io/badge/-Done-success?style=flat-square) | 2026-10-05 |
| [challenge-findings-requirements-decisions.json](./challenge-findings-requirements-decisions.json) | Per-finding decisions | ![Done](https://img.shields.io/badge/-Done-success?style=flat-square) | 2026-10-05 |

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
