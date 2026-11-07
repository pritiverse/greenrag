# 06. Patentability Preliminary Assessment & Statutory Eligibility Analysis

**Auditor:** Senior Software Architect, Patent Research Analyst & Algorithm Analyst  
**Document Target:** Legal & Technical Patentability Screening under the Indian Patents Act, 1970 and USPTO 35 U.S.C.  
**Date:** November 3, 2026  

---

## 1. Legal Frameworks Applied

### Indian Patents Act, 1970 (as amended)
1. **Section 2(1)(j):** *Novelty* — The invention must not be anticipated by publication in any document or used in the country or elsewhere in the world before the date of filing.
2. **Section 2(1)(ja):** *Inventive Step* — A feature of an invention that involves technical advance as compared to existing knowledge or having economic significance or both and that makes the invention not obvious to a person skilled in the art.
3. **Section 2(1)(ac):** *Industrial Applicability* — The invention is capable of being made or used in an industry.
4. **Section 3(k):** *Statutory Exclusion* — Prohibits patents for:
   > *"a mathematical method or a business method or a computer programme per se or algorithms"*
   - **IPO Guidelines for Examination of Computer Related Inventions (CRI, 2017):** Clarifies that computer-implemented inventions are patentable if they produce a **technical effect** or provide a **technical solution to a technical problem**.
   - **Judicial Precedents:** *Ferid Allani v. Union of India (Delhi High Court, 2019)* and *Microsoft Technology Licensing v. Assistant Controller of Patents (Delhi High Court, 2023)* affirm that inventions with technical effect, improved device functioning, or interaction with external physical systems transcend the Section 3(k) bar.

### United States Patent Law (35 U.S.C.)
1. **35 U.S.C. § 101 (Alice/Mayo Framework):**
   - *Step 2A (Prong 1):* Does the claim recite an abstract idea (mathematical calculation, method of organizing human activity)?
   - *Step 2A (Prong 2):* If so, is the abstract idea integrated into a practical application?
   - *Step 2B:* Does the claim recite an "inventive concept" (significantly more than routine, conventional activity)?
2. **35 U.S.C. §§ 102 & 103:** Novelty and Non-Obviousness against prior art.

---

## 2. Preliminary Evaluation of Candidate Inventions

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        PATENTABILITY EVALUATION SCORECARD                              │
├────────────────────────────┬─────────────┬──────────────┬──────────────┬───────────────┤
│ Evaluation Criterion       │ Candidate A │ Candidate B  │ Candidate C  │ Candidate D   │
│                            │ Trust Engine│ Cryptographic│ SCADA Replay │ Integrated    │
│                            │ (decision.py) RAG (doc_trust) (scada_anomaly) System     │
├────────────────────────────┼─────────────┼──────────────┼──────────────┼───────────────┤
│ 1. Novelty (Sec. 2(1)(j))  │ HIGH        │ HIGH         │ MODERATE     │ VERY HIGH     │
│ 2. Inventive Step (2(1)(ja)│ HIGH        │ HIGH         │ MODERATE     │ HIGH          │
│ 3. Industrial Utility      │ VERY HIGH   │ VERY HIGH    │ VERY HIGH    │ VERY HIGH     │
│ 4. Technical Contribution  │ VERY HIGH   │ HIGH         │ HIGH         │ VERY HIGH     │
│ 5. Sec. 3(k) Overcome-Risk │ MODERATE    │ LOW          │ LOW          │ LOW           │
│ 6. US Alice § 101 Viability│ STRONG      │ VERY STRONG  │ VERY STRONG  │ VERY STRONG   │
└────────────────────────────┴─────────────┴──────────────┴──────────────┴───────────────┘
```

---

## 3. Deep Evaluation: Candidate A (Trust-Aware Decision Engine)

### 1. Novelty Assessment (Section 2(1)(j) / 35 U.S.C. § 102)
- **Status:** **Potentially Novel**
- **Analysis:** Existing AI safety arbiters (e.g., Guardrails AI, NeMo Guardrails, Guidance) evaluate generation outputs against linguistic rules, toxicity filters, or semantic relevance. No publicly identified prior art synthesizes:
  1. Statistical lower confidence bound anchoring ($C = 0$ when detector touches chance).
  2. Active negative likelihood ratio security strength ($S = 1 - \frac{1 - \text{TPR}}{1 - \text{FPR}}$).
  3. Grammatical negative-lookbehind safety directive gating.
  4. Cryptographic citation verification.
  into a deterministic three-tier execution state (`ACCEPT`, `REVIEW`, `REJECT`) specifically designed to gate physical cyber-physical actuators.

### 2. Inventive Step / Non-Obviousness (Section 2(1)(ja) / 35 U.S.C. § 103)
- **Status:** **Strong Argument for Non-Obviousness**
- **Analysis:** A Person Skilled In The Art (PHOSITA) in machine learning would typically attempt to improve decision accuracy by retraining the generator or fine-tuning an end-to-end classification head. Decoupling the arbitration into an independent, external mathematical gate that explicitly treats the LLM as an uncalibrated, untrusted advisor and applies statistical calibration theory to override model outputs is contrary to conventional end-to-end AI paradigms.

### 3. Industrial Applicability (Section 2(1)(ac))
- **Status:** **Fully Satisfied**
- **Analysis:** Capable of direct industrial deployment in supervisory control of electrical wind farms, photovoltaic power plants, microgrid energy management systems (EMS), and industrial manufacturing automation.

### 4. Technical Contribution & Section 3(k) Eligibility Strategy
- **Risk Analysis under Section 3(k):**
  - *Vulnerability:* If drafted merely as a method for calculating a score $T = w_E E + w_R R + w_C C + w_S S$, the Indian Patent Office (IPO) will object under Section 3(k) as an abstract mathematical method or algorithm.
- **Overcoming Section 3(k) (CRI Guidelines Strategy):**
  - **Drafting Mandate:** Claims must NOT be drafted as pure data-processing algorithms. Claims MUST be drafted as a **computer-implemented control system for an electrical power generation facility**, where the trust engine generates a physical electrical control signal:
    > *"A system for controlling an electric power generation asset, comprising a sensor interface, a memory storing a verified knowledge base, and an arbitration processor configured to: ... generate an electrical control command to physically adjust an actuator of the power generation asset when the composite trust metric exceeds a threshold and all physical operational constraints are satisfied, and to inhibit actuation and transmit an alert to an operator terminal when the metric falls below a review floor."*
  - **Demonstrable Technical Effect:** Prevention of physical equipment destruction (e.g., avoiding turbine runaway or transformer burnout caused by hallucinated breaker closures) constitutes a patent-eligible technical effect under *Ferid Allani* and *Microsoft*.

---

## 4. Deep Evaluation: Candidate B (Cryptographic Offline RAG Build Registry)

### 1. Novelty Assessment (Section 2(1)(j) / 35 U.S.C. § 102)
- **Status:** **Potentially Novel**
- **Analysis:** While HMAC-SHA256 and digital signatures are known in standard software security, their specific application as an **offline-signed build manifest interceptor** operating within a multi-retriever RAG pipeline (validating chunk-in-document substring containment, keyword-stuffing share limits, and real event log provenance quoting) is distinct from existing vector database security solutions.

### 2. Inventive Step / Non-Obviousness (Section 2(1)(ja) / 35 U.S.C. § 103)
- **Status:** **High**
- **Analysis:** Existing vector databases (Pinecone, ChromaDB, Weaviate) treat document metadata as self-declared. The insight that self-declared metadata hashes provide zero security against in-place poisoning (demonstrated empirically: 0/17 detected by self-hash vs. 17/17 by offline HMAC) and the resulting architectural separation of the retrieval store from the cryptographic trust authority represents an inventive advance.

### 3. Technical Contribution & Section 3(k) Eligibility Strategy
- **Section 3(k) Risk:** **Low**
- **Analysis:** In cryptographic and cybersecurity patents, technical problems (unauthorized data tampering, injection attacks) are solved by technical means (cryptographic hash verification, memory-safe interceptors). Under Indian patent practice, data security mechanisms that prevent unauthorized system tampering generally survive Section 3(k) scrutiny.
- **US 35 U.S.C. § 101:** Highly favorable under Alice Step 2A (Prong 2) because it improves the functioning of a computer data retrieval system by enforcing data integrity.

---

## 5. Summary Evaluation Matrix

| Invention Candidate | Novelty | Inventive Step | Industrial Utility | Section 3(k) Hurdle | Overall Patentability Prospect |
|---|---|---|---|---|---|
| **Candidate A (Trust Engine)** | High | High | High | Moderate (Requires cyber-physical claim framing) | **Strong (High Priority)** |
| **Candidate B (Cryptographic RAG)** | High | High | High | Low (Security / Data integrity mechanism) | **Strong (Medium-High Priority)** |
| **Candidate C (SCADA Telemetry Replay)** | Moderate | Moderate | High | Low (Sensor data processing / Signal verification) | **Moderate (Medium Priority)** |
| **Candidate D (Integrated System)** | Very High | Very High | High | Low (Produces physical grid control effect) | **Very Strong (High Priority)** |

---

## 6. Crucial Legal Disclaimer

> [!IMPORTANT]
> This preliminary patentability assessment represents a rigorous technical and R&D evaluation conducted from an engineering and architecture perspective. It does **not** constitute a formal legal opinion from a licensed patent attorney or registered patent agent. Formal patentability filings require comprehensive clearance searches across official patent registries (InPASS, USPTO, EPO, WIPO) and professional patent claim drafting tailored to jurisdictional statutory requirements.

