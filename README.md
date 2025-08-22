# Taxly Single Source of Truth (SST)

This repository serves as the **single source of truth** for the Taxly MVP, a mobile‑first tax autopilot for Australian freelancers, sole‑traders and side hustlers. It centralises our product requirements, execution plan, configuration schemas and design tokens so that everyone — product, design and engineering — works off the same set of documents.

## Contents

| File | Purpose |
| --- | --- |
| [`docs/PRD.md`](docs/PRD.md) | The full **Product Requirements Document** (PRD) for the Taxly MVP, including north star, scope, personas & jobs-to-be-done, user stories with acceptance criteria, UX requirements, functional specs, data model, API & services, non‑functional requirements, analytics & metrics, legal/compliance, release plan and appendices. |
| [`docs/execution-plan.md`](docs/execution-plan.md) | The **Engineering Execution Plan** that translates the PRD into architecture decisions, guiding principles, epics, tasks, a 7‑day sprint schedule, testing strategy, risk log and final deliverables. |
| [`docs/api-contracts.md`](docs/api-contracts.md) | Example **API contracts** for the MVP endpoints (`/api/calc/weekly`, `/api/estimator`, `/api/reports/eofy`) with request/response payloads and fields. |
| [`docs/fy.schema.json`](docs/fy.schema.json) | JSON Schema describing the structure of financial year configuration files (e.g. required `meta` and `rates` keys). Useful for validating config files in CI/CD. |
| [`docs/fy_2025_au.json`](docs/fy_2025_au.json) | A placeholder configuration for the Australian FY2025 tax year. Defines meta info, income tax bands, levy rates, HECS/HELP bands, GST rate, super defaults, deduction profiles, UI tips and legal disclaimer. Populate with official ATO figures before release. |
| [`docs/design-tokens.json`](docs/design-tokens.json) | Design tokens defining the Taxly brand colour palette (navy, CTA blue, beige, soft blue, white), border radius values, shadows and spacing scale. These should be consumed by our frontend components to ensure visual consistency across the app. |

## Usage

- Treat this repository as a **read‑only reference** during implementation; do not hard‑code any values that should come from the configuration files.
- When implementing the Taxly MVP, refer to the PRD for requirements and acceptance criteria, and the execution plan for sequencing, naming conventions and testing guidelines.
- Any change to the tax calculation logic, financial year rates or design tokens should be updated in the corresponding JSON files here and reviewed via pull request.
- Use the API contract examples as a starting point for building server functions and to ensure consistent request/response shapes between client and backend.

## Contributing

Maintainers can update these documents through pull requests. Please follow these guidelines:

1. **Keep documents in sync.** If you update the PRD or execution plan, ensure related files (API contracts, config schemas) are updated accordingly.
2. **Use relative links.** Reference other files using relative paths (e.g. `docs/PRD.md`) so they resolve correctly on GitHub.
3. **Describe your changes.** Include a clear commit message and description explaining why the change was made and who reviewed it.
4. **Validate JSON.** Run JSON schema validation on any config changes before committing.

By maintaining this SST repository, the Taxly team can move quickly while ensuring everyone is aligned on what to build and how to build it.
