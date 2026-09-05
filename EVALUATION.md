# PAYLAB AI — Evaluation & Verification Strategy

## 1. Evaluation Methodology

PAYLAB AI is evaluated against three core criteria:
1. **Determinism:** Tests, status decisions, and scores are 100% reproducible and computed purely in TypeScript.
2. **Detection Accuracy:** Seeded vulnerabilities in sample code and synthetic checkout integrations are consistently flagged.
3. **Remediation Effectiveness:** When the generated fix is applied, the deterministic score demonstrates verified improvement.

---

## 2. Before / After Benchmark Results

Running PAYLAB AI against the standard **ShopKart** integration produces the following benchmark:

### Baseline (Seeded Vulnerable Configuration)
- **Configuration:** `SEEDED_SHOPKART` (No idempotency check, unverified webhook signature, trusts client status, no retry dedup).
- **Tests Passed:** 3 / 8
- **Tests Failed:** 5 / 8
- **Failures Detected:**
  - `T02`: Duplicate webhook delivery
  - `T03`: Out-of-order event handling
  - `T05`: Invalid webhook signature accepted
  - `T06`: Payment/order state mismatch (trusting client status)
  - `T07`: Missing idempotency key on capture
- **Reliability Score:** **61 / 100**
- **Critical Findings:** 3

### Remediated (Fixed Configuration)
- **Configuration:** `FIXED_SHOPKART` (Idempotency checks added, HMAC signature verification active, state buffering enabled, client status untrusted).
- **Tests Passed:** 8 / 8
- **Tests Failed:** 0 / 8
- **Reliability Score:** **100 / 100**
- **Score Delta:** **+39 points**

---

## 3. Automated Test Suite (Vitest)

Unit tests in `src/lib/simulator.test.ts` and `src/lib/codeAnalyzer.test.ts` continuously verify:
- Happy path execution
- Correct failure detection on duplicate events
- Signature verification logic
- Out-of-order state buffering
- Idempotency guard enforcement
- Score calculations and metric weightings
- Static rule matching against vulnerable code patterns
