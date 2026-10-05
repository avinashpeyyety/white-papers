---
title: "The IDE Is Not an Ops Computer: A Five-Level Maturity Ladder and Its Token Economics"
slug: agentic-maturity-ladder-token-economics
date: 2026-10-05
author: Avinash Peyyety
version: "0.1 (draft)"
contributors: SpaceXAI
series: agentic
series_order: 1
excerpt: >-
  An IDE assistant helps a person at a keyboard; it is not a computer that can run
  agents overnight. This paper defines a five-level maturity ladder for agentic data
  engineering, from IDE assistant to on-premises intelligence pods, and compares the
  levels on runtime, state, orchestration, observability and relative token cost.
tags:
  - agentic engineering
  - maturity model
  - token economics
  - agent harnesses
---

> **Status: AI-assisted draft for human review.** Figures marked *(est.)* are author estimates, not measurements. Items marked **[needs source]** still need a citation before this paper is final.

## Abstract

**Thesis: the IDE is not an ops computer.** An agent is a running system with a loop, tools, state and memory. It decides and acts. Most enterprises today have prompts and skills that run inside an IDE assistant, which is a tool for a person at a keyboard. Real agents need a *harness* that plans, calls tools, keeps state and carries on across steps, and they need a computer that stays on when the engineer goes home. This paper sets out a five-level maturity ladder for agentic data engineering. It compares each level on who hosts the runtime, where state lives, what orchestrates runs, what can be observed, and what tokens cost relative to Level 1. The practical rule is simple: find your level, and move up one rung only when measured review quality clears the bar.

## 1. Problem and why now

Enterprise data teams have adopted IDE coding assistants quickly, and controlled studies report real speed-ups on bounded tasks [1], [2]. The next goal is different: agents that triage a failed batch at 2 a.m., profile a new source overnight, or carry a multi-day migration forward with little supervision. Those jobs are *operational*. They need a process that persists, a scheduler that starts it, a trace that shows what it did, and a way to stop it.

An IDE session offers none of these by design. When the laptop lid closes, the chat buffer and the work in progress go with it. The next morning the agent has to rebuild its context from scratch, and the enterprise pays for those tokens again. As agent work grows, that re-setup cost becomes a large share of total spend.

Three things make this urgent now:

- **Harnesses have matured.** Command-line and headless agent runtimes from several vendors, plus open model-agnostic options, can now run unattended loops [3]–[5].
- **Managed cloud agents exist.** Vendors now host stateful, headless agents that run on their own computers [6], [7], which shows the pattern works, and raises questions about data control.
- **Token spend is becoming visible.** Once agents run continuously, cost per completed task matters more than cost per seat.

## 2. Background: what makes something an agent

A useful working definition comes from agent-design guidance: an agent is a system in which a model directs its own process and tool use, in a loop, to reach a goal [8]. The reason-and-act pattern [9] is the canonical loop: think, call a tool, read the result, repeat. Everything around that loop (what goes into context, which tools are exposed, how edits are applied, how long runs are compacted and checkpointed, when to stop and ask a human) is the **harness**.

Levels of automation are an established way to stage this kind of change. Human-factors research separates *what* is automated from *how much* human involvement remains [10], and the driving-automation taxonomy shows how a level scale helps buyers and regulators talk about the same thing [11]. The ladder below borrows that idea for agentic engineering.

## 3. Proposed approach: a five-level ladder

| Level | Name | Example tools | Who hosts the runtime | Horizon |
|---|---|---|---|---|
| 1 | IDE assistant | IDE assistant in the editor | You, in a snapshot VM or IDE session | Now |
| 2 | Persistent-VM IDE/CLI agent | IDE agent plus terminal; vendor CLI agents | You, on a persistent VM or desktop | Now |
| 3 | Headless harness on a persistent VM | Claude Code, Codex, OpenCode in headless mode | Your platform team | Next |
| 4 | Managed cloud agent | Devin, Codex cloud, other managed AI computers | The vendor | Next (vendor option) |
| 5 | Intelligence pods (on-premises) | Model Ops control plane hosting Level 3–4 harnesses | You, on-premises | Later |

Levels 1 and 2 make up **Phase 1**. They use what the enterprise already owns. Level 3 is the first level that needs new platform work. Level 4 is a buy option rather than a build. Level 5 is the subject of a separate paper on intelligence pods.

## 4. How it works: five dimensions per level

The ladder compares levels on five dimensions. Each one answers an operational question.

| Dimension | L1 IDE assistant | L2 Persistent-VM agent | L3 Headless harness | L4 Managed cloud agent | L5 Intelligence pods |
|---|---|---|---|---|---|
| **Computer** | Snapshot VM (IDE session) | Persistent VM or desktop | Persistent VM built by platform team | Vendor-hosted | On-premises, enterprise-hosted |
| **State** | Chat buffer; ends with the session | Files and logins on disk; no agent graph | Harness session and checkpoints on disk | Vendor memory and checkpoints | Agent registry, run state, policy |
| **Orchestration** | A person opens the IDE; no scheduler | Human at the keyboard; close it and jobs go dark | Enterprise scheduler, CI runners or Kubernetes jobs | Vendor scheduler; runs overnight | Schedule, policy, kill-switch |
| **Observe** | Assistant usage metrics only; no traces or kill-switch | None; disk is not an ops console | Existing observability and ITSM, plus audit; pull requests as handoff | Vendor traces; data and models leave the estate | Audit of agents, code, tokens and actions |
| **Tokens** | Worst: context re-paid every morning | Better orientation; still no night shift | 3–5× fewer than IDE *(est.)*; API and driver access cheapest | State compounds; almost no re-setup | Owned 24×7 tokens; rent the frontier |
| **Relative token cost** *(est., L1 = 100)* | ≈100 | ≈70 | ≈25 | ≈20 | ≈15 |

### 4.1 Why token cost falls as you climb

The drop in relative cost comes from three mechanisms, none of which needs a new model:

1. **Persistent state.** A harness that checkpoints its session doesn't have to re-read the repository, the runbook and yesterday's notes every morning.
2. **Machine-oriented access.** A headless harness can reach systems through APIs and database drivers that return small, structured results, instead of a chat loop shaped around a person. A companion paper rates these access paths per platform.
3. **Caching and routing.** A runtime that the enterprise controls can apply prompt caching [12] and route small steps to smaller models.

Very long contexts are not a substitute for managed state. Models use information in the middle of long inputs less reliably [13], so deliberate summarizing and checkpointing still matter.

### 4.2 Who builds the runtime

- **Levels 1–2:** the vendor's IDE or CLI, running on enterprise machines.
- **Level 3:** the enterprise platform team.
- **Level 4:** the vendor.
- **Level 5:** the enterprise, on-premises.

**What stays durable at every level** is the knowledge fabric (lineage, glossary, contracts, application graphs) and the skills library. **What is swappable** is the harness. Investment in fabric and skills carries forward, whichever harness wins.

## 5. Evidence and examples

The ladder is a framework, not a benchmark. The evidence it rests on is of three kinds:

- **Productivity at Level 1 is real but bounded.** Controlled experiments show faster task completion with an IDE assistant on scoped tasks [1], and field evidence shows gains for less-experienced workers [2]. Neither study measures unattended operational work.
- **Unattended agents exist.** Managed cloud agents [6], [7] and headless harnesses [3]–[5] are publicly available and run stateful, multi-step work without a person at the keyboard.
- **Task length is growing.** Measurements of how long a task frontier models can complete show that horizon has been lengthening steadily [14], which makes the cost of losing state at session end larger each year.

The relative token-cost figures (100 / 70 / 25 / 20 / 15) and the "3–5× fewer tokens at Level 3" estimate are the author's planning assumptions from hands-on use. **[needs source]**: they should be replaced by measured cost per completed task from a pilot.

## 6. Limitations

- **Estimates, not measurements.** Token-cost ratios are planning estimates and will vary by task mix, model and caching behavior.
- **Levels are not strictly ordered for every team.** Level 4 may suit some teams before Level 3; it trades data control for speed to value.
- **Data control.** At Level 4, data and models leave the estate. Many regulated workloads can't accept that without contractual and technical controls.
- **Vendor churn.** Product names in the table will change quickly. The dimensions are meant to outlast them.
- **Review quality is the gating metric, and it is hard to measure.** Trust in automation should match measured reliability [15]; teams need a review-quality metric before they climb.

## 7. Recommendations

1. **Find your level** honestly, using the five dimensions rather than tool names.
2. **Get full value from Levels 1–2 first.** Build skills, meter every call, and use the cheapest access path per platform.
3. **Treat Level 3 as a platform project,** not a tool rollout: persistent VM, scheduler integration, audit and a kill-switch.
4. **Climb one rung at a time,** only when measured review quality clears an agreed bar.
5. **Invest in what is durable** (knowledge fabric and skills) and keep harnesses swappable.
6. **Replace the estimates** in this paper with measured cost per completed task within one pilot cycle.

## Glossary

| Term | Meaning |
|---|---|
| **Harness** | The agent runtime around the model: loop, tools, state, checkpoints, token control. |
| **Orchestration** | What starts, schedules and stops agent runs. |
| **Headless** | Runs without an interactive editor or user interface. |
| **Snapshot VM** | A virtual machine reset at the end of each session. |
| **ITSM** | IT service management (incident and change tooling). |

## References

[1] S. Peng, E. Kalliamvakou, P. Cihon, and M. Demirer, "The impact of AI on developer productivity: Evidence from GitHub Copilot," arXiv:2302.06590, 2023.

[2] E. Brynjolfsson, D. Li, and L. R. Raymond, "Generative AI at work," National Bureau of Economic Research, Working Paper 31161, 2023.

[3] Anthropic, "Claude Code overview," Claude documentation. [Online]. Available: https://docs.anthropic.com/en/docs/claude-code/overview

[4] OpenAI, "Introducing Codex," May 2025. [Online]. Available: https://openai.com/index/introducing-codex/

[5] OpenCode, "OpenCode: the AI coding agent built for the terminal." [Online]. Available: https://opencode.ai

[6] Cognition, "Introducing Devin, the first AI software engineer," Mar. 2024. [Online]. Available: https://www.cognition.ai/blog/introducing-devin

[7] OpenAI, "Codex," product documentation. [Online]. Available: https://openai.com/codex/ **[needs source: confirm current cloud-agent documentation URL]**

[8] E. Schluntz and B. Zhang, "Building effective agents," Anthropic, Dec. 2024. [Online]. Available: https://www.anthropic.com/research/building-effective-agents

[9] S. Yao *et al.*, "ReAct: Synergizing reasoning and acting in language models," in *Proc. Int. Conf. Learning Representations (ICLR)*, 2023.

[10] R. Parasuraman, T. B. Sheridan, and C. D. Wickens, "A model for types and levels of human interaction with automation," *IEEE Trans. Syst., Man, Cybern. A*, vol. 30, no. 3, pp. 286–297, 2000.

[11] SAE International, "Taxonomy and definitions for terms related to driving automation systems for on-road motor vehicles," SAE J3016, 2021.

[12] Anthropic, "Prompt caching," Claude API documentation. [Online]. Available: https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching

[13] N. F. Liu *et al.*, "Lost in the middle: How language models use long contexts," *Trans. Assoc. Comput. Linguistics*, vol. 12, pp. 157–173, 2024.

[14] T. Kwa *et al.*, "Measuring AI ability to complete long tasks," arXiv:2503.14499, 2025.

[15] J. D. Lee and K. A. See, "Trust in automation: Designing for appropriate reliance," *Human Factors*, vol. 46, no. 1, pp. 50–80, 2004.
