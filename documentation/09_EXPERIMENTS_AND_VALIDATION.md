# 09. Experimental Validation, Evidence Inventory & Recommended Verification Protocols

**Auditor:** Senior Software Architect, Research Scientist & Experimental Verification Lead  
**Document Target:** Empirical Evidence Inventory and Testing Roadmap for Patent Applications  
**Date:** November 3, 2026  

---

## 1. Existing Empirical Evidence in Repository

The `GreenRAG-Sec` repository maintains a comprehensive experimental verification framework. All results are persisted in structured CSV, JSON, and Markdown files under `results/`. Below is the complete catalog of existing empirical evidence.

### A. Track C: Security, Trust & Decision Gating Evidence (`results/security/`, `results/trust/`)

```
c:\Users\priti\Desktop\greenrag_cloud_project\GreenRAG-Sec\results\
├── security\
│   ├── c2_manifest.json          # Complete synthetic attack catalogue (telemetry, poisoning, injection)
│   ├── c2_forecast.csv           # 39 (asset, attack, seed) runs measuring forecast error under attacks
│   ├── c2_knowledge.csv          # Evaluation of undefended RAG under 66 injection and poisoning attacks
│   ├── c3_summary.md             # Baseline detector evaluation (TPR, FPR, likelihood ratios)
│   ├── c4_summary.md             # Secure pipeline evaluation; proves 7.7 ms median overhead
│   └── c6_summary.md             # Deployed qwen2.5:7b-instruct evaluation (330 capture, 990 held-out trials)
└── trust\
    ├── c5_decisions.csv          # Decision engine outputs for 374 capture cases
    ├── c5_sensitivity_grid.csv   # 969-point weight sweep across (wE, wR, wC, wS)
    ├── c5_weight_sweep.png       # One-at-a-time parameter sensitivity visualization
    ├── c5_unsafe_constraint_c2.csv# Negative-lookbehind regex evaluation against C2 unprotected outputs
    └── c5_summary.md             # Master summary of decision engine performance
```

#### Key Empirical Results Documented:
1. **Prompt Injection Mitigation:**
   - Undefended GreenRAG (`qwen2.5:7b-instruct`) obeyed direct query injections in **37 of 66 cases** ($56.1\%$ attack success rate, $95\%$ Wilson CI: $[0.441, 0.674]$).
   - Under the C4 Secure Pipeline, obedience dropped to **0 of 66 cases** ($0.0\%$ attack success rate, $95\%$ CI: $[0.000, 0.055]$).
   - Across 990 held-out novel phrasing trials (15 phrasing styles $\times$ 66 queries), **0 items reached acceptance** in the Decision Engine.
2. **Document Tampering & Poisoning Interception:**
   - Self-declared SHA-256 metadata hashes caught **0 of 17** malicious modifications ($0.000$ TPR).
   - The HMAC-SHA256 offline build registry caught **17 of 17** malicious modifications ($1.000$ TPR, $95\%$ CI: $[0.805, 1.000]$) with **0 false alarms** across 189 clean KB documents ($0.000$ FPR, $95\%$ CI: $[0.000, 0.020]$).
3. **Telemetry Anomaly & Replay Defense:**
   - On Solar Plant 1, replaying an earlier sunny day distorted undefended 1-hour-ahead forecasts by $+1874.2\text{ kW}$ daylight MAE. The C3 Telemetry Detector flagged **5/5** replay days ($100\%$ detection), completely neutralizing the distortion.
   - On Wind Turbine T1, the detector flagged **31/31** replay days and **31/31** wind speed bias days ($100\%$ detection).
4. **Decision Engine Arbitration:**
   - Evaluated across 374 real execution instances.
   - Fault diagnosis recommendations reaching `ACCEPT`: **0 of 110 cases**. The engine correctly enforced mandatory `REVIEW` because the underlying B4 detector AUC lower bound touched chance ($0.500 \implies C = 0.000 < \text{floor } 0.5$).
   - Weight sensitivity sweep over 969 points showed **$96.7\%$ mean decision consistency**, demonstrating that the decision boundary is robust against arbitrary weight tuning.

---

### B. Track B: Retrieval & Fault Diagnosis Evidence (`results/kb/`, `results/faultdiag/`, `results/rag/`)

```
c:\Users\priti\Desktop\greenrag_cloud_project\GreenRAG-Sec\results\
├── kb\
│   ├── b2_chunking_metrics.csv   # Fixed vs Semantic vs Metadata chunking across 64 queries
│   └── b3_retrieval_metrics.csv  # BM25 vs Dense vs Hybrid RRF vs Cross-Encoder Rerank
├── faultdiag\
│   ├── b4_event_metrics.csv      # Anomaly model ROC/PR curves (LogReg, RF, XGB, IF)
│   └── b5_results.csv            # Diagnosis accuracy in Logged (12/12) vs Unlogged (4/12) modes
└── rag\
    ├── b6_summary.md             # LLM-as-a-judge faithfulness and hallucination metrics
    └── run2\b6_run2_summary.md   # Deterministic prompt re-evaluation
```

#### Key Empirical Results Documented:
- **Retrieval Superiority:** Hybrid RRF + Cross-Encoder reranking achieved **MRR: 0.958** and **nDCG@5: 0.835**, outperforming vector-only retrieval (MRR: 0.707, nDCG@5: 0.667).
- **RAG Faithfulness:** Faithfulness improved from $0.836$ in Basic RAG to **$0.977$** in GreenRAG; hallucination rate fell from $0.688$ (raw LLM) to **$0.016$**.

---

### C. Track A: Power Forecasting Evidence (`results/forecasting/`)

```
c:\Users\priti\Desktop\greenrag_cloud_project\GreenRAG-Sec-minmini-forecasting-cloud\results\forecasting\
├── a3_summary.md                 # 12-test-day point forecasting benchmark (RF vs XGB vs TCN)
├── a4_summary.md                 # Conformal quantile calibration metrics (80%, 90%, 95%)
└── wind\wind_summary.md          # 10-minute wind turbine series evaluation
```

#### Key Empirical Results Documented:
- **Point Forecast Accuracy:** Random Forest beat Persistence on **12/12 test days** on Solar Plant 1 (MAE: **1444.1 kW vs 2458.0 kW**).
- **Interval Coverage:** 90% CQR Mondrian achieved empirical coverage of **0.893** on Plant 1 and **0.906** on Plant 2 (within the 95% binomial confidence interval of nominal 0.90).

---

## 2. Recommended Additional Experiments for Patent Prosecution

While existing evidence is robust for academic defense, patent prosecution requires tailored comparative experiments to demonstrate **non-obviousness** and **unexpected technical effects**.

```
┌────────────────────────────────────────────────────────────────────────┐
│               RECOMMENDED PATENT VALIDATION PROTOCOLS                  │
├────────────────────────────────────────────────────────────────────────┤
│ [Exp 1] Decision Engine Ablation Study (wE, wR, wC, wS isolation)     │
│ [Exp 2] Adversarial Stress Testing against Multi-Vector Attacks        │
│ [Exp 3] Hardware-in-the-Loop (HIL) Latency & Actuation Simulation      │
│ [Exp 4] Cross-Domain Generalization to Battery Energy Storage (BESS)   │
│ [Exp 5] Statistical Significance Paired Bootstrap Hypothesis Testing  │
└────────────────────────────────────────────────────────────────────────┘
```

### Experiment 1: Component Ablation Study on Decision Engine ($T$)
- **Objective:** Defend the claim that *each* component ($E, R, C, S$) is strictly necessary and that removing any single component degrades safety arbitration.
- **Protocol:**
  - Configuration 1: Full Engine ($T = 0.35E + 0.15R + 0.30C + 0.20S$).
  - Configuration 2 (No Evidence): Set $w_E = 0$. Test whether hallucinated un-cited answers slip through.
  - Configuration 3 (No Confidence): Set $w_C = 0$. Test whether the engine dangerously accepts B4 anomaly diagnoses that operate at chance AUC.
  - Configuration 4 (No Security): Set $w_S = 0$. Test whether attacked sensor inputs pass into accepted dispatch commands.
- **Target Metrics:** False Acceptance Rate ($\text{FAR}$), False Rejection Rate ($\text{FRR}$), and catastrophic failure count.

### Experiment 2: Adversarial Stress Testing with Adaptive Multi-Turn Attacks
- **Objective:** Prove that the security perimeter resists sophisticated multi-stage evasion techniques.
- **Protocol:**
  - *Polyglot & Obfuscated Injections:* Evaluate base64-encoded, leetspeak, and multilingual prompt injection templates.
  - *Collusion Attacks:* Concurrently poison the KB while injecting subtle sensor bias ($+1.5\sigma$).
  - *Zero-Day Replay:* Replay non-consecutive historical fragments rather than entire 24-hour days.
- **Target Metrics:** Area Under Precision-Recall Curve ($\text{PR-AUC}$) and evasion breakthrough percentage.

### Experiment 3: Hardware-in-the-Loop (HIL) Substation Simulation
- **Objective:** Provide tangible proof of physical technical effect for Indian Patent Office Section 3(k) clearance and USPTO Alice Step 2B.
- **Protocol:**
  - Interface GreenRAG-Sec with an open-source grid simulator (e.g., OpenDSS or GridLAB-D).
  - Simulate an un-gated LLM recommending a breaker closure during an unverified fault vs. GreenRAG-Sec issuing a `REVIEW` state and inhibiting the breaker command.
- **Target Metrics:** Overcurrent duration (milliseconds), simulated transformer thermal degradation, and physical outage prevention.

### Experiment 4: Statistical Significance Testing Protocol
- **Objective:** Establish statistical significance with formal p-values ($p < 0.001$).
- **Protocol:**
  - Perform $B = 10,000$ paired bootstrap resamples on:
    - Faithfulness difference: GreenRAG vs. Basic RAG.
    - MRR difference: Hybrid RRF vs. Vector-only.
    - Coverage shortfall: CQR Mondrian vs. Uncalibrated Quantile Regression.
  - Report exact 95% bootstrap confidence intervals and two-sided p-values.

