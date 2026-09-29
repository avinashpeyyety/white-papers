---
title: "Tiered On-Premises Intelligence Pods: Open Models Next to Enterprise Data"
slug: tiered-on-prem-intelligence-pods
date: 2026-09-29
author: Avinash Peyyety
version: "1.0"
contributors: SpaceXAI
excerpt: >-
  Most enterprises don't need a private token factory. They need a few
  purpose-built intelligence pods that run continuous agents and open-weight
  models next to their data. This paper defines three tiers (Ops, Domain,
  Strategic), sizes them by concurrency on standard 8-GPU servers, and makes
  orchestration and audit a non-negotiable part of every pod.
tags:
  - open models
  - on-prem AI
  - AI infrastructure
  - agents
  - GPU sizing
  - air-gap
---

## Abstract

**Thesis.** Enterprise AI is splitting into two paths that should not be conflated. *Human acceleration* uses frontier models delivered as cloud subscriptions to help people build faster. *Institutional intelligence* runs always-on agents and high-value offline models next to enterprise data, under enterprise control. Most organizations do not need a private "token factory" that tries to out-serve the public cloud. They need a small number of **intelligence pods**: purpose-built on-premises systems running open-weight models for three specific jobs.

**Contribution.** This paper defines a three-tier pod line (Ops, Domain and Strategic), sizes each tier by **concurrent sessions** rather than by model size, and builds every tier from one hardware unit: the standard 8-GPU server. It also argues that no pod should ship without an **orchestration and audit control plane** covering agents, code, tokens and actions.

**What changed in this version.** Earlier drafts claimed a 2–3 trillion-parameter mixture-of-experts (MoE) model could run on roughly one GPU because few experts are active per token. That is wrong. Sparse activation reduces *compute* per token, not *memory*, since all expert weights must still be resident. Section 3 corrects the sizing with explicit memory arithmetic. The top tier's conclusion, one 8-GPU node rather than a 72-GPU rack, still holds.

## 1. Two paths, one mistake to avoid

| | Path A: human acceleration | Path B: institutional intelligence |
|---|---|---|
| **Who uses it** | Engineers, analysts, operators | Always-on agents and small analyst cohorts |
| **Delivered as** | Cloud subscriptions and coding harnesses | On-prem pods beside enterprise data |
| **Model** | Latest frontier (closed or open) | Version-pinned open-weight models |
| **Optimizes for** | Capability and speed of iteration | Control, residency, isolation, continuity |

The common mistake is a board-level false choice between "all cloud" and "build a private AI factory."

- **All-cloud continuous agents** on sensitive production systems raise egress, data residency and blast-radius concerns.
- **Fleet-wide on-prem token serving** recreates hyperscaler capital spend without hyperscaler utilization, and builders still want the newest frontier models anyway.

Pods address Path B only. They complement subscriptions; they don't replace them.

## 2. The three tiers

| Tier | Pod | Model class (open-weight) | Design concurrency | Standard GPU build | Network posture | Primary job |
|---|---|---|---|---|---|---|
| 1 | **Ops** | ~0.5T-parameter MoE | 200–500 agent sessions | 4 × 8-GPU B200-class, air-cooled (32 GPUs) | Production-adjacent | 24×7 monitoring and assisted operation of data systems |
| 2 | **Domain** | ~0.7–1.2T MoE + adapters | 50–150 domain agents | 2 × 8-GPU B200-class, air-cooled (16 GPUs) + data rack | Segmented network | Multi-agent analytics and retrieval over private corpora |
| 3 | **Strategic** | ~1–3T frontier-class open MoE | 5–20 analyst seats | 1 × 8-GPU Rubin-class, liquid-cooled + ~100 TB NFS data clone | Air-gapped, diode-fed | Deep reasoning on a daily-synced enterprise data clone |

**Note the inversion.** Tier 1 has the *most* GPUs because hundreds of always-on agents force many model replicas. Tier 3 has the *fewest* because a handful of analysts share one large model. GPU count follows concurrency, not prestige.

### Choosing a tier

- Start with **Ops** if your pain is pipeline incidents, data quality breaches and on-call load, and you have good observability but little automated remediation.
- Choose **Domain** when depth of retrieval and domain reasoning matter more than raw agent fan-out, and security wants segmentation but not a full air-gap.
- Choose **Strategic** only for a small, cleared cohort doing high-stakes analysis that must never touch an external network.
- Most organizations should land a Tier 1 pod with measured results before considering Tier 3.

## 3. Sizing by concurrency, with the memory math shown

### 3.1 The building block

Every tier uses the same unit: a standard 8-GPU server (DGX or OEM HGX class). Scale only in whole nodes. This keeps procurement, support and facilities predictable.

### 3.2 Memory first, then fan-out

A model replica must hold all of its weights in GPU memory, plus room for the key-value (KV) cache that grows with context length and concurrent sessions.

> **Weight memory ≈ total parameters × bytes per parameter**

At 8-bit precision that is about 1 byte per parameter; at 4-bit, about 0.5. **Total** parameters count, not active ones. MoE sparsity makes each token cheaper to compute, but every expert must be resident.

| Model class | Weights at FP8 | Weights at FP4 | Min. B200-class GPUs (~180 GB) | Min. Rubin-class GPUs (~288 GB) |
|---|---|---|---|---|
| ~0.5T MoE | ~500 GB | ~250 GB | 4 (FP8) / 2 (FP4, tight) | 2 / 1 (tight) |
| ~1T MoE | ~1 TB | ~500 GB | 8 / 4 | 4 / 2 |
| ~2–3T MoE | ~2–3 TB | ~1–1.5 TB | beyond one node / 8 | 8+ / 4–6 |

GPU memory figures are approximate public specifications; KV headroom is extra. Treat the table as a planning floor and pin real numbers with load tests per model release.

Then size the fleet:

> **GPUs ≈ ⌈concurrent sessions ÷ sessions per replica⌉ × GPUs per replica + spare**

where *spare* covers embedders, verifiers and high availability.

### 3.3 What that means per tier

- **Ops (32 GPUs).** A ~0.5T MoE needs roughly 2–4 B200-class GPUs per replica depending on precision, giving about 8–16 replicas across 32 GPUs. Hundreds of agent sessions share those replicas because most agent time is spent waiting on tools, not generating tokens. Entry build: 16 GPUs below ~150 sessions.
- **Domain (16 GPUs).** A ~1T MoE at FP4 needs about 4 GPUs per replica, so roughly 3–4 heavier replicas for 50–150 domain agents.
- **Strategic (8 GPUs).** A 1–3T MoE at FP4 occupies most or all of one 8-GPU Rubin-class node (~2.3 TB total HBM), using tensor and expert parallelism. Verifiers, embedders and rerankers are small models placed in remaining headroom or on CPU. One node serves 5–20 analysts at low batch. **A 72-GPU rack-scale system is not the default.** Promote to it only when measured demand shows concurrent multi-model farms, on-site post-training, or seat growth that saturates several nodes.

## 4. What every pod runs

Every tier ships the same logical stack; scale, model size and security controls change by tier.

1. **Inference and models:** a production serving engine (vLLM or TensorRT-LLM class), routing, quantization profiles, canary and rollback.
2. **Orchestration and audit control plane:** described in Section 5. Mandatory.
3. **Data and retrieval:** a Postgres-class query engine with vector search, hybrid sparse and dense retrieval, lineage-aware chunking, evaluation harnesses.
4. **Tier packs:** ops runbooks (Tier 1), domain agent packs (Tier 2), an intelligence studio for pattern detection, entity graphs and narrative briefs (Tier 3).
5. **Trust layer:** role-based access, secrets, retention policies, and diode workflows where applicable.

### Tier-specific notes

- **Ops:** agents use runbook tools across metrics, logs, catalogs and ticketing. **Every write action requires human approval.** Detection covers SLO burn, schema drift, freshness breaches and anomalous job cost.
- **Domain:** shared agent memory and policy packs; optional controlled calls to external frontier APIs for builder assistance, never on the core path for sensitive corpora.
- **Strategic:** the ~100 TB NFS clone (Section 6) is the system of record inside the enclave; no general employee chat; no model calls leave the enclave.

## 5. The control plane is not optional

A pod is a production system, not "a GPU with a chat UI." Continuous agents without orchestration become unaccountable automation.

| Object | What is captured | Control examples |
|---|---|---|
| **Agents** | Identity, version, owner, schedule, session lineage | Quotas, kill switch, environment allow-list |
| **Code and tools** | Tool registry, prompts, configs, deployments | Signed artifacts, promotion gates, rollback |
| **Tokens** | Prompt and completion units, model, cost class, tenant | Budgets, rate limits, reserved capacity |
| **Actions** | Tool calls, redacted arguments, results, approvals | Human approval for writes; dual control on enclave export |
| **Data access** | Datasets, mounts, retrieval scopes | Row-level security, enclave paths, diode promotion events |

Responsibilities: schedule and route jobs to replicas; enforce who may run which agent, on which data, with which tools, at what budget; trace every step from prompt to retrieval to tool call to approval to outcome; gate model and tool changes with evaluations; and pause a fleet, revoke a tool or freeze egress without taking the pod offline.

**Build or buy.** Hardware vendors' AI platform stacks are well suited to cluster and model lifecycle management and bring warranty-aligned support. Agent, action and token accountability is where organizations most often need portable, vendor-neutral tooling. A hybrid is a sensible default: the vendor plane keeps GPUs and models healthy, and a portable layer owns the agent audit trail.

## 6. The Strategic tier's data clone

The Strategic pod includes a dedicated enclave file service that the GPU node and analytics engines mount over NFSv4.

| Element | Planning default |
|---|---|
| Capacity | ~100 TB usable to start; grow in 50–100 TB increments. Narrow domains may need 50 TB; multi-line-of-business sets 200 TB+. Quote usable capacity after RAID and snapshot reserve. |
| Layout | Separate paths for gold data, models and scratch (for example `/enclave/clone`, `/enclave/models`, `/enclave/work`). |
| Ingest | One-way diode, then staging, then promotion into the gold path on a daily or shift cadence. No continuous writes from outside. |
| Protection | Snapshots and immutable retention on the gold path. |
| Egress | Dual-control export of finished intelligence products only, with full chain of custody. |

Local NVMe on the GPU node holds model weights cache, KV scratch and hot working sets. It is not a substitute for the clone.

## 7. Facilities and security prerequisites

| | Tier 1 (32 GPU) | Tier 2 (16 GPU) | Tier 3 (8 GPU) |
|---|---|---|---|
| **IT power** | ~57 kW for four nodes; plan ~70 kW rack | ~29–32 kW GPU + 12–18 kW data rack | ~12–18 kW GPU + 2–5 kW storage |
| **Cooling** | Air | Air (optional direct liquid) | Direct liquid + small coolant distribution unit; storage air-cooled |
| **Network** | Production-adjacent | Segmented (VRF) | Air-gap with data diode |
| **Storage** | Local NVMe | Shared NAS corpora | ~100 TB NFS enclave clone |
| **Operating model** | Platform SRE + AI ops | Domain + platform teams | Enclave with dual control |

Power figures are planning-grade, drawn from public vendor facility guidance (an 8-GPU B200-class node draws up to about 14.3 kW). Server power-supply ratings are not IT load. Always validate with the vendor's power tools and a site assessment before treating Tier 2 dense or any Tier 3 build as standard.

## 8. Reference hardware (vendor-neutral)

All major server vendors publish 8-GPU systems that fit this design, for example Dell PowerEdge XE9780, HPE ProLiant Compute XD685 and Cisco UCS C880A, alongside NVIDIA DGX. Networking can be 400/800 GbE AI Ethernet (Cisco Nexus, NVIDIA Spectrum-X, Dell Z-series) with optional InfiniBand. Enclave storage can be any enterprise scale-out NAS (Dell PowerScale, HPE Alletra and equivalents). Exact part numbers, power and cooling change often; get validated bills of materials from vendors per site.

## 9. How to adopt

| Stage | Focus | Exit criterion |
|---|---|---|
| 1. Assess | Data gravity, agent use cases, isolation needs, power and cooling reality | Tier recommendation |
| 2. Pilot | Tier 1 pod with query, agent runtime and control plane | Measured change in time to detect and repair data incidents |
| 3. Measure | Load-test sessions per replica; tune precision and batch | Pinned sizing per model release |
| 4. Expand | Domain packs; Tier 2 where retrieval depth justifies it | Automated domain throughput under audit |
| 5. Isolate | Tier 3 runbooks, diode procedures, clone quality SLAs | Audited intelligence products |

### KPIs

| KPI | Target direction | Why |
|---|---|---|
| Time to detect and repair data incidents | Down | Direct operational value |
| Share of incidents auto-triaged | Up | Agent leverage |
| Unapproved write actions | Zero | Safety |
| GPU utilization | Sustained high | Avoids stranded capital |

### Explicit non-goals

- Cheap tokens for every employee.
- Replacing subscription frontier tools for human builders.
- Training base models from scratch.
- Air-gapping everything (only Tier 3 leads with isolation).

## 10. Conclusion

The winning enterprise pattern is not moving the whole company onto private GPUs. It is a deliberate dual system: subscription frontier models that accelerate people, plus a few on-premises pods that institutionalize continuous operations and, where warranted, air-gapped reasoning over a controlled data clone. Size by concurrency, build from standard 8-GPU nodes, do the memory arithmetic honestly, and never ship an agent fleet without orchestration and audit.

## Glossary

| Term | Meaning |
|---|---|
| **MoE** | Mixture of experts: a large model where only a few expert sub-networks run per token. Cuts compute, not weight memory. |
| **HBM** | High-bandwidth memory on the GPU package; holds weights and KV cache. |
| **KV cache** | Attention state kept during generation; grows with context and concurrency. |
| **FP8 / FP4** | 8-bit and 4-bit number formats used to shrink models for inference. |
| **NVL72** | A 72-GPU liquid-cooled rack-scale system; optional scale-out, not the default. |
| **Data diode** | A one-way transfer device that lets data into an enclave but not out. |
| **SLO** | Service level objective. |
| **VRF** | Virtual routing and forwarding; network segmentation. |
