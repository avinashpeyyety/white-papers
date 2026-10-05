---
title: "The Agent Stack, Read Bottom-Up: Eight Layers from Systems of Record to the Human Gate"
slug: agent-stack-cross-section
date: 2026-10-05
author: Avinash Peyyety
version: "0.1 (draft)"
contributors: SpaceXAI
series: agentic
series_order: 5
excerpt: >-
  A cross-section of the agent stack for the data development lifecycle, in eight
  layers from the enterprise estate up to the persona and human gate. The knowledge
  fabric is load-bearing, skills are the reusable unit of work, and the harness is
  graded, not assumed. Includes a four-step starting plan.
tags:
  - agentic engineering
  - reference architecture
  - knowledge graphs
  - governance
---

> **Status: AI-assisted draft for human review.** Harness grades are author assessments *(est.)*, not benchmarks. Items marked **[needs source]** still need a citation before this paper is final.

## Abstract

**Thesis: read the agent stack bottom-up. The estate carries the authority to act, the knowledge fabric is load-bearing, and each layer above adds reach until a human gate approves outcomes.** Discussions of enterprise agents usually start at the top, with the model or the agent product. This paper starts at the bottom. It presents an eight-layer cross-section (Layers A–H) of the agent stack for the data development lifecycle (DDLC): the estate of systems of record, a knowledge fabric, tools and connectors, a skills catalog, the model core, the harness, governance and observability, and finally the persona and human gate. It grades four classes of harness against four needs (night runs, state, estate reach and governance), and closes with a four-step plan to start.

## 1. Problem and why now

Agent pilots often fail in predictable ways. The agent forgets what it learned yesterday, because nothing outside the chat holds application knowledge. It builds a new bespoke workflow per use case, so nothing is reusable. It reaches enterprise systems through improvised paths that security can't approve. And when something goes wrong, nobody can say which agent did what, with which tokens.

Each of these is a missing *layer*, not a weak model. As enterprises move from IDE assistants toward headless and managed agents, they need a shared picture of which layers they own, which they buy, and which must exist before autonomy is safe. Governance frameworks increasingly expect that picture: who acts, under what identity, with what oversight [1], [2].

## 2. Background

The **data development lifecycle** covers the work of building and running data products: specifying, building transformations, testing, deploying, operating, and maintaining quality. Agents can help at every step, but only if they can see the application's context (lineage, contracts, job topology, business glossary) and act through the platforms that own the data.

Two ideas from agent practice shape the stack. First, the loop around the model, the **harness**, strongly affects outcomes [3]. Second, tools should be exposed through a common protocol so they can be reused across harnesses; the Model Context Protocol (MCP) is the main open option [4].

## 3. Proposed approach: eight layers

| Layer | Name | Contents | Role |
|---|---|---|---|
| **H** | Persona and human gate | Data engineer, ops / L2 support, DQ analyst, tester / QA, subject-matter-expert approver | Approve outcomes, not every step |
| **G** | Governance and observability | Agent identity, policy engine, audit trail, token meter, kill-switch | Trust, identity, cost |
| **F** | Harness / agent runtime ★★ | Agent loop, checkpoint and state, prompt cache, human-in-the-loop gates, scheduler | The agent's operating system: the tightest wrap wins |
| **E** | LLM core ★★ | Enterprise LLM, model router, tool protocol, context window, fallback model | Reasoning engine |
| **D** | Skills catalog ★★ | Build pack, ops pack, DQ pack, skill registry, skill evaluations | How work gets done, reusably |
| **C** | Tools and connectors | MCP gateway, shell / CLI, computer use, Git operations, metrics API | Pull context in, execute out |
| **B** | Knowledge fabric ★★★ | Enterprise context catalog, application graphs, lineage and glossary, data contracts, transformation logic | Load-bearing: no fabric means agent amnesia |
| **A** | Estate: systems of record | Data warehouse, transformation framework, batch scheduler, IT service management, data catalog | The authority to act stays here |

★★★ = load-bearing; ★★ = critical path.

### 3.1 Four design principles

1. **The fabric is load-bearing.** Without Layer B, every session starts from zero. Agents re-derive lineage and contracts, make inconsistent choices, and burn tokens doing it.
2. **Skills over agents.** One skill per recurring activity (Layer D), not one agent per use case. A small number of agents (roughly one per persona or platform) draw on a large skills catalog.
3. **Tools pull in and execute out through the estate.** Layer C never goes *around* Layer A. Changes land through the platforms that own the data, with their own controls.
4. **Approved outcomes write back.** When a human at Layer H approves an outcome, what was learned is written back to the fabric (Layer B), so the stack gets smarter with use.

## 4. How it works: grading the harness

Layer F is where products differ most. Four harness classes, aligned with the maturity ladder, are graded A (strong), B (partial) or C (weak) on four needs:

| Harness class | Night run | State | Estate reach | Govern | Phase |
|---|---|---|---|---|---|
| **Level 1** IDE assistant | C | B | B | A | Phase 1 |
| **Level 2** Persistent-VM IDE/CLI agent | C | B | B | B | Phase 1 |
| **Level 3** Headless harness (e.g., Claude Code, Codex, OpenCode) | A | A | B | B | Phase 2 |
| **Level 4** Managed cloud agent (e.g., Devin, Codex cloud) | A | A | C | C | Phase 2–3 |

Grades are an author assessment per harness class *(est.)*, not a benchmark.

**How to read it.** IDE assistants govern well, because they run inside approved tooling, but can't run at night. Vendor CLI agents on a persistent VM improve slightly but are limited for unattended work. Headless harnesses on an enterprise-built persistent VM are the first class that is strong on both night runs and state. Managed cloud agents are strong on runtime but weak on estate reach and governance, because data and models leave the estate. Level 5 on-premises intelligence pods are not a separate harness class: they *host* Level 3–4 harnesses inside the estate.

### 4.1 Where each layer comes from

- **Layers A–B** are enterprise-owned and durable. They outlast any vendor.
- **Layers C–D** are built by the enterprise on open protocols, so they transfer between harnesses.
- **Layers E–F** are the most swappable: models and harnesses change quickly.
- **Layer G** is enterprise-owned and non-negotiable before any unattended run.
- **Layer H** is people. Its job shifts from approving each step to approving outcomes as trust is earned.

## 5. Evidence and examples

- **Context drives agent quality.** Real-world software-engineering benchmarks are hard precisely because each fix requires understanding and coordinating changes across a large codebase and its context, not just writing code [5]. Giving agents that context durably is the case for a load-bearing fabric.
- **Common tool protocols reduce rebuild.** MCP is supported across major assistants and harnesses [4], which is what lets Layers C–D survive a harness swap.
- **Excessive agency is a named risk.** Security guidance for LLM applications lists excessive agency (too much functionality, permission or autonomy) as a top risk [6]. Layer G and the "execute through the estate" principle address it directly.
- **Non-human identity.** Zero-trust architecture treats every subject, including services, as needing its own identity and least-privilege access [2]. Agents should be no exception.
- **Example: schema-change triage.** A data engineer proposes a column change. The DQ and build skills (Layer D) query lineage in the fabric (Layer B) through the catalog connector (Layer C), list affected downstream models, draft updated tests, and open a merge request through Git (Layer A authority). The token meter and audit trail (Layer G) record each call; the reviewer (Layer H) approves the outcome.

## 6. Limitations

- **The layer model is a simplification.** Real products span layers; a managed agent bundles E, F and part of G.
- **Grades will change.** Harness capabilities move fast; re-grade quarterly.
- **The fabric is expensive to seed.** Application graphs, contracts and lineage take real effort; start with one application. **[needs source: effort benchmarks for seeding application knowledge graphs]**
- **Skill counts are estimates.** "Hundreds of skills, about one agent per persona" is a planning estimate.

## 7. Recommendations: start here

1. **Seed the Layer B fabric for one application:** lineage, glossary, contracts and job topology.
2. **Capture three recurring tasks as Layer D skills,** each with an evaluation.
3. **Meter every skill call in Layer G:** identity, tokens and actions.
4. **Grade a Level 3 harness** against the table above on real tasks, and step up only when review quality clears the bar.

## Glossary

| Term | Meaning |
|---|---|
| **DDLC** | Data development lifecycle. |
| **Knowledge fabric** | Durable application context: lineage, glossary, contracts, graphs. |
| **HITL** | Human in the loop. |
| **MCP** | Model Context Protocol. |
| **Kill-switch** | A control that stops agent runs immediately. |

## References

[1] National Institute of Standards and Technology, "Artificial Intelligence Risk Management Framework (AI RMF 1.0)," NIST AI 100-1, Jan. 2023.

[2] S. Rose, O. Borchert, S. Mitchell, and S. Connelly, "Zero trust architecture," National Institute of Standards and Technology, NIST SP 800-207, Aug. 2020.

[3] E. Schluntz and B. Zhang, "Building effective agents," Anthropic, Dec. 2024. [Online]. Available: https://www.anthropic.com/research/building-effective-agents

[4] Anthropic, "Introducing the Model Context Protocol," Nov. 2024. [Online]. Available: https://www.anthropic.com/news/model-context-protocol

[5] C. E. Jimenez *et al.*, "SWE-bench: Can language models resolve real-world GitHub issues?" in *Proc. Int. Conf. Learning Representations (ICLR)*, 2024.

[6] OWASP Foundation, "OWASP Top 10 for LLM Applications 2025: LLM06 Excessive Agency," Nov. 2024. [Online]. Available: https://genai.owasp.org/llm-top-10/
