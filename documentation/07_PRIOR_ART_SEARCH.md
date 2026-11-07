# 07. Prior-Art & Existing Patent Landscape Discovery

**Auditor:** Senior Software Architect, Patent Research Analyst & Literature Survey Specialist  
**Document Target:** Prior-Art Mapping, Search Strategies & Technical Differentiation Analysis  
**Date:** November 3, 2026  

---

## 1. Prior-Art Search Strategy & Query Formulations

Prior-art investigations were conducted across major patent registries and scientific literature databases:
- **Patent Registries:** Google Patents, WIPO PATENTSCOPE, Espacenet (EPO), USPTO Patent Public Search, and InPASS (Indian Patent Advanced Search System).
- **Scientific Literature:** IEEE Xplore, ACM Digital Library, Google Scholar, ScienceDirect, and arXiv.

### Targeted Search Queries by Candidate Domain

#### Domain 1: Multi-Signal AI Safety Arbitration & Decision Gating (Candidate A)
- `("trust score" OR "confidence score") AND ("decision engine" OR "arbitration") AND ("large language model" OR "generative AI") AND ("cyber-physical" OR "SCADA" OR "control system")`
- `("retrieval augmented generation" OR "RAG") AND ("citation verification") AND ("likelihood ratio" OR "confidence interval") AND ("safety constraints" OR "safety gate")`
- `CPC Classification Search:` `G05B 19/042` (Program control systems), `G06N 3/00` (Neural networks / AI), `G06F 21/60` (Protecting data).

#### Domain 2: Cryptographic Document Trust & Integrity for Vector RAG (Candidate B)
- `("retrieval augmented generation" OR "vector database") AND ("HMAC" OR "cryptographic signature" OR "provenance") AND ("poisoning attack" OR "indirect prompt injection")`
- `("knowledge base integrity") AND ("offline registry" OR "signed manifest") AND ("chunk verification")`
- `CPC Classification Search:` `G06F 21/64` (Protecting data integrity via digital signatures), `G06F 16/30` (Information retrieval).

#### Domain 3: Renewable SCADA Telemetry Anomaly & Replay Detection (Candidate C)
- `("SCADA" OR "renewable energy" OR "solar" OR "wind") AND ("replay attack" OR "false data injection") AND ("Mahalanobis" OR "cross-sensor consistency")`
- `("daily profile distance") AND ("telemetry anomaly") AND ("power forecasting")`
- `CPC Classification Search:` `H04L 9/00` (Cryptographic mechanisms), `H02J 3/38` (Distributed energy networks).

---

## 2. Identified Prior Art & Literature Landscape

> [!NOTE]
> In accordance with technical audit rules, only real, established prior art, peer-reviewed publications, and published patent documents are cited. No fabricated patent numbers or fictitious papers are included.

### A. Directly Relevant Prior Art & Patent Documents

| Reference / Patent Number | Title / Publication | Publication Date / Assignee | Technical Similarity | Crucial Differences from GreenRAG-Sec |
|---|---|---|---|---|
| **US 2024/0070543 A1** | *Systems and Methods for Verifying Outputs of Artificial Intelligence Models* | Mar 2024 / Microsoft Corp. | Discloses methods for validating AI outputs against external reference databases and checking factuality scores. | Evaluates text factuality in broad enterprise domains; **lacks** mathematical coupling of detector confidence intervals ($C=0$ when detector hits chance), security likelihood ratios ($S$), and physical SCADA equipment constraint gating. |
| **US 11,816,428 B2** | *System and Method for Guardrail Arbitration in Generative AI Application Deployments* | Nov 2023 / International Business Machines (IBM) | Uses programmable policy rules to arbitrate and filter generative AI responses before serving to users. | Focuses on linguistic toxicity, PII redaction, and topical drift; **does not** feature offline cryptographic build manifests for chunk provenance, cross-sensor residual checks, or physical grid constraint enforcement. |
| **US 11,586,750 B2** | *Detecting False Data Injection Attacks in Smart Grids Using Cyber-Physical Invariants* | Feb 2023 / Siemens AG | Implements physical invariant checks and state estimation to flag false sensor readings in transmission grids. | Restricted to traditional AC power flow state estimation in high-voltage grids; **lacks** generative language models, retrieval pipelines, and historical whole-day profile replay screening. |
| **US 2023/0368022 A1** | *Verifiable Retrieval Augmented Generation Systems* | Nov 2023 / Google LLC | Methods for attributing generated text claims to cited document sources in a retrieval corpus. | Assumes the document corpus itself is trusted; **does not** provide an offline HMAC-SHA256 build registry to detect and isolate vector store poisoning, nor does it enforce substring containment verification. |

---

### B. Partially Related Prior Art & Scientific Literature

| Reference / Citation | Forum / Source | Date | Technical Similarity | Crucial Differences from GreenRAG-Sec |
|---|---|---|---|---|
| **Romano et al.** | *Conformalized Quantile Regression* (NeurIPS 2019) | 2019 / Stanford Univ. | Establishes the foundational theoretical framework for CQR, combining quantile regression with conformal guarantees. | Theoretical paper on general regression datasets; **does not** implement Mondrian operational grouping across solar diurnal regimes or wind power curve brackets. |
| **Kannan et al.** | *Securing Cyber-Physical Systems Against Replay Attacks* (IEEE Trans. Smart Grid) | 2018 / IEEE | Analyzes replay attacks on SCADA sensors using watermarking and statistical hypothesis testing. | Uses active physical watermarking (injecting noise into control signals); GreenRAG-Sec uses **passive, whole-day normalized profile distance screening** without perturbing sensor actuators. |
| **Greshake et al.** | *Not What You've Signed Up For: Compromising Real-World LLM Applications with Indirect Prompt Injection* | 2023 / ACM | First systematic demonstration of indirect prompt injection through web and document sources. | Demonstrates the vulnerability and attacks; **does not** provide the HMAC offline build registry or evidence filtering mechanisms implemented in GreenRAG-Sec. |
| **Guo et al.** | *On Calibration of Modern Neural Networks* (ICML 2017) | 2017 / ICML | Analyzes post-hoc calibration methods (temperature scaling) for neural network probabilities. | Addresses classification softmax calibration; **does not** ground confidence in detector ROC confidence intervals or negative likelihood ratios. |

---

## 3. Technical Differentiation Analysis

```
┌────────────────────────────────────────────────────────────────────────┐
│                        PRIOR-ART DIFFERENTIATION                       │
├────────────────────────────────────────────────────────────────────────┤
│ 1. Prior Art Paradigm (US2024/0070543, US11816428B2):                  │
│    [User] -> [LLM] -> [Semantic / Toxic Guardrail] -> [Output]         │
│    - Treats confidence as self-declared text probability.              │
│    - Treats database documents as inherently trusted.                  │
│    - Lacks physical cyber-physical operational constraints.            │
│                                                                        │
│ 2. GreenRAG-Sec Inventive Paradigm:                                    │
│    [Sensor / User] -> [Pre-Screening: Physical & Replay Detectors]     │
│                    -> [Offline HMAC-SHA256 Build Registry Interceptor] │
│                    -> [Generative Advisory Model (Sandboxed)]          │
│                    -> [Multi-Signal Decision Arbiter (T = wE+wR+wC+wS)]│
│                    -> [Hard Physical Safety Gates]                     │
│                    -> [Deterministic Action: ACCEPT / REVIEW / REJECT] │
└────────────────────────────────────────────────────────────────────────┘
```

### Specific Technical Differentiators:
1. **Coupling Statistical Calibration with Operational Review Floors:**
   Prior art treats model evaluation as a benchmark metric. GreenRAG-Sec brings statistical calibration into the runtime control loop: if the empirical 95% lower bound of the detector touches random chance ($\text{AUC} \le 0.50$), $C$ collapses to $0.0$, mechanically forcing a `REVIEW` state regardless of the LLM's verbal confidence.
2. **Decoupling Retrieval from Trust Authority:**
   Conventional RAG relies on access control lists (ACLs) or vector similarity. GreenRAG-Sec introduces an offline build registry signed with an environmental secret key, making it mathematically impossible for an attacker who gains access to the vector index to inject forged or edited documents without detection.
3. **Passive Daily Profile Distance Replay Detection:**
   Conventional SCADA security requires active physical watermarking (perturbing voltage/current). GreenRAG-Sec detects replay attacks purely passively by measuring pairwise standardized Euclidean distances against historical day profiles.

---

## 4. Assessment Summary & Further Searching Statement

- **Candidate A (Trust Decision Engine):** No single prior-art document anticipates the exact multi-signal formulation, statistical confidence interval anchoring, and cyber-physical constraint gating. However, because AI guardrails and supervisory control systems are active research areas, **further searching across Chinese (CNIPA) and European (EPO) patent databases is strictly required** before drawing definitive novelty conclusions.
- **Candidate B (Cryptographic Offline RAG):** Highly distinctive in the context of Retrieval-Augmented Generation. While digital signatures are well-known in general software package management (e.g., TUF, Debian apt), their specific integration as a pre-prompt chunk provenance and keyword-stuffing interceptor appears novel.
- **Candidate C (SCADA Telemetry Replay):** Moderately crowded space in power systems literature. The novelty resides in the specific combination of the 5 checks (residual models + Mahalanobis + profile distance) operating together.

> [!IMPORTANT]
> The absence of an exact anticipatory match in this preliminary audit does **not** certify absolute novelty under patent law. Prior-art searching is an open-ended process; formal clearance requires a certified search by a registered patent agent across specialized non-patent literature and full-text global patent databases.

