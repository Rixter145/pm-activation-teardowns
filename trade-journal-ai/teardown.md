# Trade Journal AI — Activation Teardown

**Author:** Ricardo Lo · **Date:** 2026-07-06 · **Company:** Trade Journal AI (tradejournal.ai)

> PM activation teardown — Playwright audit 2026-07-06. For Adam Hardegree.

---

## TL;DR

**Problem:** Homepage promises a **7-day free trial**, but signup forces **Stripe payment before password or dashboard**, and pricing FAQ says **no free trial** — three conflicting signals that truncate trust before activation.

**Proposed fix:** Align all surfaces to **14-day money-back guarantee** (drop "7-day trial" copy); add a **CSV-import-first path** on signup so traders see journal value before broker API connect; optional **reverse trial** (card on file, charge day 8) if you want trial language back.

**Artifact:** This doc + screenshots in this folder

---

## Evidence

| # | Step | Friction | Screenshot |
|---|------|----------|------------|
| 1 | Homepage hero | `copy_confusion` | [step_1_homepage_trial_claim.png](./screenshots/step_1_homepage_trial_claim.png) |
| 2 | Signup | `time_to_value` | [step_2_signup_pay_before_password.png](./screenshots/step_2_signup_pay_before_password.png) |
| 3 | Pricing FAQ | `copy_confusion` | [step_3_pricing_no_trial_faq.png](./screenshots/step_3_pricing_no_trial_faq.png) |
| 4 | Stripe checkout | `integration_wall` | [step_4_stripe_before_product.png](./screenshots/step_4_stripe_before_product.png) |

**Step 1 — Homepage:** Hero pricing strip reads **"$19.95/mo · $195/yr · 7-day free trial · 14-day money-back guarantee"**.

**Step 2 — Signup:** Subhead says *"Enter your email and pick a plan. You'll set your password after payment."* User must choose Monthly ($19.95) or Annual ($195) with no preview of product.

**Step 3 — Pricing FAQ:** Under "Is there a free trial?" — *"No — Trade Journal AI doesn't offer a free trial. Your card is charged at signup… covered by 14-day money-back guarantee."*

**Step 4 — Stripe:** Email + Monthly → immediate Stripe Checkout for Trade Journal AI subscription. No password, no dashboard, no sample journal until payment completes.

**Step 5 (documented):** Post-payment, core value requires **broker API connect** (Bitunix / Hyperliquid / Binance) or CSV import per [brokers page](https://tradejournal.ai/brokers) — second activation wall after pay.

---

## User story

As a **crypto trader evaluating journals**, I click **Get Started** expecting to try the product. Homepage says **7-day free trial**, but signup pushes me to **pay $19.95 before I set a password or see a dashboard**. Pricing FAQ later says there is **no free trial**. I cannot tell if I'm risking a charge or getting a real trial — so I bounce before broker sync or AI Coach ever runs.

---

## Proposed flow

### Before (current)

1. Homepage → "7-day free trial" CTA
2. Signup → pick plan → Stripe charge
3. Set password (post-payment)
4. Connect broker API or figure out CSV import
5. First trade synced → AI Coach value

### After (proposed)

1. Homepage → single promise: **"14-day money-back guarantee — full refund, email Adam"** (remove 7-day trial line)
2. Signup → email + **"Start with CSV import"** OR **"Connect broker"** path selector
3. **Option A (lower friction):** CSV import sandbox → 3 manual trades → AI Coach teaser → then plan selection
4. **Option B (keep pay-first):** Stripe checkout but signup copy matches FAQ exactly (no trial language anywhere)
5. Post-login **onboarding checklist:** (1) Import 1 trade (2) Connect broker (3) Run first Coach review

**Key changes:**

- Fix copy drift: homepage, signup, pricing FAQ must say the same thing
- Default first value = **one imported trade in journal UI**, not API keys
- Broker connect becomes step 2, not gate 1
- Measure funnel: `signup_start` → `payment_complete` → `first_trade_in_journal` → `broker_connected`

---

## Metric

**Primary:** Signup start → first trade visible in journal (manual CSV or sync)

**Secondary:** Payment complete → broker connected within 7 days

**How to measure:** Stripe webhook + in-app events on `csv_import_complete` and `broker_sync_first_run`

---

## Links

| Asset | URL |
|-------|-----|
| Teardown (this doc) | https://github.com/rixter145/pm-activation-teardowns/blob/main/trade-journal-ai/teardown.md |
| Playwright audit JSON | [audit_2026-07-06.json](./audit_2026-07-06.json) |
| Product signup | https://tradejournal.ai/signup |
| Pricing FAQ | https://tradejournal.ai/pricing |

---

*Prepared by Ricardo Lo — value-first PM activation work.*
