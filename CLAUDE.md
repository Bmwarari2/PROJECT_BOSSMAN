# CLAUDE.md — Thapsus CRM (PROJECT_BOSSMAN)

Thapsus CRM is the customer relationship product of the Thapsus suite: sales, service, marketing, partners and
customer success for UK, EU and US businesses from about 10 to 2,000 staff (SMB to mid-market). It is sold on its
own and as part of the suite with Thapsus-HR (`PROJECT_KAREN`), Thapsus Finance (`PROJECT_MULLAH`), Thapsus ERP
(`PROJECT_KASONGO`) and Thapsus Care (`ROCHE`). It owns customers, contacts, leads, opportunities, quotes and
marketing consent in the suite (`docs/suite/INTEGRATION.md` §4).

## Status: planning

Nothing is built yet. The plan is in `docs/`. The build starts when the owner approves `docs/BUILD_BRIEF.md`.
Until then, only planning documents change. Read first: `docs/PLAN.md`, `docs/FEATURES.md`,
`docs/suite/SUITE_PLAN.md`.

## How to work in this repo

- Work the phases in `docs/PLAN.md` in order. One branch per phase (`phase-<n>-<slug>`), small Conventional
  Commits (`feat(pipeline): …`, `test(consent): …`).
- Do not stop to ask the owner questions. Make the most reasonable choice, record it in `docs/DECISIONS.md`
  (dated ADR) and carry on. Human-only steps go in `docs/HUMAN_TODO.md` with exact numbered steps; build against
  mocks or sandboxes meanwhile.
- **Always ask first before:** deploying to production, deleting any cloud resource or data, or adding
  recurring cost beyond the estimate in `docs/COSTS.md`.
- Never tick a PLAN item whose acceptance criteria are not proved by a test or a check in `pnpm verify`, or by a
  HUMAN_TODO entry for human-only items. Report failures and skipped steps honestly.
- Suite documents in `docs/suite/` are identical in PROJECT_MULLAH, PROJECT_BOSSMAN and PROJECT_KASONGO. Change
  all three copies in one change.

## Scope guardrails

Out of scope: buying or reselling contact data, scraping websites or social networks, a general-purpose website
builder or CMS, an ad network, a telephony carrier (calls go through partners), payment processing (through
Finance and partners), and native mobile apps before the suite's native phase (ship an installable PWA).

## Non-negotiables

### Marketing and contact law
- Every marketing send checks the lawful basis, consent and suppression for the recipient's jurisdiction from
  the rule packs (UK PECR as amended by the Data (Use and Access) Act 2025, EU ePrivacy and GDPR, US CAN-SPAM,
  TCPA and state laws). A send with no valid basis is blocked, not warned.
- Unsubscribes take effect at once and are global for the tenant; every bulk email carries one-click unsubscribe
  (RFC 8058) and a working preference centre.
- Bulk sending requires SPF, DKIM and an aligned DMARC record on the sending domain; complaint and bounce rates
  are tracked and sending is paused above the thresholds in the rule pack.
- Call and meeting recording is off by default; when on, consent prompts follow the jurisdiction pack.
- Third-party enrichment is off by default and only through sub-processors listed in `docs/SUBPROCESSORS.md`.

### Privacy and data residency
- Thapsus is the processor; customers are controllers. UK GDPR and the Data Protection Act 2018 (as amended by the
  Data (Use and Access) Act 2025), EU GDPR, US state privacy laws.
- Home regions: **UK = europe-west2**, **EU = europe-west3** (backups only in europe-west1), **US = us-east5**
  (backups only in us-west1). All personal data, files, backups, logs, search indexes, caches, queues and emails
  stay there; enforced by tests for every region pair. No multi-region locations, no Cloud CDN for authenticated
  traffic, no Cloud Trace, Error Reporting or `_Default` log bucket.
- Mail is sent from the tenant's home region. Tracking pixels and link tracking are configurable per tenant and
  off where the recipient's jurisdiction requires consent.
- No personal data to a third party unless listed in `docs/SUBPROCESSORS.md` first.

### Security
- Tenant isolation with PostgreSQL row-level security, enabled and forced, proved by tests; the app role has no
  `BYPASSRLS` and owns no tables; tenant-to-database routing is configuration.
- Record-level sharing (owner, team, territory, role hierarchy, sharing rules) is enforced on every read path:
  API, search, reports, exports, embedded cards and AI retrieval. Tests prove it per path.
- MFA for admins; SSO can be enforced; sessions expire; sign-in rate-limited.
- Append-only audit log; exports of contacts are held for approval above a tenant threshold (as Thapsus-HR's held
  exports).
- OWASP ASVS level 2. Secrets only in Secret Manager. No service-account keys; GitHub uses workload identity.

### Quality and design
- WCAG 2.2 AA, including the customer portal and chat widget (European Accessibility Act).
- No hard-coded user-facing strings; en-GB and en-US at launch.
- Marketing and contact rules are versioned, effective-dated rule-pack data with official sources and golden tests.
- Build every screen from the suite design system (`docs/suite/DESIGN_SYSTEM.md`). CRM's accent is plum.

### AI
- Every AI feature is off by default, goes through the suite AI gateway only, drafts for a person to approve, and
  never sends, changes a stage or scores a person on model output alone. Chatbots say they are AI. No emotion
  recognition (`docs/suite/AI.md`).

## Engineering rules

- **Tests first** for consent and suppression decisions, sharing rules, assignment and routing rules, forecast
  roll-ups, quote pricing, tenant isolation and residency.
- Fictional test data only: invented companies and people, `example.invalid` emails, reserved phone ranges.
- Tenant context helper for every query; new tenant table checklist (`tenant_id`, forced RLS, policy, index
  leading with `tenant_id`, audit).
- Money as integer minor units plus ISO 4217; instants as `timestamptz`; shared time utilities.
- Nothing configurable is hard-coded: pipelines, stages, scoring rules, SLAs, consent rules, thresholds, prices.
- OpenAPI 3.1 first, `/v1`, contract tests; outbox in the same transaction; idempotent jobs; expand-and-contract
  migrations; scrubbed logs (never bodies, names, emails, phone numbers or message content).
- One OpenTofu definition for staging and production; production deploys only staging-tested digests.
- PostgreSQL full-text and `pg_trgm` for search unless load tests prove otherwise.

## Commands (planned; implemented in Phase 0)

| Command | Purpose |
| --- | --- |
| `pnpm install` / `pnpm dev` | Install / run web, api and worker locally |
| `pnpm verify` | Full gate: lint, typecheck, unit, integration, contract, sharing rules, consent, tenant isolation, residency, e2e, visual regression, Lighthouse, security scan, build, OpenTofu checks |
| `pnpm db:migrate` / `pnpm db:seed` | Apply migrations / seed the fictional demo company |

## Key documents

`docs/BUILD_BRIEF.md` (draft scope) · `docs/PLAN.md` · `docs/FEATURES.md` · `docs/RESEARCH.md` ·
`docs/DECISIONS.md` · `docs/HUMAN_TODO.md` · `docs/suite/`
