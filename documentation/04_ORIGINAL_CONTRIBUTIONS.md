# 04. Analysis of Original Technical Contributions

**Auditor:** Senior Software Architect, Research Scientist & Invention Discovery Analyst  
**Document Target:** Rigorous Demarcation of Original Contributions vs. Standard Technology Combinations  
**Date:** November 3, 2026  

---

## 1. Invention Discovery Methodology

To evaluate potential intellectual property and patentability defensibly, we must distinguish between:
1. **Standard Engineering Assembly:** Connecting standard third-party libraries (e.g., placing ChromaDB behind LangChain and calling XGBoost) does not constitute a patentable invention, regardless of architectural complexity.
2. **Domain-Specific Adaptation:** Customizing known algorithms to a specific problem without altering underlying mathematical or computational properties.
3. **Genuine Technical Contributions:** Novel mechanisms, mathematical formulations, security gating architectures, or data processing pipelines that solve an established technical problem and produce a measurable technical effect.

Below is the systematic audit of original contributions discovered in `GreenRAG-Sec`.

---

## 2. Deep Dive into Candidate Technical Contributions

### Contribution 1: Multi-Signal Trust-Aware Arbitration Architecture with Statistical Confidence Anchoring and Safety-Constrained Gating

#### 1. What Exactly is the Contribution?
An autonomous software decision framework that intercepts generative LLM advisory outputs and machine learning telemetry predictions before execution, evaluating them against four mathematically coupled, orthogonal signal channels:
$$T = w_E E + w_R R + w_C C + w_S S$$
combined with:
- Anchoring model confidence $C$ to empirical statistical lower bounds (penalizing detectors at chance AUC $0.5$ to $C=0.0$).
- Formulating active security confidence $S$ as a function of the negative likelihood ratio of empirical detector error rates ($1 - \frac{1 - \text{TPR}}{1 - \text{FPR}}$).
- Enforcing hard regular-expression negative-lookbehind operational safety constraints to prevent hazardous physical actuation directives.

#### 2. What Technical Problem Does It Solve?
Industrial supervisory systems (such as power grid energy management systems) cannot tolerate unconstrained, hallucinated, or adversarial AI actions. Standard LLM guardrails output binary flags based on prompt text alone, lacking awareness of underlying model confidence, citation validity, or physical operational envelopes.

#### 3. What is the Existing Approach?
- Guardrails AI / NeMo Guardrails: Scans generated text against semantic toxicity or topical prompt rules.
- Confidence heuristics: Uses model softmax probabilities or raw retrieval similarity scores, which are notoriously uncalibrated and prone to overconfidence on out-of-distribution inputs.

#### 4. What is Technically Different?
Instead of trusting the model's internal self-assessed probability, the engine evaluates the recommendation externally using:
1. Grounded empirical test bounds (e.g., B4 detector ROC AUC 95% confidence interval lower bound).
2. Physical domain constraints (kW power capacity, physical asset temperature ranges).
3. Hard grammatical safety boundaries with negation detection.
4. Independent verification of cryptographic chunk provenance.

#### 5. Why is this Inventive & Non-Obvious?
Conventional AI engineering assumes that if an LLM is given high-quality retrieved text, its output is safe. The GreenRAG-Sec architecture recognizes that **an anomaly detector with an AUC lower bound of 0.500 is indistinguishable from random chance**, regardless of how convincing the generated text sounds. By mathematically mapping $\text{ci\_low} = 0.500 \implies C = 0.000$, the engine automatically locks the output into a mandatory `REVIEW` state, completely neutralizing the automation hazard. This coupling between statistical test theory, cryptographic provenance, and automated physical gating is non-obvious to an ordinary practitioner in natural language processing.

#### 6. Measurable Technical Benefit
- Verified on 374 real execution cases (`results/trust/c5_summary.md`).
- **Zero** erroneous automated diagnoses accepted ($0/110$ reached `ACCEPT`).
- Prevented 100% of physical safety bypass directives from being approved.

---

### Contribution 2: Offline HMAC-SHA256 Build Registry Verification and Substring Provenance Architecture for RAG

#### 1. What Exactly is the Contribution?
A cryptographic and structural provenance architecture for Retrieval-Augmented Generation that intercepts vector database hits prior to context injection, validating them against an immutable, offline-compiled, HMAC-SHA256 signed build manifest (`trust_registry.json`) and enforcing string containment and keyword-stuffing limits.

#### 2. What Technical Problem Does It Solve?
Retrieval-Augmented Generation systems are highly vulnerable to **Knowledge Base Ingestion Poisoning** and **Indirect Prompt Injection**. Adversaries with write or append access to document stores can insert forged maintenance orders, tampered manuals, or instruction-override payloads.

#### 3. What is the Existing Approach?
Standard RAG frameworks (LangChain, LlamaIndex, Haystack) store raw text and self-declared metadata hashes directly in the vector database. If an adversary modifies a document, they simply compute and store a valid hash alongside the modified text.

#### 4. What is Technically Different?
GreenRAG-Sec decouples trust from the vector database:
1. The vector index contains embeddings and text, but **zero trust authority**.
2. Trust authority resides exclusively in an offline registry signed with a secret environment key (`GREENRAG_TRUST_KEY`) unknown to the document ingestion pipeline.
3. Chunks must satisfy substring containment within the verified parent document (`chunk_in_doc`).
4. Real equipment fault records must quote original historical logbook text verbatim (`provenance`).
5. Content words must pass statistical frequency thresholds to prevent semantic keyword stuffing.

#### 5. Why is this Inventive & Non-Obvious?
Conventional wisdom in vector search assumes that database integrity is handled by database access controls. GreenRAG-Sec recognizes that in enterprise environments, document ingestion is asynchronous and distributed across third-party vendors, OEM portals, and field technicians. By treating the vector database as an untrusted cache and enforcing offline HMAC-SHA256 build verification at the retrieval interceptor layer, the system guarantees that no poisoned chunk can ever enter the generative model's prompt.

#### 6. Measurable Technical Benefit
- Evaluated against 17 synthetic malicious injections and in-place document edits (`results/security/c3_summary.md`).
- Self-declared hashing caught **0/17** tampered documents (0% defense).
- The HMAC-signed registry caught **17/17** tampered documents (100% defense) with 0 false flags on 189 clean documents.

---

### Contribution 3: Multi-Stage Causal Telemetry Anomaly & Historical Daily Replay Detection for Renewable SCADA

#### 1. What Exactly is the Contribution?
A 5-stage pre-cleaning SCADA telemetry screening algorithm combining:
1. Physics-derived cross-sensor efficiency and thermal regression residuals.
2. Multivariate Mahalanobis distance with pseudo-inverse covariance matrices fitted exclusively on clean chronological training days.
3. Whole-day standardized profile distance metrics detecting historical replay attacks.

#### 2. What Technical Problem Does It Solve?
False Data Injection (FDI) and sensor replay attacks in renewable power plants deceive forecasting and dispatch models. Replaying an identical sunny day from last month bypasses conventional point anomaly detectors because every single reading falls within valid physical ranges.

#### 3. What is Technically Different?
Unlike standard point anomaly detectors (Isolation Forest, One-Class SVM) that look at isolated 15-minute vectors, GreenRAG-Sec:
- Compares inter-sensor physics: AC power generation must be causally proportional to solar irradiance ($AC \propto Irr$), and module temperature must obey heat dissipation regression ($T_{\text{mod}} \approx \beta_0 + \beta_1 T_{\text{amb}} + \beta_2 Irr$).
- Evaluates the whole-day profile pairwise distance against historical days, identifying exact or near-exact replay trajectories.

#### 4. Measurable Technical Benefit
- Evaluated on 30 solar attack days and 62 wind attack days (`results/security/c6_summary.md`).
- Blocked **100%** of replay attacks (31/31 wind, 10/10 solar).
- Eliminated a $+1874.2\text{ kW}$ forecasting distortion on attacked days.

---

## 3. Summary Matrix of Discovered Contributions

| Contribution Ref | System Component | Inventive Level | Primary Technical Effect | Standalone Viability |
|---|---|---|---|---|
| **TC-1** | Multi-Signal Trust Decision Engine | Novel System Architecture & Control Mechanism | Prevents unsafe or uncalibrated autonomous execution of AI recommendations in physical grids. | **High** (Can be integrated with any industrial cyber-physical AI pipeline). |
| **TC-2** | Offline HMAC Build Registry & Evidence Filter | Novel Security & Cryptographic RAG Pipeline | Eliminates vector database poisoning and indirect prompt injection before prompt synthesis. | **High** (Applicable to any enterprise or mission-critical RAG system). |
| **TC-3** | Multi-Stage SCADA Telemetry & Replay Detector | Novel Sensor Verification Algorithm Combination | Detects sophisticated FDI and replay attacks in renewable SCADA streams before ML preprocessing. | **Medium** (Specialized to renewable energy SCADA infrastructure). |
| **TC-4** | Mondrian Group-Calibrated CQR Forecaster | Domain Optimization of Known Technique | Guarantees finite-sample prediction interval coverage across diurnal solar ramps. | **Low** (Theoretical framework is established; contribution lies in empirical domain grouping). |
| **TC-5** | Hybrid BM25/Dense RRF + Reranker | Engineering Integration | Improves retrieval recall across error codes and descriptive text. | **None** (Standard retrieval best practice). |

