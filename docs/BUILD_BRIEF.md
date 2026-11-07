# BUILD BRIEF (DRAFT): Thapsus CRM, a customer relationship platform by Thapsus

> **Draft for the owner's approval, prepared on 2026-11-07** from the owner's request and the decisions recorded in
> `suite/SUITE_PLAN.md` §2. When approved, it becomes the source of truth for scope, in the same way as
> `PROJECT_KAREN/docs/BUILD_BRIEF.md` is for Thapsus-HR. Until then nothing is built.

You are the lead engineer building Thapsus CRM (codename PROJECT_BOSSMAN), a production-ready, multi-tenant CRM for
businesses of about 10 to 2,000 staff in the United Kingdom, the European Union and the United States. It owns
customers, contacts, leads, opportunities, quotes and marketing consent in the Thapsus suite, and must work with
Thapsus-HR, Thapsus Care, Thapsus Finance and Thapsus ERP when a customer buys more than one.

First: read `CLAUDE.md`, `docs/PLAN.md`, `docs/FEATURES.md` and `docs/suite/`, then show the plan for Phase 0 and
wait for the owner to start the build.

## 1. Product position

- A calm, fast CRM for sales, service and marketing on one customer record, with consent and deliverability built
  in, data kept in the customer's home region, transparent pricing and no lock-in.
- With the suite: the same customer in Finance (invoices, credit) and ERP (orders, stock), the same people as in HR
  (teams, leavers), and one sign-in.
- Out of scope: buying or reselling contact data, scraping, a general website builder, an ad network, being a
  telephony carrier, payment processing (Finance and partners), field service (ERP), native apps before the suite's
  native phase.

## 2. Non-negotiables

As in `CLAUDE.md`: marketing and contact law enforced before every send (consent service, rule packs, instant
suppression, authenticated sending, recording consent); privacy and data residency (home regions UK europe-west2, EU
europe-west3, US us-east5; mail sent from the home region); security (forced RLS plus record-level sharing on every
read path, MFA, audit, held contact exports, ASVS L2, no keys); quality (WCAG 2.2 AA including portal and widgets,
i18n, rules as sourced and effective-dated data); the suite design system with CRM's accent; optional, draft-only AI.

## 3. Scale targets (prove with load tests; record in docs/PERFORMANCE.md)

- One tenant with 2,000 users, 5 million contacts, 1 million accounts and 20 million activities.
- 500 concurrent users; search p95 under 300 ms; sharing recalculated for 2,000 users within 10 minutes.
- 1 million marketing emails a day per tenant within mailbox-provider limits; email sync for 2,000 mailboxes.
- p95 API latency under 300 ms for common reads and under 1 second for paginated lists.
- 99.9% availability; RPO 15 minutes; RTO 4 hours.

## 4. Architecture and stack

As in `docs/DECISIONS.md` ADR-0002 and `suite/PLATFORM.md`: TypeScript, pnpm and Turborepo; Next.js PWA plus
embeddable widgets and the portal; Fastify with TypeBox and OpenAPI 3.1; PostgreSQL 17 on Cloud SQL with forced RLS
and a materialised sharing model; the platform kit (`@thapsus-suite/*`); Cloud Run; a PostgreSQL-backed job queue
with an in-cell scheduler; regional Pub/Sub for suite events; the suite mail service (Amazon SES in the home region)
with separate marketing streams; OpenTofu cells; staging equals production.

## 5. Users and roles

Thapsus back-office staff; company owner; CRM administrator; sales manager; sales rep; sales development rep; service
manager; service agent; marketing manager; marketer; customer success manager; partner manager; partner portal user;
customer portal user; read-only executive. Custom roles with field-level rules, sharing by owner, team, territory and
role hierarchy.

## 6. Features

`docs/FEATURES.md` is the scope: data model; leads, routing and scoring; pipelines, quotes and sales performance;
email, calendar, meetings and sequences; telephony and messaging; marketing; consent, preferences and deliverability;
customer service; customer success, partners and field sales; reporting, data quality and platform; optional AI;
Studio configuration; integrations.

## 7. Delivery phases (complete in order; lanes may run in parallel where `PLAN.md` allows)

0 Foundations, sharing model, consent service and the published customer API · 1 Accounts, contacts, activities,
data quality and import · 2 Leads, pipelines, products and quotes · 3 Email, calendar, meetings and sequences · 4
Customer service, knowledge base and portal · 5 Marketing with consent built in (release C1) · 6 Advanced sales · 7
Telephony, recording and messaging (C2) · 8 Customer success, partners and field sales · 9 Analytics, sandboxes and
the app platform (C3) · 10 EU pack · 11 US pack · 12 Optional AI · 13 Suite links live · 14 Scale and launch
readiness. Acceptance criteria per phase: `docs/PLAN.md`.

## 8. Engineering rules

As in `CLAUDE.md`. In particular: tests first for consent and suppression decisions, sharing, assignment and routing,
forecast roll-ups, quote pricing, isolation and residency; fictional data only; `pnpm verify` green at the end of
every phase with the summary printed; human-only steps in `docs/HUMAN_TODO.md`; ask before production deploys,
deletions or new recurring cost.

## 9. Documentation

README, ARCHITECTURE (with diagrams), DESIGN (pointing to the suite design system), SECURITY, DECISIONS, COSTS,
PERFORMANCE, COVERAGE (marketing-law rule packs by jurisdiction), SUBPROCESSORS, COMPLIANCE, HUMAN_TODO, API docs,
runbooks (including deliverability incidents), DPIA template, records of processing, an accessibility conformance
report for the portal and widgets, admin and user guides per role, and migration guides from HubSpot, Pipedrive,
Salesforce and Zoho.

## 10. Definition of done

Every phase ticked in `docs/PLAN.md` with evidence; `pnpm verify` green; Google and Microsoft app verifications in
place; staging deployed in each region before its production cell; smoke and load tests meeting §3; production
defined and ready to deploy on the owner's approval; all §9 documents present; every remaining human step in
`docs/HUMAN_TODO.md`.
