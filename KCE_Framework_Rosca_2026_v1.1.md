# Knowledge Continuity Engineering: A Framework for Persistent Knowledge Across Human and AI Generations

**Mihai Roșca**
Independent Researcher, BRIDGRAI Ecosystem
Brăila, Romania, EU
ORCID: 0009-0001-1422-6209

**Date:** September 2026
**Status:** Preprint — pending peer review
**License:** CC BY 4.0

**Version:** 1.1 (2026-10-02): UKBE Core license updated to proprietary (sections L5 tables and Competing interests); no other change.
---

## Abstract

As AI systems become co-creators of organizational and scientific knowledge, a critical gap emerges: no existing discipline addresses the systematic preservation of knowledge — its meaning, provenance, intent, and authority — across transitions between AI model generations, human personnel changes, and evolving regulatory environments. This paper introduces Knowledge Continuity Engineering (KCE), a proposed discipline that treats knowledge continuity as a first-class engineering concern rather than an archival afterthought. We identify five core problems that existing disciplines fail to address in combination (model discontinuity, human discontinuity, provenance decay, semantic drift, and authority fragmentation), propose seven design principles and a five-layer reference architecture, and describe BRIDGRAI — a working system developed over 3.5 years that implements KCE principles across post-quantum cryptography, semantic validation, invariant checking, and blockchain-anchored provenance. We provide an extended framework including implementation guidance, case studies, proposed metrics, and an adoption roadmap. We argue that knowledge continuity will become as fundamental an engineering discipline as cybersecurity within the next decade, and outline the open research questions that must be addressed to formalize the field.

**Keywords:** knowledge continuity, AI governance, provenance engineering, semantic persistence, model discontinuity, post-quantum cryptography, human-AI co-creation

---

## 1. Introduction

The useful lifespan of a modern large language model (LLM) is approximately 18-36 months. Within that window, it is trained, deployed, fine-tuned, integrated into workflows, and then deprecated in favor of a successor. The knowledge it helped create — documents, code, analyses, decisions — persists, but the context in which that knowledge was produced does not. The model's biases, capabilities, limitations, and training cutoff are embedded invisibly in its outputs. When the model is replaced, this embedded context vanishes.

This is one instance of a broader problem: knowledge produced through human-AI collaboration lacks the engineering infrastructure to survive transitions. When the AI model changes, when the human operator leaves, when the regulatory environment shifts, when the organization restructures — the knowledge itself may persist as files, but its meaning, its provenance, and its authority degrade.

Existing disciplines address fragments of this problem. Knowledge management [1] organizes institutional knowledge but assumes stable human actors. Digital preservation [2] maintains bit-level integrity but does not preserve semantic context. Data engineering handles pipelines but not provenance or intent. AI alignment research [3] addresses model behavior but not the persistence of validated outputs across model generations.

No existing discipline asks the compound question: **how do you engineer a system where knowledge, its meaning, its provenance, and its authority survive the simultaneous turnover of the AI models that helped create it, the humans who validated it, and the regulatory frameworks that governed it?**

This paper proposes Knowledge Continuity Engineering (KCE) as the discipline that addresses this question. Section 2 reviews related work and identifies the gap. Section 3 defines the five problems KCE addresses. Section 4 presents seven design principles. Section 5 describes a five-layer reference architecture. Section 6 presents BRIDGRAI as a reference implementation. Section 7 discusses limitations and open questions. Sections 8–13 provide an extended framework with implementation guidance, case studies, proposed metrics, risk analysis, and an adoption roadmap.

## 2. Related Work and the Disciplinary Gap

### 2.1 Knowledge Management

The knowledge management (KM) field, established through the work of Nonaka and Takeuchi [4] on tacit and explicit knowledge, and subsequently developed into organizational practice through frameworks like SECI (Socialization, Externalization, Combination, Internalization), addresses the capture and transfer of knowledge within organizations. However, KM frameworks assume that knowledge producers are human, that organizational structures persist, and that the primary challenge is converting tacit knowledge to explicit knowledge.

None of these assumptions hold in AI-augmented environments. The knowledge producer may be an AI model that will be deprecated. The "organization" may be a solo researcher or a small team. And the primary challenge is not conversion from tacit to explicit, but preservation of context and authority across technological generations.

### 2.2 Digital Preservation

The OAIS (Open Archival Information System) reference model [2], codified as ISO 14721, provides a framework for long-term preservation of digital information. OAIS distinguishes between Content Information (the object being preserved) and Preservation Description Information (provenance, context, reference, fixity).

While OAIS addresses bit-level preservation and basic provenance, it was designed for archival contexts — libraries, museums, government records — not for active, evolving knowledge systems where new knowledge is continuously produced, validated, and revised by human-AI collaboration. OAIS does not address model discontinuity, semantic validation, or the authority problem inherent in AI co-creation.

### 2.3 Data Provenance

The W3C PROV standard [5] provides a data model for provenance information. Research in data provenance has established methods for tracking who created what, when, and from what sources [6]. However, provenance research has focused primarily on data lineage in computational workflows, not on the semantic and authority dimensions of knowledge produced through human-AI collaboration.

The specific challenge of AI-contributed provenance — documenting what an AI model generated versus what a human validated, and how the model's limitations affected the output — is not addressed by existing provenance frameworks.

### 2.4 AI Governance and Alignment

The EU AI Act (Regulation 2024/1689) [7] establishes requirements for documentation, risk management, and human oversight of AI systems. Research in AI alignment [3] addresses how AI systems can be made to behave in accordance with human values. Research in AI safety addresses failure modes and robustness.

However, AI governance focuses on the behavior of AI systems at the point of deployment, not on the long-term persistence of knowledge those systems helped produce. The AI Act requires documentation of an AI system's design and performance — but not a mechanism for ensuring that the knowledge produced by that system remains valid, attributable, and authoritative after the system is replaced.

### 2.5 The Gap

The gap is not in any single dimension but in the intersection:

| Dimension | Covered by | Missing |
|-----------|-----------|---------|
| Knowledge organization | KM | AI model rotation, solo actors |
| Bit-level preservation | OAIS | Semantic continuity, active systems |
| Data lineage | W3C PROV | AI contribution attribution, authority |
| AI system compliance | AI Act | Post-deployment knowledge persistence |
| Security of knowledge | InfoSec | Semantic integrity, not just access control |

KCE sits at this intersection. It is not a replacement for any of these disciplines but a bridge that coordinates their application to the problem of knowledge persistence across generational transitions.

### 2.6 Concurrent Industry Usage

The term "knowledge continuity" — and variations including "knowledge continuity engineering" — appears in industry contexts prior to and concurrent with this work. We document these uses to distinguish what exists from what this paper contributes.

Glean [10] published "What is knowledge continuity in engineering" (November 2025), framing knowledge continuity as the preservation of institutional expertise across generations of engineering professionals. The focus is organizational knowledge management — capturing what experienced engineers know before they retire or change roles.

StandIn [11] published "Knowledge Continuity in Engineering: A Practical Framework" (February 2026), addressing the preservation of working context when personnel change roles or depart. The framework provides practical guidance for teams managing handoffs but does not address AI model rotation, cryptographic provenance, or formal validation mechanisms.

The Open Engineering Continuity organization on GitHub [12] (July 2026) explores the intersection of knowledge preservation across humans, AI agents, and organizations. This is the closest existing work to KCE in scope, though at the time of writing it has not produced a formal framework with defined problems, principles, or architecture.

Adjacent academic work includes IEEE's "Atlas: A Framework for ML Lifecycle Provenance & Transparency" [13], which addresses provenance in machine learning pipelines, and Cisco's "Model Provenance Constitution" [14], which proposes standards for documenting AI model lineage. Both address subsets of P1 (model discontinuity) and P3 (provenance decay) but not the compound problem of simultaneous human, model, and regulatory transitions.

**What this paper adds beyond concurrent usage:** The existing industry usage treats knowledge continuity as a *practice* — a set of recommendations for specific scenarios (employee handoffs, model documentation). This paper proposes KCE as a *discipline*: formally defined problems (P1-P5) with mathematical statements, testable design principles (K1-K7), a layered reference architecture, an evidence classification scale, proposed metrics, and a working reference implementation. The distinction is analogous to the difference between ad hoc security practices and the formal discipline of information security engineering.

## 3. The Five Problems

### 3.1 P1: Model Discontinuity

When an AI model is deprecated, the knowledge it contributed to loses its creation context. The biases, capabilities, training data cutoff, and behavioral characteristics of the model are embedded in its outputs but are not preserved as metadata. A document generated with GPT-4 in 2024 carries different implicit assumptions than one generated with a successor model in 2027, but this difference is typically invisible.

**Formal statement:** Given knowledge artifact K produced with model M_v at time t, and model M_{v+1} deployed at time t+1, the creation context C(K, M_v, t) is lost unless explicitly preserved by an external mechanism.

### 3.2 P2: Human Discontinuity

When a human knowledge holder departs — through retirement, death, career change, or organizational restructuring — their tacit understanding of the knowledge they created or validated is lost. This is well-documented in KM literature [4] but acquires new dimensions in AI-augmented environments: the human may be the only one who knows why they accepted an AI's output, what modifications they made, and what they rejected.

**Formal statement:** Given human validator H who approved artifact K based on criteria C_H, the approval context C(K, H, C_H) is lost when H departs unless externalized into an auditable record.

### 3.3 P3: Provenance Decay

Over time, the chain of "who created what, when, why, and with what authority" becomes progressively harder to reconstruct. This is especially acute in AI-augmented environments where the line between human-authored and AI-generated content is often undocumented.

**Formal statement:** The provenance function P(K) → (creator, time, method, authority) degrades with rate proportional to the number of unrecorded transformation steps applied to K.

### 3.4 P4: Semantic Drift

The meaning of terms, metrics, thresholds, and classification rules evolves over time. A validation rule that was appropriate in 2024 may be inappropriate in 2030 due to changes in regulatory frameworks, social norms, or scientific understanding. If the rule is preserved without its temporal context, it may be applied incorrectly.

**Formal statement:** Given semantic rule R defined at time t in context C_t, applying R at time t+n in context C_{t+n} produces valid results only if C_t ≈ C_{t+n} for the dimensions relevant to R. Without explicit versioning of both R and C, semantic validity cannot be assessed.

### 3.5 P5: Authority Fragmentation

As AI systems participate more actively in knowledge creation, the question "who is accountable for this knowledge?" becomes harder to answer. The AI model cannot be held accountable. The model provider may disclaim responsibility for outputs. The human operator may have accepted the output without full review. The organization may have changed leadership.

**Formal statement:** For any knowledge artifact K, there must exist at least one identifiable, accountable human agent A such that A can be held responsible for K's validity and can authorize its modification or revocation.

## 4. The Seven Principles of KCE

Based on analysis of the five problems and the gaps in existing disciplines, we propose seven design principles for Knowledge Continuity Engineering:

**K1: Knowledge Must Outlive Its Creator.** Every critical knowledge artifact must be anchored to a persistence mechanism that functions independently of the person or model that created it.

**K2: Meaning Requires Context.** A fact without its creation context — when, why, by whom, under what assumptions, with what limitations — is a liability, not an asset. Context must be stored as structured metadata, not as informal documentation.

**K3: Provenance Is Not Optional.** Every knowledge artifact must have a verifiable, cryptographically anchored chain of origin. "We believe X created this" is not acceptable; "here is the signed, timestamped evidence that X created this" is.

**K4: Validation Decays.** Knowledge validated at time t may be invalid at time t+n. Systems must include explicit mechanisms for re-validation, expiration, and honest labeling of validation status.

**K5: Human Anchor Required.** Every knowledge chain must terminate in at least one identifiable, accountable human. AI may assist at every step, but a human must be identifiable as the authority of last resort. This is a design requirement, not a political statement — it is the only way to maintain legal accountability under current frameworks [7].

**K6: Auditability Over Trust.** Systems must be designed so that any actor — present or future, internal or external — can independently verify any claim without relying on trust in the original creator. This requires cryptographic integrity checking, public verification mechanisms, and honest reporting of system capabilities and limitations.

**K7: Graceful Degradation Across Generations.** When a component is deprecated, replaced, or becomes unavailable — whether an AI model, a database, an API, a human operator, or an entire organization — the system must continue to function at reduced capability, not fail silently or catastrophically.

## 5. Reference Architecture: The KCE Stack

We propose a five-layer reference architecture. Each layer addresses a subset of the five problems and can be implemented independently, though full KCE compliance requires all five.

### 5.1 L1: Creation Layer

**Purpose:** Capture the full context of knowledge production.

**Requirements:**
- Authorship attribution distinguishing human and AI contributions
- AI model identification (model name, version, provider, known limitations)
- Intent declaration (stated purpose of the knowledge artifact)
- Input provenance (what sources were used)
- Transformation record (what modifications were applied to AI outputs)

**Addresses:** P1 (model discontinuity), P3 (provenance decay)

### 5.2 L2: Validation Layer

**Purpose:** Verify knowledge semantically, logically, and ethically before it enters the persistent store.

**Requirements:**
- Semantic validation (does the artifact mean what it claims to mean?)
- Adversarial testing (can the artifact be manipulated or misinterpreted?)
- Human review and approval with recorded rationale
- Evidence classification (a graded scale from unverified concept to legally proven)
- Temporal tagging (under what conditions is this validation valid?)

**Addresses:** P4 (semantic drift), P5 (authority fragmentation)

### 5.3 L3: Persistence Layer

**Purpose:** Store knowledge with integrity guarantees that survive technological transitions.

**Requirements:**
- Cryptographic hashing for integrity verification
- Blockchain or qualified electronic timestamping (eIDAS 2.0 [8]) for existence proofs
- Version control with immutable history
- Encrypted storage for sensitive knowledge
- Format-independent storage (knowledge must be recoverable even if the original software is unavailable)

**Addresses:** P2 (human discontinuity), P3 (provenance decay)

### 5.4 L4: Governance Layer

**Purpose:** Manage authority, access, accountability, and compliance.

**Requirements:**
- Authority chain definition (who can create, validate, modify, revoke)
- Human Anchor assignment per knowledge domain
- Compliance mapping to applicable regulations (GDPR [9], AI Act [7], eIDAS 2.0 [8])
- Access control with audit trail
- Conflict resolution protocols (when validators disagree)

**Addresses:** P5 (authority fragmentation), P4 (semantic drift)

### 5.5 L5: Continuity Layer

**Purpose:** Engineer the transitions between generations — of models, people, organizations, and regulatory frameworks.

**Requirements:**
- Model migration protocols (explicit procedures for knowledge transfer when an AI model is replaced)
- Succession instruments (legal and technical mechanisms for human transitions)
- Knowledge re-validation schedules (periodic review of previously validated knowledge)
- Degradation protocols (defined behavior when a layer fails or a component is unavailable)
- Cross-generation testing (verification that knowledge produced under one configuration remains accessible under its successor)

**Addresses:** P1 (model discontinuity), P2 (human discontinuity), P4 (semantic drift)

## 6. Reference Implementation: BRIDGRAI

BRIDGRAI (Bridging Responsible Intelligence for Digital Governance, Research, and AI Innovation) is a working ecosystem developed over 3.5 years by a solo researcher as part of independent AI safety and governance work. It was not designed as a KCE implementation — the discipline was named after the system was substantially built. However, it implements KCE principles across all five layers, making it a useful reference implementation.

### 6.1 Implementation Mapping

| KCE Layer | BRIDGRAI Component | Implementation |
|-----------|-------------------|----------------|
| L1: Creation | Git history + harvest methodology | Single-author Git with AI contribution tracking. AI-generated code is reviewed, corrected, and the corrections are documented. |
| L2: Validation | TVE Core + Backbone | TVE: 6-pillar semantic analysis (semantic, intent, emotional, disinfo, logic, context). Backbone: 7 invariant axioms (Ω-1 through Ω-7) verified by automated probe. |
| L3: Persistence | UKBE Core + HASN + Tezos | UKBE: ML-DSA-87 post-quantum signing (NIST FIPS 204). HASN: PostgreSQL with AES-256-GCM authenticated encryption. Tezos: 128 IP assets timestamped on mainnet. |
| L4: Governance | CASP + Legal Framework | CASP: covenant-based AI-human agreements with semantic rules. Legal framework: CIaaS governance architecture with 8 defined layers. |
| L5: Continuity | Succession plan + licensing | Transgenerational succession instruments for designated beneficiary. Proprietary code with documented succession; the published science (CC BY 4.0) ensures the knowledge survives any single entity. |

### 6.2 Honest Assessment

BRIDGRAI demonstrates that KCE principles are implementable, but it has significant limitations:

**What works:**
- Post-quantum signing (ML-DSA-87) provides cryptographic provenance that is resistant to quantum computing attacks on classical signatures
- Blockchain timestamping on Tezos provides immutable existence proofs
- The 6-pillar validation engine detects explicit manipulation patterns in Romanian and English
- The 7-axiom invariant system enforces behavioral constraints on AI agents
- Single-author Git history provides unambiguous attribution

**What is partial:**
- TVE Core is v0.1 heuristic (pattern matching, not NLP) — sophisticated manipulation evades detection
- The governance layer is technically implemented but the legal structure (entity formation, succession instruments) is not yet formalized
- HASN provides persistent audit but is a single server, not a distributed system
- Cross-model validation has been practiced (outputs verified across Claude, GPT, Grok, DeepSeek) but not formally protocolized

**What is aspirational:**
- IPFS integration for distributed immutable storage is planned but not implemented
- Formal KCE maturity model does not exist
- Independent external audit has not been conducted
- The system has been tested by one person, not by an organization

### 6.3 Reproducibility

All BRIDGRAI components except API keys are available for inspection:
- UKBE Core: public repository with 14 test files, 175 tests
- TVE Core: public repository with documented pillar architecture
- Tezos transactions: publicly verifiable on mainnet via tzkt.io
- Paper P6: DOI 10.5281/zenodo.21269201

The system can be reproduced by installing the Python packages, configuring environment variables, and running the FastAPI server. No proprietary infrastructure is required beyond a PostgreSQL database and a Tezos wallet.

## 7. Discussion

### 7.1 Why Now

Three trends make KCE necessary now rather than in the future:

1. **AI model rotation is accelerating.** The interval between major model releases has shortened from years to months. Knowledge produced with one model generation must survive into the next.

2. **Regulatory frameworks are crystallizing.** The EU AI Act [7] and eIDAS 2.0 [8] create legal requirements for documentation, transparency, and auditability that are difficult to retrofit. Systems designed without continuity in mind will face compliance costs that increase with each generation.

3. **The cost of knowledge loss is becoming measurable.** As organizations depend more heavily on AI-augmented workflows, the loss of institutional knowledge during model transitions or personnel changes produces visible failures: repeated work, contradictory outputs, compliance gaps, and unreproducible results.

### 7.2 Limitations of This Work

This paper has several limitations that must be stated honestly:

1. **Single implementation.** BRIDGRAI is one system built by one person. The generalizability of its architecture to other contexts — enterprise, government, multi-team — is untested.

2. **No controlled evaluation.** We have not conducted controlled experiments comparing KCE-compliant systems with non-KCE systems on knowledge continuity metrics. Such metrics do not yet exist in standardized form.

3. **The discipline is proposed, not validated.** Naming a discipline does not establish it. KCE will become a real discipline only when multiple independent implementations exist, formal evaluation methods are developed, and the research community engages with its premises.

4. **Regulatory interpretation.** Our mapping of KCE to existing regulations (GDPR, AI Act, eIDAS 2.0) reflects our interpretation, not authoritative legal guidance.

### 7.3 Open Research Questions

1. **Minimum viable KCE:** What is the smallest implementation that provides meaningful knowledge continuity? Not every system needs all five layers.

2. **KCE maturity model:** A structured maturity scale (analogous to CMMI) would help organizations assess their current state and plan improvement.

3. **Knowledge continuity metrics:** How do you measure whether knowledge continuity has been achieved? Possible dimensions include: provenance completeness, semantic validity over time, authority chain integrity, and degradation resilience.

4. **AI contribution attribution:** When an AI model contributes to knowledge creation, how should this be documented in a legally defensible and scientifically rigorous way?

5. **Cross-jurisdictional continuity:** Knowledge produced under one regulatory framework (e.g., EU GDPR + AI Act) may need to be used under another. How does KCE handle jurisdictional transitions?

6. **Economic models:** Who pays for knowledge continuity? The creator, the consumer, the platform, or the regulator? Different funding models create different incentive structures.

7. **Right to be forgotten vs. obligation to remember:** GDPR Article 17 creates a right to erasure. KCE principle K1 requires knowledge to outlive its creator. When personal data is embedded in persistent knowledge, how are these obligations balanced?

## 8. Implementation Guide: From Principles to Practice

### 8.1 The Minimum Viable KCE System

Not every organization needs all five layers of the KCE stack. We propose a graduated implementation approach:

**Tier 1: Foundational (0-6 months)**

- L1 Creation Layer: Implement structured metadata capture for all knowledge artifacts. Minimum fields: `author_human`, `author_ai_model`, `creation_timestamp`, `intent`, `validation_status`.
- L3 Persistence Layer: Cryptographic hashing (SHA-256) + version control (Git) + backup strategy.
- K5 Human Anchor: Assign at least one identifiable human responsible for each knowledge domain.

**Tier 2: Operational (6-18 months)**

- L2 Validation Layer: Implement semantic validation rules + evidence classification scale (E0-E5).
- L3 Persistence Layer (extended): Blockchain timestamping for high-value artifacts.
- K6 Auditability: Public verification mechanism (anyone can check integrity without trusting creator).

**Tier 3: Mature (18-36 months)**

- L4 Governance Layer: Authority chains, access control with audit trails, compliance mapping.
- L5 Continuity Layer: Model migration protocols, succession instruments, degradation protocols.
- K7 Graceful Degradation: Tested failure modes for each component.

### 8.2 Implementation Checklist by Layer

#### L1: Creation Layer

Required:

```yaml
# Metadata schema — mandatory fields per artifact
artifact_id: UUID
human_author: identifier (ORCID, email)
ai_contributors:
  - model_name: string
    version: string
    provider: string
    role: string  # "draft generation", "review", "translation", etc.
creation_timestamp: ISO 8601
intent: free text
input_provenance:
  - source_artifact_id or URI
transformation_record:
  - timestamp: ISO 8601
    actor: human or model identifier
    action: string
    rationale: free text
```

Enforcement mechanisms:
- Git commit hooks that reject commits without metadata
- API middleware that validates metadata on artifact creation
- CI/CD pipeline that fails builds with incomplete metadata

Optional but recommended:
- Intent classification taxonomy (research, operational, legal, educational)
- Confidence level declaration (how certain is the creator about accuracy?)

#### L2: Validation Layer

Required:
- Semantic validation rules engine (DSL or YAML rule definitions)
- Automated testing of rules against artifacts
- Human override mechanism with recorded rationale
- Evidence classification labels (E0-E5, see Appendix A)
- Adversarial testing framework: minimum 3 attack vectors per artifact type, documented failure modes, remediation procedures

Optional but recommended:
- Multi-validator consensus protocol (2+ humans must agree for E4+)
- Temporal validity tagging (this validation expires on YYYY-MM-DD)

#### L3: Persistence Layer

Required:
- Cryptographic integrity: SHA-256 hash of every artifact, hash stored separately from artifact, periodic integrity verification (monthly minimum)
- Version control: immutable history (no force-push), signed commits (GPG or SSH), branch protection rules
- Backup strategy: 3-2-1 rule (3 copies, 2 media types, 1 offsite), tested restoration procedure (quarterly)

Optional but recommended:
- Blockchain timestamping (Tezos, Bitcoin, or equivalent)
- Distributed storage (IPFS or equivalent) for redundancy
- Format migration plan (what happens when file formats become obsolete?)

#### L4: Governance Layer

Required:
- Authority chain definition: who can create, validate, modify, and revoke artifacts in each domain
- Human Anchor registry: mapping of knowledge domains to accountable humans, with succession plan for each anchor
- Compliance mapping: applicable regulations identified, article-by-article compliance evidence, audit trail for compliance checks
- Conflict resolution protocol: what happens when validators disagree

Optional but recommended:
- Role-based access control (RBAC) with time-based constraints
- Automated compliance checking (rules engine)
- Cross-jurisdictional handling procedures

#### L5: Continuity Layer

Required:
- Model migration protocol: documented procedure for replacing AI models, validation of artifacts under new model, rollback procedure
- Succession instruments: legal mechanism for transferring authority, technical mechanism for transferring access, knowledge transfer procedure
- Degradation protocols: defined behavior when each component fails, recovery procedures

Optional but recommended:
- Cross-generation testing (quarterly verification that old artifacts work with new components)
- Formal succession procedures (legal + technical + operational acknowledgment)

### 8.3 Common Implementation Pitfalls

**Pitfall 1: Metadata as Afterthought**

Problem: Metadata is added after artifact creation, leading to incomplete or inaccurate records.

Solution: Enforce metadata at creation time. Git hooks, API middleware, CI/CD failures. Make it impossible to create an artifact without metadata.

**Pitfall 2: Trust Without Verification**

Problem: System assumes creators are honest and competent. No independent verification mechanism.

Solution: Implement K6 (Auditability Over Trust). Every claim must be independently verifiable. Cryptographic integrity checking, public verification mechanisms.

**Pitfall 3: Human Anchor as Checkbox**

Problem: Human Anchor is assigned but never consulted. Becomes a bureaucratic formality, not real accountability.

Solution: Require Human Anchor signature for high-value artifacts (E4+). Implement periodic reviews where anchors must re-validate or revoke.

**Pitfall 4: Blockchain as Proof of Truth**

Problem: Timestamping on blockchain without understanding what it proves. Blockchain proves existence at time T — not truth, not authority, not ongoing validity.

Solution: Clearly document what blockchain proves and what it does not. Complement with validation (L2) and governance (L4) mechanisms.

**Pitfall 5: Perfection Paralysis**

Problem: Organization waits for perfect KCE implementation before starting. Never starts.

Solution: Start with Tier 1. Iterate. A partial implementation is infinitely better than no implementation.

## 9. Case Studies: KCE in Practice

### 9.1 Case Study 1: Solo Researcher (BRIDGRAI) — Real

**Context:** Independent researcher building AI safety and governance ecosystem over 3.5 years. Single human operator, multiple AI collaborators (Claude, GPT, Grok, DeepSeek, Qwen), no institutional backing, no external funding.

**KCE Implementation:**

| Layer | Implementation | Status |
|-------|---------------|--------|
| L1: Creation | Git history with AI contribution tracking. Harvest methodology: AI generates draft, human reviews/corrects/integrates. Corrections documented per artifact. | Active |
| L2: Validation | TVE Core (6-pillar semantic validation, v0.1 heuristic). Backbone (7-axiom invariant system, Ω-1 through Ω-7). 175 automated tests (UKBE Core). | Functional |
| L3: Persistence | Tezos blockchain (128 IP assets timestamped). PostgreSQL + AES-256-GCM (HASN). UKBE Core ML-DSA-87 post-quantum signing. Git with full history. | Active |
| L4: Governance | CASP covenant-based agreements. CIaaS legal framework (22 sections, 8 governance layers). Single-author authority chain. | Partial (legal entity pending) |
| L5: Continuity | Transgenerational succession plan for designated beneficiary. Proprietary licensing with documented succession. | In progress |

**Outcomes:**

- Knowledge survives model rotation: framework built across 5+ AI model families remains coherent as individual models are deprecated
- Knowledge is independently verifiable: Tezos transactions are public, tests are runnable, Git history is inspectable
- Single-author provenance is unambiguous: no co-author confusion, no corporate IP disputes

**Limitations:**

- Single point of failure (one human)
- No external validation achieved (E4 level pending — peer review in progress)
- Legal structure not formalized (entity formation pending)
- TVE validation is v0.1 heuristic — sophisticated manipulation evades detection

**Lessons Learned:**

1. Start with persistence (L3), not governance (L4). Cryptographic hashing + blockchain provides immediate, cheap, irreversible value. Governance can come later.
2. AI contribution tracking is non-negotiable. Without it, provenance decays within months. With it, you can reconstruct the full human-AI collaboration years later.
3. Cross-model methodology is itself a KCE practice: validating outputs across multiple AI models creates adversarial redundancy.

### 9.2 Case Study 2: Small AI Startup — Hypothetical

**Context:** 5-person startup building AI-powered medical diagnosis tool. 18-month runway.

**Proposed KCE Implementation:**

Month 1-3 (Tier 1):
- Metadata schema for all code, models, and datasets
- Git with signed commits + branch protection
- Human Anchors assigned: CEO (strategy), data scientist (model validation), lead engineer (code quality)
- SHA-256 hashing + monthly integrity checks

Month 4-9 (Tier 2):
- Semantic validation rules for medical claims (must cite sources)
- Evidence classification: all artifacts labeled E0-E5
- Blockchain timestamping for model weights and training data
- Multi-validator consensus: 2 humans must approve E3+ artifacts

Month 10-18 (Tier 3):
- Authority chains: who can modify validated medical claims
- Compliance mapping: applicable medical device and data protection regulations
- Model migration protocol: procedure for transitioning between AI model generations
- Succession plan: what if the data scientist leaves

**Expected benefits:** Audit-ready documentation, knowledge resilience against personnel changes, demonstrable provenance for regulatory submissions.

**Expected challenges:** Development overhead (estimated 10-15% additional time), cultural adoption, budget allocation for blockchain and compliance tooling.

*Note: This case study is hypothetical. The specific benefits and challenges would depend on the startup's domain, regulatory environment, and team composition. No financial projections are provided because no empirical data exists to support them.*

### 9.3 Case Study 3: Large Enterprise — Hypothetical

**Context:** Pharmaceutical company, 10,000+ employees, 50+ year history, multiple AI systems across R&D, manufacturing, and regulatory affairs. Heavily regulated (FDA, EMA).

**Proposed KCE Implementation:**

Phase 1 — Assessment (3 months):
- Audit existing knowledge management practices
- Identify high-risk domains (clinical trial data, regulatory submissions, core algorithms)
- Map current state against KCE Compliance Checklist (Appendix B)

Phase 2 — Pilot (6 months):
- Select 2-3 high-risk domains for KCE pilot
- Implement Tier 1 + Tier 2 for pilot domains
- Measure baseline metrics, then monthly improvement

Phase 3 — Scale (12 months):
- Roll out to all high-risk domains
- Implement Tier 3 (governance + continuity)
- Integrate with existing systems (ERP, document management, laboratory information systems)

Phase 4 — Optimize (ongoing):
- Continuous improvement based on metrics
- Cross-jurisdictional handling (different regulatory agencies)
- Model migration drills as AI systems evolve

**Expected benefits:** Cross-jurisdictional regulatory readiness, preservation of decades of R&D knowledge across personnel changes, reduced liability exposure.

**Expected challenges:** Scale (legacy systems integration), cultural change management, multi-year implementation timeline.

*Note: This case study is hypothetical. Implementation details would vary significantly by organization. The pharmaceutical context is chosen because it illustrates the intersection of regulatory pressure, long knowledge lifespans, and AI adoption — all conditions where KCE is most critical.*

## 10. Proposed Metrics for Knowledge Continuity

### 10.1 The Measurement Problem

How do you know if KCE is working? We propose four measurement dimensions. These metrics are **proposed, not validated** — they have not been tested across multiple implementations. They are offered as a starting point for the research community to refine.

### 10.2 Dimension 1: Provenance Completeness

**Metric: Provenance Coverage Ratio (PCR)**

```
PCR = (Artifacts with complete provenance) / (Total artifacts)
```

Complete provenance requires:
- Human author identified (K5)
- AI contributors documented, if any (K1, K3)
- Creation timestamp recorded (K3)
- Intent documented (K2)
- Input provenance documented (K3)
- Cryptographic hash stored separately (K3)

Measurement method: Automated scan of metadata completeness, reported monthly.

Interpretation guidance:
- Low PCR indicates systemic gaps in provenance capture
- PCR should increase over time as enforcement mechanisms mature
- Artifacts created before KCE adoption will have lower provenance — track separately

*No target values are proposed. Appropriate thresholds depend on the domain, regulatory environment, and risk tolerance of the organization. Setting arbitrary targets without empirical data would undermine the credibility of the metric.*

### 10.3 Dimension 2: Semantic Validity

**Metric: Semantic Validity Ratio (SVR)**

```
SVR = (Artifacts with current valid semantics) / (Total artifacts requiring validation)
```

Validity requires:
- Validation status labeled (E0-E5)
- Re-validation schedule defined
- Last re-validation within schedule
- No semantic drift detected since last validation

Measurement method: Combination of automated drift detection and manual re-validation sampling (percentage and frequency determined by domain risk level).

Interpretation guidance:
- SVR naturally decreases over time as knowledge ages — this is expected, not a failure
- Rapidly declining SVR may indicate environmental changes (new regulations, new science, model upgrades) that require systematic re-validation
- SVR is most useful as a trend indicator, not an absolute score

### 10.4 Dimension 3: Authority Integrity

**Metric: Authority Chain Integrity (ACI)**

```
ACI = (Artifacts with intact authority chains) / (Total artifacts)
```

Intact authority chain requires:
- Human Anchor assigned and currently active
- Authority chain documented (who can create, validate, modify, revoke)
- No orphaned artifacts (artifacts whose Human Anchor has departed without successor)
- Conflict resolution protocol exists and has been tested

Measurement method: Automated scan for orphaned artifacts (Human Anchor departed, no successor assigned), combined with quarterly authority chain audit.

Interpretation guidance:
- ACI drops when people leave without succession — track as early warning indicator
- Orphaned artifact count is the most actionable sub-metric
- ACI is most meaningful in organizations with personnel turnover

### 10.5 Dimension 4: Degradation Resilience

**Metric: Degradation Resilience Score (DRS)**

```
DRS = (Successful degradation tests) / (Total degradation tests)
```

Degradation test scenarios (examples — actual scenarios depend on implementation):
- AI model unavailable: Can system function with manual processes?
- Blockchain unavailable: Can system function with local hashes?
- Human Anchor unavailable: Can system function with delegated authority?
- Database unavailable: Can system function with cached data?
- Network unavailable: Can system function offline?

Measurement method: Periodic chaos engineering exercises — deliberately disable components, measure system response against documented degradation protocols.

Interpretation guidance:
- DRS measures preparedness, not perfection — a system that fails gracefully scores higher than one that claims to never fail
- Low DRS in specific scenarios directs investment toward those failure modes
- DRS should be measured through actual testing, not theoretical analysis

### 10.6 Composite Score

**Metric: KCE Maturity Score (KMS)**

```
KMS = w₁·PCR + w₂·SVR + w₃·ACI + w₄·DRS
```

Where w₁ + w₂ + w₃ + w₄ = 1.0.

**We deliberately do not propose specific weights.** The relative importance of provenance, validity, authority, and resilience depends on the domain:

- A pharmaceutical company may weight SVR heavily (semantic validity of drug data is life-critical)
- A financial institution may weight ACI heavily (authority over trading decisions is regulatory-critical)
- A solo researcher may weight PCR heavily (provenance is the primary defense against IP disputes)
- A military application may weight DRS heavily (resilience under adversarial conditions is paramount)

Each organization must determine its own weights based on its risk profile. Publishing a "universal" weight set would imply a precision that does not exist.

**Maturity levels (qualitative, not quantitative):**

| Level | Description |
|-------|-------------|
| Nascent | No systematic KCE practices. Knowledge continuity is accidental. |
| Developing | Some KCE practices in place, typically L1 + L3. Gaps in validation and governance. |
| Established | KCE practices across all high-risk domains. Metrics tracked. Some gaps remain. |
| Mature | Comprehensive KCE across most domains. Regular testing. External audit conducted. |
| Exemplary | KCE is a core organizational capability. Continuous improvement. Contributes to field. |

*These levels are descriptive, not prescriptive. An organization at "Developing" level may be perfectly adequate for its risk profile. The goal is not to reach "Exemplary" but to reach the level appropriate for the domain.*

## 11. Risk Categories: Why KCE Matters

KCE mitigates four categories of organizational risk. We describe each qualitatively — the specific financial impact depends on the organization's size, domain, regulatory environment, and existing practices.

### 11.1 Regulatory Risk

**Scenario:** Organization uses AI to produce regulated outputs (medical submissions, financial reports, educational materials). Regulator asks: "What model generated this? What were its limitations? Who validated it? Can you prove it existed at the time claimed?"

Without KCE: Cannot answer. Submission rejected, delayed, or penalized.

With KCE: Complete provenance (L1), validation record (L2), cryptographic proof (L3), authority chain (L4).

Relevance is highest for: healthcare, finance, education (AI Act high-risk categories), any organization producing AI-assisted regulatory submissions.

### 11.2 Operational Risk

**Scenario:** Key knowledge holder departs. They built critical AI system or validated critical knowledge. No documentation, no succession plan.

Without KCE: Months of reverse-engineering. Risk of introducing errors. System becomes unmaintainable "legacy."

With KCE: Complete documentation (L1), validation status (L2), succession instruments (L5).

Relevance is highest for: small teams, startups, any organization with concentrated knowledge in few individuals.

### 11.3 Legal Risk

**Scenario:** Organization is challenged on AI-generated content (IP dispute, liability claim, accuracy challenge). Challenger asks: "Who created this? What model? Who validated it? Can you prove provenance?"

Without KCE: Cannot prove due diligence. Assumed liability.

With KCE: Provenance trail (L1, L3), validation record (L2), authority chain (L4).

Relevance is highest for: content-producing organizations, any entity making public claims based on AI-assisted analysis.

### 11.4 Reputational Risk

**Scenario:** Public failure due to knowledge discontinuity — contradictory AI outputs, retracted claims, regulatory rejection.

Without KCE: Ad hoc response. No audit trail. Difficulty explaining what went wrong.

With KCE: Rapid root-cause identification (audit trails). Transparent communication (provenance records). Demonstrated due diligence.

Relevance is highest for: public-facing organizations, regulated industries, organizations dependent on trust.

### 11.5 The Compounding Effect

These risks compound. A regulatory inquiry (11.1) triggered by a departed employee's undocumented work (11.2) leads to legal liability (11.3) and public disclosure (11.4). KCE mitigates the chain, not just individual links.

## 12. Adoption Roadmap

### 12.1 Phase 0: Awareness (1-2 weeks)

**Goal:** Understand KCE, assess current state, identify priorities.

Activities:
- Assess current state using KCE Compliance Checklist (Appendix B)
- Identify high-risk knowledge domains (which knowledge would cause most damage if lost?)
- Identify stakeholders who need to be involved (technical, legal, compliance)

Deliverables:
- Current state assessment (checklist score)
- High-risk domain inventory
- Stakeholder map

### 12.2 Phase 1: Pilot (3-6 months)

**Goal:** Implement Tier 1 KCE in 2-3 high-risk domains. Learn what works.

Activities:
- Select pilot domains from high-risk inventory
- Implement Tier 1 (metadata capture, cryptographic hashing, version control, Human Anchor assignment)
- Measure: what percentage of new artifacts have complete provenance?
- Document lessons learned

Deliverables:
- Tier 1 implementation in pilot domains
- Lessons learned document
- Decision: expand or adjust

### 12.3 Phase 2: Expansion (6-12 months)

**Goal:** Expand to all high-risk domains. Add Tier 2.

Activities:
- Roll out Tier 1 to all high-risk domains
- Implement Tier 2 (validation rules, evidence classification, blockchain timestamping)
- Train users (workshops, documentation)
- Integrate with existing workflows
- Begin measuring all four KCE dimensions (PCR, SVR, ACI, DRS)

Deliverables:
- Tier 1 + Tier 2 in all high-risk domains
- Training materials
- Quarterly metrics reports

### 12.4 Phase 3: Maturation (12-24 months)

**Goal:** Implement Tier 3. Achieve comprehensive KCE.

Activities:
- Implement Tier 3 (authority chains, compliance mapping, model migration protocols, succession instruments, degradation protocols)
- Expand Tier 1 + Tier 2 to medium-risk domains
- Conduct chaos engineering exercises (quarterly)
- Consider external audit

Deliverables:
- Full Tier 1-3 in high-risk domains
- Chaos engineering reports
- External audit report (if conducted)

### 12.5 Phase 4: Optimization (ongoing)

**Goal:** Continuous improvement.

Activities:
- Expand to lower-risk domains as capacity allows
- Optimize based on metrics (focus on weakest dimension)
- Annual model migration drill
- Contribute to KCE community (case studies, tools, standards proposals)

## 13. Conclusion

Knowledge Continuity Engineering addresses a problem that is becoming more visible with each AI model generation: the systematic loss of meaning, provenance, intent, and authority as the technological and human foundations of knowledge change.

This paper does not claim to have solved this problem. It proposes a framework — five problems, seven principles, five architectural layers — and presents one working implementation that demonstrates the principles are realizable. The extended framework provides implementation guidance, case studies, measurement proposals, risk framing, and an adoption roadmap.

Three points bear repeating:

1. **Start with Tier 1.** Metadata + hashing + Human Anchor. This is achievable in weeks, not months, and provides immediate value.

2. **Metrics are proposed, not proven.** PCR, SVR, ACI, DRS need validation across multiple implementations before they can be considered standards. We offer them as starting points.

3. **KCE is risk management, not perfection.** The goal is not zero knowledge loss — that is impossible. The goal is that knowledge loss is detected, quantified, and mitigated rather than silent and catastrophic.

The founding claim of KCE is modest: **knowledge continuity is an engineering problem, and engineering problems deserve engineering disciplines.**

The world is building AI systems whose knowledge will be orphaned within three years, operated by organizations whose key people will change within five years, under regulatory frameworks that will transform within ten years. The question is not whether knowledge continuity matters, but whether we address it deliberately or discover its absence through failure.

---

## Data Availability

All BRIDGRAI components referenced in this paper are publicly available:
- Source code: github.com/amidigiart (75+ public repositories)
- Blockchain transactions: Tezos mainnet, wallet tz1bmw3igCLN8N6CqgLBzJ9dyRb79E2Tdu5Q
- Test suites: 175 automated tests (UKBE Core), additional tests across ecosystem components
- Documentation: amiecosystems.org

## Funding

This work was developed independently over 3.5 years (2022–2026) with zero external funding. No grants, investors, or institutional support were received. All development was performed on consumer hardware using open-source tools.

## Competing Interests

The author declares no competing interests. The author is the sole developer of BRIDGRAI and the ami* ecosystem. Potential commercial licensing revenue from UKBE Core (proprietary, commercially licensed) is disclosed but does not constitute a competing interest for the academic claims made in this paper.

## AI Contribution Statement

This paper was produced using AI assistance at multiple stages, documented in accordance with KCE principle K3 (Provenance Is Not Optional):

- **Sections 1–7, Appendices A–B:** Drafted with assistance from Claude (Anthropic). All content reviewed, corrected, and adopted by the human author.
- **Sections 8–13:** Initial structure generated by Qwen (Alibaba Cloud). Subsequently reviewed by the author: fabricated financial projections removed, metrics reframed as proposals without empirical validation, case studies 2 and 3 explicitly marked as hypothetical, duplicate content removed. All claims in the final text are the responsibility of the human author.
- **Section 2.6:** Concurrent usage research assisted by Claude and ChatGPT (OpenAI). Citations verified by the author.

The author bears full responsibility for all claims, including those in AI-assisted sections. This disclosure follows the DSEI III (Declare, Show, Explain, Iterate) methodology described in the author's prior work.

---

## References

[1] Nonaka, I., & Takeuchi, H. (1995). *The Knowledge-Creating Company*. Oxford University Press.

[2] CCSDS. (2012). Reference Model for an Open Archival Information System (OAIS). CCSDS 650.0-M-2. ISO 14721:2012.

[3] Russell, S. (2019). *Human Compatible: Artificial Intelligence and the Problem of Control*. Viking.

[4] Nonaka, I. (1994). A dynamic theory of organizational knowledge creation. *Organization Science*, 5(1), 14-37.

[5] W3C. (2013). PROV-DM: The PROV Data Model. W3C Recommendation.

[6] Simmhan, Y. L., Plale, B., & Gannon, D. (2005). A survey of data provenance in e-science. *ACM SIGMOD Record*, 34(3), 31-36.

[7] European Parliament. (2024). Regulation (EU) 2024/1689 laying down harmonised rules on artificial intelligence (AI Act).

[8] European Parliament. (2024). Regulation (EU) 2024/1183 amending Regulation (EU) No 910/2014 as regards establishing the European Digital Identity Framework (eIDAS 2.0).

[9] European Parliament. (2016). Regulation (EU) 2016/679 on the protection of natural persons with regard to the processing of personal data (GDPR).

[10] Glean. (2025). "What is knowledge continuity in engineering." Blog post, November 2025. https://www.glean.com

[11] StandIn. (2026). "Knowledge Continuity in Engineering: A Practical Framework." Blog post, February 2026. https://www.standin.co

[12] Open Engineering Continuity. (2026). GitHub organization, July 2026. https://github.com/open-engineering-continuity

[13] IEEE. (2025). "Atlas: A Framework for ML Lifecycle Provenance & Transparency." IEEE Explore.

[14] Cisco. (2025). "Model Provenance Constitution." Cisco Blogs. https://blogs.cisco.com

---

## Appendix A: Evidence Classification Scale

For use in KCE implementations — each knowledge artifact should carry one of the following labels:

| Level | Label | Definition | Example |
|-------|-------|-----------|---------|
| E0 | Idea | Unverified concept | "We could use Kuramoto oscillators for consensus" |
| E1 | Implemented | Code or prototype exists | Working Python implementation with unit tests |
| E2 | Reproducible | Can be independently reproduced | Public repository with installation instructions and test suite |
| E3 | Tested | Documented experimental results | 175 passing tests, documented failure modes |
| E4 | Externally validated | Independent audit or peer review | Zenodo DOI, arXiv review, third-party audit |
| E5 | Legally proven | Provenance and rights documented juridically | Blockchain timestamp + chain of title + registered IP |

## Appendix B: KCE Compliance Checklist

A minimal checklist for assessing whether a system implements KCE principles:

- [ ] All knowledge artifacts have identified human authors (K5)
- [ ] AI contributions are documented separately from human contributions (K1, K3)
- [ ] Creation context (model, version, date, intent) is recorded as structured metadata (K2)
- [ ] Cryptographic integrity checking is implemented (K3, K6)
- [ ] Existence proofs are anchored to an external, immutable system (K3)
- [ ] Validation status is labeled using a graded scale (K4)
- [ ] Re-validation schedules exist for time-sensitive knowledge (K4)
- [ ] At least one Human Anchor is assigned per knowledge domain (K5)
- [ ] All claims are independently verifiable without trusting the creator (K6)
- [ ] The system has defined behavior for each component's failure (K7)
- [ ] Succession plan exists for human knowledge holders (K7)
- [ ] Model migration protocol exists for AI component replacement (K7)

## Appendix C: L1 Metadata for This Paper

This paper applies its own methodology. The following L1 Creation Layer metadata documents the provenance of this artifact:

```yaml
artifact_id: KCE-PAPER-ZENODO-v1.0
human_author:
  name: Mihai Roșca
  orcid: 0009-0001-1422-6209
  location: Brăila, Romania, EU
  role: author, reviewer, corrector, final authority
ai_contributors:
  - model_name: Claude
    provider: Anthropic
    role: draft assistance (sections 1-7, appendices A-B), concurrent usage research (section 2.6)
  - model_name: Qwen
    provider: Alibaba Cloud
    role: initial structure generation (sections 8-13)
  - model_name: ChatGPT
    provider: OpenAI
    role: concurrent usage research (section 2.6), independent gap verification
creation_timeline:
  founding_document: 2026-09-09
  base_paper_draft: 2026-09-09
  extended_framework_draft: 2026-09-09
  unified_zenodo_version: 2026-09-09
intent: >
  Propose Knowledge Continuity Engineering as a formal discipline.
  Provide framework (problems, principles, architecture) and reference implementation.
  Establish priority via blockchain timestamp and DOI.
input_provenance:
  - BRIDGRAI ecosystem (3.5 years development, 128 IP assets on Tezos)
  - Prior publication: P6 (DOI 10.5281/zenodo.21269201)
  - KCE founding document (SHA-256: 92d989727fda66fe72205b950faa756435be2d90943546e8bfa57726457947d3)
transformation_record:
  - timestamp: 2026-09-09
    actor: Mihai Roșca
    action: Reviewed and corrected AI-generated sections 8-13
    rationale: >
      Removed fabricated financial projections (ROI table with €355K-4.5M figures).
      Removed arbitrary metric thresholds (PCR ≥ 0.80 etc.).
      Removed unjustified KMS weights (0.3/0.3/0.2/0.2).
      Marked case studies 2-3 as hypothetical.
      Removed duplicate KMS section.
  - timestamp: 2026-09-09
    actor: Mihai Roșca
    action: Added §2.6 concurrent industry usage
    rationale: Prior art transparency — term exists in industry, formalization is original
  - timestamp: 2026-09-09
    actor: Mihai Roșca
    action: Reconciled BRIDGRAI numbers to current state
    rationale: 128 IP assets (manifest v2.1), 175 UKBE tests (verified), 14 test files
blockchain_anchors:
  kce_founding_document:
    tezos_tx: oneEyqTWeVBBtcS5SzzLboUsRrrqCXJX8nuQxLCURzZZSA6gBBC
    sha256: 92d989727fda66fe72205b950faa756435be2d90943546e8bfa57726457947d3
  kce_papers_manifest_v2_1:
    tezos_tx: ooeVNb1qYNLSJK4voVy7FtDvoNqDfj4TjQVATg5NhRT6QQz3j54
validation_status: E2 (Reproducible — public repositories, verifiable blockchain)
evidence_target: E4 (pending Zenodo DOI + peer review)
```
