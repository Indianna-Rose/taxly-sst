# Product Requirements Document (PRD) — Taxly MVP (10× Strengthened)

## North Star
**Simple, Smart, Sorted.** Deliver a clean, demo-ready MVP that calculates weekly set-asides, tracks progress, captures expenses & deductions, estimates EOFY outcomes, and exports an accountant-ready pack — fast, safe, and compliant.

## 1) Overview & Objectives

### Problem
Freelancers/sole traders juggle multiple income streams, no PAYG withholding, fragmented tools, and low confidence—leading to under-saving, last-minute panic, and avoidable tax debt. (Target segment: Australian ABN holders; side-hustlers with PAYG+ABN; early adopters in rideshare, tradies, and independent creators.)

### Vision
A mobile-first PWA that proactively tells you **what to save this week**, keeps you **on track**, surfaces **profession-specific deductions**, and generates a **one-tap EOFY pack**. Brand: friendly, human, built for freelancers, not accountants.

### Success Criteria (first 30 days post-launch MVP)
- **Activation:** ≥70% of new users save a Weekly Goal in first session.
- **Engagement:** ≥50% add ≥1 expense within 7 days; ≥35% return within 7 days.
- **Output:** ≥30% generate an EOFY Report (demo); ≥20% run the Estimator.
- **CSAT:** In-app CSAT ≥4.5/5; refund/payable estimate comprehension ≥85% (survey).
- **Risk/Compliance:** 0 P1 incidents; 100% screens show disclaimer where required.

## 2) Scope (MVP vs Later)

### In-Scope (MVP week‑ 1, buildable in 7 days)
1. **Weekly Tax Saving Calculator** (income → weekly set-asides; toggles: GST, HECS/HELP, Super).
2. **Dashboard Summary** (YTD earned, tax saved so far, on-track status, business-health pie).
3. **Goal Breakdown** (total goal, component goals, monthly contributions, reminder toggle).
4. **Smart Expenses (lite)** (CRUD, category, receipt upload, “likely deductible” rule tag).
5. **Deductions Assistant (lite)** (profession presets, Smart Deduction Meter, checklist, add custom).
6. **Rough Tax Return Estimator (lite)** (high-level refund/payable + missing-info checklist).
7. **AI EOFY Reports Pack (lite)** (downloadable PDF; email to accountant).

### Explicitly Out‑of‑Scope (stubbed nav only)
- Invoicing.
- Budget Planner.
- Bank links/auto-transfer.
- Premium tier.
- Super planner.
- Financial readiness score.

## 3) Personas & Top Jobs‑To‑Be‑Done

### Persona A — Freelancer/Contractor (ABN)
- **JTBD-1:** Know how much to set aside weekly so I never get a tax shock.
- **JTBD-2:** Track income from multiple gigs in one place.
- **JTBD-3:** Capture receipts fast and tag likely deductions.
- **JTBD-4:** See if I’m **on track** and what to fix if I’m behind.
- **JTBD-5:** Hand a clean EOFY pack to my accountant.

### Persona B — Side-Hustler (PAYG + ABN)
- **JTBD-1:** Understand ABN tax impact on top of my salary.
- **JTBD-2:** Toggle HECS/HELP and see repayment effect.
- **JTBD-3:** Get profession-specific deduction guidance.
- **JTBD-4:** Receive weekly nudges/reminders to save.
- **JTBD-5:** Generate a one-tap EOFY summary.

## 4) User Stories & Acceptance Criteria (Gherkin)

> 32 stories spanning the 7 MVP features (abbrev. below—testable, concise).

### Weekly Calculator
1. **Calculate weekly set‑asides (GST=Yes, HECS=On).**
   - Given I entered weekly income “1250”, GST “Yes”, HECS “On”, Super “11%”.
   - When I press **Calculate**.
   - Then I see rows for **Income Tax**, **GST**, **Medicare Levy**, **HECS**, **Super** with non‑negative amounts.
   - And I see **Save as Weekly Goal** and **Reminder** toggle.

2. **Validate numeric input & bounds.**
   - Given the income field is empty or “‑50”.
   - When I press **Calculate**.
   - Then I see an inline error “Enter a positive amount” and no result card.

3. **Annualisation correctness.**
   - Given income “2000” per week.
   - When I calculate.
   - Then the module annualises at 52 weeks for tax/levy/HECS logic (configurable).

4. **HECS toggle off removes repayment.**
   - Given HECS “Off”.
   - When I calculate.
   - Then **HECS/HELP** row shows — and $0.

5. **Save as Weekly Goal.**
   - Given a visible result card.
   - When I toggle **Save as Weekly Goal**.
   - Then a goal record is stored and next **Dashboard** reflects this.

### Dashboard & Goals
6. **On‑track status.**
   - Given saved weekly goals total $400 and I contributed $200 by mid‑week.
   - When I open **Dashboard**.
   - Then the status reads **“You’re on track”** if projected contributions ≥ goal by week end.

7. **Business Health pie.**
   - Given contributions and expenses exist.
   - When I open **Dashboard**.
   - Then I see a pie of saved/spent/profit with legend.

8. **Goal Breakdown detail.**
   - Given a saved goal.
   - When I open **Goal Breakdown**.
   - Then I see component bars for Income Tax, GST, Super with % complete and monthly list.

9. **Weekly reminder toggle.**
   - Given I’m on Goal Breakdown.
   - When I toggle **Weekly Reminder**.
   - Then I receive a local PWA notification on the chosen day/time.

### Expenses (lite)
10. **Add expense with deduction hint.**
    - Given I open **Expenses**.
    - When I add category “Internet”, amount “70.00”, upload a receipt.
    - Then it appears in list and is flagged **Likely deductible**.

11. **Edit/Delete expense.**
    - Given an existing expense.
    - When I edit/delete.
    - Then changes persist and totals refresh.

12. **Empty state.**
    - Given I have no expenses.
       When I open **Expenses**.
       Then I see a friendly prompt to add the first expense.

### Deductions Assistant (lite)
13. **Preset by profession.**
    - Given my profile profession is “Freelancer”.
    - When I open **Deductions**.
    - Then I see 5–7 suggested categories & a **Smart Deduction Meter**.

14. **Mark as tracked increases meter.**
    - Given 3/7 tracked.
    - When I mark another suggestion as tracked.
    - Then the meter updates to 4/7.

15. **Add custom deduction.**
    - Given I need a custom item.
    - When I add title/notes.
    - Then it appears in checklist and counts toward the meter.

### Estimator (lite)
16. **Refund/payable estimate.**
    - Given income + expenses + goals exist.
    - When I open **Estimator**.
    - Then I see **Refund/Payable** range and a **Missing info** checklist.

17. **Estimator disclaimer.**
    - Given I view results.
    - Then I see **“General information only, not tax advice.”** banner.

### EOFY Report (lite)
18. **Generate PDF.**
    - Given data exists.
    - When I tap **Generate EOFY Report**.
    - Then I can download a PDF with Income, Tax Saved, Expenses, and the final estimate.

19. **Send to accountant.**
    - Given a generated report.
    - When I tap **Email to Accountant** and enter address.
    - Then the PDF is sent and I see success confirmation.

### Cross‑cutting
20. **Auth gating** (basic email/OAuth).
21. **RLS security** (only my rows visible).
22. **Offline expense add** (queues until online).
23. **Settings saves** (GST/HECS/Super %).
24. **Accessibility labels** (WCAG AA).
25. **Input formats** (AUD, dd/mm/yyyy).
26. **Error surfaces** (400/401/500).
27. **Help tooltips** (calc & estimator).
28. **Demo data toggle** (for investor demos).
29. **Analytics events fire**.
30. **Config validation** (rates JSON schema).
31. **PII export/delete** (Settings → Privacy).
32. **Legal banners** visible on calc/estimator/report.

## 5) End‑to‑End User Flows (textual)

**Happy Path:** Sign‑up → Profile (GST/HECS/Super, profession) → Weekly Calculator → Save Goal → Dashboard shows on‑track → Add Expense (receipt) → Deductions Assistant (mark tracked) → Estimator (range + checklist) → Generate/Email EOFY PDF.

**Edge/Error States:**
- **No Goal:** Dashboard shows empty state + CTA **Create your weekly plan**.
- **Behind on Track:** Status flips to **“You need to save more”** with CTA **Top up now**.
- **Offline:** Expenses create locally; upload & sync when back online.
- **Invalid Config:** Calculator disabled with “Rates unavailable—please try later.”

## 6) UX Requirements & Visual System

**Component Inventory:** Card, Stat, Progress bar, Pie (Recharts), Tile (deductions), Checklist, File upload, Sticky CTA, Tooltip, Toast, Empty state.

**Navigation:** Bottom tabs (Home, Weekly Calc, Tips, More); top‑right menu (Settings/Legal).

**Accessibility:** WCAG 2.1 AA; focus states; input labels; minimum tap targets.

**Microcopy Tone:** Human, encouraging, no jargon. Examples: “Nice! You’re on track.” “Let’s sort this.”

### Design Tokens

| Token        | Value                                  |
|--------------|----------------------------------------|
| navy         | #00115C                                |
| ctaBlue      | #2457C5                                |
| beige        | #FFFDF9                                |
| softBlue     | #EAF3F8                                |
| white        | #FFFFFF                                |
| radius       | 12px (cards/buttons)                   |
| shadow       | 0 2px 6px rgba(0,0,0,0.10)            |
| spacingScale | 4, 8, 12, 16, 24, 32                   |

## 7) Functional Specifications (per feature)

### Weekly Calculator
- **Inputs:** weekly income (number), GST (Y/N), HECS (Y/N), Super % (0‑20), State, Business type.
- **Outputs:** set‑asides for Income Tax, GST, Medicare Levy, HECS, Super; total weekly save.
- **Validation:** income > 0; Super % within bounds; toggles respected.
- **Logic:** annualise weekly → apply FY bracket config → compute levy/HECS → compute GST (if registered) → super suggestion from user % (not advice).
- **Reminders:** weekly local PWA notification (day/time in Settings).
- **Disclaimers:** persistent banner.

### Dashboard
- **Cards:** YTD earned (sum incomes), Tax saved so far (sum contributions), On‑track status (projection vs goal), Business health pie.
- **Rules (on‑track):** if (current_week_contrib + scheduled_reminder) ≥ weekly_goal → on‑track else behind.
- **Empty states:** friendly CTAs.

### Goal Breakdown
- **View:** total goal; component bars with %; contributions list (month‑key).
- **Actions:** toggle reminder; add manual contribution.

### Expenses (lite)
- **CRUD:** date, category, amount, note, receipt (image/pdf to storage).
- **Rule tag:** `deductible_likely = true` for mapped categories (configurable).
- **Monthly insight:** “Where you can save” (rule‑based sentence generator).

### Deductions Assistant (lite)
- **Profiles:** profession → suggested categories (config); typical evidence; mini‑guides (markdown).
- **Meter:** `tracked_count / typical_count`.
- **Actions:** mark as tracked; add custom.

### Estimator (lite)
- **Inputs:** incomes, expenses, goals (proxy for set‑asides), HECS toggle.
- **Outputs:** refund/payable **range** (conservative vs optimistic); missing‑info checklist.
- **Disclaimers:** visible.

### EOFY Report (lite)
- **PDF sections:** Income, Tax saved, Expenses, Est. refund/owed, Notes/Disclaimers.
- **Delivery:** download; email via backend function (with audit log).

## 8) Calculation Logic (AUS, configurable)

- **Income Tax:** annual income via `weekly * 52`; apply resident brackets: `base_tax + marginal_rate * (amount – threshold)`.
- **Medicare Levy:** base rate (default 2%); low‑income & family thresholds; phase‑in slope.
- **GST:** 10% of taxable sales (if registered).
- **HECS/HELP:** banded repayment % by annual income (toggle controls inclusion).
- **Super:** user‑defined % × income.
- **Rounding:** nearest dollar for display; store cents internally.

### Config Schema (keys only, placeholders):
```json
{
  "meta": { "jurisdiction": "AU", "financial_year_label": "FY2025" },
  "rates": {
    "income_tax": { "resident": [{ "min": 0, "max": 0, "base_tax": 0, "marginal_rate": 0.0 }] },
    "medicare_levy": { "base_rate": 0.02, "low_income_threshold": 0, "phase_in_upper": 0 },
    "hecs_help": { "bands": [{ "min": 0, "max": 0, "rate": 0.00 }] },
    "gst": { "rate": 0.10 },
    "super": { "suggested_default_percent": 0.11, "min_percent": 0.00, "max_percent": 0.20 }
  }
}
```
Populate from official FY sources before launch; ship with placeholders + JSON‑schema validation in CI.

## 9) Data Model (MVP ERD tables)

- **users**(id, email, created_at, display_name, state, business_type, gst_registered bool, hecs_enabled bool, super_rate numeric, profession text)
- **incomes**(id, user_id, date, source text, gross_amount numeric, gst_collected numeric, notes text)
- **weekly_goals**(id, user_id, week_start_date, income_estimate numeric, income_tax_goal numeric, gst_goal numeric, super_goal numeric, medicare_goal numeric, hecs_goal numeric)
- **contributions**(id, user_id, date, type enum('income_tax','gst','super','medicare','hecs'), amount numeric, month_key text)
- **expenses**(id, user_id, date, category text, amount numeric, deductible_likely bool, note text, receipt_url text)
- **deduction_profiles**(id, profession text, category text, guide_md text, typical_evidence text, typical_count int)
- **tracked_deductions**(id, user_id, category text, is_tracked bool, custom_title text, notes text)
- **config_tax_years**(id, fy_label text, start_date date, end_date date)
- **config_rates**(id, fy_id, type text, key text, value jsonb)
- **tips**(id, audience text, title text, body text, created_at)

**Relationships & RLS:** `user_id` FK on all user tables; RLS: row‑owner read/write only; storage paths scoped per user.

## 10) APIs & Services (MVP)

### Compute Endpoints
- **POST /api/calc/weekly** → `{ inputs }` → `{ components, total }`
- **POST /api/estimator** → `{ incomes, expenses, toggles }` → `{ refundRange, missingChecklist }`
- **POST /api/reports/eofy** → `{ userId, fy }` → `{ pdfUrl }`

### CRUD Endpoints
- `GET/POST/PUT/DELETE /api/expenses`
- `GET/POST /api/goals` (save weekly goal; list by week)
- `GET/POST /api/contributions`
- `GET /api/dashboard` (aggregates)

### Example
```json
// /api/calc/weekly (req)
{ "weeklyIncome": 1250, "gstRegistered": true, "hecsEnabled": true, "superRate": 0.11, "state": "NSW", "businessType": "sole_trader" }

// (res)
{
  "components": {
    "incomeTax": 240.00,
    "gst": 125.00,
    "medicare": 25.00,
    "hecs": 15.00,
    "super": 137.50
  },
  "total": 542.50,
  "disclaimer": "General information only, not tax advice."
}
```

**Error codes & retries:**
- `400`: invalid input (field level messages).
- `401`: unauthenticated; `403`: RLS deny.
- `409`: config missing; `503`: downstream unavailable (retry with backoff).
- Offline: queue writes, replay on reconnect.

## 11) Non‑Functional Requirements

- **Performance:** TTI < 3s on 4G; p95 compute endpoints < 250ms; PDF generation < 5s.
- **Security:** OIDC auth; RLS enforced; AES‑256 at rest; HTTPS/TLS 1.2+; secrets in env.
- **Reliability:** 99.5% (MVP demo period); zero data loss for queued writes.
- **Privacy:** APP compliance; user data portable/deletable; PII export/delete in Settings.
- **Observability:** Sentry (errors), PostHog (events); uptime ping; audit log for report emails.
- **Accessibility:** WCAG 2.1 AA verified via axe checks in CI.

## 12) Analytics & Metrics

**Events** (with core props)
- `signup` { method }
- `calc_run` { weeklyIncome, gstRegistered, hecsEnabled }
- `goal_saved` { totalWeekly, components }
- `contribution_added` { type, amount }
- `expense_added` { category, amount, receipt: bool }
- `deduction_tracked` { category, isCustom }
- `estimator_run` { inputsCompleteness }
- `report_generated` { fy, pages }
- `report_emailed` { toDomain }

**North‑Star:** % of new users generating EOFY report within 30 days.

**Activation Funnel:** signup → calc_run → goal_saved → expense_added → estimator_run → report_generated.

**Dashboards:** DAU/MAU, 7‑day retention, completion rates, error budgets, CSAT.

## 13) Compliance, Risk & Legal

- **Disclaimers:** On Calculator, Estimator, Report—“General information only, not tax advice.”
- **Config Accuracy Risk:** All rates pulled from config; CI JSON‑schema validation; feature flag to disable calculator if config invalid.
- **Privacy (APP):** Minimum data; purpose‑limited; retention policy; data portability & delete.
- **Security Risk:** RLS tests in CI; signed URLs for receipts; least‑privilege keys.
- **Misinterpretation Risk:** Tooltips + “Learn more” links; conservative estimator bands.

## 14) Release Plan (7‑Day MVP)

**Day 1 — Infra:** Next.js + TS + Tailwind; Supabase (Auth, Postgres, Storage); RLS; `.env.example`; CI (lint/test); Sentry/PostHog.

**Day 2 — Core Logic:** `lib/tax/*` (incomeTax, medicare, hecs, gst, super); unit tests; config loader with schema.

**Day 3 — Weekly Calc UI:** Form + result card; Save Goal; Reminder toggle (local PWA); validations.

**Day 4 — Dashboard & Goals:** Cards, on‑track logic, business pie; Goal Breakdown page.

**Day 5 — Expenses & Deductions:** CRUD + uploads; rule‑based “likely deductible”; Assistant (meter + checklist + custom).

**Day 6 — Estimator & Report:** Estimator (range + checklist); PDF (React‑PDF); Email to accountant; Disclaimers.

**Day 7 — Polish & QA:** Accessibility, axe pass; demo data toggle; analytics events; deploy to Vercel; smoke E2E (Playwright).

**Environments:** Staging (seed data) / Production (gated).

**Go/No‑Go:** All P0 tests pass; no open P1 bugs; CSAT dry‑run ≥4.5.

## 15) Open Questions & Assumptions

- FY seed: start with FY2025 placeholders—confirm final rates source & update cadence.
- Initial profession presets: Freelancer, Tradie, Rideshare—confirm top 7 categories each.
- Email delivery: Supabase function vs Resend—choose based on deliverability in staging.
- Estimator range method: define ±band (conservative vs optimistic) heuristic.
- Consent language for emailing accountant & including totals—legal review.

## 16) Appendices

### A) Sample Config JSON Skeleton
See `fy_2025_au.json` and `fy.schema.json` in this repo. These files provide placeholders and validation for tax rates and thresholds. Populate from official sources before launch and validate via JSON schema in CI.

### B) Test Plan Summary
- **Unit:** All `lib/tax/*` functions (band edges, monotonicity, toggles).
- **E2E (Playwright):**
  - (1) signup → calc → save goal → dashboard on‑track.
  - (2) expense add (receipt) → deductions meter tick.
  - (3) estimator → generate/email report.
- **Accessibility:** axe; keyboard nav; contrast checks.
- **Perf smoke:** Lighthouse > 90 mobile; p95 API < 250ms.

### C) Glossary
- **ATO** – Australian Taxation Office.
- **BAS** – Business Activity Statement.
- **GST** – Goods & Services Tax.
- **HECS/HELP** – student loan repayment.
- **Super** – superannuation retirement fund.
- **Set‑aside** – amount put aside each week for future tax liabilities.
- **EOFY** – end of financial year.
- **RLS** – Row-Level Security.
- **PWA** – Progressive Web App.

### D) Definition of Done (MVP)
- All MVP features implemented & covered by tests.
- JSON config validated in CI; **no hard‑coded tax rates**.
- Legal disclaimers present on calculator, estimator, and report screens.
- Deployment live with demo data and Loom walkthrough.
- Metrics dashboard live; Sentry zero P1 alerts in last 24h.

---

**Brand, mission, target users, and GTM emphases above are grounded in the Taxly Business Proposition Report.**
