# PAYLAB AI — Demo Walkthrough Guide

## 1. Quick Start

1. Start development server:
   ```bash
   npm run dev
   ```
2. Navigate to [http://localhost:3000](http://localhost:3000)

---

## 2. 5-Minute Pitch & Judge Demo Flow

### Step 1: The Problem (0:00 - 0:30)
- Highlight that payment integrations typically only test happy paths.
- Real-world production failures arise from webhook retries, duplicate deliveries, out-of-order events, and unverified signatures.

### Step 2: Fictional Integration — ShopKart (0:30 - 1:00)
- Open `/demo`.
- Introduce ShopKart: a sample checkout system with subtle real-world vulnerabilities.

### Step 3: "BREAK MY PAYMENT SYSTEM" (1:00 - 2:00)
- Click the **💥 BREAK MY PAYMENT SYSTEM** button.
- Watch live chaos tests execute one by one.
- Notice the test results: **Duplicate webhook fails**, **Out-of-order event fails**, **Invalid signature accepted**.
- Baseline Reliability Score: **61 / 100**.

### Step 4: AI Root-Cause Analysis (2:00 - 3:00)
- Click **RUN AI ROOT CAUSE**.
- Review the AI explanation grounded directly in the deterministic test failures.
- See the breakdown of production risks (double fulfillment, forgery vulnerabilities).

### Step 5: AI Fix Generation & Diff (3:00 - 3:45)
- Click **GENERATE FIX →**.
- Inspect the side-by-side Before/After diff adding HMAC signature verification and idempotency keys.

### Step 6: Rerun & Before/After Proof (3:45 - 4:30)
- Click **RUN TESTS AGAIN →**.
- Watch all tests pass and the reliability score surge to **100 / 100** (+39 improvement).

### Step 7: Custom Code Analyzer (4:30 - 5:00)
- Visit `/analyze`.
- Paste custom webhook handler code to see instant static rule detection + AI developer explanations.
