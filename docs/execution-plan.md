Taxly MVP: Engineering Execution Plan

This document outlines the complete technical strategy, architecture, and 7‑day build plan to deliver the Taxly MVP. It is designed to be immediately actionable by a small, high‑velocity engineering team.

## 1. Execution Strategy
### 1.1. Architecture & Stack
We will build a modern, scalable, and secure Progressive Web App (PWA) using a Jamstack architecture. This approach prioritizes performance, developer experience, and rapid iteration.

**Frontend:** Next.js (with TypeScript) deployed on Vercel.
- *Why:* Provides a best‑in‑class developer experience, static site generation (SSG) for marketing pages, server‑side rendering (SSR) for dynamic app views, and an integrated, zero‑config deployment pipeline. TypeScript ensures type safety, reducing runtime errors.

**Backend & Database:** Supabase (Postgres, Auth, Storage, Edge Functions).
- *Why:* Supabase is an open‑source Firebase alternative that gives us a powerful Postgres database, authentication, file storage, and serverless functions out of the box. Its Row‑Level Security (RLS) is critical for ensuring data privacy and is a core part of our security model.
- *Trade‑off (vs. Firebase):* While Firebase is mature, Supabase's use of standard Postgres offers greater flexibility, data portability, and a more familiar SQL interface, which de‑risks future data engineering needs. The integrated stack significantly reduces infrastructure overhead for an MVP. **Decision:** Lock in Supabase.

**Styling:** Tailwind CSS with shadcn/ui components.
- *Why:* A utility‑first CSS framework allows for rapid UI development without leaving HTML. shadcn/ui provides accessible, unstyled components that we can quickly theme to match our brand, avoiding the overhead of a heavy component library.

**Key Libraries:**
- **State Management:** zustand (lightweight, simple).
- **Forms:** react‑hook‑form with zod for validation.
- **Data Fetching:** react‑query (via tRPC for end‑to‑end typesafe APIs).
- **PDF Generation:** @react‑pdf/renderer.
- **Analytics:** PostHog (for product analytics), Sentry (for error tracking).
- **Testing:** Jest/Vitest (unit), Playwright (E2E).

### 1.2. Guiding Principles (MVP)
- **Compliance & Security First:** All tax calculations must be driven by a version‑controlled config file. All user data must be protected by strict RLS policies. Legal disclosures are non‑negotiable.
- **P0 Features Only:** We will ruthlessly prioritize the core user journey: Calculate → Save Goal → Track Expenses → Estimate → Report. Anything else is a distraction.
- **Config‑Driven Logic:** Tax rates, thresholds, and deduction categories will be managed in a versioned JSON file (`tax_config_fy2025.json`). No hard‑coded tax logic. This allows for quick updates without redeploying the entire application.
- **Mobile‑First, PWA‑Ready:** The UI must be flawless on mobile. We will implement the necessary service workers and manifest files to ensure the app is installable.
- **Demo‑Ready at All Times:** The main branch must always be deployable. We will use a demo user account seeded with realistic data to facilitate investor and user demos.

## 2. Work Breakdown (Epics → Stories → Tasks)

### Epic 1: Foundation & Infrastructure (**P0**)
**User Story:** As a Developer, I need a secure, scalable, and automated foundation so that I can build and deploy features efficiently.

**Tasks:**
- **INFRA‑01:** Initialize Supabase project (DB, Auth, Storage).
  - **AC:** Project created, API keys and DB URL stored securely in project secrets.
- **INFRA‑02:** Set up Next.js project with TypeScript, Tailwind CSS, and shadcn/ui.
  - **AC:** Project runs locally. `pnpm install` works. Tailwind theme configured with brand tokens.
- **INFRA‑03:** Configure Vercel deployment pipeline connected to GitHub.
  - **AC:** Pushing to `main` deploys to production. Pushing to `develop` deploys to staging.
- **INFRA‑04:** Implement database schema using Supabase SQL migrations.
  - **AC:** All tables from the PRD data model are created. RLS is enabled on all user‑data tables.
- **INFRA‑05:** Implement basic email/password and Google OAuth authentication with Supabase Auth.
  - **AC:** Users can sign up, log in, and log out. A `users` table row is created on signup.
- **INFRA‑06:** Set up base RLS policies for all tables (user can only access their own data).
  - **AC:** Write and run SQL tests to verify that `user_id = auth.uid()` policies are enforced.

### Epic 2: Core Tax Calculation Engine (**P0**)
**User Story:** As a Freelancer, I want to accurately calculate my weekly tax set‑asides so I can save with confidence.

**Tasks:**
- **CALC‑01:** Create `tax_config_fy2025.json` with schema and placeholder values.
  - **AC:** JSON file exists and is validated by a schema (`.schema.json`).
- **CALC‑02:** Build config loader service (`/lib/tax/config.ts`).
  - **AC:** Service loads and validates the JSON config at build time. Throws an error if config is invalid.
- **CALC‑03:** Implement core tax calculation functions (`/lib/tax/calculations.ts`).
  - **AC:** Pure functions exist for `calculateIncomeTax()`, `calculateMedicareLevy()`, `calculateHecs()`. All are driven by the loaded config.
- **CALC‑04:** Write comprehensive unit tests for all calculation functions.
  - **AC:** Tests cover zero income, bracket boundaries, and edge cases for all tax components. 100% test coverage on this module.
- **CALC‑05:** Create the `/api/calc/weekly` Supabase Edge Function.
  - **AC:** Endpoint accepts `weeklyIncome` and user toggles, returns a JSON object with component breakdowns and a total.

### Epic 3: User Interface & Experience (**P0/P1**)
**User Story:** As a User, I want a clean, intuitive, and responsive interface to manage my finances.

**Tasks:**
- **UI‑01 (P0):** Build the Weekly Calculator form component.
  - **AC:** Form includes input for weekly income and toggles for GST, HECS, and Super. Uses `react-hook-form` and `zod` for validation.
- **UI‑02 (P0):** Build the Calculator Results card component.
  - **AC:** Displays breakdown of tax components. Includes "Save as Weekly Goal" button and legal disclaimer.
- **UI‑03 (P0):** Implement the "Save Goal" functionality.
  - **AC:** Clicking "Save as Weekly Goal" writes a new record to the `weekly_goals` table for the user.
- **UI‑04 (P0):** Build the main Dashboard UI.
  - **AC:** Displays YTD earned, tax saved, and "on‑track" status. Includes a placeholder for the business health pie chart.
- **UI‑05 (P1):** Build the Business Health pie chart using Recharts.
  - **AC:** Chart correctly visualizes saved/spent/profit based on user data.
- **UI‑06 (P0):** Build the Expenses page with CRUD functionality.
  - **AC:** User can add, view, edit, and delete expenses. Form includes category, amount, and receipt upload.
- **UI‑07 (P0):** Implement receipt upload to Supabase Storage.
  - **AC:** Uploaded files are stored in a user‑scoped folder (`/receipts/{user_id}/{expense_id}`). A signed URL is used for secure access.
- **UI‑08 (P1):** Build the Deductions Assistant UI.
  - **AC:** Displays profession‑based suggestions. "Smart Deduction Meter" updates as items are tracked.
- **UI‑09 (P0):** Build the Tax Estimator UI.
  - **AC:** Displays a high‑level refund/payable estimate and a checklist of missing info. Includes disclaimer.
- **UI‑10 (P0):** Build the EOFY Report generation flow.
  - **AC:** A button triggers a serverless function to generate a PDF. User can download the PDF or email it.

### Epic 4: Reporting & Exporting (**P0**)
**User Story:** As a Freelancer, I want to generate a clean EOFY pack to send to my accountant.

**Tasks:**
- **RPT‑01:** Create a Supabase Edge Function for the `/api/estimator`.
  - **AC:** Function aggregates user's income, expenses, and contributions to provide a rough estimate.
- **RPT‑02:** Design the PDF report template using `@react-pdf/renderer`.
  - **AC:** Component renders a clean summary of income, expenses, tax saved, and the final estimate.
- **RPT‑03:** Create the `/api/reports/eofy` Edge Function to generate and return the PDF.
  - **AC:** Function fetches user data, renders the React component to a PDF stream, and returns it.
- **RPT‑04:** Implement "Email to Accountant" functionality using Resend via an Edge Function.
  - **AC:** Function takes an email address, attaches the generated PDF, and sends the email. A success/error toast is shown to the user.

## 3. Dependencies & Sequencing
The project can be heavily parallelized.

**Path 1 (Backend/Infra):** INFRA‑01 → INFRA‑04 → INFRA‑06 → CALC‑01 → CALC‑02 → CALC‑03 → CALC‑04 → CALC‑05 → RPT‑01 → RPT‑03.
- This path focuses on setting up the entire backend foundation and core business logic. Can be done by one backend-focused engineer.

**Path 2 (Frontend):** INFRA‑02 → INFRA‑03 → INFRA‑05 → UI‑01 → UI‑02 → UI‑03 → UI‑04 → UI‑06 → UI‑07 → UI‑09 → UI‑10.
- This path focuses on building all the UI components and pages, initially against mock data, then integrating with the backend as endpoints become available. Can be done by one/two frontend-focused engineers.

**Critical Path:** The core calculation engine (CALC tasks) must be completed before the frontend calculator (UI‑01/02) can be fully integrated and tested. The dashboard (UI‑04) depends on goals being saved (UI‑03). The estimator (RPT‑01) depends on expenses being tracked (UI‑06).

## 4. 7‑Day Sprint Plan (MVP)
**Team:** 1 Backend/Infra Dev, 1 Frontend Dev, 1 Founder (acting as PM/QA).

| Day | Backend/Infra (Dev 1) | Frontend (Dev 2) | Founder (PM/QA) | Milestone |
|---|---|---|---|---|
| **Day 1** | (P0) INFRA‑01, 02, 04. Set up Supabase, Next.js, DB Schema. | (P0) INFRA‑03, 05. Set up Vercel, Auth UI flow. | Finalize `tax_config_fy2025.json` values. Prepare test cases. | Infra Ready |
| **Day 2** | (P0) INFRA‑06, CALC‑01, 02, 03. Implement RLS & core tax logic. | (P0) UI‑01, 02. Build Calculator form & result card (mock data). | Review UI components. Write E2E test scripts. | Core Logic Done |
| **Day 3** | (P0) CALC‑04, 05. Unit test logic & build API endpoint. | (P0) UI‑03, 04. Integrate calculator API, build Save Goal & Dashboard UI. | Manual QA on Calculator flow. Seed demo user data. | Calculator E2E |
| **Day 4** | (P0) RPT‑01. Build Estimator API. | (P0) UI‑06, 07. Build Expenses CRUD page with file uploads. | Manual QA on Dashboard & Expenses. Document demo script. | Demo Ready |
| **Day 5** | (P0) RPT‑02, 03. Build PDF generation template & API. | (P0) UI‑09. Build Estimator UI and integrate API. | Manual QA on Estimator. Accessibility check (keyboard nav, labels). | Features Complete |
| **Day 6** | (P0) RPT‑04. Build "Email to Accountant" API. (P2) Bug fixes/polish. | (P0) UI‑10. Build EOFY Report UI. (P1) UI‑05. Add pie chart. | Full E2E QA. Run through all user flows. Performance check (Lighthouse). | QA Start |
| **Day 7** | (P0) Final bug fixes. Deploy to production. Monitor logs. | (P0) Final UI polish. Add analytics events (PostHog). | Final sign‑off. Prepare Loom walkthrough video. | Deploy & Launch |

## 5. Resource Allocation
- **Founder (PM/QA):** Owns the PRD, defines priorities, performs all manual QA, prepares demo materials, and validates that the final product meets the business objectives.
- **Dev 1 (Backend/Full‑Stack):** Owns the entire Supabase setup, database schema, serverless functions, core tax logic, and RLS policies.
- **Dev 2 (Frontend/Full‑Stack):** Owns the entire Next.js application, all UI components, state management, data fetching, and styling.

This split allows for maximum parallelization. The two developers should have a daily sync to align on API contracts.

## 6. Testing & QA Strategy
- **Unit Tests (Jest/Vitest):** Target all functions in `/lib/tax/calculations.ts`. Coverage must be 100%. Test all income brackets, toggles (GST/HECS), and edge cases (zero, negative incomes).
- **Integration Tests:** Target Supabase Edge Functions. Write tests to ensure API endpoints return expected data structures and respect RLS policies.
- **End‑to‑End (E2E) Tests (Playwright):**
  - **Flow 1:** Sign up → Run Calculator → Save Goal → Verify Dashboard updates.
  - **Flow 2:** Log in → Add an Expense with a receipt → Verify it appears in the list.
  - **Flow 3:** Log in → Run Estimator → Generate EOFY Report PDF → Download and verify content.
- **Manual QA Checks:**
  - **Accessibility:** Run axe on every page; ensure keyboard navigation is seamless.
  - **Performance:** Run Lighthouse on key pages (Dashboard, Calculator). Target >90 for Performance on mobile.
  - **Compliance:** Verify legal disclosures present on Calculator, Estimator, and final PDF Report.

## 7. Risk Log & Mitigations
| Risk | Impact | Likelihood | Mitigation |
|------|--------|------------|------------|
| Incorrect Tax Calculations | Critical | Medium | 100% unit test coverage on all calculation logic; config-driven rates; feature flag to disable calculator if config invalid. |
| Data Breach / Security Flaw | Critical | Low | RLS enforced on all tables; peer review of policies; signed URLs; secure secrets. |
| 7‑Day Timeline Slip | High | High | Drop P1/P2 tasks if necessary; maintain demo-ready state from Day 4; ruthlessly prioritise P0 tasks. |
| Third‑Party Service Failure (Supabase/Vercel) | Medium | Low | Accept risk for MVP; set up Sentry for immediate notifications; plan fallback tasks (mock data). |

## 8. Final Deliverables
- **Working MVP:** Deployed live on Vercel (frontend) and Supabase (backend).
- **Demo Data:** A pre‑configured user account seeded with realistic income, expenses, and goals to showcase all features.
- **Loom Walkthrough:** A 5‑minute video demonstrating the end-to-end user flow, recorded by the founder.
- **Passing Tests:** All P0 unit and E2E tests passing in the CI/CD pipeline. The final deployment is gated on this.
