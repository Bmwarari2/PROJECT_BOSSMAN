# Development plan — Thapsus CRM (PROJECT_BOSSMAN)

Status: plan for owner approval, 2026-11-07. Scope: `FEATURES.md`. Research and dates: `RESEARCH.md`. Suite context:
`suite/SUITE_PLAN.md` (stages and owner decisions D-1 to D-15), `suite/INTEGRATION.md` (CRM owns customers, contacts,
leads, opportunities, quotes and marketing consent).

## 1. Shape of the plan

- **Sales first, then service and marketing, all on one record.** The UK release (C1) is a complete CRM for an SMB or
  mid-market firm leaving spreadsheets, Pipedrive, HubSpot Starter or Zoho Standard: records, pipelines, quotes, email
  and calendar, a service desk and consent-safe marketing.
- **Consent is a gate, not a feature** (ADR-0005). Marketing cannot ship until the consent service and the UK rule pack
  (PECR as amended on 5 Feb 2026) are proved by golden tests. EU and US packs follow (`RESEARCH.md` §2).
- **Customers are shared records** (ADR-0006). CRM publishes its customer API and events in Phase 0, so Finance and ERP
  can build against a mock while CRM builds the real thing (D-7).
- **Releases.** **C1** UK sales, service and marketing · **C2** advanced sales and telephony · **C3** customer success,
  partners and field sales · **EU** and **US** packs.
- **Every phase ends** with `pnpm verify` green, docs updated, this file ticked with evidence, and a commit. Nothing is
  ticked without a test or check that proves it, or a HUMAN_TODO entry for a human-only step.

## 2. Phases and suite stages

| Phase | Name | Suite stage | Release |
| --- | --- | --- | --- |
| 0 | Foundations, sharing model, consent service and the published customer API | 0 | — |
| 1 | Accounts, contacts, activities, data quality and import | 1 | C1 |
| 2 | Leads, pipelines, products and quotes | 1 | C1 |
| 3 | Email, calendar, meetings and sequences | 1–2 | C1 |
| 4 | Customer service, knowledge base and portal | 2 | C1 |
| 5 | Marketing with consent built in | 2 | C1 |
| 6 | Advanced sales: CPQ, contracts, forecasting, territories, commissions | 3 | C2 |
| 7 | Telephony, recording and messaging | 4 | C2 |
| 8 | Customer success, partners and field sales | 4 | C3 |
| 9 | Analytics, sandboxes and the app platform | 4 | C3 |
| 10 | EU pack | 4 | EU |
| 11 | US pack | 4 | US |
| 12 | Optional AI features | 3–4 | Each release |
| 13 | Suite links live | 3 | All |
| 14 | Scale and launch readiness | 2–5, continuous | All |

## 3. Phases

### Phase 0 — Foundations, sharing model, consent service and the published customer API

- Monorepo on `@thapsus-suite/*` (`apps/web` including embeddable form and chat widgets and the portal, `apps/api`,
  `apps/worker`, `packages/crm`, `packages/rules`, `packages/db`); `pnpm verify` with every gate in CLAUDE.md plus
  **sharing** and **consent** gates.
- Tenancy with forced RLS and the isolation suite, Thapsus ID sign-in, permissions catalogue (`crm.<module>.<action>`),
  audit, outbox, i18n, the suite shell with CRM's accent, Studio wired to CRM records.
- **Sharing model** (ADR-0003): organisation-wide defaults per object, ownership, teams and role hierarchy, manual and
  criteria-based shares, materialised access applied on every read path (API, search, reports, exports, cards, AI
  retrieval).
- **Consent service** (ADR-0005) and the rule-pack framework, with the UK pack skeleton: lawful basis per person,
  purpose and channel; immutable consent events with evidence and wording versions; subscriber type and recipient
  country on every contact point; global and per-channel suppression.
- **The customer API and events** (`customer.*`, `contact.*`, `quote.accepted`) published as OpenAPI and AsyncAPI with a
  mock, so Finance and ERP can start.
- OpenTofu cell and global roots; UK staging after the owner approves `COSTS.md`. Fictional seed (a UK B2B services
  firm with 40 users) and a synthetic generator (5 million contacts, 1 million accounts, 20 million activities).

**Acceptance:** isolation, residency and sharing suites pass (a user never sees a record the model denies, on any read
path); the consent service blocks a send with no valid basis in golden tests; the customer API mock passes its
contract tests; full export including configuration works (EU Data Act).

### Phase 1 — Accounts, contacts, activities, data quality and import (C1)

- Accounts with parent and child hierarchy and roll-ups; contacts linked to several accounts with role labels; typed
  associations between any records; an optional person-account (B2C) mode per tenant.
- Activity timeline (tasks, calls, meetings, notes, emails) that respects field-level permissions; files with preview;
  saved views, tags, static and dynamic lists; field history into the audit log; multi-currency (rates from Finance
  when linked).
- Data quality: duplicate rules with fuzzy matching (`pg_trgm`) and merge with field-level survivorship (reversible);
  normalisation (E.164 phones, ISO countries, domains); bulk clean-up jobs; first-party enrichment from email
  signatures and company registries (Companies House first); field-level provenance with retention clocks that do not
  reset on update (the CNIL KASPR decision).
- Import and migration: CSV and Excel with mapping, dedupe, dry run and rollback by batch; productised importers from
  HubSpot, Pipedrive, Salesforce, Zoho and Dynamics; Mailchimp, Brevo and Klaviyo list import that preserves
  unsubscribes, bounces and consent (never re-subscribes).
- Privacy workflows for customers as controllers: find, export, erase, restrict, object; a "privacy complaint" case type
  with the UK 30-day acknowledgement (from 19 Jun 2026); Article 14 notice workflow for imported or enriched people.

**Acceptance:** merge and unmerge golden tests; a 1-million-row import completes in the background with a correct
reconciliation report; imported opt-outs stay opted out (tested).

### Phase 2 — Leads, pipelines, products and quotes (C1)

- Lead object or lifecycle stages per tenant; accessible web forms with consent capture; email-to-lead.
- Assignment rules (criteria, round-robin, weighted, capacity) that skip absent reps (availability from Thapsus-HR when
  linked, dates only); which rule fired is recorded; speed-to-lead SLA timers and escalation.
- Lead conversion with duplicate check; rule-based lead and account scoring (fit plus engagement, with decay),
  explainable.
- Deals: multiple pipelines, stages with required fields and guidance, stalled-deal alerts, opportunity teams and
  revenue splits, win and loss reasons, competitors; board, list, calendar and forecast views.
- Products and price books (local, or read from ERP when linked); quotes with Studio templates and versions; discount
  approval chains (approvers from the HR reporting line when linked); e-signature through DocuSign or Adobe Acrobat
  Sign; accepted quote to ERP order or Finance invoice.
- Goals and basic forecasting; standard reports (pipeline, funnel, revenue, activity) and dashboards.

**Acceptance:** assignment golden scenarios (absence, capacity, weights); scoring golden tests with explanations;
quote totals in minor units with tax from Finance's codes; a won-quote event reaches the Finance and ERP mocks once.

### Phase 3 — Email, calendar, meetings and sequences (C1)

- Two-way email and calendar sync with Google Workspace (Gmail and Calendar APIs) and Microsoft 365 (Microsoft Graph
  only; Exchange Online blocks EWS from 1 Oct 2026); auto-logging rules and private-email exclusions; Gmail add-on and
  Outlook add-in; templates, snippets and merge fields through message catalogues; open and click tracking
  switchable per tenant; scheduled send and per-mailbox limits; shared team inboxes.
- Meeting scheduler: personal, round-robin and team booking pages; buffers and working hours; time-zone aware;
  accessible.
- Sequences: email, call and task steps (manual LinkedIn tasks; no LinkedIn API, as LinkedIn takes no new Sales
  Navigator partners); auto-unenrol on reply, meeting, bounce or unsubscribe; quiet hours in the recipient's time zone;
  throttling per mailbox and domain; every step passes the consent service first.

**Acceptance:** sync round-trips against Google and Microsoft sandboxes; sequence golden tests (unenrol, quiet hours,
throttles, consent block).

### Phase 4 — Customer service, knowledge base and portal (C1)

- Tickets and cases with queues, priorities, statuses and parent and child; an omnichannel inbox (email, web form, chat
  widget at launch; WhatsApp, SMS and voice in Phase 7); SLAs and entitlements with business-hours calendars per IANA
  zone and holidays from rule packs; rules-based routing (skills and capacity later); macros.
- Knowledge base (internal and public, versions, approvals, translations); a customer self-service portal and chat
  widget built to WCAG 2.2 AA with an accessibility conformance report (customers' EU consumer services fall under the
  European Accessibility Act); a rule-based bot; CSAT, CES and NPS surveys.
- Cases linked to orders, lots and serials from ERP when linked.

**Acceptance:** SLA golden tests across time zones and holidays; portal and widget pass axe with zero serious issues and
a keyboard-only test.

### Phase 5 — Marketing with consent built in (C1 complete)

- Preference centre and subscription types per brand and locale; double opt-in; consent evidence (who, when, channel,
  wording version, source).
- UK rule pack: PECR regulation 22 (consent or soft opt-in for individual subscribers, sole traders included), the
  charity soft opt-in only for details collected from 5 Feb 2026, regulation 21 TPS and CTPS screening, the cookie
  exemptions of 5 Feb 2026 (first-party analytics with an opt-out; sales profiling still needs consent), the right to
  object as a permanent suppression.
- Sending: authenticated customer domains (SPF, DKIM 2048-bit, aligned DMARC) with guided DNS and continuous checks;
  separate marketing and transactional streams; RFC 8058 one-click unsubscribe on every marketing message and a visible
  link; bounce and complaint processing with an automatic pause at 0.1% complaints and a stop near 0.3% (Gmail,
  Yahoo and Outlook.com requirements); sending from the home region through the suite mail service.
- Segments (static, dynamic, cross-object), an accessible drag-and-drop email builder, forms and landing pages on
  region-pinned hosting, journeys (branches, waits, goals, re-entry), A/B tests, events and webinars (Zoom and Teams),
  attribution (first, last, multi-touch), first-party website tracking under the consent rules.

**Acceptance:** golden tests for every UK rule (including the charity cut-off date); a send without a valid basis is
blocked (never only warned); a test campaign to seeded mailboxes passes SPF, DKIM and DMARC and exposes RFC 8058
headers; complaint thresholds pause a campaign in tests.

**Release C1 gate:** a design-partner company runs fictional sales, service and marketing end to end in staging.

### Phase 6 — Advanced sales: CPQ, contracts, forecasting, territories, commissions (C2)

- CPQ: bundles, options, configuration rules, guided selling (ERP's configurator when linked).
- Contracts with dates, renewal notices and a clause library; subscriptions and renewals with ARR and MRR (billing
  truth stays in Finance); auto-created renewal deals.
- Forecasting with categories, hierarchy roll-ups, manager adjustments and snapshots; quotas; territories (rule-based,
  hierarchical, several models, assignment preview); playbooks; account planning (org charts, relationship maps,
  whitespace); mutual action plans shared with buyers through the portal.
- Commissions: plans, statements and approvals; approved amounts go to Thapsus-HR as pay inputs (HR never runs payroll;
  Finance's UK payroll in a later phase, or the customer's payroll).

**Acceptance:** forecast roll-up golden tests; territory reassignment preview equals the result; commission golden
tests.

### Phase 7 — Telephony, recording and messaging (C2 complete)

- A CTI adapter framework (Twilio first; Aircall and RingCentral adapters), click-to-call, call logging, voicemail, a
  power dialer (no predictive dialler: TCPA and Ofcom abandoned-call risk).
- Recording with a consent engine (UK and EU notices; US all-party states, or unknown location treated as all-party);
  the jurisdiction decision stored with the recording; recording off by default.
- Two-way SMS; WhatsApp Business Platform with per-message cost estimates shown before campaigns (pricing changes of 1
  Oct 2026) and Meta's terms applied (no use of WhatsApp data to train shared models); social inbox (optional).

**Acceptance:** consent engine golden tests per jurisdiction; STOP handling within the shortest applicable limit.

### Phase 8 — Customer success, partners and field sales (C3)

- Customer success: rule-based health scores with visible weights, playbooks and onboarding plans, renewal and
  expansion pipelines, a product-usage events API, churn-risk alerts.
- Partner relationship management: partner portal with deal registration, lead distribution, tiers and MDF.
- Field sales: maps, nearby accounts, route planning, check-ins with location captured only at check-in (never
  continuous tracking of employees), offline PWA with conflict review.

### Phase 9 — Analytics, sandboxes and the app platform (C3 complete)

Cross-object report builder, historical snapshots, pipeline waterfall, forecast against quota, scheduled reports that
respect permissions at send time, export to a regional BigQuery dataset in the cell; developer sandboxes with
configuration promotion; UI extensions; Zapier, Make and n8n connectors; bulk API; an app marketplace (later, reviewed).

### Phase 10 — EU pack

National ePrivacy variants as rule-pack data (for example Germany's consent rule for most B2B email under the UWG),
EU languages, Article 14 notice templates, double opt-in defaults where expected, EU telephony numbers and WhatsApp
templates, the Data Act exit pack and infrastructure-jurisdiction page (core from Phase 0).

### Phase 11 — US pack

CAN-SPAM (opt-outs within 10 business days, postal address); TCPA (prior express written consent for marketing calls
and texts, AI voices count as artificial voices, the revised revocation rules from 25 Nov 2026) and state mini-TCPAs
(Florida, Oklahoma, Texas and others: hours, frequency caps, consent types) evaluated before every dial or send;
National and state do-not-call screening; state privacy laws for B2C tenants (consumer rights, Global Privacy Control,
opt-out of sale and sharing suppressing ad-audience sync); California automated decision-making safeguards from 1 Jan
2027; US telephony numbers and 10DLC registration.

### Phase 12 — Optional AI features

Off by default, through the suite AI gateway, drafts only (`suite/AI.md`, ADR-0007): email and reply drafts; call and
meeting summaries only where recording consent passed; record updates proposed from emails into an approval queue;
deal-risk signals (rules first, model commentary optional); next-step suggestions; an AI forecast column beside the
rep's commit; a service chatbot answering only from published articles with citations, disclosing that it is AI and
handing over to a person; case summaries and routing suggestions; knowledge-article drafts from resolved cases;
segments and reports from plain language; marketing content drafts (labelled); duplicate suggestions; an MCP server
with read and draft-only tools. **Never**: emotion recognition of employees, AI scoring of reps' performance, autonomous
first messages to prospects, or outbound AI voice calls.

### Phase 13 — Suite links live

Finance (customer financial fields, invoices, payment status, credit holds, quote to invoice when there is no ERP,
subscriptions to recurring invoices); ERP (catalogue, prices, availability, quote to order, order and shipment
status, returns linked to cases); HR (users, teams, managers, availability dates, leavers lose access, commission
results as pay inputs); Care (UK only: commissioners, referrers and suppliers as business accounts; never enquiries
about prospective residents). All per `suite/INTEGRATION.md` with its test list.

### Phase 14 — Scale and launch readiness

Load tests against the mid-market targets (2,000 users and 5 million contacts per tenant; 500 concurrent users;
search p95 under 300 ms; sharing recalculated for 2,000 users within 10 minutes; 1 million marketing emails a day per
tenant within provider limits); threat model; ASVS L2 review; penetration test; accessibility audit including the
portal and widgets; restore and zone-failure drills; staging in each region before its production cell; runbooks.

## 4. Dependencies

| Needs | From | By phase |
| --- | --- | --- |
| Platform kit (core, db, auth, Studio, shell, mail service) | `thapsus-platform` | 0 |
| Users, teams, availability, leavers | Thapsus-HR partner scopes (`PROJECT_KAREN/docs/SUITE_INTEGRATION_PLAN.md` S.5) | 2 (mock), 13 |
| Tax codes, rates, invoices, credit status | Finance | 2 (mock), 13 |
| Catalogue, prices, availability, orders | ERP | 2 (local price books), 13 |

## 5. Risks specific to CRM

| Risk | Mitigation |
| --- | --- |
| Deliverability problems harm every tenant on shared infrastructure | Authenticated customer domains, warm-up, per-tenant limits, automatic pause and stop thresholds, separate streams |
| Marketing law breaches by customers | Consent service blocks sends without a basis; rule packs with sources; evidence for every consent |
| Integration gates (Gmail security assessment, Microsoft verification, LinkedIn closed) | Start HUMAN_TODO B-3 and B-4 early; LinkedIn only as manual tasks and profile URLs |
| Sharing rules leak data through a side path | Sharing tests on every read path, including search, exports, cards and AI retrieval |
| Competing with HubSpot and Salesforce | Lead on suite value (one customer record with Finance and ERP), consent and residency, transparent pricing |
