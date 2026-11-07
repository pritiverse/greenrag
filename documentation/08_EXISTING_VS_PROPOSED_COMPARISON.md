# 08. Technical Comparison: Conventional Approaches vs. GreenRAG-Sec

**Auditor:** Senior Software Architect, R&D Technical Auditor & Algorithm Analyst  
**Document Target:** Empirical, Feature-by-Feature Benchmark Comparison  
**Date:** November 3, 2026  

---

## 1. Comparative Architecture Overview

To provide defensible evidence of technical advance (as required by Section 2(1)(ja) of the Indian Patents Act and 35 U.S.C. § 103), this document contrasts conventional engineering approaches against the implementations in `GreenRAG-Sec`. Every metric reported is drawn directly from executed experimental runs in `results/`.

```
┌────────────────────────────────────────────────────────────────────────┐
│                     SYSTEM COMPARISON MATRIX                           │
├──────────────────────────────┬─────────────────────────────────────────┤
│ Conventional Approaches      │ GreenRAG-Sec Proposed Implementation    │
├──────────────────────────────┼─────────────────────────────────────────┤
│ • Unconstrained LLM output   │ • Sandboxed advisory with Trust Engine  │
│ • Self-declared chunk hashes │ • Offline HMAC-SHA256 Build Registry    │
│ • Point power forecasting    │ • CQR Mondrian group-calibrated bounds  │
│ • Single vector retrieval    │ • Hybrid RRF + Cross-Encoder Reranking  │
│ • Point anomaly heuristics   │ • Causal residuals + Mahalanobis+Replay │
└──────────────────────────────┴─────────────────────────────────────────┘
```

---

## 2. Feature-by-Feature Technical Comparison Tables

### Table 1: Security & Document Provenance in RAG Systems (Candidate B)

| Feature / Dimension | Conventional Approach | GreenRAG-Sec Implementation | Technical Difference | Empirical Evidence in Repository | Primary Technical Advantage |
|---|---|---|---|---|---|
| **Document Integrity** | Self-declared metadata hashes stored alongside chunk in vector DB. | Offline HMAC-SHA256 signed build manifest (`GREENRAG_TRUST_KEY`). | Detaches trust authority from database storage layer. | Blocked **17/17** tampered documents; self-hash caught **0/17** (`results/security/c3_summary.md`). | **100% Tampering Detection** (vs. 0% baseline). |
| **Chunk Authenticity** | Assumed authentic if returned by nearest-neighbor index. | Exact normalized substring verification (`chunk_in_doc`). | Enforces that chunk text strictly exists in signed parent document. | Prevents out-of-band chunk injection without parent document context. | **Guaranteed Text Provenance**. |
| **Indirect Prompt Injection** | Vector DB indexes raw text; LLM receives injection in context window. | Multi-tier regex & semantic screener + HMAC trust filter. | Detects roleplay escapes, override directives, and delimiters before LLM prompt generation. | Dropped injection obedience from **37/66** (undefended) to **0/66** (`results/security/c6_summary.md`). | **Zero Injection Obedience** (Complete mitigation). |
| **Keyword Stuffing Defense** | No frequency checks; attacker stuffs terms to game BM25/Dense score. | Statistical content-word share threshold ($1.5 \times \text{clean max}$). | Identifies abnormal word frequency distributions in candidate documents. | Caught synthetic keyword stuffing documents with 0 false positives on clean KB (`c3_summary.md`). | **Robustness against Search Index Gaming**. |
| **System Overhead** | N/A (Baseline) | Cryptographic interceptor & verification filters. | Executed in-process using optimized hash comparison. | Added only **7.7 ms** median latency; faithfulness remained identical at **0.970 vs 0.970** (`c4_summary.md`). | **Negligible Latency Penalty (< 10 ms)**. |

---

### Table 2: AI Safety Arbitration & Decision Control (Candidate A)

| Feature / Dimension | Conventional Approach | GreenRAG-Sec Implementation | Technical Difference | Empirical Evidence in Repository | Primary Technical Advantage |
|---|---|---|---|---|---|
| **Action Execution** | Direct autonomous execution of LLM recommendations. | LLM is strictly an advisor; actions gated by Decision Engine ($T$). | Generative model cannot trigger physical actuator commands without trust gating. | 110 of 110 uncalibrated diagnoses forced to `REVIEW` (`results/trust/c5_summary.md`). | **Elimination of Catastrophic Automation Errors**. |
| **Confidence Scoring** | Self-assessed softmax probabilities or verbal confidence ("*I am 95% sure*"). | Statistical lower confidence bound anchoring ($C = \max(0, \frac{\text{ci\_low} - 0.5}{0.5})$). | Mathematical penalty anchors confidence to empirical validation lower bound; chance AUC collapses $C \to 0$. | B4 detector AUC CI $[0.500, 0.950] \implies C = 0.000$, blocking unsafe automation (`decision.py#L201`). | **Calibrated, Verifiable Confidence**. |
| **Security Metric** | Binary pass/fail heuristic. | Negative likelihood ratio score ($S = 1 - \frac{1 - \text{TPR}}{1 - \text{FPR}}$). | Quantifies how much a passing check actually lowers posterior attack probability based on test data. | Evaluated across all 4 detector tiers; unverified checks default $S \to 0$ (`c5_summary.md`). | **Theoretically Grounded Security Strength**. |
| **Safety Directives** | General prompt instructions ("*Be safe and do not give bad advice*"). | Deterministic regex engine with negative lookbehind. | Detects safety bypasses, alarm overrides, and deferrals while allowing negative advice ("*never bridge*"). | Blocked 100% of safety-bypass attacks with **0 false alarms** on 154 clean outputs (`c5_summary.md`). | **Zero Safety-Directive Hallucinations**. |
| **Weight Robustness** | Fixed heuristic weights without sensitivity validation. | 969-point systematic weight grid sweep on 0.05 step. | Analyzes decision stability across all non-negative convex combinations summing to 1. | 96.7% mean decision agreement across the entire 969-point weight grid (`results/trust/c5_summary.md`). | **Decision Stability & Low Parameter Sensitivity**. |

---

### Table 3: SCADA Telemetry Anomaly & Replay Detection (Candidate C)

| Feature / Dimension | Conventional Approach | GreenRAG-Sec Implementation | Technical Difference | Empirical Evidence in Repository | Primary Technical Advantage |
|---|---|---|---|---|---|
| **Replay Attack Defense** | Individual point anomaly checks (Isolation Forest, SVM). | Pairwise daily standardized Euclidean profile distance ($\epsilon_{\text{replay}}$). | Evaluates full-day multi-sensor trajectory against all historical records. | Blocked **31/31** wind replay attacks and **10/10** solar replay attacks (`results/security/c6_summary.md`). | **100% Detection of Perfect Historical Replays**. |
| **Sensor Cross-Consistency** | Independent sensor limits (e.g. $0 \le AC \le 2000$). | Physics-derived residual regressions ($AC \propto Irr$; $T_{\text{mod}} \propto T_{\text{amb}} + Irr$). | Cross-validates electrical power against solar irradiance and thermal balance. | Flagged solar irradiation bias and module temperature spikes (`results/security/c6_summary.md`). | **Physics-Grounded Anomaly Detection**. |
| **Forecast Error Impact** | Unchecked attacked data causes severe forecast distortion. | Telemetry gate suppresses corrupted sensor stream before preprocessing. | Attacked telemetry days receive $S = 0 \implies \text{REJECT}$. | Prevented a **$+1874.2\text{ kW}$** daylight MAE forecasting error on solar replay days (`c6_summary.md`). | **Significant Operational Error Reduction**. |

---

### Table 4: Power Generation Forecasting Uncertainty (Adapted Method)

| Feature / Dimension | Conventional Approach | GreenRAG-Sec Implementation | Technical Difference | Empirical Evidence in Repository | Primary Technical Advantage |
|---|---|---|---|---|---|
| **Point Forecasting** | Naive Persistence ($\hat{y}_{t+h} = y_t$). | Causal feature engineering (26 lags) + Random Forest / XGBoost. | Captures diurnal cycles, rolling statistics, and weather cross-lags. | RF beat Persistence on **12/12 test days** on Solar Plant 1 (MAE: **1444.1 kW vs 2458.0 kW**, a 1039.7 kW drop). | **41.2% Reduction in Solar Point Error**. |
| **Prediction Intervals** | Uncalibrated Quantile Regression or Gaussian $\pm 1.96\sigma$. | Conformal Quantile Regression (CQR) with Mondrian Day/Night grouping. | Adjusts quantile boundaries via finite-sample non-conformity scores grouped by solar regime. | Achieved **0.893** empirical coverage on Plant 1 and **0.906** on Plant 2 for 90% nominal target (`a4_summary.md`). | **Guaranteed Finite-Sample Coverage**. |

---

## 3. Summary of Core Technical Advantages

```
┌────────────────────────────────────────────────────────────────────────┐
│                  MEASURED TECHNICAL PERFORMANCE GAINS                  │
├────────────────────────────────────────────────────────────────────────┤
│ 1. Security Robustness:                                                │
│    - Prompt injection obedience: 56.1% (undefended) -> 0.0% (GreenRAG) │
│    - Knowledge base poisoning detection: 0% (self-hash) -> 100% (HMAC) │
│    - Telemetry replay detection: 0% (baseline) -> 100% (GreenRAG)      │
│                                                                        │
│ 2. AI Factuality & Reliability:                                        │
│    - Faithfulness: 0.836 (Basic RAG) -> 0.977 (GreenRAG Hybrid)        │
│    - Hallucination rate: 0.688 (LLM alone) -> 0.016 (GreenRAG)        │
│                                                                        │
│ 3. Operational Grid Safety:                                            │
│    - Catastrophic automated decisions: 0 accepted of 110 trials        │
│    - Computational overhead added by security perimeter: 7.7 ms median │
└────────────────────────────────────────────────────────────────────────┘
```

