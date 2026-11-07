# 12. Missing Information, Experimental Gaps & Next Steps for Patent Filing

**Auditor:** Senior Software Architect, R&D Technical Auditor & Patent Research Analyst  
**Document Target:** Pre-Filing Technical Roadmap, Missing Evidence & Action Plan  
**Date:** November 3, 2026  

---

## 1. Identified Technical & Experimental Gaps

While `GreenRAG-Sec` contains an exceptionally rigorous prototype implementation and comprehensive offline evaluation suites, several technical gaps and unverified assumptions must be addressed before filing a Complete Patent Specification.

```
┌────────────────────────────────────────────────────────────────────────┐
│                        PRE-FILING GAP ANALYSIS                         │
├────────────────────────────────────────────────────────────────────────┤
│ 1. Actuation Interface: Prototype outputs typed Pydantic objects,     │
│    not real industrial SCADA protocols (IEC 61850 / DNP3 / Modbus).   │
│ 2. Telemetry Scope: Wind anomaly detector (B4) operated on offline     │
│    CSV batches; real-time streaming latency was not measured.         │
│ 3. Attack Diversity: Prompt injection screener tested English text     │
│    only; polyglot and token-smuggling variants remain untested.        │
│ 4. Authentication: Security prototype ran unauthenticated (auth.py=0b)│
│    omitting role-based access control (RBAC) token validation.         │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Detailed Technical Deficiencies to Address

### Gap 1: Physical Actuation Hardware Interface (Crucial for Section 3(k))
- **Current Reality:**
  `src/backend/trust/decision.py` outputs a Pydantic `Decision` object containing `"decision": "accept" | "review" | "reject"`. The software stops there; it does not actually transmit a binary or analog control signal to an electrical circuit breaker or inverter setpoint controller.
- **Why It Matters for Patenting:**
  Under Section 3(k) of the Indian Patents Act, examiners frequently issue initial objections stating that generating a software decision is merely an "algorithm or computer program per se".
- **Action Required:**
  Implement a lightweight hardware-in-the-loop (HIL) or industrial protocol driver (e.g., using `pymodbus` or an open-source IEC 61850 client). Demonstrate that an `ACCEPT` verdict executes a Modbus coil write (e.g., closing a breaker contactor), while a `REVIEW` or `REJECT` verdict raises an alarm line and inhibits coil writing.

---

### Gap 2: Adversarial Multi-Turn and Polyglot Injection Testing
- **Current Reality:**
  `src/backend/security/injection_detector.py` uses 8 English regex patterns and zero-width unicode normalization.
- **Vulnerability:**
  The detector is vulnerable to:
  1. Base64, hex, or rot13 encoded injection directives.
  2. Non-English instructions (e.g., instructions in German, Spanish, or Hindi).
  3. Hyphenated, spaced, or token-smuggled keywords (e.g., `i-g-n-o-r-e`).
- **Action Required:**
  Add a pre-tokenization normalization layer that detects base64 strings and transliterates common non-English keywords, and rerun the 990-item held-out evaluation suite to prove defense persistence.

---

### Gap 3: Formal Paired Bootstrap Statistical Significance
- **Current Reality:**
  The repository provides point estimates and Wilson score confidence intervals for individual proportions (`c6_summary.md`), but lacks formal hypothesis testing (p-values from paired bootstrap tests) comparing GreenRAG-Sec directly against competitive baselines.
- **Action Required:**
  Execute a paired bootstrap test ($B = 10,000$ iterations) measuring:
  - $\Delta \text{Faithfulness}$ between GreenRAG and Basic RAG ($p < 0.001$).
  - $\Delta \text{Harm (MAE)}$ between gated telemetry and undefended telemetry under replay attacks.

---

### Gap 4: Token Authentication & Zero-Trust Production Envelope
- **Current Reality:**
  `src/backend/security/auth.py` is an empty file (0 bytes). The system relies entirely on local network trust.
- **Action Required:**
  Incorporate JSON Web Token (JWT) or mTLS cryptographic client authentication to prove that the trust engine cannot be bypassed via unauthenticated direct API injection.

---

## 3. Prioritized Action Checklist Before Approaching a Patent Attorney

### Phase 1: Immediate Engineering Tasks (Weeks 1–3)
- [ ] **Task 1.1:** Build a simulated IEC 61850 / Modbus hardware actuator module in `src/backend/actuator/grid_relay.py` demonstrating physical circuit breaker tripping upon `REJECT` / `REVIEW`.
- [ ] **Task 1.2:** Run polyglot and encoded injection attack stress tests against `injection_detector.py`.
- [ ] **Task 1.3:** Complete the paired bootstrap significance script (`experiments/statistical_significance.py`) and log formal p-values into `results/statistics/`.

### Phase 2: Documentation & Patent Drafting Support (Weeks 4–6)
- [ ] **Task 2.1:** Prepare high-resolution vector system flowcharts and block diagrams of the Decision Engine and HMAC Build Registry.
- [ ] **Task 2.2:** Consolidate all mathematical formulas, variable definitions, and parameter tables into a unified Patent Drafting Annex.
- [ ] **Task 2.3:** Conduct a preliminary InPASS and USPTO text search using the exact claim terms formulated in `10_INVENTION_DISCLOSURES.md`.

### Phase 3: Engagement with Registered Patent Agent (Weeks 7–8)
- [ ] **Task 3.1:** Present `EXECUTIVE_SUMMARY.md` and `10_INVENTION_DISCLOSURES.md` to a licensed patent attorney / patent agent registered with the Indian Patent Office (IPO) and USPTO.
- [ ] **Task 3.2:** File an Indian Provisional Patent Application covering Candidate 1 (Trust Decision Engine) and Candidate 2 (Cryptographic RAG Registry) to secure the earliest priority date.
- [ ] **Task 3.3:** Maintain complete confidentiality of the repository until the priority filing is stamped and confirmed.

