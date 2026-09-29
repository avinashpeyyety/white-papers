---
title: "From Workbench to Factory: Agent Harnesses and a Persistent Agent Platform"
slug: agent-harnesses-persistent-platform
date: 2026-09-29
author: Avinash Peyyety
version: "0.1"
contributors: Grok Build
excerpt: >-
  IDE-bound coding agents running in non-persistent VMs cap autonomy. This
  paper argues for a portfolio of model-native harnesses (Claude Code, Codex,
  Grok Build) with a model-agnostic fallback (OpenCode), running on persistent
  agent workspaces with shared MCP skills, a token gateway, a governed estate
  broker, and multi-agent orchestration, with the IDE kept as the human cockpit.
tags:
  - agent harnesses
  - agentic engineering
  - persistent workspaces
  - MCP
  - data engineering
---

<div class="box"><b>Our point of view in one paragraph.</b> Phase 1 proved that the data lifecycle can be packaged as skills for Copilot agents inside VS Code. Phase 2 asks agents to work for longer, with less supervision, and across more of the estate. The IDE cannot provide that. Today's setup runs the IDE in snapshot virtual machines that don't persist, and the IDE's agent loop was built around a human at the keyboard. We recommend adding a specific platform layer under the IDE: persistent agent workspaces that run a <i>portfolio</i> of model-native harnesses (Claude Code, OpenAI Codex, xAI Grok Build), with one model-agnostic harness (OpenCode) as the common fallback. These harnesses should share one skills and knowledge layer, one gateway for estate access, and one path for metering tokens and auditing actions. VS Code stays the human cockpit, not the agents' runtime.</div>

<h2>1. Context: where Phase 1 leaves us</h2>
<p>The Phase 1 workbench deliberately used only Copilot in VS Code. Every activity in the data development lifecycle (DDLC) is packaged as a Copilot skill. IDE extensions give agents a local execution loop with few round trips to the estate, while the estate stays the system of record. Work state and memory are saved to cloud drives so context carries across sessions. The core asset is a per-application knowledge graph, served through the EIL layer. That graph gives agents the lineage, contracts, job topology and entity context they need for data quality triage, rule generation, refactors and spec-driven builds.</p>
<p>That design was right for Phase 1: it kept a human in the loop, used the approved tooling, and proved the skill and knowledge model. The next question is which foundational unlocks Phase 2 needs for greater agent autonomy. Harnesses are the first. This paper sets out the problem, the reasoning, and a specific platform to build on.</p>

<h2>2. Problem statement</h2>
<p>Greater autonomy means agents that plan, execute and verify multi-step work over hours, coordinate with other agents, and act on estate platforms such as schedulers, ITSM, the data warehouse, transformation, data observability and the data catalog. Five limits in the current setup block this.</p>
<table><tr><th>Limit</th><th>What happens today</th><th>Why it blocks autonomy</th></tr>
<tr><td><b>1. The runtime isn't persistent</b></td><td>VS Code runs in snapshot VMs. When the session ends, processes, caches, local indexes and in-progress state are gone.</td><td>An agent can't run overnight, resume a long job, keep a warm local index of the codebase, or hold a working environment between tasks. Every session starts cold.</td></tr>
<tr><td><b>2. Round trips to the estate are limited</b></td><td>The IDE's agent loop is designed for a person reviewing each step. Access to estate systems goes through extensions and tool calls that were never meant for long unattended chains.</td><td>Autonomous work needs many cheap act-and-verify cycles against real systems: run a job, read the logs, check data quality results, fix, re-run. The IDE caps how far that loop can go.</td></tr>
<tr><td><b>3. Token efficiency</b></td><td>Tokens are consumed through a human-oriented assistant, with context rebuilt each session and little control over model choice, caching or routing.</td><td>Agent work needs cheap, fast tokens tuned for machines rather than people: prompt caching, routing small steps to smaller models, and compacting context. Without this, long runs cost too much.</td></tr>
<tr><td><b>4. Long-context work</b></td><td>Context is limited by the session and the IDE's context handling. Memory on cloud drives helps but isn't managed by the agent runtime itself.</td><td>Migrations, estate-wide refactors and multi-day investigations need context that is managed deliberately: summarizing, checkpointing and retrieving from the knowledge graph.</td></tr>
<tr><td><b>5. Multi-agent orchestration</b></td><td>There is effectively one agent per IDE session.</td><td>Factory mode needs parallel agents (build, test, data quality, documentation) with handoffs, shared state and a coordinator. The IDE isn't built to host that.</td></tr></table>
<p>VS Code with Copilot is itself a harness: an IDE with agent capabilities attached. The issue isn't that it's a poor tool. It is optimized for a human in the loop, and Phase 2 needs a runtime optimized for the agent.</p>

<h2>3. Why harnesses matter, and why more than one</h2>
<h3>3.1 What a harness is</h3>
<p>A harness is the software loop around a model. It decides what goes into context, which tools the model can call and how, how file edits and shell commands run, how results come back, how long runs are compacted, and when to stop or ask for help. With the same model, a different harness can produce very different results.</p>
<h3>3.2 Harnesses are co-trained with their models</h3>
<p>Frontier labs now post-train their models on their own harness. Supervised fine-tuning and reinforcement learning happen on the same tool schemas, edit formats and agent protocol the harness exposes. The result is a tight fit, so each lab's model performs best in the harness it was trained in:</p>
<ul><li><b>Claude models</b> in <b>Claude Code</b> (Anthropic)</li><li><b>GPT models</b> in <b>Codex</b> (OpenAI)</li><li><b>Grok models</b> in <b>Grok Build</b> (xAI)</li></ul>
<p>So harnesses and models can't be freely mixed without cost. Running a model outside its native harness usually works, but often with lower efficiency and reliability. You typically see more failed tool calls, worse edits and more tokens per completed task. This is our working hypothesis, drawn from vendor positioning and our own lab use. The pilot in Section 6 is designed to measure it on our own tasks.</p>
<h3>3.3 Why one harness isn't enough</h3>
<p>No single model family leads on every task type. Model leadership also changes often, sometimes within weeks. Standardizing on one harness locks us into one lab's model roadmap. Standardizing on the IDE locks us into a human-paced loop.</p>
<h3>3.4 Where a model-agnostic harness fits</h3>
<p>Open, model-agnostic harnesses such as <b>OpenCode</b> can drive most models through one interface. They give us a consistent baseline, easy model switching, a fallback when a vendor harness is unavailable, and a way to run open-weight or internally hosted models. The trade-off is that a general harness is unlikely to match each native harness on its own model. We recommend OpenCode as the <i>baseline and fallback</i>, not the only harness. We'll also track other options that emerge, including Copilot's own agent and command-line modes.</p>

<h2>4. The specific platform: what sits under the harnesses</h2>
<p>Harnesses on their own are only operational tooling. Autonomy comes from the platform they run on. We propose six specific components, each with a clear job:</p>
<table><tr><th>Component</th><th>What it is</th><th>What it unlocks</th></tr>
<tr><td><b>A. Persistent agent workspaces</b></td><td>Long-lived cloud development machines per agent or team (container or VM), with persistent disk, warm dependencies, local code indexes and scheduled runs. These replace the snapshot VMs as the agents' runtime.</td><td>Overnight and multi-day runs; resume after interruption; the agents' "own computer."</td></tr>
<tr><td><b>B. Harness layer</b></td><td>Claude Code, Codex and Grok Build installed in each workspace in headless or command-line mode, with OpenCode as baseline and fallback. Selected per task through a simple routing policy.</td><td>Each model runs in its native harness; no lock-in to one vendor.</td></tr>
<tr><td><b>C. Model and token gateway</b></td><td>One enterprise endpoint for all model traffic, providing authentication, per-team budgets, prompt caching, routing to smaller models for small steps, and cost metering per task.</td><td>Cheap, fast tokens for machines, with cost visibility and control.</td></tr>
<tr><td><b>D. Shared skills and knowledge layer</b></td><td>Our Phase 1 DDLC skills and the per-application knowledge graph (EIL), exposed through the Model Context Protocol (MCP), an open standard every major harness supports.</td><td>Build a skill once and use it in every harness; Phase 1 work carries forward instead of being rebuilt per vendor.</td></tr>
<tr><td><b>E. Estate access broker</b></td><td>A governed gateway to schedulers, ITSM, the data warehouse, transformation, data observability and the data catalog. It uses scoped service identities, read-first defaults, approval gates for writes, and rate limits.</td><td>Many act-and-verify cycles against real systems, safely. The estate stays the system of record.</td></tr>
<tr><td><b>F. Orchestration, memory and audit</b></td><td>A coordinator for multi-agent runs (task queue, handoffs, shared state), durable memory and checkpoints, and full traces of every prompt, tool call and estate action.</td><td>Parallel agents in factory mode; explainability, replay and compliance evidence.</td></tr></table>
<p><b>The IDE's new role.</b> VS Code with Copilot stays the place where engineers review, steer and approve. Engineers connect to the persistent workspaces from the IDE, watch agent runs, and step in at approval gates. The investment in the Phase 1 cockpit is kept. It is moved to supervising agents rather than being their only runtime.</p>

<h2>5. Options we considered</h2>
<table><tr><th>Option</th><th>Strengths</th><th>Weaknesses</th><th>View</th></tr>
<tr><td><b>1. Stay Copilot-only in VS Code</b></td><td>Often already approved; no new vendors; familiar to engineers.</td><td>Doesn't fix the non-persistent runtime, the limits on long unattended loops or token control. Autonomy is capped.</td><td>Keep as the cockpit. Not enough as the runtime.</td></tr>
<tr><td><b>2. Standardize on one model-agnostic harness (OpenCode)</b></td><td>One tool to secure and support; model choice stays open.</td><td>Likely less efficient than each native harness on its own model; depends on community support.</td><td>Adopt as baseline and fallback.</td></tr>
<tr><td><b>3. Standardize on one vendor harness</b></td><td>Best fit for that one model; simpler contract.</td><td>Locked into one lab's roadmap; loses whenever another model leads a task type.</td><td>Not recommended.</td></tr>
<tr><td><b>4. Multi-harness portfolio on a shared platform (recommended)</b></td><td>Each model runs where it performs best; skills and governance are shared; vendors can be swapped as the market moves.</td><td>More harnesses to secure and license; needs the platform components in Section 4.</td><td><b>Recommended</b>, subject to a measured pilot.</td></tr></table>

<h2>6. How we'll prove it: a measured pilot</h2>
<p>We recommend a time-boxed pilot on real DDLC work before any broad rollout.</p>
<ul>
<li><b>Task set:</b> 20 to 30 representative tasks from the Phase 1 backlog, for example data quality triage from an observability alert, a transformation model refactor, a scheduler job failure investigation, spec-to-pipeline build with tests, and lineage-driven impact analysis.</li>
<li><b>Contenders:</b> Copilot in VS Code (current baseline), Claude Code, Codex, Grok Build and OpenCode. Every contender uses the same MCP skills and the same estate sandbox.</li>
<li><b>Metrics per task:</b> completed correctly (yes or no, with human review), tokens and cost per completed task, wall-clock time, estate round trips, human interventions, and unattended run length before the agent needs help.</li>
<li><b>Decision rule:</b> keep a harness in the portfolio only where it clearly beats the baseline on the task types it's routed to. Otherwise, route that work to OpenCode or Copilot.</li>
</ul>
<p>No results exist yet. The pilot produces the evidence; numbers should come only from it.</p>

<h2>7. Risks and how we manage them</h2>
<table><tr><th>Risk</th><th>Mitigation</th></tr>
<tr><td>Agents take harmful actions on the estate</td><td>Read-first access; writes pass approval gates; scoped service identities; sandbox first; full audit trail.</td></tr>
<tr><td>Data leaves the enterprise boundary</td><td>All model traffic goes through the enterprise gateway under approved data-processing terms; no direct consumer endpoints.</td></tr>
<tr><td>Token costs grow out of control</td><td>Per-team budgets, cost metering per task, caching and routing to smaller models; the pilot measures cost per completed task.</td></tr>
<tr><td>Too many tools to support</td><td>Shared skills through MCP and one platform layer, so a harness is a replaceable component; the portfolio is pruned based on pilot results.</td></tr>
<tr><td>Vendor harnesses change quickly</td><td>OpenCode fallback; no skills built for one vendor only; quarterly re-evaluation.</td></tr></table>
