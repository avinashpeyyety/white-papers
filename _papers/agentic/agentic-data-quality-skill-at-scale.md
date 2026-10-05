---
title: "Agentic Data Quality: From Hand-Built Rules to a Data Quality Skill at Scale"
slug: agentic-data-quality-skill-at-scale
date: 2026-10-05
author: Avinash Peyyety
version: "0.1 (draft)"
contributors: SpaceXAI
series: agentic
series_order: 4
excerpt: >-
  Data quality coverage is limited by how fast people can write rules. This paper
  describes a three-stage path: an agent learns from existing hand-built rules, then
  proposes coverage across every quality dimension with a human approving, and finally
  the proven pattern becomes a reusable skill that expands coverage across domains.
tags:
  - data quality
  - agentic engineering
  - skills
  - human in the loop
---

> **Status: AI-assisted draft for human review.** The coverage curve described here is illustrative, not measured. Items marked **[needs source]** still need a citation before this paper is final.

## Abstract

**Thesis: agentic data quality moves from hand-built rules to a DQ skill at scale.** Most enterprises protect their data with rules written one at a time by engineers and analysts. Coverage is strong on the dimensions that are easy to check (completeness, validity, uniqueness) and thin on the rest (timeliness, consistency, accuracy). This paper proposes a three-stage journey. **Stage 1:** existing rules are ingested as a baseline and the agent learns their patterns, thresholds and naming. **Stage 2:** the agent profiles data, proposes rules across all six dimensions and drafts tests, and a human approves every rule before it goes live. **Stage 3:** the proven pattern is packaged as a reusable skill, so coverage expands across domains with little marginal effort. The recommended first step is to run Stage 2 on one domain, then package the skill.

## 1. Problem and why now

Data quality (DQ) rules are automated checks on data: a key must be unique, a date must fall in range, a feed must land by 6 a.m. Each rule takes someone to understand the data, choose a threshold, write the check and maintain it. As the estate grows, rule writing doesn't keep up, and the gaps fall on the dimensions that are hardest to express.

The cost of gaps is well known: silent errors propagate downstream into reports and models, and data dependencies are a major source of hidden technical debt in machine-learning systems [1]. Research and industry systems have shown that data validation can be automated with profiling and constraint suggestion [2], [3]. What has changed is that language-model agents can now read existing rules, data contracts and business controls, and draft rules in the team's own conventions. The bottleneck shifts from *writing* rules to *approving* them.

## 2. Background: the six dimensions

This paper uses six widely used DQ dimensions [4], [5]:

| Dimension | Meaning |
|---|---|
| **Completeness** | Required values are present. |
| **Validity** | Values have the right format and fall in the allowed domain. |
| **Uniqueness** | No duplicate keys or records. |
| **Timeliness** | Data lands when it is expected. |
| **Consistency** | Data agrees across sources and over time. |
| **Accuracy** | Data matches a trusted reference. |

Other frameworks define related sets of characteristics [6]; the approach here works with any agreed list.

## 3. Proposed approach: a three-stage journey

| Stage | Maturity level | Horizon | What happens |
|---|---|---|---|
| **1. Manual rules ingested** | Level 1 | Now | Existing hand-built rules are loaded as the baseline. The agent learns patterns, thresholds and naming. |
| **2. Agentic DQ, human-led** | Level 2 | Next | The agent profiles data, proposes rules and drafts tests. A human reviews and approves every rule. |
| **3. DQ skill at scale** | Level 3+ | Long-term | The proven pattern is packaged as a reusable skill and expands across domains with little marginal effort. |

### 3.1 Coverage by dimension and stage

| Dimension | Stage 1 | Stage 2 | Stage 3 |
|---|---|---|---|
| Completeness | Hand-built baseline | Agent proposes, human approves | Skill auto-expands |
| Validity | Hand-built baseline | Agent proposes, human approves | Skill auto-expands |
| Uniqueness | Hand-built baseline | Agent proposes, human approves | Skill auto-expands |
| Timeliness | Sparse or gap | Agent drafting | Skill auto-expands |
| Consistency | Sparse or gap | Agent proposes, human approves | Skill auto-expands |
| Accuracy | Sparse or gap | Agent drafting | Skill auto-expands |

The expected shape is that rule coverage rises from low to growing to broad across the stages, while the effort to cover each new domain falls. This curve is **illustrative, not measured**.

## 4. How it works

### Stage 1: learn from what exists

The agent reads the current rule set, the tests in the transformation layer, data contracts and any documented business and IT controls. It builds a model of the team's conventions: how rules are named, which thresholds are used for which column types, which checks run before load and which after. No new rules go live in Stage 1. The output is a coverage map by dimension that shows where the gaps are.

### Stage 2: propose, with a human approving

For one domain, the agent:

1. **Profiles** the data: scans tables to learn distributions, null rates, key cardinality, arrival times and cross-source relationships.
2. **Proposes rules** across all six dimensions, written in the team's conventions and linked to the profile evidence that justifies each threshold.
3. **Drafts tests** in the team's existing testing framework, for example as tests in the transformation layer [7].
4. **Routes each rule to a human** (a DQ analyst or data owner) who approves, edits or rejects it. Nothing goes live without approval.

Rejections and edits are captured as feedback. They are the training signal for the skill.

### Stage 3: package the skill

Once Stage 2 results are stable, the pattern is packaged as a **skill**: reusable agent instructions, tools and evaluations. A new domain then gets profiled, proposed and reviewed with the same skill, and the evaluation set checks that the skill still produces rules that reviewers accept. Packaging follows the emerging standard pattern for agent skills [8], so the same skill can run in an IDE assistant today and a headless harness later.

### Where accuracy and timeliness come from

These two dimensions need inputs that profiling alone can't give. Accuracy needs a trusted reference (a master record, a ledger, an upstream source of truth). Timeliness needs the expected arrival schedule, which usually lives in the batch scheduler. The agent can *draft* these rules from contracts and scheduler metadata, but they need more human judgment, which is why the table shows them as "agent drafting" in Stage 2.

## 5. Evidence and examples

- **Automated constraint suggestion works at scale.** Production systems have profiled large datasets and suggested data-validation constraints automatically for years [2], [3]. The agentic step adds the ability to read unstructured inputs (contracts, control descriptions) and to match a team's conventions.
- **Human approval is the control.** The design keeps a person in the loop for every rule, consistent with risk-management guidance that calls for human oversight of AI-driven decisions with operational impact [9].
- **Example: a new feed.** A customer-events feed lands with no rules. In Stage 2, the agent profiles it, proposes not-null and accepted-value checks on key columns, a uniqueness check on the event identifier, a freshness check timed to the scheduler window, and a reconciliation count against the source. The DQ analyst approves four, tightens one threshold and rejects one; the edits feed back into the skill.

No coverage or effort figures are claimed for any real deployment. **[needs source: pilot measurements of rule acceptance rate and coverage gain]**

## 6. Limitations

- **Review becomes the bottleneck.** If the agent proposes faster than people can approve, the queue grows. Prioritize by data criticality and batch the review.
- **Bad baselines teach bad habits.** Stage 1 learns from existing rules, including their mistakes. Review the baseline, not just the new proposals.
- **Thresholds drift.** Data changes; rules need periodic re-profiling.
- **Accuracy needs references.** Where no trusted reference exists, accuracy coverage stays limited regardless of automation.
- **Access and privacy.** Profiling touches production data; it needs scoped, read-only access and masking where required.

## 7. Recommendations

1. **Run Stage 2 on one domain:** ingest the existing rules, let the agent propose, and have humans approve every rule.
2. **Measure** rule acceptance rate, coverage by dimension and reviewer time per rule.
3. **Package the skill (Stage 3)** once acceptance is stable, with an evaluation set built from approved and rejected proposals.
4. **Reuse it enterprise-wide,** domain by domain, tracking effort per new domain.
5. **Keep the human gate** for any rule that can block a load or page an on-call engineer.

## Glossary

| Term | Meaning |
|---|---|
| **DQ** | Data quality. |
| **Rule** | An automated check on data. |
| **Profiling** | Scanning data to learn its shape. |
| **Skill** | Packaged, reusable agent instructions, tools and evaluations. |
| **Human in the loop** | A person approves before a rule goes live. |

## References

[1] D. Sculley *et al.*, "Hidden technical debt in machine learning systems," in *Advances in Neural Information Processing Systems 28 (NeurIPS)*, 2015.

[2] S. Schelter, D. Lange, P. Schmidt, M. Celikel, F. Biessmann, and A. Grafberger, "Automating large-scale data quality verification," *Proc. VLDB Endowment*, vol. 11, no. 12, pp. 1781–1794, 2018.

[3] E. Breck, N. Polyzotis, S. Roy, S. E. Whang, and M. Zinkevich, "Data validation for machine learning," in *Proc. Conf. Machine Learning and Systems (MLSys/SysML)*, 2019.

[4] DAMA UK Working Group, "The six primary dimensions for data quality assessment," DAMA UK, Oct. 2013.

[5] DAMA International, *DAMA-DMBOK: Data Management Body of Knowledge*, 2nd ed. Basking Ridge, NJ, USA: Technics Publications, 2017.

[6] *Software Engineering — Software Product Quality Requirements and Evaluation (SQuaRE) — Data Quality Model*, ISO/IEC 25012:2008.

[7] dbt Labs, "Add data tests to your DAG," dbt Developer Hub. [Online]. Available: https://docs.getdbt.com/docs/build/data-tests

[8] Anthropic, "Equipping agents for the real world with Agent Skills," Oct. 2025. [Online]. Available: https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills

[9] National Institute of Standards and Technology, "Artificial Intelligence Risk Management Framework (AI RMF 1.0)," NIST AI 100-1, Jan. 2023.
