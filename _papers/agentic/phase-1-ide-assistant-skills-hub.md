---
title: "Nothing New to Buy: Phase 1 Agentic Delivery with an IDE Assistant, a Skills Library and an IDE Hub"
slug: phase-1-ide-assistant-skills-hub
date: 2026-10-05
author: Avinash Peyyety
version: "0.1 (draft)"
contributors: SpaceXAI
series: agentic
series_order: 3
excerpt: >-
  Phase 1 of agentic delivery needs no new platform. An IDE assistant, a library of
  reusable skills for build, operations and data quality, and one IDE extension that
  routes and meters every call are enough to deliver measurable gains, and everything
  built now is reused by later autonomy.
tags:
  - agentic engineering
  - skills
  - IDE assistants
  - data engineering
---

> **Status: AI-assisted draft for human review.** KPIs and exit criteria are proposals, not measured results. Items marked **[needs source]** still need a citation before this paper is final.

## Abstract

**Thesis: Phase 1 is an IDE assistant plus skills plus one IDE hub, with nothing new to buy.** Enterprises starting agentic delivery are tempted to buy agent platforms and frameworks before they know which work agents should do. This paper argues for a deliberately narrow Phase 1 (Levels 1–2 of the maturity ladder) with two opportunity areas only: a **skills library** for build, operations and data quality work, and a single **IDE extension that acts as the hub** for every persona. The IDE assistant, the IDE and the skills do the work; enterprise platforms stay the authority. The paper sets five ground rules, six starter use cases with owner personas and proposed KPIs, a five-step persona flow, and 90-day exit criteria that gate the move to a headless-harness pilot.

## 1. Problem and why now

Most enterprises already license an IDE assistant. Usage is uneven: some engineers get large gains, most use it for autocomplete, and leaders can't see where value is created. At the same time, vendors are pitching new agent frameworks, each wanting its own integration, identity and budget.

Buying a platform before capturing the work creates agent sprawl: many bespoke agents, each built for one use case, none reusable, and none measured. The opposite approach is to treat recurring engineering activities as the unit of value, capture each as a reusable *skill*, and measure every use. Skills built this way are what later autonomy will run, so Phase 1 work is not throwaway.

Why now: assistants have gained agent and terminal modes, the Model Context Protocol gives a common way to expose tools [1], and packaged "skills" (instructions, scripts and resources an agent loads when relevant) are becoming a standard pattern across vendors [2].

## 2. Background

A **skill** here is a versioned, reusable package of prompt, context, checks and a worked example, with a pass/fail evaluation. It is closer to a runbook or a code module than to an agent. An **IDE hub** is one IDE extension, built once and published to every persona, that lets users pick a skill, pulls the right context, and routes and meters every call.

The approach draws on two established ideas. First, controlled studies show IDE assistants speed up well-scoped tasks [3], and field studies show the gains are largest where best practice is captured and shared [4]. Skills are a way to capture that best practice. Second, operational knowledge is most reliable when it is written down as reviewed, versioned procedure rather than held by individuals. **[needs source: SRE/runbook practice reference]**

## 3. Proposed approach

### 3.1 Five ground rules

| Rule | What it means |
|---|---|
| **Use what we own** | The IDE assistant plus the existing stack. No new framework without a clear exit and run-cost case. |
| **Skills, not agents** | One reusable skill per recurring activity, never a new agent per use case. |
| **Capture, don't invent** | Every skill starts as real, human-led work done with an agent this sprint. |
| **Estate is authority** | Agents pull context in and execute out through enterprise platforms, never around them. |
| **Meter everything** | Every skill call goes through the IDE hub. If it can't be measured, it isn't scaled. |

### 3.2 Two opportunity areas

1. **Build the skills library (build, operations, data quality).** Hand-hold agents on regular work, then reverse-capture it as scalable skills. After each run, capture the prompt, estate context, tool calls, checks and fixes as a versioned skill with a worked example and a pass/fail evaluation. Peer-review it, publish it to the catalog, and reuse it next sprint.
2. **IDE extensions as the hub.** One extension with launchpads for each persona (data engineer, operations / L2 support, DQ analyst, tester), each with its skill pack and work queue. A context panel sits beside the code with glossary, lineage, ownership and DQ scores. Every call flows through one funnel, so coverage and gains are measured, not estimated.

## 4. How it works

### 4.1 Six Phase 1 use cases

| # | Area | Use case | Owner persona | Proposed KPI |
|---|---|---|---|---|
| 1 | Build | Transformation model and SQL generated from a spec, with tests | Data engineer | Spec-to-merge-request cycle time |
| 2 | Build | Warehouse schema-change impact analysis and merge-request review | Data engineer (reviewer) | Defects caught before merge |
| 3 | Ops | Failed-batch return to service and job SLA health | Ops / L2 support | Time to return to service |
| 4 | Ops | Incident triage with a root-cause-analysis pack; blast radius from lineage and the model dependency graph | Ops / L2 support | Triage time per incident |
| 5 | DQ | Synthesize DQ rule packs from data contracts and business and IT controls | DQ analyst | Share of columns with at least one rule |
| 6 | DQ | Pre-load assertions in the warehouse, timed to scheduler windows, with data observability as the backstop | DQ analyst, tester | Defects caught before load |

### 4.2 The persona flow in the IDE hub

1. **Pick a skill** from the persona launchpad and work queue.
2. **Pull context:** glossary, lineage, DQ scores.
3. **Local loop:** generate, run the transformation locally, validate on a development warehouse, with minimal estate calls.
4. **Checkpoint:** save work state so long tasks resume across sessions; a human reviews.
5. **Commit:** open a merge request; the call is metered.

### 4.3 Four questions before building anything

- Does it reuse the IDE assistant and the IDE?
- Did it come from a recurring activity?
- Can a teammate reuse it next week?
- Will it survive a model or framework change?

## 5. Evidence and examples

- **Gains from IDE assistants on scoped tasks** are documented in a randomized experiment [3] and in field data [4]. Phase 1 targets exactly these bounded tasks.
- **Reuse beats rebuild.** Exposing skills through an open protocol [1] and packaging them in a portable format [2] means the same skill can later run in a headless harness without being rewritten. This is the main reason Phase 1 work isn't throwaway.
- **Data quality tests in the transformation layer** are a standard practice [5]; Phase 1 adds an agent that drafts them from contracts and controls, with a human approving each one.
- **Example: failed batch.** An ops engineer opens the hub, picks the return-to-service skill, and the skill pulls the job status, recent log lines and dependency chain through the scheduler API. It proposes a fix; the engineer approves; the call is metered against the "time to return to service" KPI.

No outcome numbers are claimed. The KPIs above are proposals to be baselined in the first sprint.

## 6. Limitations

- **Human-paced.** Phase 1 runs at the speed of the person at the keyboard. It does not provide overnight or unattended work.
- **Speed is not headcount.** Faster delivery shifts work toward expert review; capacity gains need the later unlocks described in the companion paper on autonomy.
- **Skill quality varies.** Without peer review and evaluations, a skills library becomes a prompt dump.
- **Hub build cost.** One extension is small, but it still needs an owner, a release process and security review.
- **Metering needs care.** Usage counts are easy; time saved per persona needs a baseline measured before rollout.

## 7. Recommendations

1. **Adopt the five ground rules** as Phase 1 policy.
2. **Stand up the IDE hub** with launchpads for four personas and a metered call path.
3. **Capture the six starter skills** from this sprint's real work, each with a worked example and a pass/fail evaluation.
4. **Use 90-day exit criteria (proposed)** to gate a Level 3 headless-harness pilot:
   - at least six skills published, each with a pass/fail evaluation;
   - 100% of skill calls metered through the IDE hub;
   - time saved per persona measured against a baseline;
   - at least one skill reused by a second team;
   - review quality tracked, so the team is ready for a Level 3 pilot.
5. **Say no to new agent platforms** in Phase 1 unless they meet the "use what we own" exit and run-cost test.

## Glossary

| Term | Meaning |
|---|---|
| **Skill** | Versioned, reusable prompt, context and checks with a worked example and an evaluation. |
| **IDE hub** | One IDE extension that routes and meters every skill call. |
| **Return to service** | Restarting or repairing a failed batch. |
| **RCA** | Root-cause analysis. |
| **SLA** | Service-level agreement. |

## References

[1] Anthropic, "Introducing the Model Context Protocol," Nov. 2024. [Online]. Available: https://www.anthropic.com/news/model-context-protocol

[2] Anthropic, "Equipping agents for the real world with Agent Skills," Oct. 2025. [Online]. Available: https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills

[3] S. Peng, E. Kalliamvakou, P. Cihon, and M. Demirer, "The impact of AI on developer productivity: Evidence from GitHub Copilot," arXiv:2302.06590, 2023.

[4] E. Brynjolfsson, D. Li, and L. R. Raymond, "Generative AI at work," National Bureau of Economic Research, Working Paper 31161, 2023.

[5] dbt Labs, "Add data tests to your DAG," dbt Developer Hub. [Online]. Available: https://docs.getdbt.com/docs/build/data-tests
