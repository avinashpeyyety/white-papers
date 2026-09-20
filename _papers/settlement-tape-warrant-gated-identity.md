---
title: "Public Tape, Not System of Record"
slug: settlement-tape-warrant-gated-identity
date: 2026-09-18
updated: 2026-09-20
author: Avinash Peyyety
version: "0.3"
contributors: Grok Build
excerpt: >-
  A shared append-only tape can be the public proof layer for claims and
  settlement. It cannot be the system of record for institutional and personal
  records. This paper holds that split: tape, proof layer, and public index on
  one side; warrant-gated identity, freeze rail, and local SoR on the other.
tags:
  - settlement
  - proof layer
  - public tape
  - systems of record
  - warrant-gated identity
  - freeze rail
---

## Abstract

**Thesis.** A shared, append-only **tape** can be the **public proof layer** for claims and settlement. It cannot be the **system of record (SoR)** for all institutional and personal records. Design must hold that split. Do not collapse “ledger,” “spine,” and “SoR” into one object.

**Contribution.** The paper defends a regulated **proof layer** over existing books: banks, treasuries, registries, and clouds remain SoR; platforms are **index and UI**. It specifies **warrant-gated identity** (identity-in-escrow, not anonymity) with predicate, window, split keys, **freeze rail**, and an audit of every reveal. It lists what may be public on the tape and what must stay private off it.

**Vocabulary.** Prefer *tape*, *proof layer*, *public index*, *warrant-gated identity*, *freeze rail*. Do not say “the blockchain is the SoR” or “spine of all records everywhere” except as positions this paper dismantles.

*Working draft (v0.3). Creative architecture, not a finished protocol or statute.*

---

## 1. Why the split matters

Closed books starve public contestability. Open “name-stripped” firehoses re-identify people and still do not answer who a court can compel, who holds title, or who may freeze stolen value.

A public **tape** helps with the first problem: ordered claims—who said, paid, or signed what at time *T*—and resistance to silent rewrite of the **public copy**. A **SoR** still has to answer the second: jurisdiction, correction, freeze, judgment, and custody of the underlying books.

If we treat the tape as the spine of every deed, diagnosis, payroll file, and warehouse inventory, we get a cartel with a Merkle tree—and states will fork, sanction, or refuse the node set. Physics and law stay local. Hashes may point at those books. The books do not move onto a social or payment feed.

---

## 2. Argument spine

1. **What a tape solves.** Ordered public claims and resistance to silent rewrite of the public copy.
2. **What a SoR must still answer.** Who a court can compel; who holds title; who may correct error; who may freeze stolen value; under which statute and jurisdiction.
3. **Identity stripped from a public expense firehose is not anonymity.** Amount, time, merchant, and graph re-identify. Call it **pseudonymous** at best.
4. **Warrant-gated unseal is identity-in-escrow, not anonymity.** Specify predicate, window, split keys, freeze rail, and an audit of every reveal. “Mismanagement” is not a predicate.
5. **Edit/suspend limits do not remove the editor.** The party that can freeze, seal a minor, or honor a judgment is the real SoR. The chain is the **log**; compensating entries plus legal overlay beat silent mutation.
6. **A few global tapes (platform or consortium) are a cartel with a Merkle tree.** States will fork, sanction, or refuse the node set. There is no single spine for “records everywhere.”
7. **Physics and law stay local.** Deeds, diagnoses, payroll, DNS/BGP, warehouses. Hashes may point at those books. The books do not move onto the social or payment tape.

---

## 3. Claims this paper defends

| Public (on the tape / proof layer) | Private (off the tape; held by SoR) |
|------------------------------------|-------------------------------------|
| Hash, amount, asset class, timestamp | Legal identity |
| Proof that stated rules passed | Invoice line, SKU, medical detail |
| Aggregates and entity-level patterns (with anti-reID) | Title, routing, institutional secrets |

- Aggregates can be public; itemized lives and institutional secrets cannot.
- **Viable end-state:** regulated proof layer over existing books—banks, treasuries, registries, clouds remain SoR; platforms are **index and UI**.

---

## 4. Claims this paper rejects

- A world API of every purchase with names peeled off.
- X (or any two or three platforms) as the internet’s operating ledger, including institutional resources.
- Court-only unlock with **no freeze path** and **no enumerated predicate**.
- Equating a high-volume conversation or payment feed with title, tax, or clinical record-keeping.
- “The blockchain is the SoR” / “spine of all records everywhere.”

---

## 5. Layered design (tape ≠ SoR)

| Layer | Role | Who sees it |
|-------|------|-------------|
| **Tape (proof layer)** | Append-only ordered claims; public copy hard to rewrite silently | Anyone (machine-readable) |
| **Public index** | Entity/pattern surfaces for scrutiny of institutions and markets | Anyone |
| **Sealed identity map** | Binding from commitments to legal persons | Escrow; warrant-gated |
| **Local SoR books** | Title, clinical, payroll, tax, warehouse truth | Controllers under statute |

The **tape** is not the SoR. The **SoR** is whoever can correct, freeze, and answer a court. The tape is the log the public can check.

Covered public actors (governments, listed companies, large NGOs, utilities, vendors under disclosure rules) may settle under **stable public entity IDs**. Natural persons use commitments linked to the sealed map. Small private entities follow threshold rules: above a public-interest bar, entity disclosure; below it, stronger aggregation.

---

## 6. Making the public copy useful without a face dump

### 6.1 Pattern objects, not person graphs

Publish analysis-friendly objects: entity–entity edges (aggregated), time-bucketed flow matrices, anonymized cluster features, program/budget tags for public money, fee and latency telemetry. Person-level multi-hop graphs stay off the public index unless lawfully opened.

### 6.2 Anti-reidentification by construction

Stripping names is not anonymity. Harden the public index with *k*-thresholds, time coarsening where needed, amount banding or range proofs for person-adjacent flows, no durable personal merchant-sequence APIs, auxiliary-data rules against correlating the index with cameras/loyalty scrapes to name persons, and optional differential privacy for statistical releases.

### 6.3 Verifiable claims over sealed stores

Anyone can query the tape and public index. Where claims need sealed inputs, prefer public verifiable queries (ZK/MPC) that return a yes/score without opening the identity map to the analyst. Challenge protocols let entities publish attestations the public can demand proofs against.

### 6.4 Free and fair access to the proof layer

The tape and public index should be free to read and bulk-download. Funding is infrastructure (settlement fees, entity disclosure, public budget)—not per-query rents that price out journalists. Same tape for everyone; no premium shadow history. Safe harbor for publishing index-level findings; naming persons without unlock is not protected.

---

## 7. Warrant-gated identity and freeze rail

Warrant-gated unseal is **identity-in-escrow**, not anonymity. Someone holds the sealed map (threshold custodians). That is honest. The public never needed that map to scrutinize institutions on the proof layer.

| Requirement | Design bar |
|-------------|------------|
| **Enumerated predicate** | Published triggers (fraud, tax, civil dispute, public corruption, national security)—not “mismanagement” as a vibe |
| **Pattern-first petition** | Court sees reproducible index evidence before identity opens |
| **Freeze rail** | Fast spend clamp / hold with short TTL and public *existence* log (not the sealed payload)—courts alone are too slow for ongoing theft |
| **Split keys** | Threshold unseal; no single bank, agency, or foundation alone |
| **Narrow window** | This purpose, these commitments, this time range—not lifetime graph dump |
| **Audit of every reveal** | Logged, appealable; excess scope challengeable; no dump-to-world-API |
| **Foreign rails** | MLA and protocol holds; clear failure modes when a rail refuses |

Standing rules may allow public-interest petitioners (with bond/sanctions for abuse). Unseal for court and parties is not republication of faces onto the public index.

---

## 8. Compensating entries, not silent mutation

Edit/suspend limits do not remove the editor. Whoever can freeze, seal a minor, or honor a judgment is operating as SoR. Prefer **compensating entries** on the tape plus legal overlay over silent mutation of history. The public copy becomes harder to lie about; law plus local books remain the spine of title and obligation.

---

## 9. No single global spine

A handful of platform or consortium tapes is still a cartel with a Merkle tree. States will fork, sanction, or refuse the node set. There is no single spine for “records everywhere.” Interoperability of **proofs and hashes** is enough; migration of every local SoR onto one feed is neither feasible nor desirable.

---

## 10. Record types that must stay off the tape

Point *at* these with hashes if needed; do not move the books onto the social or payment tape:

- Real property deeds and land title
- Clinical and diagnostic records
- Payroll and tax filings as itemized person/org detail
- DNS/BGP and critical routing control planes as authoritative SoR
- Warehouse and inventory systems of record
- Itemized invoice lines, SKUs, and trade secrets

Public: hash, amount, asset, timestamp, proof that stated rules passed, and carefully designed aggregates. Private: legal identity, invoice line, SKU, medical, title, routing.

---

## 11. Free, fair, practical phase-in

- Start with public money and large regulated entities; expand by actor class.
- Merchant/PSP adapters map today’s rails into tape commitments without rewriting all retail UX at once.
- Reuse LEI / tax / procurement IDs; do not invent a parallel naming universe.
- Near-real-time for large entity settlements; batched privacy-preserving publish for person-adjacent flows.
- Analysts citing tape height and reproducible queries get strong safe harbor; naming persons without unlock does not.

---

## 12. Standards relative to today

| Today | Proposed bar |
|-------|----------------|
| Opaque private books | Public proof layer for covered activity |
| FOIA / annual reports (late, filtered) | Continuous machine-readable public index |
| Bank/AML black boxes | Public pattern detection + verifiable challenges |
| Name-stripped leaks that still re-ID | Anti-reID by construction + legal teeth |
| Unilateral custodian disclosure | Threshold, logged, narrow warrant-gated unseal + freeze rail |
| Scrutiny only by the powerful | Equal free access to the proof layer |
| Trust us | Reproducible queries at a tape height |
| “Chain = SoR for everything” | Tape = log; local SoR + law = spine |

---

## 13. Worked sketch (illustrative)

1. City *C* and Vendor *V* (public entities) settle on the tape under public IDs; each city’s and vendor’s own books remain SoR for appropriation and title.
2. A journalist’s model flags fee cascades *C* → shell → *V* on the public index; no natural persons named; query cites tape height.
3. Petition seeks warrant-gated unseal for the shell’s beneficial owners in window *W* only; freeze rail already held spend.
4. Court grants narrow unlock. The public still does not get a dump of every citizen who paid a parking ticket—and land title never lived on the tape.

---

## 14. Failure modes

- Re-ID arms race on the public index — fund red teams; treat as first-class.
- Entity theater (shells without sealed beneficial-owner maps for court).
- Freeze abuse — automatic expiry and damages for bad faith.
- Capture of key-holders — diversify trustees.
- Analysis monopoly — forbid exclusive tape deals.
- Jurisdiction shopping — disclosure of opaque foreign hops for covered entities.
- Platform-as-SoR capture — reject X (or any 2–3 platforms) as the operating ledger for institutional resources.

---

## 15. Bottom line

The **public copy** can be made harder to lie about. **Law plus local systems of record** remain the spine. This paper’s contribution is the **split**, the **unseal and freeze design**, and the **list of record types that must stay off the tape**.

Not a world API of every private purchase. A regulated **proof layer** and **public index** over existing books—with **warrant-gated identity** when due process requires faces.

---

## 16. Open questions (next drafts)

1. Public-interest thresholds for mandatory entity IDs.
2. Exact *k* / DP parameters by sector.
3. Standing rules for citizen petitions without spam.
4. Governance of open detector packs.
5. Cross-border unseal and freeze treaties vs protocol holds.
6. How self-custody wallets participate in sealed maps without recreating exchange chokepoints.
7. Annual re-ID audits and freeze/unseal statistics as the measure of “higher standard.”
8. Interoperability of multiple tapes without converging to a single cartel node set.

---

## Changelog

| Version | Date | Notes |
|---------|------|-------|
| 0.1 | 2026-09-18 | Initial outline: settlement tape vs anonymous SoR; warrant-gated identity. |
| 0.2 | 2026-09-18 | Unlimited public scrutiny of entities/patterns without faces; free/fair/practical; court unlock. |
| 0.3 | 2026-09-20 | Thought inject: tape ≠ SoR; proof layer / public index vocabulary; defend–reject claims; freeze rail + enumerated predicate; no platform-as-operating-ledger; record types that must stay off the tape; title reframed to *Public Tape, Not System of Record*. |
