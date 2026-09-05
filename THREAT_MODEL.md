# PAYLAB AI — Threat Model & Security Considerations

## 1. Scope & Goals

PAYLAB AI is an educational, defensive developer tool for testing and validating payment integration resilience. This document outlines threat vectors in payment integrations, risks tested by PAYLAB AI, and safeguards embedded within the tool itself.

---

## 2. Payment Integration Threat Vectors Evaluated

| Threat / Vulnerability | Vector | Impact | PAYLAB Mitigation & Detection |
|---|---|---|---|
| **Webhook Signature Forgery** | Unauthenticated POST to webhook URL | Attacker sends fake `payment.captured` event to trigger free order fulfillment | Static check for missing HMAC verification + Chaos Test `T05` |
| **Replay / Duplicate Webhooks** | Gateway network retry or malicious replay | Order fulfilled twice, double inventory deduction, or multiple shipments | Idempotency event ID cache check + Chaos Test `T02` |
| **Client Status Tampering** | Frontend sends `{ status: "paid" }` directly to server | Attacker modifies frontend parameters without completing payment | Server-side verification check + Chaos Test `T06` |
| **Race Condition / Out-of-Order Delivery** | Async network jitter delivers `CAPTURED` before `AUTHORIZED` | Order stuck in limbo or transitions to invalid state | Transition table validation + Chaos Test `T03` |
| **Retry Storms & Double Charges** | Rapid network retries on payment capture call | Multiple charges on customer account or duplicate capture calls | Idempotency key requirement + Chaos Test `T07` & `T08` |
| **Credential Leakage** | Hardcoded `rzp_live_*` or webhook secret in code | Exposure of merchant gateway credentials | Pattern detector flags hardcoded keys in `codeAnalyzer.ts` |

---

## 3. PAYLAB AI Application Safeguards

1. **No Live Payment Gateway Secrets:**
   - PAYLAB AI never prompts for, stores, or processes real live payment credentials (`rzp_live_*`).
   - All tests run against in-memory synthetic state machines and mock payloads.

2. **Server-Side AI API Access:**
   - Gemini API keys are processed strictly in server Route Handlers (`/api/*`) and are never leaked to client bundles.

3. **Deterministic Air-Gap:**
   - AI outputs are never executed dynamically as unvetted arbitrary code. Fix diffs are displayed as structured syntax-highlighted code suggestions for human review.
