# 03. Comprehensive Algorithmic & Methodological Breakdown

**Auditor:** Senior Software Architect, Research Scientist & Algorithm Analyst  
**Document Target:** Complete Mathematical and Algorithmic Analysis of `GreenRAG-Sec`  
**Date:** November 3, 2026  

---

## 1. Algorithmic Overview & Taxonomy

The codebase contains a spectrum of algorithmic implementations categorized into three distinct classes:
1. **Potentially Original Methods:** Novel mathematical formulations and security architectures independently engineered for this project.
2. **Modified & Adapted Methods:** Established statistical or machine learning techniques customized to physical grid constraints and domain dynamics.
3. **Existing & Standard Methods:** Industry-standard algorithms utilized as foundational baselines.

```
┌────────────────────────────────────────────────────────────────────────┐
│                        ALGORITHMIC CLASSIFICATION                      │
├────────────────────────────────────────────────────────────────────────┤
│ POTENTIALLY ORIGINAL:                                                  │
│   [Alg 1] Multi-Dimensional Trust-Aware Decision Engine (decision.py)  │
│   [Alg 2] Offline HMAC-SHA256 Build Registry Verification (doc_trust)  │
│   [Alg 3] 5-Stage Physics-Grounded SCADA Anomaly Detector (scada_anom) │
│                                                                        │
│ MODIFIED / ADAPTED:                                                    │
│   [Alg 4] Conformal Quantile Regression with Mondrian Diurnal Grouping │
│   [Alg 5] Hybrid BM25/Dense RRF with Cross-Encoder & Sibling Expansion │
│   [Alg 6] Dual-Phase Regex/Heuristic Prompt Injection Screener         │
│                                                                        │
│ STANDARD BASELINES:                                                    │
│   [Alg 7] Random Forest & XGBoost Point Power Regressors               │
│   [Alg 8] Isolation Forest Unsupervised Anomaly Detection              │
│   [Alg 9] Naive & Daily Persistence Time-Series Baselines              │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 2. In-Depth Analysis of Potentially Original Methods

### Algorithm 1: Multi-Dimensional Trust-Aware Decision Engine ($T$-Engine)

- **Source File & Class:** [`src/backend/trust/decision.py`](file:///c:/Users/priti/Desktop/greenrag_cloud_project/GreenRAG-Sec/src/backend/trust/decision.py#L9-L47): `DecisionEngine`, `decide_core()`, `components()`, `check_constraints()`.
- **Classification:** **Potentially Original Method**

#### Mathematical Formulation
The trust engine computes a scalar trust score $T \in [0, 1]$ via a convex combination of four orthogonal normalized signals:
$$T = w_E E + w_R R + w_C C + w_S S$$
subject to:
$$w_E, w_R, w_C, w_S \ge 0, \quad w_E + w_R + w_C + w_S = 1$$
Default weights: $w_E = 0.35, w_R = 0.15, w_C = 0.30, w_S = 0.20$.

Where each constituent signal is rigorously defined:

1. **Evidence Quality ($E \in [0, 1]$):**
   $$E = V_{\text{cit}} \times F_{\text{route}} \times F_{\text{supp}}$$
   - **Citation Validity ($V_{\text{cit}}$):** 
     $$V_{\text{cit}} = \frac{|\{c \in \mathcal{C}_{\text{cited}} : c \in \mathcal{E}_{\text{trusted}}\}|}{|\mathcal{C}_{\text{cited}}|}$$
     If $|\mathcal{C}_{\text{cited}}| = 0$ for text-generation routes, $V_{\text{cit}} = 0$. For numeric/data-computed routes, $V_{\text{cit}} = 1.0$. If any evidence used is untrusted, $E = 0.0$.
   - **Route Factor ($F_{\text{route}}$):** $1.0$ for generated or computed answers; $0.845$ for extractive fallback (the empirical answer-relevance ratio between extractive quotes and generated answers measured in Track B).
   - **Evidence Support ($F_{\text{supp}}$):** $1.0$ for strong support, $0.5$ for weak support, $0.0$ for unsupported assertions.

2. **Retrieval Relevance ($R \in [0, 1]$):**
   $$R = \frac{1}{|\mathcal{E}_{\text{used}}|} \sum_{e \in \mathcal{E}_{\text{used}}} \sigma(\ell_e) = \frac{1}{|\mathcal{E}_{\text{used}}|} \sum_{e \in \mathcal{E}_{\text{used}}} \frac{1}{1 + e^{-\ell_e}}$$
   where $\ell_e$ is the unbounded cross-encoder logit score from `ms-marco-MiniLM-L-6-v2`. Because the cross-encoder is pre-trained as a binary relevance classifier, the logistic sigmoid $\sigma(\ell_e)$ maps logits directly into calibrated relevance probabilities.

3. **Underlying Model Confidence ($C \in [0, 1]$):**
   $C$ is grounded strictly in empirical statistical lower bounds from calibration testing:
   - *For Fault Diagnosis:* Rescaled lower bound of the 95% confidence interval of the detector's Area Under the ROC Curve ($\text{AUC}_{\text{low}}$), anchored at chance ($0.5$):
     $$C_{\text{diag}} = \min\left(1.0, \max\left(0.0, \frac{\text{AUC}_{\text{low}} - 0.5}{0.5}\right)\right)$$
     *(Remark: For B4 detector, $\text{AUC}_{\text{low}} = 0.500 \implies C = 0.000$, correctly penalizing an unproven detector).*
   - *For Knowledge Q&A:* Lower bound of the 95% Wilson score interval of LLM faithfulness ($\text{Faith}_{\text{low}} = 0.938$).
   - *For Forecasting:* Normalized coverage shortfall below nominal target:
     $$C_{\text{fc}} = \min\left(1.0, \max\left(0.0, 1 - \frac{\max(0, \alpha_{\text{nom}} - \text{Cov}_{\text{low}})}{1 - \alpha_{\text{nom}}}\right)\right)$$

4. **Active Security Strength ($S \in [0, 1]$):**
   Defined by the weakest active defense check operating on the request, parameterized by the empirical True Positive Rate ($\text{TPR}$) and False Positive Rate ($\text{FPR}$) from baseline evaluation:
   $$S = \min_{k \in \mathcal{K}_{\text{active}}} \left[ 1 - \frac{1 - \text{TPR}_k}{1 - \text{FPR}_k} \right]$$
   where $\frac{1 - \text{TPR}}{1 - \text{FPR}}$ is the negative likelihood ratio ($\text{LR}^-$). If any required check is bypassed or fails, $S = 0.0$.

#### Decision Gating & Operational Constraint Rules
The engine maps $(T, E, R, C, S)$ and operational facts into three discrete states:
- **`REJECT`:** Triggered if any hard constraint fails, or untrusted evidence was utilized, or the request was blocked by security pre-screeners, or $T < 0.40$.
  - *Hard Safety Constraint (`no_unsafe_directive`):* Employs regular expressions with negative lookbehind:
    $$\text{Unsafe Patterns} \cap \neg(\text{Negation Preceding}) \neq \emptyset \implies \text{REJECT}$$
    Detects directives such as: `bypass safety`, `bridge interlock`, `disable protection`, `ignore alarm`, `defer maintenance`.
- **`ACCEPT`:** Allowed *only if all* conditions hold:
  1. $T \ge 0.70$
  2. Component floors satisfied: $E \ge 0.5, R \ge 0.3, C \ge 0.5, S \ge 0.5$
  3. No soft constraint failed (e.g. action references attributed component)
  4. No knowledge base tampering detected in the request window
- **`REVIEW`:** Default mandatory fallback for all other requests.

#### Step-by-Step Execution Flow
1. Ingest `Recommendation` and attached upstream metadata.
2. Re-verify security signatures independently via `DecisionEngine.verify_security()`.
3. Compute signals $E, R, C, S$ via `components()`.
4. Run regex and domain constraints via `check_constraints()`.
5. Execute `decide_core()` to evaluate thresholds, floors, and hard gates.
6. Return typed `Decision` object with explicit audit reasons.

#### Difference from Conventional Solutions
Standard RAG frameworks (LangChain, LlamaIndex, Guardrails AI) either execute LLM directives directly or use superficial keyword blocklists. They do not maintain an independent mathematical arbitration layer coupling empirical detector confidence bounds ($C$) with likelihood ratio security strengths ($S$) and hard operational physical constraints.

---

### Algorithm 2: Offline HMAC-SHA256 Build Registry Verification

- **Source File & Class:** [`src/backend/security/doc_trust.py`](file:///c:/Users/priti/Desktop/greenrag_cloud_project/GreenRAG-Sec/src/backend/security/doc_trust.py#L45-L163): `DocumentTrust`, `build_registry()`, `verify_registry()`, `check_hit()`.
- **Classification:** **Potentially Original Method**

#### Mathematical Formulation & Architecture
In conventional vector search, documents declare their own content hash (e.g., `doc.content_sha256 = SHA256(doc.text)`). This self-declared hash is vulnerable to in-place tampering: an adversary editing a document simply computes and writes a valid self-hash.

To break this vulnerability, GreenRAG-Sec establishes an **Offline Signed Build Registry**:
1. During offline build (`src.backend.kb.build`), the canonical mapping of document identifiers to cryptographically re-calculated content hashes is assembled:
   $$\mathcal{M} = \{ \text{doc\_id}_i : \text{SHA256}(\text{text}_i) \}_{i=1}^N$$
2. The canonical JSON serialization of $\mathcal{M}$ is signed with a secret HMAC key held strictly in the backend execution environment (`GREENRAG_TRUST_KEY`):
   $$\Sigma = \text{HMAC-SHA256}_{K_{\text{trust}}}(\text{Canonical}(\mathcal{M}))$$
3. At runtime, when retriever hits are generated, every retrieved chunk text $\tau_{\text{chunk}}$ is verified through a 7-stage check:
   - **`registered`:** $\text{doc\_id} \in \mathcal{M}$ (Blocks unauthorized added documents).
   - **`registry_hash`:** $\mathcal{M}[\text{doc\_id}] == \text{SHA256}(\text{doc.text})$ (Blocks in-place edited documents).
   - **`chunk_in_doc`:** $\text{Squash}(\tau_{\text{chunk}}) \subseteq \text{Squash}(\text{doc.text})$ (Blocks chunk injection / out-of-band injection).
   - **`provenance`:** If real SCADA event, verifies that logged event description is quoted verbatim:
     $$\text{Logged}(\text{farm}, \text{event\_id}) \in \text{doc.text}$$
   - **`stuffing`:** Content-word frequency does not exceed clean threshold:
     $$\max_{w \in \mathcal{W}_{\text{content}}} \frac{\text{Count}(w)}{|\mathcal{W}_{\text{content}}|} \le 1.5 \times \theta_{\text{clean}}$$
   - **`injection`:** Passes prompt injection screener.

#### Experimental Evidence
In `results/security/c3_summary.md`:
- Self-declared hashing caught **0/17** malicious documents (attacker computes matching hash).
- Offline HMAC-signed registry caught **17/17** injected and tampered documents with **0.000 FPR** on 189 clean KB documents.

---

### Algorithm 3: 5-Stage Physics-Grounded SCADA Telemetry & Replay Detector

- **Source File & Class:** [`src/backend/security/scada_anomaly.py`](file:///c:/Users/priti/Desktop/greenrag_cloud_project/GreenRAG-Sec/src/backend/security/scada_anomaly.py#L75-L193): `TelemetryDetector`, `slot_flags()`, `day_report()`.
- **Classification:** **Potentially Original Method**

#### Mathematical Formulation
The detector intercepts telemetry prior to causal data cleaning. All thresholds are calibrated strictly on clean **chronological training days only** using the 99.9th percentile ($q = 0.999$):

1. **Physical Bounds:** Fixed operational envelopes:
   $$x_t \notin [x_{\min}, x_{\max}] \implies \text{flag}_{\text{bounds}} = 1$$
2. **Maximum Rate of Change:**
   $$|\Delta x_t| = |x_t - x_{t-1}| > \theta_{\text{rate}, x}^{(q)} \implies \text{flag}_{\text{rate}} = 1$$
3. **Cross-Sensor Physical Residuals:**
   - *Solar Inverter Efficiency Residual:* Models AC power as a direct function of solar irradiance through the origin:
     $$k_{\text{ac}} = \frac{\sum_{\text{day}} AC_t \cdot Irr_t}{\sum_{\text{day}} Irr_t^2}, \quad r_{\text{ac}} = |AC_t - k_{\text{ac}} \cdot Irr_t| > \theta_{r_{\text{ac}}}^{(q)}$$
   - *Solar Thermal Balance Residual:* Ordinary least squares regression of module temperature on ambient temperature and irradiance:
     $$\hat{T}_{\text{mod}} = \beta_0 + \beta_1 T_{\text{amb}} + \beta_2 Irr_t, \quad r_{\text{temp}} = |T_{\text{mod}} - \hat{T}_{\text{mod}}| > \theta_{r_{\text{temp}}}^{(q)}$$
   - *Wind Turbine Power Curve Residual:* Binned median power curve learned over 0.5 m/s wind speed intervals:
     $$r_{\text{wind}} = |P_{\text{theoretical}} - \text{PowerCurve}(v_{\text{wind}})| > \theta_{r_{\text{wind}}}^{(q)}$$
4. **Multivariate Mahalanobis Anomaly Distance:**
   Standardizes feature vector $\mathbf{z}_t = (\mathbf{x}_t - \boldsymbol{\mu}) / \boldsymbol{\sigma}$ and evaluates:
   $$D_M(\mathbf{z}_t) = \sqrt{\mathbf{z}_t^T \boldsymbol{\Sigma}^{-1} \mathbf{z}_t} > \theta_{\text{maha}}^{(q)}$$
   where $\boldsymbol{\Sigma}^{-1}$ is the Moore-Penrose pseudo-inverse of the training covariance matrix.
5. **Pairwise Historical Profile Replay Distance:**
   Evaluates whole-day normalized vectors $\mathbf{P}_d \in \mathbb{R}^{96 \times 4}$ against all historical days $d' < d$:
   $$D_{\text{day}}(\mathbf{P}_d, \mathbf{P}_{d'}) = \frac{1}{|\mathcal{S}_{\text{valid}}|} \sum_{t \in \mathcal{S}_{\text{valid}}} |\mathbf{P}_d(t) - \mathbf{P}_{d'}(t)|$$
   A day is flagged as a replay attack if:
   $$\min_{d' < d} D_{\text{day}}(\mathbf{P}_d, \mathbf{P}_{d'}) < \epsilon_{\text{replay}} = 0.5 \times \min_{i \ne j \in \text{Train}} D_{\text{day}}(\mathbf{P}_i, \mathbf{P}_j)$$

#### Experimental Evidence
In `results/security/c6_summary.md`:
- Successfully blocked **31/31** wind replay attacks, **5/5** Plant 1 solar replay attacks, and **5/5** Plant 2 solar replay attacks ($100\%$ detection).
- Prevented a $+1874.2\text{ kW}$ mean forecast error on solar replay days.

---

## 3. In-Depth Analysis of Modified & Adapted Methods

### Algorithm 4: Conformal Quantile Regression (CQR) with Mondrian Group Calibration

- **Source File & Class:** [`src/backend/forecasting/uncertainty.py`](file:///c:/Users/priti/Desktop/greenrag_cloud_project/GreenRAG-Sec-minmini-forecasting-cloud/src/backend/forecasting/uncertainty.py#L58-L100): `conformal_quantile()`, `cqr_interval()`, `_per_group()`.
- **Classification:** **Adapted Method** (Customized to diurnal and wind power dynamics)

#### Technical Mechanism
1. Trains base Multi-Quantile XGBoost models to output lower quantile $\hat{q}_{\alpha/2}(x)$ and upper quantile $\hat{q}_{1 - \alpha/2}(x)$ for nominal coverage $1 - \alpha$.
2. Computes conformity non-conformity scores on the held-out **calibration split only**:
   $$E_i = \max(\hat{q}_{\alpha/2}(x_i) - y_i, \, y_i - \hat{q}_{1 - \alpha/2}(x_i))$$
3. Applies **Mondrian Grouping**: Divides the calibration set into disjoint operational regimes $\mathcal{G}$:
   - *Solar:* Daytime slots ($Irr > 0.01$) vs. Nocturnal slots ($Irr \le 0.01$).
   - *Wind:* Cut-in, rated power, and cut-out wind speed bins.
4. Computes finite-sample conformal adjustment quantile per group:
   $$Q_{1-\alpha}(E, \mathcal{G}_k) = \text{Sort}(E_{\mathcal{G}_k})\left[ \left\lceil (n_k + 1)(1 - \alpha) \right\rceil \right]$$
5. Calibrated prediction interval:
   $$I(x) = \left[ \hat{q}_{\alpha/2}(x) - Q_{1-\alpha}, \, \hat{q}_{1-\alpha/2}(x) + Q_{1-\alpha} \right]$$

#### Difference from Conventional Approaches
Standard conformal prediction pools all hours together. In solar forecasting, pooling causes nighttime zero-power slots (which have zero error) to artificially narrow daytime intervals, leading to massive daytime under-coverage during cloudy ramps. Mondrian calibration enforces exact group-conditional coverage.

---

### Algorithm 5: Hybrid RRF Retrieval with Cross-Encoder Rerank & Sibling Expansion

- **Source File:** [`src/backend/retrieval/hybrid_retriever.py`](file:///c:/Users/priti/Desktop/greenrag_cloud_project/GreenRAG-Sec/src/backend/retrieval/hybrid_retriever.py).
- **Classification:** **Adapted Method**

#### Technical Mechanism
1. Sparse retrieval: BM25 token-matching ranking list $\mathcal{R}_{\text{sparse}}$.
2. Dense retrieval: Cosine similarity over `all-MiniLM-L6-v2` dense embeddings $\mathcal{R}_{\text{dense}}$.
3. Reciprocal Rank Fusion (RRF) with constant $k=60$:
   $$\text{RRF\_Score}(d) = \frac{1}{60 + \text{Rank}_{\text{sparse}}(d)} + \frac{1}{60 + \text{Rank}_{\text{dense}}(d)}$$
4. Top-$M$ fused candidates are passed through `ms-marco-MiniLM-L-6-v2` cross-encoder for joint query-document interaction scoring.
5. Window Expansion: For the highest-scoring chunks, adjacent sibling chunks from the original source document are merged to prevent truncation of operational fault procedures.

---

## 4. Algorithmic Baseline Implementations (Standard Techniques)

1. **Random Forest & XGBoost Point Power Forecasters:**
   - Standard gradient-boosted trees and bagged ensembles trained on 26 causal lag features.
   - Evaluated in `experiments/forecasting_models.py`.
2. **Isolation Forest SCADA Anomaly Detector:**
   - Standard recursive random partitioning trees fitted on Wind Farm A sensor features.
   - Evaluated in `experiments/fault_diagnosis.py` (achieved event AUC 0.594).
3. **Naive & Daily Persistence Forecasters:**
   - Naive: $\hat{y}_{t+h} = y_t$.
   - Daily Persistence: $\hat{y}_{t+h} = y_{t+h - 24\text{h}}$.
   - Proved exceptionally competitive on 10-minute wind forecasting due to high short-term wind velocity autocorrelation.

