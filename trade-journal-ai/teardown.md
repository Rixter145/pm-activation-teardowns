# Trade Journal AI — Activation Teardown

**Trade Journal AI** · tradejournal.ai · July 2026

---

## TL;DR

**Problem:** Homepage promises a **7-day free trial**, but signup forces **Stripe payment before password or dashboard**, and pricing FAQ says **no free trial** — three conflicting signals that truncate trust before activation.

**Proposed fix:** Align all surfaces to **14-day money-back guarantee** (drop "7-day trial" copy); add a **CSV-import-first path** on signup so traders see journal value before broker API connect; optional **reverse trial** (card on file, charge day 8) if you want trial language back.

---

## Evidence

| Step | What happened | Screenshot |
|------|---------------|------------|
| 1 | Homepage hero advertises "7-day free trial" alongside 14-day money-back guarantee | [step_1_homepage_trial_claim.png](./screenshots/step_1_homepage_trial_claim.png) |
| 2 | Signup requires plan selection and payment before password; no product preview | [step_2_signup_pay_before_password.png](./screenshots/step_2_signup_pay_before_password.png) |
| 3 | Pricing FAQ states there is no free trial — contradicts homepage | [step_3_pricing_no_trial_faq.png](./screenshots/step_3_pricing_no_trial_faq.png) |
| 4 | Email + plan routes directly to Stripe checkout before dashboard access | [step_4_stripe_before_product.png](./screenshots/step_4_stripe_before_product.png) |
| 5 | Post-payment, core value requires broker API connect or CSV import — second wall after pay | — |

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
- Measure funnel: signup start → payment complete → first trade in journal → broker connected

---

## Metric

**Primary:** Signup start → first trade visible in journal (manual CSV or sync)

**Secondary:** Payment complete → broker connected within 7 days

**How to measure:** Stripe webhook + in-app events on csv import complete and broker sync first run

---

## Links

| Resource | URL |
|----------|-----|
| Signup | https://tradejournal.ai/signup |
| Pricing | https://tradejournal.ai/pricing |
| Brokers | https://tradejournal.ai/brokers |
