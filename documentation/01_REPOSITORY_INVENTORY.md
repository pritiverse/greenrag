# 01. Repository Technical Inventory & Component Census

**Repository Target:** `c:\Users\priti\Desktop\greenrag_cloud_project`  
**Auditor:** Senior Software Architect, R&D Technical Auditor & Algorithm Analyst  
**Audit Date:** November 3, 2026  
**Baseline Git Topologies:**
- `GreenRAG-Sec`: Integrated repository clone (Branch: `pritika-security-trust`, tracking `origin/pritika-security-trust`, Commit: `2f0a645`)
- `GreenRAG-Sec-pritika`: Standalone track C Git repository (Branch: `main`, Commit: `1bb5553`)
- `GreenRAG-Sec-minmini-forecasting-cloud`: Standalone modular export folder for Track A
- `WORKSPACE_TEAM_BREAKDOWN.md`: Master integration matrix across all tracks

---

## 1. Executive Summary of the Inventory

The workspace is organized into a modularized multi-track renewable energy intelligence and security architecture titled **GreenRAG-Sec**. The system integrates:
1. **Track A (Forecasting & Cloud):** 1-hour-ahead solar and wind power forecasting with conformal quantile uncertainty quantification and Azure cloud migration capabilities.
2. **Track B (Knowledge Base, Hybrid RAG, Fault Diagnosis & Console):** A 189-document hybrid retrieval pipeline (BM25 + ChromaDB `all-MiniLM-L6-v2` + Reciprocal Rank Fusion + Cross-Encoder reranking), Wind Farm A anomaly detection, evidence-grounded diagnosis generation, and full web console.
3. **Track C (Security Perimeter & Trust-Aware Decision Engine):** A comprehensive threat model (30 threats across 8 layers), synthetic attack generation suite, a 4-tier security detector perimeter (physical/cross-sensor SCADA anomaly detector, prompt injection classifier, offline HMAC-SHA256 signed document trust verifier, and evidence filter), and an autonomous Trust-Aware Decision Engine ($T = 0.35E + 0.15R + 0.30C + 0.20S$) enforcing hard operational constraints.
4. **Shared Infrastructure:** FastAPI backend router, 6-stage experimental ablation orchestrator (Ablations A–F), ops readiness/warmup endpoints, and results reflection APIs.

---

## 2. Comprehensive Component Inventory

### A. Core Backend Modules & Services

| Component / Subsystem | Exact Path | Core Function / Technical Responsibility | Implementation Status | Code Lineage / Provenance |
|---|---|---|---|---|
| **Trust Decision Engine** | `src/backend/trust/decision.py` | Multi-signal trust arbiter computing $T = w_E E + w_R R + w_C C + w_S S$, evaluating hard/soft constraints, and enforcing `ACCEPT`, `REVIEW`, `REJECT` states. | **Complete** (397 lines) | **Original Implementation** (Pritika Kasinathan, Track C) |
| **Trust Signal Evaluator** | `src/backend/trust/signals.py` | Translates raw pipeline responses into normalized statistical trust signals ($E, R, C, S$) using calibrated benchmark inputs. | **Complete** (128 lines) | **Original Implementation** (Pritika Kasinathan, Track C) |
| **Trust Service API Layer** | `src/backend/trust/service.py` | FastAPI endpoint handlers for `POST /decision`; enforces server-side computation of recommendations. | **Complete** (130 lines) | **Original Implementation** (Pritika Kasinathan, Track C) |
| **HMAC Document Trust Verifier** | `src/backend/security/doc_trust.py` | Offline-built HMAC-SHA256 registry validation, chunk-in-document substring check, keyword-stuffing frequency check, and logged provenance quotation verifier. | **Complete** (163 lines) | **Original Implementation** (Pritika Kasinathan, Track C) |
| **Evidence Filter** | `src/backend/security/evidence_filter.py` | Drops untrusted chunks prior to prompt generation; routes to extractive fallback when evidence is contaminated. | **Complete** (78 lines) | **Original Implementation** (Pritika Kasinathan, Track C) |
| **SCADA Telemetry Anomaly Detector** | `src/backend/security/scada_anomaly.py` | 5-tier pre-cleaning sensor spoofing detector: physical bounds, rate-of-change, cross-sensor residuals, Mahalanobis distance, and pairwise daily profile replay distance. | **Complete** (193 lines) | **Original Implementation** (Pritika Kasinathan, Track C) |
| **Prompt Injection Screener** | `src/backend/security/injection_detector.py` | Weighted regex and heuristic pattern detector testing for override instructions, fake authority, role change, and delimiter escapes. | **Complete** (65 lines) | **Original Implementation** (Pritika Kasinathan, Track C) |
| **Held-Out Injection Test Fixture** | `src/backend/security/injection_eval_set.py` | Curated dataset of 15 held-out prompt injection variants and 15 benign technical domain queries. | **Complete** (64 lines) | **Original Dataset** (Pritika Kasinathan, Track C) |
| **Synthetic Attack Generator** | `src/backend/security/attacks.py` | Generates 5 telemetry attacks (FDI, bias, scaling, spikes, replay), 4 KB poisoning attacks, and 66 prompt injection attacks. | **Complete** (254 lines) | **Original Implementation** (Pritika Kasinathan, Track C) |
| **Attack Impact Evaluator** | `src/backend/security/attack_eval.py` | Measures empirical degradation (MAE increase, coverage shortfall, hallucination rate) under active attacks. | **Complete** (118 lines) | **Original Implementation** (Pritika Kasinathan, Track C) |
| **Hardened Retrieval Pipeline** | `src/backend/security/secure_pipeline.py` | Hardened RAG pipeline coordinating Sanitizer -> Detector -> Hybrid Retrieval -> HMAC Trust -> Filter -> LLM. | **Complete** (298 lines) | **Original Implementation** (Pritika Kasinathan, Track C) |
| **Security Lab API Router** | `src/backend/api/lab.py` | FastAPI router for interactive attack execution and live detector metric reflection (`/security/lab/*`). | **Complete** (338 lines) | **Original Implementation** (Pritika Kasinathan, Track C) |
| **Security Auth Stub** | `src/backend/security/auth.py` | Placeholder module for token authentication; documented exclusion from local workstation prototype scope. | **Stub** (0 lines / Empty) | Architectural boundary exclusion |
| **Hybrid Retriever** | `src/backend/retrieval/hybrid_retriever.py` | Fuses BM25 sparse lexical search and ChromaDB dense embeddings via Reciprocal Rank Fusion ($k=60$) with Cross-Encoder reranking. | **Complete** (182 lines) | Adapted from standard RRF & Cross-Encoder libraries (Tarunica Senthil, Track B) |
| **Query Classifier** | `src/backend/retrieval/query_classifier.py` | Regex and semantic router directing quantitative queries to deterministic CSV engines to avoid arithmetic hallucinations. | **Complete** (42 lines) | Original Rule Engine (Tarunica Senthil, Track B) |
| **Corrective Retrieval Engine** | `src/backend/retrieval/corrective_retrieval.py` | Placeholder for active corrective web/external retrieval (CRAG). | **Stub** (0 lines / Empty) | Incomplete / Planned feature |
| **Vector Store Abstraction** | `src/backend/retrieval/local_store.py` & `azure_store.py` | Vector store implementations wrapping ChromaDB (local) and Azure AI Search (cloud). | **Complete** (246 lines) | Integration wrapper around ChromaDB & Azure SDK |
| **KB Taxonomy & Provenance Schema** | `src/backend/kb/taxonomy.py` & `schema.py` | 13-component standardized wind/solar fault taxonomy and strictly typed Pydantic metadata schemas. | **Complete** (220 lines) | Domain engineering based on real SCADA datasets |
| **KB Content Generator & Assembler** | `src/backend/kb/content.py` & `build.py` | Compiles 189 knowledge base documents: 99 real SCADA event records + 90 labelled synthetic equipment manuals. | **Complete** (980 lines) | Domain dataset construction & offline build tool |
| **Semantic & Fixed Chunkers** | `src/backend/kb/chunking.py` & `ingestion/chunkers.py` | Evaluates Fixed-size, Semantic, and Metadata-aware chunking strategies across 64 labelled queries. | **Complete** (350 lines) | Comparative pipeline |
| **Fault Diagnosis ML Models** | `src/backend/faultdiag/models.py` | Anomaly detection algorithms (Logistic Regression, Random Forest, XGBoost, Isolation Forest) for Wind Farm A SCADA telemetry. | **Complete** (186 lines) | Standard Scikit-Learn & XGBoost implementations |
| **Evidence-Grounded Diagnosis Service** | `src/backend/faultdiag/diagnosis.py` & `service.py` | Associates telemetry anomaly clusters with retrieved KB chunks; produces structured `DiagnosisReport` in logged and unlogged modes. | **Complete** (490 lines) | Original Service (Tarunica Senthil, Track B) |
| **Causal Forecasting Preprocessor** | `src/backend/forecasting/preprocess.py` | Strict chronological data partitioner (Train/Val/Calib/Test) with causal rolling aggregations and train-only scalers. | **Complete** (280 lines) | Original pipeline (Minmini K / Chirag Das) |
| **Forecasting Feature Engineering** | `src/backend/forecasting/features.py` | Computes 26 causal features: autoregressive lags (1, 3, 6, 24), diurnal lags, rolling statistics, weather metrics, and solar angles. | **Complete** (240 lines) | Original pipeline (Minmini K / Chirag Das) |
| **Forecasting Point Models** | `src/backend/forecasting/models.py` | Point predictors: Naive Persistence, Daily Persistence, Random Forest, XGBoost, LSTM, and Temporal Convolutional Networks (TCN). | **Complete** (310 lines) | Comparative pipeline using Scikit-Learn, XGBoost, PyTorch |
| **Conformal Quantile Uncertainty (CQR)** | `src/backend/forecasting/uncertainty.py` | Conformal Quantile Regression with Mondrian group calibration (daylight vs. night regimes; wind power curve brackets). | **Complete** (282 lines) | Adapted from Romano et al. (2019) with domain grouping |
| **Wind Turbine Forecaster** | `src/backend/forecasting/wind.py` | 10-minute resolution wind power forecasting pipeline with conformal bounds. | **Complete** (195 lines) | Adaptation of solar pipeline to wind dynamics |
| **Forecasting Service API** | `src/backend/forecasting/service.py` | FastAPI endpoints powering `POST /forecast` and `GET /forecast/series`. | **Complete** (160 lines) | Present in `minmini` folder, exported to shared |
| **Azure Cloud Adapters** | `src/backend/cloud/blob.py`, `secrets.py`, `appinsights.py` | Cloud abstractions for Azure Blob Storage, Azure Key Vault, and Application Insights. | **Complete** (290 lines) | Standard Azure SDK wrapper implementations |
| **Application Orchestrator** | `src/backend/orchestrator.py` | Intent router executing experimental Ablation configurations A through F. | **Complete** (145 lines) | Foundational scaffolding (Chirag Das) |
| **FastAPI Core & Ops Routers** | `src/backend/main.py`, `api/ops.py`, `api/results.py`, `api/catalog.py` | Application entrypoint, CORS handling, static asset serving, health/readiness checks, dynamic results reflection APIs. | **Complete** (420 lines) | Shared infrastructure |

---

### B. User Interface & Presentation Layer

| Component | Path | Function | Status | Implementation Details |
|---|---|---|---|---|
| **Operational Console Web App** | `src/console/index.html`, `src/console/css/app.css`, `src/console/js/*` | Complete operational operator dashboard: Assistant, Fault Diagnosis, Forecast, Security Lab, and Results tabs with light/dark theme. | **Complete** (12 files) | Vanilla JS, CSS3, Chart.js; responsive design |
| **Legacy Frontend** | `src/frontend/index.html`, `app.js`, `styles.css` | Earlier lightweight web interface prototype. | **Complete** (3 files) | Superseded by `src/console` |

---

### C. Experimentation, Benchmarking & Evaluation Suites

| Experiment Harness | File Path | Objective & Evaluated Scope | Status |
|---|---|---|---|
| **A2 Feature Analysis** | `experiments/feature_engineering.py` | 26-feature importance ranking and correlation analysis for solar plants. | **Complete & Verified** |
| **A3 Point Model Benchmark** | `experiments/forecasting_models.py` | Systematic cross-model comparison (Persistence vs. RF vs. XGB vs. TCN) across 12 test days. | **Complete & Verified** |
| **A4 Conformal Calibration** | `experiments/forecasting_uncertainty.py` | Evaluates empirical coverage against nominal 80%, 90%, 95% levels across calibration splits. | **Complete & Verified** |
| **B2 Chunking Benchmark** | `experiments/chunking_comparison.py` | Compares Fixed-size, Semantic, and Metadata-aware chunking on 64 queries. | **Complete & Verified** |
| **B3 Retrieval Benchmark** | `experiments/retrieval_comparison.py` | Evaluates BM25, Dense, Hybrid RRF, and Cross-Encoder Rerank (MRR, nDCG@5, Recall@5). | **Complete & Verified** |
| **B4 Anomaly Detection Sweep** | `experiments/fault_diagnosis.py` | Leave-one-asset-out ROC/PR curve generation for Wind Farm A telemetry. | **Complete & Verified** |
| **B5 Grounded Diagnosis Eval** | `experiments/fault_rag.py` | Evaluates diagnosis accuracy and cited chunk attribution in Logged and Unlogged modes. | **Complete & Verified** |
| **B6 RAG Faithfulness Benchmark** | `experiments/rag_evaluation.py` | LLM-as-a-judge faithfulness, answer relevance, and hallucination rate scoring. | **Complete & Verified** |
| **C2 Synthetic Attack Benchmark** | `experiments/attack_simulation.py` | Evaluates undefended system vulnerability under telemetry, poisoning, and injection attacks. | **Complete & Verified** |
| **C3 Detector Performance Eval** | `experiments/detector_evaluation.py` | Evaluates TPR, FPR, and negative likelihood ratios for all 4 security detector tiers. | **Complete & Verified** |
| **C4 Hardened Pipeline Eval** | `experiments/secure_pipeline_eval.py` | Tests end-to-end question answering under active knowledge base poisoning. | **Complete & Verified** |
| **C5 Trust Engine Sensitivity** | `experiments/decision_engine_eval.py` | Runs 969-point grid sweep over trust weights ($w_E, w_R, w_C, w_S$) and decision thresholds. | **Complete & Verified** |
| **C6 Deployed Security Evaluation** | `experiments/security_evaluation.py` | Large-scale 330-item capture and 990-item held-out injection trials using `qwen2.5:7b-instruct`. | **Complete & Verified** |

---

### D. Automated Test Suites

| Test File | Target Module Tested | Test Types | Result Status |
|---|---|---|---|
| `tests/test_threat_model.py` | Threat matrix coverage, table consistency, path validity | Static integrity & schema tests | 7 Passed / 2 Failed (due to modular split path references) |
| `tests/test_attacks.py` | Telemetry, poisoning, and prompt injection synthetic generators | Unit & fixture tests | Complete |
| `tests/test_detectors.py` | SCADA anomaly, prompt injection, and HMAC document trust checks | Algorithmic unit tests | Complete |
| `tests/test_decision_engine.py` | Trust formula, constraint evaluation, floor gating, weight bounds | Deterministic regression tests | Complete |
| `tests/test_secure_pipeline.py` | End-to-end secure RAG pipeline and metadata attachment | Integration tests | Complete |
| `tests/test_secure_api.py` | FastAPI `/secure-ask` and `/security/lab/*` endpoints | API integration tests | Complete |
| `tests/test_security_evaluation.py` | Metric aggregation, Wilson score confidence intervals | Statistical verification tests | Complete |
| `tests/test_kb.py`, `test_kb_chunking.py` | Knowledge base ingestion, chunking, and schemas | Data pipeline tests | Complete |
| `tests/test_hybrid_retrieval.py` | RRF fusion, reranker scoring, sibling expansion | Retrieval unit tests | Complete |
| `tests/test_faultdiag.py`, `test_fault_rag.py` | Anomaly models and diagnosis report generation | ML & pipeline tests | Complete |
| `tests/test_forecasting_*.py` | Preprocessing, causal feature engineering, point models, CQR | Time-series regression tests | Complete |

---

## 3. Disconnected, Unused, or Incomplete Components

1. **`src/backend/security/auth.py` (Stub):**
   - Contains 0 bytes. The threat model (`docs/threat_model.md`, threats AP-1 to AP-3) explicitly documents that authentication and role-based access control were deferred from the local prototype scope.
2. **`src/backend/retrieval/corrective_retrieval.py` (Stub):**
   - Contains 0 bytes. Corrective RAG (CRAG) web query generation was designed as a future extension but is disconnected from the operational pipeline.
3. **Cross-Repository Path Dependencies in Modular Exports:**
   - In `GreenRAG-Sec` (branch `pritika-security-trust`), forecasting modules (`src/backend/forecasting/service.py`) were factored into `team_split/minmini/` (exported to `GreenRAG-Sec-minmini-forecasting-cloud`). Running unit tests directly inside `GreenRAG-Sec` without `$env:PYTHONPATH` spanning the shared directory triggers `ModuleNotFoundError: 'src.backend.forecasting.service'`.
4. **Hardcoded Local LLM Dependency:**
   - Generation modules rely on a running local Ollama instance serving `qwen2.5:7b-instruct` or `llama3.2:3b`. If Ollama is offline, the pipeline gracefully falls back to extractive quotes (`fallback_relevance_ratio = 0.845`), but full generation is suspended.

---

## 4. Code Lineage & Originality Classification

```
┌────────────────────────────────────────────────────────────────────────┐
│                        CODEBASE LINEAGE AUDIT                          │
├────────────────────────────────────────────────────────────────────────┤
│ [1] Genuinely Original R&D Implementations (~45% of backend codebase)  │
│     - Trust-Aware Decision Engine (decision.py, signals.py)            │
│     - Offline HMAC-SHA256 Build Registry Verification (doc_trust.py)   │
│     - Multi-Stage Renewable SCADA Anomaly Detector (scada_anomaly.py)  │
│     - Evidence Integrity Filter & Hardened Pipeline (secure_pipeline)  │
│     - Knowledge Base Taxonomy & Provenance Schemas (taxonomy, schema)  │
│     - Synthetic Attack Simulation Suite (attacks.py, attack_eval.py)   │
│                                                                        │
│ [2] Domain Adaptations of Known Algorithms (~30% of backend codebase)  │
│     - Conformal Quantile Regression with Mondrian Diurnal Grouping     │
│     - Reciprocal Rank Fusion + Cross-Encoder Rerank with Sibling Exp.  │
│     - Anomaly Attribution to Equipment Subsystems (diagnosis.py)       │
│                                                                        │
│ [3] Standard Library & Boilerplate Implementations (~25% of backend)  │
│     - FastAPI Routing & CORS Middleware (main.py, ops.py)              │
│     - Scikit-Learn / XGBoost Baseline Point Forecasters (models.py)    │
│     - Azure SDK Storage & Secret Connectors (blob.py, secrets.py)      │
│     - Vector Store Wrappers (local_store.py using ChromaDB)            │
└────────────────────────────────────────────────────────────────────────┘
```

