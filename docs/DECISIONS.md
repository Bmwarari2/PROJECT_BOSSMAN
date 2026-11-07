# Decisions — Thapsus CRM

Dated architecture and product decisions. Format: context, decision, consequences. Suite-wide decisions are
summarised in `docs/suite/SUITE_PLAN.md` §2; this file records how they apply here.

## ADR-0001 — Owner decisions for the new products (2026-11-07)

**Context.** The owner asked for a CRM (codename PROJECT_BOSSMAN) as part of a suite with Thapsus-HR, Thapsus
Care, a finance product and an ERP, and answered seven scoping questions on 2026-11-07.

**Decision.**
- Markets: UK, EU and US home-region cells from day one; UK marketing-law content at launch, EU and US packs
  follow (D-1).
- Client size: SMB to mid-market, about 10 to 2,000 staff (D-2).
- Design: the shared suite design system with CRM's own accent (D-5).
- Mobile: an installable PWA first, including offline field-sales visits; a native app later with the suite (D-6).
- Build order: the platform kit first, then Finance, CRM and ERP in parallel (D-7).

**Consequences.** Contact and marketing rules are per jurisdiction and effective-dated from the start. CRM is the
customer and contact owner in the suite (`docs/suite/INTEGRATION.md` §4) so its customer API is a Phase 1
deliverable for Finance and ERP.

## ADR-0002 — Stack and platform kit (2026-11-07)

**Context.** As for Finance and ERP: the new products reuse Thapsus-HR's three-region model and its extracted
code (D-9, D-10).

**Decision.** TypeScript, pnpm and Turborepo; `apps/web` (Next.js App Router PWA, plus the embeddable chat and
form widgets and the customer portal), `apps/api` (Fastify, TypeBox, OpenAPI 3.1, `/v1`), `apps/worker` (outbox,
sequences, campaign sending, email and calendar sync, SLA timers); `packages/crm` (domain rules: assignment,
scoring, forecasting, sharing), `packages/rules` (contact and marketing law packs), `packages/db`; shared
`@thapsus-suite/*` packages.

**Consequences.** Same as Finance ADR-0002.

## ADR-0003 — Record-level sharing on top of tenant RLS (2026-11-07)

**Context.** Sales teams need records visible to their owner, team, territory and managers, not to everyone in the
tenant. Tenant isolation is already enforced by forced RLS.

**Decision.** Keep tenant isolation in RLS. Add record-level access as a sharing model evaluated in one place
(`packages/crm/sharing`): organisation-wide defaults per object (private, read, read and write), ownership, team
and role hierarchy, territories, manual shares and criteria-based sharing rules. Sharing is materialised into a
per-tenant access table that the API, search, reports, exports, embedded cards and AI retrieval all join against;
an RLS policy on that table backs the application check for the most sensitive objects.

**Consequences.** Every read path has a sharing test. Recomputing access after a re-org runs as a background job
with progress.

## ADR-0004 — Email: sync from the customer's mailbox, send marketing through our regional service (2026-11-07)

**Context.** Salespeople work in Gmail and Outlook; marketing sends need deliverability controls and residency.

**Decision.** One-to-one email and calendars sync two-way with Google Workspace (Gmail and Calendar APIs) and
Microsoft 365 (Graph), with per-user OAuth consent and least-privilege scopes; messages are stored in the home
region. Marketing and sequence email is sent through the suite mail service (Amazon SES in the home region),
from the customer's authenticated domain (SPF, DKIM, DMARC alignment checked before the first send), with
one-click unsubscribe and complaint and bounce handling.

**Consequences.** Google's restricted-scope verification and Microsoft publisher verification are human steps
(HUMAN_TODO). Shared-IP reputation needs warm-up and per-tenant sending limits.

## ADR-0005 — Consent and suppression are rule-pack decisions, enforced before every send (2026-11-07)

**Context.** UK PECR (as amended by the Data (Use and Access) Act 2025), EU ePrivacy and GDPR, US CAN-SPAM, TCPA
and state laws differ in when consent is needed and how fast an opt-out must apply (dates in `docs/RESEARCH.md`).

**Decision.** A single consent service decides, for each recipient, channel and purpose, whether a send is
allowed, using the recipient's jurisdiction, the tenant's recorded lawful basis, the consent and preference
records and the suppression lists. The rules live in effective-dated rule packs with sources. A send without a
basis is blocked, never only warned. Every decision is logged (counts and reasons, not content).

**Consequences.** Golden tests per jurisdiction; marketing features cannot ship without the consent service.

## ADR-0006 — Customers are shared records with field-level owners (2026-11-07)

**Context.** Finance owns credit and payment terms; ERP owns delivery preferences; CRM owns the relationship.

**Decision.** CRM owns the customer and contact record. Finance-owned and ERP-owned field groups are read-only
copies in CRM when linked, labelled with the owner and last sync time (`docs/suite/INTEGRATION.md` §1.3).
Without a link, CRM owns every field.

**Consequences.** Field ownership is part of each field's metadata so Studio, the API and imports respect it.

## ADR-0007 — AI features are optional and draft-only (2026-11-07)

**Context.** The owner wants AI available but optional (D-13). The EU AI Act's transparency obligations apply to
chatbots, and emotion recognition in the workplace is prohibited (details in `docs/RESEARCH.md`).

**Decision.** Every AI feature in `docs/FEATURES.md` §AI is off by default at three levels, runs only through the
suite AI gateway, and drafts for a person. A service chatbot always says it is AI and hands over to a person on
request. No emotion or sentiment inference about employees; no automated decision with legal or similarly
significant effect about a person. Call transcription and summaries need the call-recording consent rules to
pass first.

**Consequences.** Each feature has an evaluation suite and a non-AI path.

## ADR-0008 — The US cell runs in us-central1 (Iowa) (owner decision, 2026-11-07)

**Context.** The US cell was planned in us-east5 (Columbus). Cloud Scheduler, Cloud Tasks and single-region Document
AI are not offered there, which the suite needs. The owner allowed the US cell to run in any compatible US region
(suite decision D-16).

**Decision.** The US cell runs in **us-central1 (Iowa)**; backups and replicas stay in **us-west1 (Oregon)**. It is
the only US region with every service this product uses, checked on Google's pages on 2026-11-07
(`docs/suite/PLATFORM.md` §5.1a, `docs/suite/RESEARCH_PLATFORM.md` addendum). Generative AI in the US cell uses
Gemini in us-central1, or Claude on the us-east5 regional endpoint, which is the only US region serving Claude
regionally. Google processes requests to every US regional endpoint within the US multi-region, so the data stays in
the United States either way.

**Consequences.** The region map, the residency tests (every region pair) and the US settings files use us-central1
and us-west1 from Phase 0. The residency guard allows us-east5 for the AI gateway's Claude endpoint only, and refuses
it for every other resource. The US folders' organisation policy is `in:us-central1-locations` and
`in:us-west1-locations`.
