# Research — Thapsus CRM

> Research dossier prepared on 2026-11-07 for the plan in `PLAN.md` and `FEATURES.md`. Every regulatory statement carries its
> source and status (in force, applies from a date, proposed, platform policy, unverified). Facts are re-checked when the matching
> phase starts; anything marked UNVERIFIED must be checked against the primary source before it becomes rule-pack data.
> Where this dossier and the plan differ, the plan records the reason.


Prepared 7 November 2026 for the Thapsus product and engineering leads. Covers the feature benchmark,
regulation in force or coming (UK, EU, US), AI features, integration points with the Thapsus suite
(MULLAH, KASONGO, Thapsus-HR, Thapsus Care) and external services, no-code customisation and market
pricing.

## How to read this

**Status labels for every regulatory statement**

| Label | Meaning |
| --- | --- |
| IN FORCE | Law that applies today (7 Nov 2026). |
| APPLIES FROM <date> | Adopted law that is not yet applying. |
| PROPOSED | Not adopted; may change or be dropped. |
| POLICY | Platform or vendor rule (Google, Microsoft, Meta, LinkedIn), not law, but enforced commercially. |
| UNVERIFIED | Could not be confirmed against a primary or official source in this session. |
| (secondary) | Confirmed only by law-firm or vendor commentary, not the official text. |

**Competitor codes used in the feature tables**

| Code | Product |
| --- | --- |
| SF | Salesforce (Sales Cloud, Service Cloud, Marketing Cloud, Agentforce) |
| HS | HubSpot (Sales, Service, Marketing, Content, Data/Operations, Commerce hubs; Agent Hub) |
| MS | Microsoft Dynamics 365 Sales and Customer Service (with Customer Insights and Power Platform) |
| ZO | Zoho CRM / Zoho One |
| PD | Pipedrive |
| FW | Freshworks (Freshsales, Freshdesk, Freddy AI) |
| AT | Attio |
| MO | monday CRM |
| CL | Close |
| CR | Creatio (no-code CRM) |
| SG | SugarCRM, now branded SugarAI (https://www.sugarai.com/pricing) |
| OD | Odoo CRM (and adjacent Odoo apps) |
| CO | Copper |

Competitor columns reflect vendor pricing and feature pages fetched in this session plus product
knowledge to mid-2026. Availability usually depends on tier or a paid add-on. Cells were not each
re-verified one by one; treat them as a benchmark, not a procurement statement.

**Tier labels for Thapsus CRM**

- **Core** = launch. What an SMB or mid-market buyer needs to leave spreadsheets, Pipedrive, HubSpot
  Starter or Zoho Standard.
- **Advanced** = parity with HubSpot Professional, Zoho Enterprise, Dynamics 365 Sales Enterprise.
- **Enterprise** = parity with Salesforce Core/Advanced and Dynamics Premium for 1,000+ seat customers.

---

## 0. Executive summary: ten findings that shape scope

1. **UK PECR and UK GDPR changed on 5 February 2026.** The Data (Use and Access) Act 2025
   (Commencement No. 6) Regulations 2026, SI 2026/82, commenced cookie exemptions (s.112, Sch 12),
   the charity soft opt-in (s.114), new PECR enforcement and penalties (s.115, Sch 13), recognised
   legitimate interests (s.70, Sch 4) and the new automated decision-making articles 22A–22D (s.80).
   Complaint handling (s.103) followed on **19 June 2026**. IN FORCE.
   https://www.legislation.gov.uk/uksi/2026/82/made
2. **EU marketing and cookie law is still the 2002 ePrivacy Directive.** The ePrivacy Regulation was
   withdrawn (announced 11 Feb 2025). The Digital Omnibus proposal to move cookie rules into the GDPR
   (Art 88a) has not been adopted, and the Council compromise of June 2026 deleted it (secondary).
   Build a per-jurisdiction rules engine, not a single "EU" rule.
3. **EU AI Act timing after the Digital Omnibus on AI, Regulation (EU) 2026/1744** (OJ 24 Jul 2026, in
   force 27 Jul 2026): Art 5 prohibitions apply since 2 Feb 2025 (including emotion recognition of
   employees, so of call-centre agents); Art 50 transparency (chatbots, AI-generated content) applies
   since **2 Aug 2026**; machine-readable marking (50(2)) for systems already on the market before
   then by **2 Dec 2026**; Annex III high-risk moved to **2 Dec 2027**.
   https://artificialintelligenceact.eu/ai-act-explorer/digital-omnibus/
4. **TCPA:** the FCC "one-to-one consent" rule was vacated by the Eleventh Circuit on **24 Jan 2025**
   (Insurance Marketing Coalition v FCC, No. 24-10277). The FCC's revised consent-revocation rules
   (Report and Order adopted 30 Sep 2026, Federal Register 26 Oct 2026) take effect **25 Nov 2026**:
   revocations honoured within 10 business days, category-specific revocation for informational
   messages, and optional "exclusive" opt-out methods (secondary: Womble Bond Dickinson, Troutman).
5. **US state privacy:** 19 comprehensive state laws are in force by 1 Jan 2026; four more were
   enacted in 2026 (Oklahoma and Louisiana from 1 Jan 2027, Alabama from 1 May 2027, Vermont from
   1 Jan 2028) per the IAPP tracker updated 29 June 2026. Only California covers B2B contacts and
   employees. California's ADMT and risk-assessment regulations took effect **1 Jan 2026**; ADMT
   compliance for significant decisions starts **1 Jan 2027**; first risk-assessment attestations
   are due **1 Apr 2028**. https://www.cppa.ca.gov/announcements/2025/20250923.html
6. **Mailbox-provider rules are now hard gates.** Gmail moved to temporary and permanent rejections of
   non-compliant bulk mail from **November 2025**; Outlook.com applies the same SPF, DKIM and DMARC
   bar to senders of more than 5,000 messages a day since **5 May 2025** (Junk now, rejection with
   `550 5.7.515` later). One-click unsubscribe (RFC 8058) and a spam rate below 0.3% are required.
   POLICY.
7. **EU Data Act switching duties apply to SaaS since 12 Sep 2025**, and **from 12 Jan 2027 no
   switching or egress charges may be imposed** (Art 29). The contract must give a maximum two-month
   notice period, a 30-day transition, at least 30 days to retrieve data, a full list of exportable
   data and free open interfaces (Arts 25, 30). Export and exit become product features.
   https://eur-lex.europa.eu/eli/reg/2023/2854/oj/eng
8. **WhatsApp Business Platform:** per-message pricing since 1 Jul 2025; from **1 Oct 2026** service
   messages beyond 1,000 per number per month are charged again; Meta's terms (s.4.7) bar AI
   providers whose AI is the "primary" functionality and forbid using WhatsApp data to train models.
   The European Commission ordered interim measures on **9 Jun 2026** (case AT.41034) restoring access
   for third-party AI assistants. Business customer-service bots remain allowed. POLICY plus
   competition law.
9. **Market:** Salesforce replaced Enterprise and Unlimited with **Core ($195), Advanced ($395) and Max
   ($550)** per user per month on **3 Sep 2026**, bundling Slack, Tableau Next and Flex Credits.
   Every leader now sells AI as consumption credits (Salesforce Flex Credits, HubSpot Credits at
   $0.01, Microsoft Copilot Credits, Attio, Close and monday AI credits, Freshworks sessions).
10. **Integration gates that affect the roadmap:** LinkedIn states it is **not accepting new Sales
    Navigator API (SNAP) partners**; Gmail sync needs restricted scopes and an **annual CASA security
    assessment**; Exchange Online **blocks EWS from 1 Oct 2026**, so Outlook sync must use Microsoft
    Graph. Third-party enrichment is risky: the CNIL fined KASPR **EUR 240,000** (Dec 2024) for
    scraping LinkedIn contact data.

**Surprises:** HubSpot's own AI page now brands its agents as **"Agent Hub"** (Breeze survives only
as "Breeze Assistant"); SugarCRM has rebranded to **SugarAI**; Meta now bills its own "Business
Agent" on WhatsApp per token ($2 per million tokens); Creatio launched an "Unlimited" pricing model
on 1 May 2026 (secondary); and Microsoft reportedly stopped publishing Dynamics 365 release-wave plans
from September 2026 (secondary, UNVERIFIED).

---

## 1. Feature catalogue benchmarked against leaders

### 1.1 Data model: accounts, contacts, households

| Feature | Tier | Competitors with it | Notes for Thapsus |
| --- | --- | --- | --- |
| Accounts/companies with parent-child hierarchy and roll-ups | Core | All; hierarchy views in SF, HS, MS, ZO, FW, CR, SG, OD | Shared "party" master with MULLAH and KASONGO (see section 4). |
| Contacts linked to several accounts with role labels | Advanced | SF (contacts to multiple accounts), HS (association labels), MS (connections), AT (relationships), CR | Needed for consultants, board members, agency staff. |
| Typed associations between any records (many-to-many, labelled) | Advanced | HS, AT, SF, MS, CR | Generic association table keyed by object type. |
| Person accounts / B2C mode | Advanced | SF (Person Accounts), MS (contact-only records), HS (contacts without companies), ZO, OD, CR | Turn on per tenant; affects lead conversion and dedupe. |
| Households (grouping individuals, shared address, relationships) | Enterprise | SF (Financial Services Cloud households), MS (financial services data model), CR (financial services) | Useful for B2C, wealth, education; not for care residents (those stay in Thapsus Care). |
| Custom objects | Core (limited count) / Advanced (more) | SF, HS (Enterprise), MS, ZO (custom modules), FW (Enterprise), AT (3/5/12/unlimited objects by plan), CL (Scale), CR, SG, OD (Studio); PD only custom fields | Metadata-driven; see section 5. |
| Custom fields incl. formula, roll-up, lookup, multi-select, currency, date-time with zone | Core | All | Store money as minor units plus ISO 4217; dates with explicit IANA zone. |
| 360-degree activity timeline | Core | All | Must respect field-level and sensitive-data permissions in the timeline. |
| Files with preview and versioning | Core | All | Region-pinned storage per tenant. |
| Saved views, filters, tags, static and dynamic lists | Core | All | |
| Multi-currency with dated exchange rates | Advanced | SF, MS, ZO, HS, PD, FW, SG, OD, CO (Business) | Rates from MULLAH when installed. |
| Field history tracking | Core | SF, MS, ZO, HS (property history), FW (Enterprise audit logs), SG, OD | Feed the append-only audit log. |

### 1.2 Leads, capture, routing and scoring

| Feature | Tier | Competitors with it | Notes for Thapsus |
| --- | --- | --- | --- |
| Lead object or lifecycle stages on contacts | Core | Lead object: SF, MS, ZO, SG, CR, OD; lifecycle stages: HS, FW; leads inbox: PD | Offer both models per tenant; default to lead object for B2B. |
| Web forms (embedded, hosted), chat-to-lead, email-to-lead | Core | All (CL and AT mainly via integrations) | Forms must carry consent capture and accessible markup (EAA, WCAG 2.2 AA). |
| Criteria-based assignment rules | Core | SF, MS, ZO, HS, FW, OD, SG, CR, MO | Ordered rule sets; audit which rule fired. |
| Round-robin, weighted and capacity-based assignment, skipping absent reps | Advanced | SF (queues, Omni-Channel), HS (rotate records), MS, ZO, FW, MO, OD | Read absence from Thapsus-HR (section 4.3). |
| Lead conversion to account, contact and opportunity with duplicate check | Core | SF, MS, ZO, SG, CR, OD, FW | |
| Rule-based lead scoring (fit plus engagement, with decay) | Core | HS (5 scores Starter, 10 Professional), ZO, SF (Account Engagement), MS (Customer Insights), OD, CR, SG (Sugar Market), FW | Rule-based is explainable; keep predictive scoring optional AI. |
| Account scoring for account-based selling | Advanced | HS, SF, MS, SG | |
| Speed-to-lead SLA timers and escalation | Advanced | SF (Flow), MS (sequences), HS (workflows), ZO | |
| Website visitor identification (reverse IP, buyer intent) | Enterprise, optional | PD (Web Visitors add-on), HS (buyer intent), ZO (SalesIQ), FW | Needs cookie consent in the EU; UK analytics exemption does not cover sales profiling. |

### 1.3 Opportunities, pipelines and stages

| Feature | Tier | Competitors with it | Notes for Thapsus |
| --- | --- | --- | --- |
| Deals with amount, currency, stage, probability, close date, next step | Core | All | |
| Multiple pipelines per team, product line or record type | Core | All (HS: 15, 100 and 350 pipelines by tier) | |
| Stage gates: required fields, exit criteria, guidance per stage | Advanced | SF (Path), MS (business process flows), ZO (Blueprint), HS (pipeline rules and approvals), CR, OD | |
| Stalled-deal ("rotting") alerts | Core | PD, HS, ZO, CL | |
| Line items from product catalogue | Core | SF, HS, MS, ZO, PD, FW, CR, SG, OD, MO | Catalogue mastered in KASONGO when installed. |
| Opportunity teams and revenue splits | Advanced | SF, HS (deal splits), MS (sales team), ZO | Feeds commissions. |
| Board, list, calendar and forecast views | Core | All | |
| Win/loss reasons and competitor tracking | Core | All | |
| Mutual action plans / deal rooms shared with buyers | Enterprise | HS (deal plans), SF (via partners and Digital Sales Rooms), others via partners | External sharing needs portal security. |

### 1.4 Activities, email and calendar

| Feature | Tier | Competitors with it | Notes for Thapsus |
| --- | --- | --- | --- |
| Tasks, calls, meetings, notes, reminders | Core | All | |
| Two-way calendar sync (Google, Microsoft 365) | Core | All | Graph and Google Calendar APIs. |
| Two-way email sync (Gmail, Microsoft 365) with auto-logging rules and private-email exclusions | Core | All | Gmail restricted scopes need annual CASA; Outlook via Graph only after EWS blocking (section 4.6). |
| Open and click tracking | Core | All | Make per-tenant switchable; some EU regulators treat tracking pixels as terminal-equipment access (UNVERIFIED for 2026 positions). |
| Templates, snippets, merge fields, attachments library | Core | All | All strings through message catalogues; templates per locale. |
| Shared/team inboxes | Advanced | HS (inbox), FW, ZO, SF, MS, MO (unified comms hub), CL | |
| Gmail add-on, Outlook add-in, browser extension | Core | SF, HS, MS, ZO, PD, CO (Gmail-native), CL, FW | |
| Scheduled send, send-time limits per mailbox | Core | All | Protects customer domains' reputation. |

### 1.5 Sequences and meeting scheduling

| Feature | Tier | Competitors with it | Notes for Thapsus |
| --- | --- | --- | --- |
| Sequences/cadences: email, call and task steps, manual LinkedIn tasks | Advanced | SF (Sales Engagement cadences), HS (sequences, 5,000 per account), MS (sequences), ZO (cadences), FW (Pro), AT (Pro), MO (Pro), CL (workflows), CO (email series, Business), OD | Auto-unenrol on reply, meeting booked, bounce or unsubscribe; quiet hours by recipient time zone. |
| Throttling per mailbox and per domain | Advanced | HS (500 or 1,000 per user per day), SF, CL | Ties to section 2.4 deliverability. |
| Meeting scheduler: personal, round-robin and team pages, buffer and working hours | Core | HS, ZO (Bookings), SF (Scheduler), PD (Scheduler), FW, CL, OD (Appointments), MS (via Bookings) | Accessible booking pages; time-zone aware. |

### 1.6 Telephony, recording and messaging

| Feature | Tier | Competitors with it | Notes for Thapsus |
| --- | --- | --- | --- |
| Click-to-call via embedded softphone and a CTI adapter framework | Advanced | SF (Open CTI, Service Cloud Voice), HS (500, 3,000, 12,000 minutes by tier), MS (Teams phone, Contact Center), ZO (built-in calling, PhoneBridge), PD (Caller), FW, CL (native), OD (VoIP), CR, SG | Start with Twilio plus CTI adapters (Aircall, RingCentral). |
| Power and predictive dialers | Enterprise | CL (power dialer on Growth, predictive on Scale), FW (Freshcaller), SF and HS via partners | Predictive dialling carries TCPA and Ofcom abandoned-call risk; ship power dialer only. |
| Call recording with consent announcements | Advanced | CL (30, 90 days, unlimited retention by tier), HS, FW, SF, MS, ZO, PD | All-party consent logic (section 2.3). |
| Transcription and summaries | Advanced (AI) | CL (Call Assistant), HS (Notetaker), SF (conversation insights), MS (Copilot), ZO, AT (call intelligence, Pro), MO (notetaker) | Optional AI; regional speech models. |
| SMS (two-way) | Advanced | CL, HS, SF, MS, ZO, PD, FW | TCPA, state mini-TCPA, PECR reg 22. |
| WhatsApp Business Platform | Advanced | SF, HS, MS, ZO, FW, OD, CR | Meta terms s.4.7, per-message pricing (section 2.6). |
| Social inbox and publishing | Advanced | HS, SF (Marketing Cloud), ZO (Zoho Social), MS, FW, OD | Platform API terms change often; treat as optional. |

### 1.7 Products, quotes, CPQ, contracts, subscriptions

| Feature | Tier | Competitors with it | Notes for Thapsus |
| --- | --- | --- | --- |
| Products and price books (multiple lists, currencies, volume and tier pricing) | Core | SF, HS, MS, ZO, PD, FW, CR, SG, OD, MO | Price lists mastered in KASONGO when installed. |
| Quotes with branded PDF templates and versioning | Core | SF, HS, MS, ZO, PD (Smart Docs), FW, CR, SG, OD, MO (20/50/250 quotes per month by tier), CO | |
| Discount approval thresholds and approval chains | Advanced | SF, HS (quote and pipeline approvals), MS, ZO (Enterprise), OD, FW, SG | Approver hierarchy from Thapsus-HR. |
| CPQ: bundles, options, configuration rules, guided selling | Enterprise | SF (Revenue Cloud, from $200 per user per month), ZO (CPQ, Professional and up), FW (CPQ), OD (configurator), MS (partners), HS (basic) | Large build; phase after launch. |
| E-signature (native or integrated) | Advanced | HS (quotes), ZO (Zoho Sign), PD (Smart Docs), OD (Sign), SF and MS via DocuSign or Adobe | Start with DocuSign and Adobe Acrobat Sign integrations. |
| Contracts (terms, dates, renewal notice, clause library) | Advanced / Enterprise CLM | SF (contracts; CLM in Revenue Cloud), ZO (Zoho Contracts), SG, OD | |
| Subscriptions, renewals, ARR/MRR and auto-created renewal deals | Advanced | SF (Revenue Cloud), HS (Commerce Hub), ZO (Zoho Billing), OD (Subscriptions), SG (churn signals) | Billing truth stays in MULLAH. |
| Payment links and invoices from the CRM | Advanced | HS (Commerce Hub), MO (invoices), ZO (Books), OD | Delegate to MULLAH; Stripe for card links. |

### 1.8 Sales performance management

| Feature | Tier | Competitors with it | Notes for Thapsus |
| --- | --- | --- | --- |
| Forecasting with categories (pipeline, best case, commit, closed), hierarchy roll-ups, manager adjustments, snapshots | Advanced | SF, HS (hierarchy forecasting on Enterprise), MS, ZO (from Standard), FW (Enterprise), SG (Advanced+), CR, PD (forecast view), OD | Hierarchy from Thapsus-HR or CRM roles. |
| Quotas and goals | Advanced | SF, HS (goals), MS, ZO, PD, FW | |
| Territories (rule-based, hierarchical, several models, assignment preview) | Enterprise | SF (territory management), MS, ZO (Enterprise), FW (Pro), SG, OD (sales teams) | |
| Commissions / incentive compensation | Enterprise | SF (Spiff in Max; incentive compensation add-on), OD (commissions); others via partners (CaptivateIQ, QuotaPath) | Thapsus-HR "never runs payroll": CRM calculates commission statements, exports to the customer's payroll provider. |
| Sales playbooks and guided selling | Advanced | HS (playbooks), SF (Sales Programs from $100), MS (playbooks), ZO (Blueprint), SG (Smart Guides, Premier+), CR | |
| Account planning: whitespace, org charts, relationship maps | Enterprise | SF (account plans), MS (org charts), SG, HS (partial) | |
| Coaching and call scorecards | Enterprise | SF (Sales Coach agent), CL (call coaching, Scale), MS | Never infer agent emotions (AI Act Art 5(1)(f)). |

### 1.9 Partner relationship management and field sales

| Feature | Tier | Competitors with it | Notes for Thapsus |
| --- | --- | --- | --- |
| Partner portal: deal registration, lead distribution, partner tiers, MDF, co-selling | Enterprise | SF (PRM from $10 per login per month), MS (Power Pages), ZO (portals, Professional), CR, OD (resellers) | External identities per tenant; EAA accessibility. |
| Field sales: maps, nearby accounts, route planning, check-ins with location | Advanced | SF (Maps, in Max), ZO (RouteIQ, check-in), SG (geo mapping, Advanced+), FW, PD, MS, OD | Location captured only at check-in, never continuous tracking of employees (UK GDPR, EU employee monitoring rules). |
| Offline mobile with sync and conflict handling | Advanced | SF, MS, ZO, FW, PD (partial) | Installable PWA with offline cache, consistent with the suite's PWA rule. |

### 1.10 Marketing automation

| Feature | Tier | Competitors with it | Notes for Thapsus |
| --- | --- | --- | --- |
| Segments and lists (static, dynamic, cross-object) | Core | All | |
| Email campaign builder (drag-and-drop, dynamic content, preview, accessibility checker) | Core (basic) / Advanced | HS, SF, MS, ZO (Campaigns), PD (Campaigns add-on), FW, MO (Pro mass email), CR, SG (Sugar Market), OD, CO | Accessible templates by default. |
| Forms and landing pages | Core (forms) / Advanced (pages) | HS, SF, MS, ZO, FW, CR, SG, OD | Host on region-pinned infrastructure; no CDN caching of personal data. |
| Journeys: multi-step, branching, waits, goals, re-entry rules | Advanced | HS (Professional), SF (Marketing Cloud), MS (Customer Insights – Journeys), ZO (customer journeys, Professional), CR, SG, OD | |
| A/B and multivariate tests | Advanced | HS, SF, MS, ZO, FW, SG, OD | |
| Events and webinars (registration, Zoom/Teams sync, check-in, follow-up) | Advanced | SF, MS, HS (via apps), ZO (Backstage), OD (Events), CR | |
| Ad-audience sync (Google Customer Match, Meta, LinkedIn Matched Audiences) | Advanced | HS (ads tools), SF, MS, ZO | Hashed upload; EU/UK consent for ad use; CCPA "sharing" opt-out and GPC suppression. |
| Social publishing and monitoring | Enterprise, optional | HS, SF, ZO, OD | |
| Attribution (first, last, multi-touch; campaign influence) | Advanced / Enterprise | HS (multi-touch on Professional and up), SF (campaign influence), MS, ZO, SG | |
| SMS and WhatsApp campaigns | Advanced | SF, HS, MS, ZO, FW, OD | Marketing templates billed per message by Meta. |
| Preference centre and subscription types | Core | HS, SF, MS, ZO, FW, OD | See 1.15. |
| Website tracking and analytics | Advanced | HS, SF, MS, ZO, FW, OD | UK: first-party statistics allowed without consent with an opt-out (PECR Sch A1 para 5); EU: consent. |

### 1.11 Customer service

| Feature | Tier | Competitors with it | Notes for Thapsus |
| --- | --- | --- | --- |
| Tickets/cases with queues, priorities, statuses, parent/child | Core (service module) | SF, HS, MS, ZO (Desk), FW (Freshdesk), CR, SG (Serve), OD (Helpdesk), MO (service) | |
| Omnichannel inbox (email, chat, WhatsApp, SMS, social, voice) | Advanced | SF, HS, MS, ZO, FW, CR | |
| SLAs, entitlements, business hours, escalations | Advanced | SF, HS (conditional SLAs, Professional), MS, ZO, FW, CR, SG | Business-hours calendars per IANA zone; holiday calendars as data. |
| Skills- and capacity-based routing | Enterprise | SF (Omni-Channel), MS (unified routing, Enterprise), HS (advanced routing, Enterprise), FW | |
| Macros and canned responses | Core | HS (500 macros Professional, 1,500 Enterprise), SF, MS, ZO, FW | |
| Knowledge base (internal and public, versions, approvals, multilingual) | Core / Advanced | HS (1 base on Starter, 25 on Professional), SF, MS, ZO, FW, CR, OD | Source of truth for AI answers with citations. |
| Customer self-service portal | Advanced | SF, MS (portals), HS, ZO, FW, CR, OD | In EAA scope when the customer serves consumers in the EU. |
| Live chat widget and rule-based bot | Core / Advanced | All service products | AI bot disclosure under AI Act Art 50(1). |
| CSAT, CES and NPS surveys | Advanced | HS (Professional), SF (Feedback Management add-on), MS, ZO, FW, OD | |
| Contact centre (IVR, WFM, quality management) | Enterprise / partner | MS (Contact Center $110), SF (Contact Center), FW | Integrate rather than build. |
| Field service (work orders, dispatch) | Out of launch scope | SF, MS, ZO, OD | Belongs with KASONGO or later. |

### 1.12 Customer success

| Feature | Tier | Competitors with it | Notes for Thapsus |
| --- | --- | --- | --- |
| Health scores (rule-based, weighted signals) | Advanced | HS (customer success workspace, 50 health scores), SF (partners), SG (Sugar Predict churn signals), FW | Explainable weights shown to users. |
| CS playbooks, onboarding plans, task templates | Advanced | HS, SF, CR, ZO | |
| Renewal and expansion pipelines | Advanced | SF, HS, ZO, SG | |
| Product-usage ingestion (events API) | Enterprise | SF (Data 360), HS (custom events), SG | |
| Churn-risk alerts | Advanced (rules) / optional AI | SG, HS, SF | |

### 1.13 Reporting and dashboards

| Feature | Tier | Competitors with it | Notes for Thapsus |
| --- | --- | --- | --- |
| Standard reports: pipeline, funnel conversion, revenue, activity, leaderboards | Core | All | |
| Custom report builder (cross-object), dashboards, scheduled delivery | Core / Advanced | All (HS: 10/30/75/100 dashboards by tier) | Scheduled reports by email are personal data leaving the app; respect permissions at send time. |
| Historical trending, pipeline waterfall, snapshots | Advanced | SF, HS, MS, ZO, CL | |
| Forecast versus quota | Advanced | SF, HS, MS, ZO, FW | |
| Warehouse export / BI connectors | Enterprise | SF (Tableau Next), MS (Power BI), ZO (Analytics), HS (Data Hub) | BigQuery in the tenant's region only. |
| Natural-language report building | Optional AI | SF, HS (Data Agent), MS, ZO, SG | |

### 1.14 Data quality and enrichment

| Feature | Tier | Competitors with it | Notes for Thapsus |
| --- | --- | --- | --- |
| Duplicate rules (fuzzy match on name, email, domain, phone) and merge with field-level survivorship | Core | SF, HS, MS, ZO, FW, CR, SG, OD, AT, PD | `pg_trgm` and full-text search fit the suite's search rule. |
| Validation rules | Core / Advanced | SF, MS (business rules), ZO, HS (property validation), CR, SG | |
| Normalisation (E.164 phones, ISO countries, domains) and bulk clean-up jobs | Core | All | |
| First-party enrichment (email signatures, domain to company) | Advanced | HS, SF, ZO, CO, PD (Smart Contact Data) | Low GDPR risk if derived from the customer's own correspondence. |
| Company registries (Companies House, EU registers, SEC EDGAR) for firmographics | Advanced | SF and HS via partners | Mostly non-personal data; prefer this. |
| Third-party person enrichment (ZoomInfo, Apollo, Dun and Bradstreet, Cognism, HubSpot Breeze Intelligence) | Enterprise, optional | HS (enrichment now in Smart CRM, secondary), SF, MS (Sales Premium "AI-powered data enrichment"), SG (Premier+), ZO | Controller must give Art 14 notices; each provider in `SUBPROCESSORS.md`; record source per field; bounded retention (CNIL KASPR decision). |

### 1.15 Consent and preference centre

| Feature | Tier | Competitors with it | Notes for Thapsus |
| --- | --- | --- | --- |
| Lawful basis per purpose per person (consent, legitimate interests, contract) | Core | HS, SF (consent data model), MS (consent centre), ZO, FW | Customers are controllers; give them a record of basis and evidence. |
| Consent evidence: who, when, channel, wording version, source page or form, IP where lawful | Core | HS, SF, MS | Immutable consent events. |
| Subscription types and preference centre page (per brand, per locale) | Core | HS, SF, MS, ZO, OD | |
| Double opt-in | Core | HS, ZO, OD, SF, MS | Mandatory default for DE-located recipients (practice; UNVERIFIED as legal rule). |
| Global and per-channel suppression (email, phone, SMS, WhatsApp, post) | Core | All | |
| Do-not-call screening: UK TPS and CTPS, US National DNC, state DNC lists | Advanced | SF, MS, ZO via partners; CL | TPS/CTPS licence is a human task. |
| Global Privacy Control and CCPA opt-out of "sale/sharing" | Core (US tenants) | HS, SF | Suppress from ad-audience sync. |
| Jurisdiction rules engine (PECR, national ePrivacy, CAN-SPAM, TCPA and mini-TCPAs, CASL optional) | Core | Partial in all | Effective-dated rule data with sources, like the HR jurisdiction packs. |
| Data subject request tooling (find, export, erase, restrict, object) | Core | HS, SF (Privacy Center add-on), MS, ZO | DUAA complaint-handling (30-day acknowledgement) as a workflow. |

### 1.16 Import and migration

| Feature | Tier | Competitors with it | Notes for Thapsus |
| --- | --- | --- | --- |
| CSV/XLSX import: mapping, dedupe, dry run, rollback by import batch | Core | All | |
| Migration from Salesforce, HubSpot, Pipedrive, Zoho, Dynamics via their APIs (objects, custom fields, owners, activities, files, pipelines) | Advanced | HS (migration services), ZO (Zwitch), PD (import partners), AT, MO | Productised importers are a strong acquisition lever. |
| Mailchimp/marketing-list import preserving unsubscribes, bounces and consent | Core | HS, ZO, OD | Never re-subscribe imported opt-outs. |
| Data Act-compliant export of everything (records, metadata, files, audit) | Core (EU) | Partial in all | Art 30(3) structured, commonly used, machine-readable format. |

### 1.17 Platform: APIs, marketplace, mobile

| Feature | Tier | Competitors with it | Notes for Thapsus |
| --- | --- | --- | --- |
| REST API (OpenAPI 3.1), bulk API, webhooks, idempotency keys | Core | All | Versioned `/v1`, as in the suite rules. |
| Developer sandboxes and config promotion | Enterprise | SF, MS, ZO (Enterprise developer sandbox), FW (Enterprise sandbox), HS (Enterprise), CR | |
| UI extensions (cards, iframes, custom actions) | Advanced | SF, HS, MS, ZO, PD, AT | |
| App marketplace with OAuth apps and review | Enterprise | SF (AppExchange/AgentExchange), HS, MS (AppSource), ZO, PD, FW, MO | |
| Zapier, Make, n8n connectors | Core | All | |
| MCP server for customers' own AI agents | Advanced, optional | SF (hosted MCP), HS (remote MCP, GA 2026), AT, PD, ZO (secondary for all) | Off by default; OAuth scopes; audit per tool call. |
| Mobile app with offline | Advanced | All (native apps) | Installable PWA. |

### 1.18 Administration, security and trust (enterprise buyer expectations)

| Feature | Tier | Competitors with it | Notes for Thapsus |
| --- | --- | --- | --- |
| Roles, permission sets, field-level security, record sharing rules, teams/business units | Core / Advanced | SF, MS, ZO, HS, FW (field-level permissions, Enterprise), CR | RLS for tenant isolation; app-level sharing for record access. |
| SSO (SAML, OIDC), MFA, session policies, IP allow-lists | Core (SSO on Advanced) | All at upper tiers (AT SSO and SCIM on Enterprise) | |
| SCIM provisioning | Advanced | SF, MS, HS, AT (Enterprise), ZO | From Thapsus-HR or the customer's IdP. |
| Audit log export, login history | Core | SF, MS, ZO, FW (Enterprise), HS (Enterprise) | Append-only. |
| Field-level encryption, customer-managed keys | Enterprise | SF (Shield), ZO (Enterprise field encryption), MS | Per-region KMS keys. |
| Data residency selection | Core (Thapsus differentiator) | SF (Hyperforce regions), HS (EU data centre), MS, ZO (UK/EU DCs) | UK, EU, US cells. |
| Retention policies and legal hold | Advanced | SF (Archive, Max), MS, HS (partial) | |

---

## 2. Regulation as of November 2026

### 2.1 United Kingdom

| Topic | Rule | Status | Source | CRM implication |
| --- | --- | --- | --- | --- |
| Lawful basis for B2B prospecting | UK GDPR Art 6(1)(f) legitimate interests with a three-part test (purpose, necessity, balancing). | IN FORCE | https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/lawful-basis/legitimate-interests/when-can-we-rely-on-legitimate-interests/ | Record a legitimate-interests assessment reference per purpose; show basis on each contact. |
| Direct marketing named as an example legitimate interest | New UK GDPR Art 6(11)(a): processing necessary for direct marketing "may" be a legitimate interest (with intra-group admin transfers and network security). The balancing test still applies. Inserted by DUAA s.70. | IN FORCE from 5 Feb 2026 | https://www.legislation.gov.uk/ukpga/2025/18/section/70 ; https://www.legislation.gov.uk/uksi/2026/82/made | Helps postal and B2B marketing; does not override PECR consent rules for electronic marketing. |
| Recognised legitimate interests | New Art 6(1)(ea) with an Annex 1 list (for example public security, emergencies, crime, safeguarding). No balancing test, necessity still applies. Direct marketing is **not** a recognised legitimate interest. | IN FORCE from 5 Feb 2026 | Same as above; ICO DUAA data protection summary https://ico.org.uk/about-the-ico/what-we-do/legislation-we-cover/data-use-and-access-act-2025/the-data-use-and-access-act-2025-duaa-summary-of-the-changes/data-protection/ | Offer "recognised legitimate interest" as a basis option, but not for marketing purposes. |
| Right to object to direct marketing | Absolute (UK GDPR Art 21(2)–(3)). ICO page updated 2 Nov 2026. | IN FORCE | https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/individual-rights/individual-rights/right-to-object/ | Objection becomes a permanent suppression across all channels and sequences. |
| Notice when data is collected from elsewhere | UK GDPR Art 14: inform within a reasonable period, at the latest one month, or at first communication. | IN FORCE | https://www.legislation.gov.uk/eur/2016/679/article/14 | Bought lists, enrichment and event attendee imports need a notice workflow. |
| Electronic mail marketing (email, SMS) to individuals | PECR reg 22: consent or the "soft opt-in" for similar products and services to existing customers, with an opt-out at collection and in every message. Applies to "individual subscribers" (including sole traders and some partnerships). Corporate subscribers are outside reg 22 but reg 23 (sender identity, valid contact address) applies. | IN FORCE (reg 22 text verified on legislation.gov.uk; the reg 23 and corporate-subscriber detail not re-read) | https://www.legislation.gov.uk/uksi/2003/2426/regulation/22 ; https://ico.org.uk/for-organisations/direct-marketing-and-privacy-and-electronic-communications/guide-to-pecr/electronic-and-telephone-marketing/electronic-mail-marketing/ | Contact field "subscriber type" (individual, sole trader, corporate) drives eligibility. |
| Charity soft opt-in | New PECR reg 22(3A) (DUAA s.114): charities may email/text supporters who expressed interest in or supported their charitable purposes, if an opt-out was given at collection and in each message; only for details collected on or after 5 Feb 2026; ICO final guidance 28 Apr 2026. | IN FORCE from 5 Feb 2026 | https://ico.org.uk/about-the-ico/media-centre/news-and-blogs/2026/04/charities-given-new-flexibility-to-contact-supporters-under-data-law-change/ | Tenant flag "registered charity"; capture date on each contact point so the 5 Feb 2026 cut-off can be enforced. |
| Cookies and similar technologies | New PECR Schedule A1 (DUAA s.112, Sch 12): no consent needed for (a) transmission, (b) strictly necessary for a requested service (security, fraud, faults, authentication, recording user inputs), (c) statistics to improve the service if information plus a simple free objection are given, (d) adapting appearance or functionality on the same conditions, (e) emergency location. Advertising and cross-site tracking still need consent. | IN FORCE from 5 Feb 2026 | https://www.legislation.gov.uk/ukpga/2025/18/schedule/12 | CRM web-tracking script: UK tenants may run first-party analytics with an opt-out; visitor identification for sales still needs consent. |
| PECR enforcement and fines | DUAA s.115 and Sch 13 replace the PECR enforcement regime with the DPA 2018 toolkit; maximum penalties align with UK GDPR (up to GBP 17.5m or 4% of worldwide turnover). Pre-5 Feb 2026 conduct stays under the old regime (regs 8–11 of SI 2026/82). | IN FORCE from 5 Feb 2026 (penalty amounts per regulation: secondary) | https://www.legislation.gov.uk/uksi/2026/82/made ; https://www.policypros.co.uk/cookie-consent-changes-duaa-guide/ | Raises the stakes for customers; consent and suppression features become a selling point. |
| PECR breach notification | DUAA s.111 commenced 20 Aug 2025 (SI 2025/904). | IN FORCE (secondary) | https://bratby.law/data-use-and-access-act-2025-commencement/ | Relevant to Thapsus as a communications-adjacent service only if it provides public electronic communications services (it does not). |
| Telephone marketing | PECR reg 21: no live marketing calls to numbers on TPS/CTPS or to people who objected; automated calls need consent (reg 19). | IN FORCE (not re-verified this session) | https://www.legislation.gov.uk/uksi/2003/2426/regulation/21 | TPS/CTPS screening integration; block dialler on listed numbers. |
| Automated decision-making | New UK GDPR Arts 22A–22D (DUAA s.80): a significant decision is "solely automated" without meaningful human involvement; special category data restricted (22B); safeguards: information, representations, human intervention, contest (22C). Savings for decisions before 5 Feb 2026. | IN FORCE from 5 Feb 2026 | https://www.legislation.gov.uk/ukpga/2025/18/section/80 | CRM AI must keep a human in the loop for any decision with legal or similar effect (credit holds, pricing eligibility for consumers). |
| Data protection complaints | DPA 2018 s.164A (DUAA s.103): controllers must facilitate complaints and acknowledge within 30 days; applies to complaints received from 19 June 2026. | IN FORCE from 19 Jun 2026 | https://www.legislation.gov.uk/uksi/2026/82/made | Ship a "privacy complaint" case type with a 30-day acknowledgement SLA. |
| ICO direct-marketing guidance | Consent and legitimate interests are the two likely bases; legitimate interests is not available where PECR requires consent. | Guidance (current) | https://ico.org.uk/for-organisations/direct-marketing-and-privacy-and-electronic-communications/sending-direct-marketing-choosing-your-lawful-basis/ | Build the rules engine to evaluate PECR first, then UK GDPR basis. |
| Call recording | Investigatory Powers Act 2016 and the Interception by Businesses Regulations 2018 permit business recording for listed purposes with notice; UK GDPR transparency applies. | IN FORCE (not re-verified this session) | https://www.legislation.gov.uk/uksi/2018/356/contents/made | Recording notice prompts; retention per tenant. |

### 2.2 European Union

| Topic | Rule | Status | Source | CRM implication |
| --- | --- | --- | --- | --- |
| Lawful basis for B2B marketing | GDPR Art 6(1)(f) and Recital 47 ("direct marketing may be regarded as carried out for a legitimate interest"); EDPB Guidelines 1/2024 on legitimate interest. | GDPR IN FORCE; EDPB Guidelines 1/2024 published as a consultation version (feedback 9 Oct to 20 Nov 2024); final adoption UNVERIFIED | https://eur-lex.europa.eu/eli/reg/2016/679/oj ; https://www.edpb.europa.eu/our-work-tools/documents/public-consultations/2024/guidelines-12024-processing-personal-data-based_en | Same LIA record as UK. |
| Unsolicited electronic communications | ePrivacy Directive 2002/58/EC Art 13: prior consent for email/SMS marketing to natural persons, soft opt-in for existing customers (Art 13(2)); treatment of legal persons left to Member States (for example Germany requires consent for most B2B email under UWG s.7: not re-verified). | IN FORCE | https://eur-lex.europa.eu/eli/dir/2002/58/oj | Country-specific rule packs keyed on recipient country. |
| Cookies and tracking | ePrivacy Directive Art 5(3): consent for storage/access except strictly necessary. | IN FORCE | Same as above | Consent banner integration for CRM web tracking and forms in the EU. |
| ePrivacy Regulation | Proposal withdrawn; announced in the Commission 2025 work programme (11 Feb 2025); withdrawal reported as published in the OJ in Oct 2025 (secondary). | WITHDRAWN | https://www.hunton.com/privacy-and-cybersecurity-law-blog/european-commission-withdraws-eprivacy-regulation-and-ai-liability-directive-proposals | No single EU regulation coming; keep national variants. |
| Digital Omnibus (data/GDPR/cookies) | Commission proposal of Nov 2025 would move cookie rules into GDPR (new Art 88a/88b) and add browser-signal consent. Council compromise (June 2026) deleted Art 88a and the browser signal; trilogues not started as of Oct 2026 (secondary). | PROPOSED | https://www.legal500.com/intelligence/european-union/privacy/digital-omnibus-what-the-proposed-changes-mean-for-gdpr-privacy-and-cookies ; https://www.osborneclarke.com/what-our-clients-are-talking-about/digital-regulation/digital-omnibus-package | Do not design for it yet; keep rules as data so a change is configuration. |
| Third-party enrichment | CNIL fined KASPR EUR 240,000 (decided 5 Dec 2024, published 19 Dec 2024): no legal basis for collecting LinkedIn contact details users had restricted, retention reset on every update, late English-only notices, generic answers on data sources. | Decision (enforcement precedent) | https://www.cnil.fr/en/data-scraping-kaspr-fined-eu240000 | Field-level provenance, retention clocks that do not reset, Art 14 notice tooling, no scraping features. |
| Data protection by design for profiling | GDPR Art 22: no solely automated decisions with legal or similarly significant effects without a listed exception and safeguards. | IN FORCE | https://eur-lex.europa.eu/eli/reg/2016/679/oj | Lead scoring is normally not a significant decision; B2C eligibility or pricing decisions can be. |

**EU AI Act (Regulation (EU) 2024/1689, as amended by Regulation (EU) 2026/1744)**

| Provision | What it means for a CRM | Status | Source |
| --- | --- | --- | --- |
| Art 5 prohibited practices, incl. Art 5(1)(f) emotion recognition in the workplace and education | Inferring emotions or intentions of **employees** from biometric data (voice, face) is prohibited; the Commission guidelines give a call centre tracking its own staff's emotions as an example. Inferring **customers'** emotions in a call centre is not prohibited by Art 5(1)(f), but safeguards are needed so that the same tool is not used on agents. | IN FORCE since 2 Feb 2025 | https://www.covingtonblogs.com/2025/06/04/european-commission-guidelines-on-prohibited-ai-practices-under-the-eu-artificial-intelligence-act/ ; https://cms.law/en/gbr/legal-updates/eu-commission-issues-guidelines-on-prohibited-ai-practices-part-ii |
| Art 4 AI literacy | Reworded by the Omnibus to a duty to "take measures to support" AI literacy of staff using AI. | IN FORCE (since 2 Feb 2025; amended text in force 27 Jul 2026) | https://artificialintelligenceact.eu/ai-act-explorer/digital-omnibus/ |
| GPAI model obligations | Apply to model providers (for example Google, Anthropic), not to Thapsus as an integrator, unless Thapsus fine-tunes and places a model on the market. | IN FORCE since 2 Aug 2025 | https://www.aiacto.eu/en/blog/digital-omnibus-ai-act-report-2027 |
| Art 50(1) | AI systems that interact directly with people (service chatbot, AI SDR replying to prospects, AI voice agent) must tell people they are dealing with AI unless obvious. | IN FORCE since 2 Aug 2026 | https://artificialintelligenceact.eu/article/50/ |
| Art 50(2) | Providers of systems generating synthetic text, audio, image or video must mark outputs in a machine-readable, detectable way (exemption for assistive editing that does not substantially alter input). Systems placed on the market before 2 Aug 2026 have until **2 Dec 2026**. | IN FORCE for new systems since 2 Aug 2026; APPLIES FROM 2 Dec 2026 for legacy systems | https://artificialintelligenceact.eu/ai-act-explorer/digital-omnibus/ |
| Art 50(3) | Deployers of emotion recognition or biometric categorisation must inform exposed people. | IN FORCE since 2 Aug 2026 | https://artificialintelligenceact.eu/article/50/ |
| Art 50(4) | Deployers publishing AI-generated text on matters of public interest must disclose unless human-reviewed with editorial responsibility; deep fakes always disclosed. | IN FORCE since 2 Aug 2026 | Same |
| Annex III high-risk (incl. emotion recognition systems, employment uses such as task allocation and performance monitoring, creditworthiness of natural persons) | Requirements now apply from **2 Dec 2027**. A CRM that allocates leads to employees by AI or monitors sales-rep performance could be "employment" high-risk; keep those features rule-based or human-decided. | APPLIES FROM 2 Dec 2027 | https://transition-pathways.europa.eu/adv-man/legislation/ai-act-digital-omnibus-high-risk-deadlines-postponed-machinery-carved-out |
| New prohibitions (non-consensual intimate imagery, CSAM generation) | Not relevant to CRM functions, but the image-generation features of marketing tools must keep safeguards. | APPLIES FROM 2 Dec 2026 | https://artificialintelligenceact.eu/ai-act-explorer/digital-omnibus/ |

Text-only sentiment analysis is generally outside "emotion recognition" because the AI Act definition
relies on biometric data; this reading is from commentary, not the guidelines' text (UNVERIFIED).

**EU Data Act (Regulation (EU) 2023/2854), Chapter VI switching**

| Article | Obligation for a SaaS CRM | Status | Source |
| --- | --- | --- | --- |
| Art 23 | Remove commercial, technical, contractual and organisational obstacles to switching, multi-vendor use, porting data. | IN FORCE since 12 Sep 2025 | https://eur-lex.europa.eu/eli/reg/2023/2854/oj/eng |
| Art 25 | Written contract with: max **2-month** notice to start switching; max **30-day** transitional period (extendable once by customer; up to 7 months if technically unfeasible, notified within 14 working days); exit and continuity support; exhaustive list of exportable data and digital assets; **at least 30 days** data-retrieval period; full erasure afterwards. | IN FORCE since 12 Sep 2025 | Same |
| Art 26 | Describe switching procedures, formats and restrictions; reference an up-to-date register of data structures and formats. | IN FORCE | Same |
| Art 28 | Publish the jurisdiction of ICT infrastructure and measures against unlawful international government access to non-personal data. | IN FORCE | Same |
| Art 29 | Until 12 Jan 2027 switching charges capped at direct cost; **from 12 Jan 2027 no switching charges**, including data egress. | IN FORCE; zero-charge rule APPLIES FROM 12 Jan 2027 | https://digital-strategy.ec.europa.eu/en/policies/data-act-explained |
| Art 30 | Free open interfaces for all customers and destination providers; export all exportable data in a structured, commonly used, machine-readable format; follow common specifications within 12 months of publication. | IN FORCE | https://eur-lex.europa.eu/eli/reg/2023/2854/oj/eng |
| Art 31 | Custom-built services exempt from some duties; non-production trial versions exempt from the chapter; tell customers before contracting. | IN FORCE | Same |

**European Accessibility Act (Directive (EU) 2019/882)**

| Point | Detail | Status | Source |
| --- | --- | --- | --- |
| Application | Services provided to consumers after **28 June 2025**, including e-commerce, consumer banking, e-books, electronic communications, passenger transport services. | IN FORCE | https://eur-lex.europa.eu/eli/dir/2019/882/oj/eng |
| Scope for a CRM | Thapsus CRM is a B2B tool and is not itself an in-scope service, but portals, chat widgets, forms, booking pages and emails that customers present to EU consumers form part of the customer's in-scope service. Customers will require conformance evidence (EN 301 549 / WCAG). | Analysis | https://commission.europa.eu/strategy-and-policy/policies/justice-and-fundamental-rights/disability/union-equality-strategy-rights-persons-disabilities-2021-2030/european-accessibility-act_en |
| Microenterprises | Service providers with fewer than 10 staff and turnover or balance sheet up to EUR 2m are exempt (Art 4(5)). | IN FORCE | https://eur-lex.europa.eu/eli/dir/2019/882/oj/eng |
| Transitional rules | Products used to provide a service before 28 June 2025 may continue for 5 years; self-service terminals up to 20 years (Recital 101; Art 32 text not retrieved). | IN FORCE (Art 32 detail UNVERIFIED) | Same |

### 2.3 United States

**Email: CAN-SPAM Act**

| Rule | Status | Source |
| --- | --- | --- |
| Accurate headers, non-deceptive subject lines, ad identification, valid physical postal address, working opt-out for at least 30 days, opt-outs honoured within **10 business days**, no fee or extra data to opt out, no selling opted-out addresses; both the advertiser and the sender can be liable. **B2B email is covered.** Penalty up to **USD 53,088 per email** (FTC page, January 2024 figure; later inflation adjustments UNVERIFIED). | IN FORCE | https://www.ftc.gov/business-guidance/resources/can-spam-act-compliance-guide-business |

**Calls and texts: TCPA (47 U.S.C. 227; 47 CFR 64.1200)**

| Rule | Status | Source |
| --- | --- | --- |
| FCC "one-to-one consent" and "logically and topically related" requirements (2023 order, Part III.D) **vacated** on 24 Jan 2025 by the Eleventh Circuit in Insurance Marketing Coalition v FCC (No. 24-10277). Prior express written consent for telemarketing robocalls/robotexts still required. | IN FORCE (vacatur) | https://media.ca11.uscourts.gov/opinions/pub/files/202410277.pdf |
| 2024 consent-revocation order: most provisions effective 11 Apr 2025; the "revoke all" provision was delayed to 11 Apr 2026 and then to 31 Jan 2027 (FCC DA 26-12). | Superseded in part | https://docs.fcc.gov/public/attachments/DA-26-12A1.pdf ; https://www.burr.com/telephone-consumer-protection-act/the-fcc-delays-effective-date-of-tcpa-revoke-all-rule-until-january-31-2027 |
| **Revised revocation rules**: Report and Order adopted 30 Sep 2026, published in the Federal Register 26 Oct 2026, effective **25 Nov 2026**. Revocations honoured within a reasonable time, max **10 business days**; for informational calls/texts a revocation can be limited to that category; a telemarketing revocation stops all telemarketing from that caller; callers may designate exclusive revocation methods (automated voice/key-press, standard text keywords such as STOP, or a designated website or number), clearly disclosed; otherwise "any reasonable means". FNPRM comments due 25 Nov 2026, replies 28 Dec 2026 (proposals: 7 business days, two-way texting, mandatory revoke-all option, affiliates). | APPLIES FROM 25 Nov 2026 (secondary; Federal Register text not opened) | https://www.womblebonddickinson.com/us/node/21530 ; https://www.troutman.com/insights/fcc-revises-tcpa-revocation-of-consent-rules-that-were-set-to-go-into-effect-in-january/ |
| Calling hours 8 am to 9 pm recipient local time; company-specific do-not-call lists; National DNC registry. | IN FORCE (not re-verified this session) | https://www.ecfr.gov/current/title-47/chapter-I/subchapter-B/part-64/subpart-L/section-64.1200 |
| AI-generated voices count as "artificial or prerecorded voice" under the TCPA (FCC 24-17 Declaratory Ruling, CG Docket 23-362, adopted 2 Feb 2024, released 8 Feb 2024), so outbound AI voice agents need prior express consent (written, for marketing). | IN FORCE | https://docs.fcc.gov/public/attachments/FCC-24-17A1.pdf |

**State "mini-TCPA" laws**

| State | Key rules | Status | Source |
| --- | --- | --- | --- |
| Florida (Fla. Stat. 501.059, amended 2021 and narrowed by HB 761 in 2023) | Prior express written consent (signature incl. checkbox) for unsolicited sales calls using "an automated system for the selection **or** dialing" of numbers or a recorded message; text plaintiffs must first reply STOP and the sender has **15 days** to stop; damages USD 500 or actual, up to treble if wilful. | IN FORCE | https://www.leg.state.fl.us/statutes/index.cfm?App_mode=Display_Statute&URL=0500-0599/0501/Sections/0501.059.html |
| Oklahoma Telephone Solicitation Act (15 O.S. 775C.1–775C.6) | Modelled on Florida's 2021 text; effective 1 Nov 2022. | IN FORCE (secondary) | https://www.bytebacklaw.com/2026/07/looking-back-on-the-last-year-of-state-level-tcpa-updates/ |
| Texas (SB 140) | Telephone-solicitation law extended to text messages, registration duties; effective 1 Sep 2025. | IN FORCE (secondary) | https://natlawreview.com/article/texas-expands-its-mini-tcpa-text-messages-now-covered-litigation-risk-rises |
| Maryland, Connecticut, Washington, Virginia and others | Stricter calling hours, call-frequency caps, consent rules. Maryland's Stop the Spam Calls Act effective 1 Jan 2024 (not re-verified). | IN FORCE (secondary) | https://www.manatt.com/insights/newsletters/tcpa-connect/state-mini-tcpa-telemarketing-laws-continue-to-p |

CRM implication: a per-state calling/texting policy table (hours, frequency caps, consent type, holiday
rules) evaluated before dial or send; STOP keywords processed instantly; consent evidence stored per
number with the form wording.

**State comprehensive privacy laws** (IAPP tracker chart, updated 29 June 2026,
https://assets.contentstack.io/v3/assets/bltd4dd5b2d705252bc/blt90070182553bdfe6/6a429c09099ffcd63cefaad7/State_Comp_Privacy_Law_Chart.pdf)

| State | Law | Effective | Status on 7 Nov 2026 |
| --- | --- | --- | --- |
| California | CCPA as amended by CPRA | 1 Jan 2020 (CPRA amendments 1 Jan 2023) | IN FORCE |
| Virginia | Consumer Data Protection Act | 1 Jan 2023 | IN FORCE |
| Colorado | Colorado Privacy Act | 1 Jul 2023 | IN FORCE |
| Connecticut | Data Privacy Act | 1 Jul 2023 | IN FORCE |
| Utah | Consumer Privacy Act | 31 Dec 2023 | IN FORCE |
| Texas | Data Privacy and Security Act | 1 Jul 2024 | IN FORCE |
| Oregon | Consumer Privacy Act | 1 Jul 2024 | IN FORCE |
| Montana | Consumer Data Privacy Act | 1 Oct 2024 | IN FORCE |
| Iowa | Consumer Data Protection Act | 1 Jan 2025 | IN FORCE |
| Delaware | Personal Data Privacy Act | 1 Jan 2025 | IN FORCE |
| Nebraska | Data Privacy Act | 1 Jan 2025 | IN FORCE |
| New Hampshire | (comprehensive privacy law) | 1 Jan 2025 | IN FORCE |
| New Jersey | (comprehensive privacy law) | 15 Jan 2025 | IN FORCE |
| Tennessee | Information Protection Act | 1 Jul 2025 | IN FORCE |
| Minnesota | Consumer Data Privacy Act | 31 Jul 2025 | IN FORCE |
| Maryland | Online Data Privacy Act | 1 Oct 2025 | IN FORCE |
| Indiana | Consumer Data Protection Act | 1 Jan 2026 | IN FORCE |
| Kentucky | Consumer Data Protection Act | 1 Jan 2026 | IN FORCE |
| Rhode Island | Data Transparency and Privacy Protection Act | 1 Jan 2026 | IN FORCE (no cure period; list of specific third parties) |
| Oklahoma | (SB 546; signed 2026) | 1 Jan 2027 | APPLIES FROM 1 Jan 2027 |
| Louisiana | Data Privacy Act (SB 386; 2026) | 1 Jan 2027 | APPLIES FROM 1 Jan 2027 |
| Alabama | Personal Data Protection Act (2026) | 1 May 2027 | APPLIES FROM 1 May 2027 |
| Vermont | Data Privacy and Online Surveillance Act (S 0071; 2026) | 1 Jan 2028 | APPLIES FROM 1 Jan 2028 |

Florida's narrower Digital Bill of Rights (1 Jul 2024) is excluded from most counts. Laws other than
California's generally exempt employee and B2B contact data (confirmed for Indiana by
https://www.koleyjessen.com/insights/publications/new-state-privacy-laws-effective-january-1-2026-indiana-kentucky-and-rhode-island;
others secondary). CRM implication: B2C tenants need consumer-rights workflows (access, deletion,
correction, opt-out of targeted advertising, sale and profiling; universal opt-out signals such as GPC
in a growing number of states). Alabama's GPC requirement is disputed between sources (UNVERIFIED).

**California CPPA regulations (ADMT, risk assessments, cybersecurity audits)**

| Item | Date | Status | Source |
| --- | --- | --- | --- |
| Approved by OAL | 22 Sep 2025 (announced 23 Sep 2025) | Done | https://cppa.ca.gov/regulations/ccpa_updates.html |
| Regulations effective; risk-assessment duty starts | 1 Jan 2026 | IN FORCE | https://www.cppa.ca.gov/announcements/2025/20250923.html |
| ADMT used for significant decisions must comply (pre-use notice, opt-out, access) | 1 Jan 2027 | APPLIES FROM 1 Jan 2027 | Same |
| Existing processing risk assessments completed | 31 Dec 2027 | APPLIES FROM (secondary) | https://www.hunton.com/privacy-and-cybersecurity-law-blog/newly-approved-ccpa-regulations-have-staggered-deadlines-for-compliance |
| Risk-assessment attestation and summary to CPPA | 1 Apr 2028 | APPLIES FROM 1 Apr 2028 | https://www.cppa.ca.gov/announcements/2025/20250923.html |
| Cybersecurity audit certifications | 1 Apr 2028 (revenue over USD 100m), 1 Apr 2029 (USD 50–100m), 1 Apr 2030 (under USD 50m) | APPLIES FROM those dates | Same |

The definition of "significant decision" (financial or lending, housing, education, employment,
healthcare) and the removal of behavioural advertising from ADMT scope in the final text are from
memory of the 2025 drafts and were not re-read in this session (UNVERIFIED). CRM implication: lead
scoring for B2B sales is unlikely to be ADMT for a significant decision; CRM used by lenders, landlords
or insurers for consumer eligibility could be, so AI scoring must be optional, explainable and
overridable.

**Call-recording consent.** Most secondary sources list 11 all-party consent states: California,
Delaware, Florida, Illinois, Maryland, Massachusetts, Montana, Nevada, New Hampshire, Pennsylvania,
Washington; Oregon requires all-party consent for in-person conversations only; Michigan and
Connecticut are disputed (secondary: https://viirtue.com/call-recording-consent-laws-by-state-2026-guide/).
CRM implication: when any participant's number or location is in an all-party state, or unknown, play
a recording notice and capture consent; store the jurisdiction decision with the recording.

**Other US AI rules relevant to CRM.** Colorado's AI Act (SB 24-205) was due to apply from 30 Jun
2026 after a 2025 delay; whether it took effect or was amended again in 2026 is UNVERIFIED. State bot
disclosure laws (for example California's 2019 bot disclosure law, Utah's AI Policy Act) require
disclosure of AI in sales conversations (UNVERIFIED this session).

### 2.4 Email deliverability rules (POLICY)

| Provider | Who | Requirements | Enforcement | Source |
| --- | --- | --- | --- | --- |
| Gmail (personal accounts) | All senders: SPF or DKIM, valid forward and reverse DNS, TLS, RFC 5322, spam rate below 0.3%. Bulk senders (about 5,000+ messages a day to Gmail; status never expires): SPF **and** DKIM (1024-bit minimum, 2048 recommended), DMARC (p=none allowed) with From-domain alignment, one-click unsubscribe (RFC 8058 `List-Unsubscribe-Post`) plus visible body link for marketing and subscribed mail, honour unsubscribes within 48 hours (recommended). Transactional mail exempt from one-click. Messages to Google Workspace recipients are not covered. | Feb 2024 start; June 2024 one-click and spam-rate mitigation cut-off; **from November 2025 temporary and permanent rejections** | https://support.google.com/a/answer/81126 ; https://support.google.com/a/answer/14229414 |
| Yahoo | All senders: SPF or DKIM, spam rate under 0.3%, DNS, RFC 5321/5322. Bulk senders: SPF and DKIM, DMARC at least p=none and passing with relaxed alignment, one-click List-Unsubscribe (RFC 8058 POST recommended), visible unsubscribe link, process within **2 days**. | Since Feb 2024, gradual | https://senders.yahooinc.com/best-practices/ |
| Microsoft Outlook.com (hotmail.com, live.com, outlook.com) | Domains sending over 5,000 messages a day: SPF pass, DKIM pass, DMARC at least p=none aligned with SPF or DKIM; recommended working unsubscribe, list hygiene, consent. | Since **5 May 2025**: non-compliant mail to Junk; rejection with `550; 5.7.515 Access denied` at a date to be announced (not yet announced in the source). | https://techcommunity.microsoft.com/blog/microsoftdefenderforoffice365blog/strengthening-email-ecosystem-outlook%E2%80%99s-new-requirements-for-high%E2%80%90volume-senders/4399730 |

CRM implications: per-tenant authenticated sending domains with guided DNS set-up and continuous
checks; separate marketing and transactional streams and IPs; RFC 8058 headers on every marketing
message; complaint-rate monitoring with automatic campaign pause at 0.1% and hard stop near 0.3%;
bounce and feedback-loop processing; per-mailbox throttles for sequences sent through users' own
Gmail/Microsoft accounts.

### 2.5 Summary of regulatory dates that land in the next 18 months

| Date | Event | Status |
| --- | --- | --- |
| 25 Nov 2026 | Revised TCPA consent-revocation rules effective | APPLIES FROM (secondary) |
| 2 Dec 2026 | AI Act Art 50(2) marking deadline for systems placed on the market before 2 Aug 2026; new Art 5 prohibitions | APPLIES FROM |
| 1 Jan 2027 | CCPA ADMT compliance; Oklahoma and Louisiana privacy laws | APPLIES FROM |
| 12 Jan 2027 | EU Data Act: no switching or egress charges | APPLIES FROM |
| 1 May 2027 | Alabama privacy law | APPLIES FROM |
| 2 Dec 2027 | AI Act Annex III high-risk obligations | APPLIES FROM |
| 1 Apr 2028 | CPPA risk-assessment attestations; first cybersecurity audit certifications | APPLIES FROM |
| 1 Jan 2028 | Vermont privacy law | APPLIES FROM |

### 2.6 WhatsApp Business Platform (POLICY and competition law)

| Point | Detail | Status | Source |
| --- | --- | --- | --- |
| Pricing model | Per delivered message since **1 Jul 2025** (conversation pricing deprecated); rates by category (marketing, utility, authentication) and recipient country; user-initiated messages free. | POLICY, current | https://developers.facebook.com/docs/whatsapp/pricing/ |
| Service messages | Free since 1 Nov 2024; **from 1 Oct 2026 each business number gets 1,000 free service messages per month, then charged**; utility templates inside the open customer-service window charged again from 1 Oct 2026. Eligible governments and non-profits keep free service messages to 31 Dec 2027. | POLICY | Same |
| Free entry point | Click-to-WhatsApp ad conversations open a 7-day free window (except Meta Business Agent messages). | POLICY | Same |
| Meta Business Agent | Meta's own AI agent, billed at USD 2.00 per million tokens (about 4–5 cents a message), added 1 Jul 2026. | POLICY | Same |
| AI Providers clause (Meta Terms for WhatsApp Business Platform s.4.7; page last modified 23 Sep 2026) | AI/LLM providers may not use the platform where AI is the **primary** (not incidental or ancillary) functionality; country exceptions listed separately; if a business uses an AI provider as its Solution Provider, WhatsApp data (even aggregated or derived) must not be used to build or improve AI models, except fine-tuning a model for that business's exclusive use. Separate AI Providers policy effective 16 Feb 2026, updated 12 May 2026. | POLICY | https://www.facebook.com/legal/Meta-Terms-for-WhatsApp-Business-Platform ; https://developers.facebook.com/docs/whatsapp/pricing/ |
| EU competition case AT.41034 | Commission opened proceedings in Dec 2025 after Meta announced (Oct 2025) exclusion of rival general-purpose AI assistants from 15 Jan 2026; Statement of Objections Feb 2026; supplementary SO Apr 2026; **interim measures 9 Jun 2026** requiring Meta to restore access on the terms that applied before 15 Oct 2025 (free) until the case ends. | Decision in force; case open (secondary detail) | https://competition-cases.ec.europa.eu/cases/AT.41034 ; https://digitalpolicyalert.org/event/40520-european-commission-adopted-interim-measures-requiring-meta-to-restore-third-party-ai-assistants-access-to-whatsapp-under-equivalent-conditions |

CRM implication: an AI service bot that answers a customer's own customers about that customer's
business is "incidental" and allowed; Thapsus must contractually and technically ensure WhatsApp
content is never used to train or improve shared models; show per-message cost estimates before
campaigns.

### 2.7 Requirements derived from regulation (checklist for the PRD)

| Requirement | Driven by | Tier |
| --- | --- | --- |
| Lawful-basis and consent ledger per person, purpose and channel, immutable, with evidence and wording versions | UK/EU GDPR, PECR, TCPA, Florida FTSA | Core |
| Effective-dated jurisdiction rule packs (UK PECR incl. charity soft opt-in cut-off of 5 Feb 2026; EU national ePrivacy; CAN-SPAM; TCPA and state mini-TCPAs; CASL optional) with source URLs and verified-on dates | All marketing law | Core |
| Subscriber type (individual, sole trader, corporate) and recipient country on every contact point | PECR reg 22, ePrivacy Art 13 | Core |
| Universal suppression and instant STOP/unsubscribe handling (email under 48 hours, TCPA within 10 business days at most, Florida 15-day) | Gmail/Yahoo, CAN-SPAM, TCPA, FTSA | Core |
| Art 14 notice workflows for imported/enriched data; field-level provenance; non-resetting retention | GDPR, CNIL KASPR | Core |
| DSAR, objection and complaint workflows with 30-day acknowledgement for UK complaints | UK GDPR, DPA 2018 s.164A | Core |
| Authenticated sending (SPF, DKIM 2048, DMARC alignment), RFC 8058, complaint-rate guardrails | Gmail, Yahoo, Outlook.com | Core |
| Recording-consent engine (all-party states, UK/EU notices) | US state law, UK IPA regs, GDPR | Advanced |
| AI disclosure in chat, voice and autonomous email replies; machine-readable marking of AI-generated content | AI Act Art 50 | Advanced (with AI features) |
| No voice/face emotion inference on employees; no AI performance scoring of reps | AI Act Art 5(1)(f), Annex III | Core guardrail |
| Data Act exit pack: export of all data and configuration, switching terms, zero switching fees from 12 Jan 2027, infrastructure jurisdiction page | EU Data Act | Core (EU) |
| Accessible customer-facing components (forms, portal, chat, emails) to WCAG 2.2 AA with an accessibility conformance report | EAA, WCAG | Core |

---

## 3. AI features in CRM in 2026

### 3.1 What the leaders ship

| Vendor | Assistant and agents (2026) | Pricing model | MCP / external agents | Sources |
| --- | --- | --- | --- | --- |
| Salesforce | Agentforce (formerly Einstein Copilot); Sales: SDR agent (answers prospects, handles objections, books meetings), Sales Coach (role-play), Buyer Agent; Service Agent; marketing Campaign Optimizer; commerce agents; Agentforce Voice; Agent Builder and Agent Script; Observability; multi-agent orchestration; Atlas reasoning engine; Data 360 grounding | Flex Credits (500k, 1M, 2.75M per org per year in Core, Advanced, Max), "Resolutions", or per user; Agentforce for Sales add-on from USD 125 per user per month | "Agentforce MCP support" for verified MCP servers; hosted MCP servers for Salesforce data GA (secondary) | https://www.salesforce.com/agentforce/ ; https://www.salesforce.com/sales/pricing/ |
| HubSpot | Breeze Assistant; **Agent Hub**: Prospecting Agent (watches buying signals, drafts outreach), Customer Agent (multichannel service), Data Agent (answers from CRM, calls, emails, documents), Content, Campaign and Nurture agents, Smart Deal Progression, Revenue Agent (invoice follow-ups), Agent Builder (custom agents) | HubSpot Credits (500, 3,000, 5,000 included per month by tier; USD 0.01 per credit; no rollover) | Connector for Claude; remote MCP server GA in April 2026 (secondary) | https://www.hubspot.com/products/artificial-intelligence ; https://www.hubspot.com/pricing/sales |
| Microsoft | Copilot in Dynamics 365 Sales (email and meeting help, opportunity summaries, natural-language insights) in Enterprise; Sales Qualification Agent (GA Oct 2025, researches and engages leads, hands off only those with purchase intent); Sales Research Agent; Sales Opportunity Agent; Sales Close Agent (listed in Enterprise; retirement claimed by one partner, UNVERIFIED); Sales agent in Microsoft 365 Copilot; Customer Service agents (Customer Intent, Customer Knowledge Management, Case Management, Quality Evaluation) | Copilot Credits (pay-as-you-go or pre-purchase; Sales Premium includes 1,000 per user per month) | Copilot Studio; MCP via Copilot Studio (not verified this session) | https://www.microsoft.com/en-us/dynamics-365/products/sales/pricing ; https://www.microsoft.com/en-us/dynamics-365/products/customer-service/pricing ; https://learn.microsoft.com/en-us/dynamics365/release-plan/2025wave2/sales/dynamics365-sales/planned-features |
| Zoho | Zia predictions, anomaly detection, best time to contact; Zia Agents with no-code Agent Studio and Agent Marketplace (announced Feb 2025); "AI agents" listed from Professional | Agent building free, model usage billed (secondary) | Zoho MCP server (secondary) | https://www.businesswire.com/news/home/20250204066995/en/Zoho-Corporation-Announces-Zia-Agents-AI-Platform-Supporting-Autonomous-Agents-Across-Organizations-Broad-Portfolio ; https://www.zoho.com/crm/zohocrm-pricing.html |
| Pipedrive | AI Sales Assistant (patterns, lost-deal reasons, next actions, win-probability notifications), AI email writer and thread summariser; "Pulse" agentic features rolling out (secondary) | Included in plans; rollout uneven (secondary) | Native MCP server on all plans with token limits (secondary) | https://www.pipedrive.com/en/features/ai-sales-assistant |
| Freshworks | Freddy AI Agent (service bot), Freddy AI Copilot, Freddy AI Insights | AI Agent USD 49 per 100 sessions; 500 free trial sessions | Not verified | https://www.freshworks.com/crm/pricing/ |
| Attio | Ask Attio (natural-language questions over records, calls, emails), AI research agent and AI attributes, call intelligence (Pro) | Seat credits plus workspace credits, add-on packs | Vendor-hosted MCP server with OAuth (secondary) | https://attio.com/pricing |
| Close | "Chloe" AI features, lead summaries, suggested actions, Call Assistant (records, transcribes, summarises) | AI credits per user (500 to 2,000); Call Assistant USD 50 per org per month plus USD 0.02 per minute | Not verified | https://www.close.com/pricing |
| monday CRM | AI credits, AI agents, meeting notetaker, AI data fields ("Standard AI"); AI workflow builder ("Pro AI") | AI variants of each plan | Not verified | https://monday.com/crm/pricing |
| SugarAI | AI summaries, assistant, meeting preparation, smart prompts (Advanced+); Sugar Predict for churn, cross-sell and reorder signals using ERP data; Zoom/Teams transcript ingestion | Edition-based | Not verified | https://www.sugarai.com/pricing |
| Odoo | "Agentic AI" listed in the Custom plan | Plan-based | Not verified | https://www.odoo.com/pricing |
| Creatio | Creatio AI and AI Studio in the no-code platform; Unlimited model from 1 May 2026 (secondary) | Per user plus AI credits (secondary) | Not verified | https://www.eesel.ai/blog/creatio-pricing |

### 3.2 Concrete AI feature list for Thapsus CRM, with controls and cautions

All AI features are **off by default** at tenant level, then enabled per feature and per role, with a
per-tenant credit budget. Every invocation is written to the audit log (feature, user, model, model
region, prompt template version, retrieved record IDs, output hash, human decision). No customer data
is used to train shared models. Models run on regional endpoints in the tenant's home region; a
feature is unavailable where no in-region model exists.

| # | AI feature | Who ships it | Optional, human-approved, auditable design | Regulatory caution |
| --- | --- | --- | --- | --- |
| 1 | Email and reply drafting (sales and service) | SF, HS, MS, ZO, PD, FW, CL, CO, SG | Draft only; user must press send; "AI-assisted" flag stored on the activity; template and tone controls | AI Act Art 50(2) machine-readable marking for generated text (assistive-editing exemption may apply when a human substantially writes); disclose in autonomous replies |
| 2 | Call and meeting summaries, action items, follow-up tasks | SF, HS (Notetaker), MS, CL, AT, MO, SG | Generated only for recordings with captured consent; summary editable; tasks created as suggestions | Recording consent (all-party states, UK/EU notices); minimise special category data in transcripts |
| 3 | Conversation intelligence (topics, competitor mentions, talk ratio, objections) | SF, HS, MS, CL, AT | Aggregate coaching insights; rep opt-in visibility; no automatic scoring of individuals | Using it to evaluate employee performance can be Annex III employment high-risk (from 2 Dec 2027) and an employee-monitoring DPIA trigger |
| 4 | Sentiment on text (emails, tickets) | ZO, FW, MS, SF | Show as a hint, never as a status that drives automatic outcomes | Do **not** ship voice/face emotion recognition; never apply to employees (Art 5(1)(f)); if any emotion recognition on customers, Art 50(3) notice and Annex III high-risk |
| 5 | Deal-risk signals and pipeline inspection | SF, HS, MS, SG, CL | Explainable signals (no activity for N days, single-threaded, close date slipped); rule-based first, ML optional | Low risk (business data) |
| 6 | Next-best-action recommendations | SF, MS, ZO, PD, CL, HS | Suggestions with reasons; accept/dismiss logged | Avoid consumer eligibility decisions (UK Art 22A–22C, GDPR Art 22, CCPA ADMT from 1 Jan 2027) |
| 7 | Forecast assistance (AI forecast alongside rep commit) | SF, MS, HS, ZO | Separate "AI forecast" column; humans own the submitted number | Low risk |
| 8 | Data capture from emails, calls and signatures (create/update contacts and fields) | SF, HS, MS, ZO, PD, AT | Proposed changes queue; bulk approve; field-level provenance "AI from email X" | Accuracy principle; Art 14 notice for newly created people |
| 9 | Company research and enrichment agent | HS (Data Agent), SF, AT, SG, MS | Uses licensed sources and public company data; cites sources; writes to "suggested" fields | Third-party personal data enrichment: sub-processor listing, Art 14 notices, CNIL KASPR precedent |
| 10 | Natural-language reports and questions over CRM data | SF, HS, MS, ZO, AT, SG | Generates a report definition the user can inspect; respects the user's record permissions (query runs as the user under RLS) | Permission leakage is the main risk; never bypass sharing rules |
| 11 | Service chatbot answering from the knowledge base with citations | SF (Service Agent), HS (Customer Agent), MS, FW (Freddy AI Agent), ZO | Only published KB articles; citations shown; confidence threshold then hand-off to a human; per-channel enablement | Art 50(1) "you are talking to AI" disclosure; EAA accessibility of widget; WhatsApp AI Providers terms |
| 12 | Case summarisation, classification, routing and reply suggestions | SF, HS, MS, FW, ZO | Suggestions to agents; routing changes logged; agents can override | Low risk; avoid special category inference |
| 13 | Knowledge-article drafting from resolved cases | SF, HS, MS, FW | Draft to KB approval workflow | Remove personal data from drafts |
| 14 | Agentic SDR / prospecting (find leads, write and send sequences, reply to prospects) | SF (SDR agent), HS (Prospecting Agent), MS (Sales Qualification Agent), ZO | Default "human approves every first message"; per-tenant daily caps; automatic stop on reply sentiment of "not interested"; full transcript in audit | PECR/ePrivacy consent for electronic marketing; CAN-SPAM; Art 50(1) disclosure when the agent converses; deliverability limits |
| 15 | AI voice agents (inbound answering, outbound calls) | SF (Agentforce Voice), MS (Contact Center) | Inbound only at first; announces itself as AI; warm transfer to humans | TCPA treats AI voices as artificial voice (consent needed); recording consent; Art 50(1) |
| 16 | Predictive lead and account scoring | SF, HS (Enterprise predictive lead scoring), MS, ZO, FW, OD | Shown beside rule-based score with top factors; tenant can disable | Profiling transparency; ADMT if used for consumer significant decisions |
| 17 | Duplicate detection and merge suggestions | SF, HS, ZO | Suggestions only; merges reversible | Low risk |
| 18 | Marketing content generation (emails, landing pages, social posts, translations, subject-line tests) | HS (Content, Campaign agents), SF, ZO, MO | Draft to approval; brand-voice library; generated images labelled | Art 50(2) marking; Art 50(4) for public-interest text; copyright review |
| 19 | Custom agent builder (no-code) | SF (Agent Builder), HS (Agent Builder), ZO (Agent Studio), MS (Copilot Studio), CR | Agents get explicit tool scopes; any write or send requires approval unless the tenant admin grants autonomy per action; dry-run mode | Tenant becomes deployer; give a DPIA template |
| 20 | MCP server so customers' own AI agents can use the CRM | SF, HS, AT, PD, ZO (secondary) | Off by default; OAuth with per-tool scopes (read, write, delete separate, as Salesforce does); per-call audit; rate limits; region-pinned endpoint | Data leaves to the customer's chosen AI provider: customer is controller, but document in DPA; no special category fields exposed by default |
| 21 | Sales coaching role-play | SF (Sales Coach) | Practice only; no scores stored against employees by default | Employee monitoring caution |

### 3.3 AI governance pattern (reusable across the Thapsus suite)

- One **AI gateway** shared with Thapsus Care's (Care's constraints already require one): per-tenant
  enablement, per-feature flags, model routing to in-region endpoints, prompt-template versioning,
  redaction of special category and sensitive fields, cost metering into credits, kill switch.
- **Human approval queue** for any AI action that sends a message, changes a record owned by someone
  else, or changes money, price or eligibility.
- **Transparency artefacts** per feature: purpose, data used, model and provider, region, known
  limitations; downloadable for customers' DPIAs and Art 50 notices.
- **Evaluation harness** with fictional data for each prompt template before release.

---

## 4. Integration points

### 4.1 Thapsus suite: shared contract

| Concern | Recommendation |
| --- | --- |
| Customer master | One "party" model (organisation, person, household) with suite-wide IDs; CRM owns prospects and relationships; MULLAH owns billing accounts; KASONGO owns ship-to/sold-to operational data; mapping table rather than duplication. |
| Events | Outbox events per product (`crm.deal.won`, `mullah.invoice.paid`, `kasongo.order.shipped`, `hr.employee.left`), idempotent consumers, same tenant and region cell. |
| Identity | Users and teams from Thapsus-HR or the customer's IdP via SCIM; one sign-in across products. |
| Residency | Integrations only within the tenant's home region cell; no cross-region calls. |
| Audit | Cross-product actions carry a correlation ID into each product's append-only audit log. |

### 4.2 Accounting (MULLAH)

| Direction | Data | Purpose |
| --- | --- | --- |
| CRM to MULLAH | Account to customer creation (legal name, registration number, VAT/tax IDs, billing address, currency, payment terms, billing contacts) | Quote-to-cash without re-keying |
| CRM to MULLAH | Accepted quote or won deal to sales order or invoice draft; subscription start, amendments, renewals | Invoicing stays in MULLAH (revenue recognition, tax, e-invoicing) |
| MULLAH to CRM | Invoices, credit notes, payment status, overdue balance, ageing, DSO | Account page finance tab; renewal and collections context |
| MULLAH to CRM | Credit limit, credit hold flag, risk rating | Block quote acceptance or order creation above limit; approval path |
| MULLAH to CRM | Exchange rates, tax codes, payment terms master data | Consistent quoting |
| Both | Stripe customer and payment-link references | Card payments for self-serve quotes |

### 4.3 ERP (KASONGO)

| Direction | Data | Purpose |
| --- | --- | --- |
| KASONGO to CRM | Product catalogue, bundles, units of measure, price lists, discounts, customer-specific prices | Quote lines and CPQ rules |
| KASONGO to CRM | Stock availability and available-to-promise by warehouse and date | Promise dates on quotes |
| CRM to KASONGO | Sales orders from accepted quotes | Fulfilment |
| KASONGO to CRM | Order status, shipments, tracking numbers, delivery events, back-orders | Account timeline and service context |
| Both | Returns (RMA) and warranty claims linked to cases | Service module |
| KASONGO to CRM | Installed base / assets and service contracts | Renewals, entitlements, field service later |

### 4.4 HR (Thapsus-HR)

| Direction | Data | Purpose and caution |
| --- | --- | --- |
| HR to CRM | User provisioning (SCIM): joiners, movers, leavers | Leavers: revoke access immediately, reassign open records and sequences, stop scheduled sends from their mailbox |
| HR to CRM | Teams, managers, org chart, cost centres, locations | Forecast roll-ups, approval chains, territory alignment |
| HR to CRM | Absence and working patterns (availability only, not reason) | Round-robin skips absent reps; SLA business hours; never pass sickness or other sensitive reasons |
| CRM to HR | Commission statements and approved amounts | HR never runs payroll: export to the customer's payroll provider; keep salary data out of the CRM |
| HR to CRM | Target/quota inputs where set in HR | Quotas |
| Both | Employee as contact (internal stakeholders) | Avoid mixing employee personal data into marketing segments |

### 4.5 Care sector (Thapsus Care)

Thapsus Care already plans an "enquiries and admissions" pipeline (its Phase 3). Recommended split:

| Item | Recommendation |
| --- | --- |
| Source of truth for enquiries about care places | Thapsus Care. Enquiries contain health and care needs (special category data), funding status and family details; they must stay in Care's UK-only (europe-west2) store with its clinical-safety and data-protection controls. |
| What the CRM may hold | Referral-source relationships (hospital discharge teams, local authority commissioners, social workers, GP practices, NHS ICBs as B2B organisations and professional contacts), marketing consent of families who opt in to newsletters or open days, campaign attribution for enquiries (counts and source, not needs). |
| Pipeline engine | Reuse a shared pipeline/stage engine library rather than syncing enquiry records, so Care gets CRM-grade pipeline features without moving data. |
| Family contacts | Third parties to the resident; often vulnerable or acting under lasting power of attorney; contact purposes must be separated (care communication versus marketing); no marketing without consent; respect the soft opt-in rules (Care providers are rarely charities, but some are). |
| Cautions | No health data in CRM free-text or AI prompts; separate DPIA for any sync; Care's CLAUDE.md rules (UK residency, no personal data in logs, records of processing) apply to anything shared. |

### 4.6 Standard external integrations

| Integration | Expected capability | Constraints found |
| --- | --- | --- |
| Gmail and Google Calendar | Two-way email and calendar sync, send-as, threading, tracking, Gmail add-on | Gmail scopes such as `gmail.readonly`, `gmail.modify`, `gmail.compose` and full `mail.google.com` are **restricted**: verification plus an **annual CASA security assessment** (AL1 or AL2) (https://support.google.com/cloud/answer/13464325 ; https://support.google.com/cloud/answer/13465431). Limited Use rules on data use apply (UNVERIFIED this session). |
| Microsoft 365 / Outlook / Teams | Email and calendar sync via Microsoft Graph, Outlook add-in, Teams meeting creation and transcripts, Teams notifications | **Exchange Online blocks EWS requests from 1 Oct 2026**; Graph is the target, with gaps (archive mailboxes, public folders) (https://techcommunity.microsoft.com/blog/exchange/retirement-of-exchange-web-services-in-exchange-online/3924440). |
| Zoom | Meeting links, recordings and transcripts into the timeline | Zoom Marketplace app review; recording consent. |
| Slack | Deal and case notifications, approvals in Slack | Salesforce bundles Slack in its editions; Slack is a Salesforce company. |
| LinkedIn Sales Navigator | Embedded profiles, InMail, CRM sync | LinkedIn: "We are not currently accepting new partners for access to the LinkedIn Sales Navigator API" (https://learn.microsoft.com/en-us/linkedin/sales/). Do not scrape (CNIL KASPR). Offer manual "LinkedIn task" steps in sequences and profile URL fields only. Microsoft Relationship Sales bundles Sales Navigator with Dynamics (10-seat minimum). |
| Telephony: Twilio, Aircall, RingCentral, Dialpad, Vonage | Click-to-call, call logging, recordings, SMS, WhatsApp (Twilio as BSP) | Each provider and region in `SUBPROCESSORS.md`; recording storage in-region. |
| E-signature: DocuSign, Adobe Acrobat Sign, Dropbox Sign | Send quotes and contracts for signature; status webhooks; signed PDF back to record | EU qualified electronic signatures (eIDAS) only via qualified trust service providers when customers need them. |
| Stripe | Payment links on quotes, subscription status | Coordinate with MULLAH as billing owner. |
| Mailchimp (and Brevo, Klaviyo) | Migration of audiences, tags, unsubscribes, bounces; optional ongoing sync | Preserve suppression; never re-subscribe. |
| Zapier, Make, n8n | Triggers and actions | Region of the automation platform is outside Thapsus control; warn admins. |
| Ad platforms (Google, Meta, LinkedIn) | Audience sync and offline conversions | Consent for ads purposes; CCPA "sharing" opt-out and GPC suppression. |
| Accounting for non-MULLAH tenants (Xero, QuickBooks, Sage) | Customer and invoice sync | Same contract as section 4.2 via adapters. |
| Data providers (Companies House, Dun and Bradstreet, ZoomInfo, Cognism) | Firmographics and contact enrichment | Sub-processor or independent controller analysis per provider; Art 14 notices. |

---

## 5. No-code customisation (feature level)

| Capability | SF | HS | MS | ZO | CR | AT | PD | Thapsus target |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Custom objects | Yes | Yes (Enterprise) | Yes (Dataverse tables) | Yes (custom modules) | Yes | Yes (by plan) | No (custom fields) | Core: limited count; Advanced: more |
| Custom fields incl. formula and roll-up | Yes | Yes (calculation properties) | Yes (calculated, rollup columns) | Yes (formula) | Yes | Yes | Yes (formula on higher tiers) | Core |
| Pipelines and stage definitions per team | Yes | Yes | Yes (business process flows) | Yes (layouts and Blueprint) | Yes | Yes (lists/stages) | Yes | Core |
| Record types (different picklists, layouts, processes per type) | Yes | Partial (pipelines, record customisation) | Yes (forms per type) | Yes (layouts) | Yes | Partial | No | Advanced |
| Page layouts and dynamic forms per role/profile | Yes (Lightning App Builder, Dynamic Forms) | Yes (record page customisation by team) | Yes (model-driven forms, role-based forms) | Yes (layouts, Canvas designer) | Yes (Freedom UI designer) | Yes (record views) | Partial | Advanced |
| Validation rules | Yes (formula) | Yes (property rules) | Yes (business rules) | Yes | Yes | Partial | Partial | Core (simple) / Advanced (expressions) |
| Workflows/automation (triggers, conditions, actions, delays, branching) | Yes (Flow) | Yes (workflows: 50/300/1,000 by tier) | Yes (Power Automate) | Yes (workflow rules, Blueprint) | Yes (BPM engine) | Yes (workflows, credit-metered) | Yes (automations) | Core (simple) / Advanced (branching, scheduled, cross-object) |
| Approval processes (multi-step, delegation, escalation) | Yes | Yes (deal, quote approvals) | Yes (Power Automate approvals) | Yes (Enterprise) | Yes | No | No | Advanced |
| Assignment rules and queues | Yes | Yes | Yes | Yes | Yes | Partial | Partial | Core |
| Email templates with merge fields and conditional blocks | Yes | Yes | Yes | Yes | Yes | Yes | Yes | Core |
| Custom buttons and actions | Yes | Yes | Yes | Yes (Enterprise) | Yes | Partial | Partial | Advanced |
| Sandboxes and change promotion | Yes | Yes (Enterprise) | Yes (environments) | Yes (Enterprise) | Yes | No | No | Enterprise |
| Field-level security and permission sets | Yes | Yes (Enterprise) | Yes (column security) | Yes | Yes | Partial | Partial | Advanced |
| Translations of custom labels | Yes | Partial | Yes | Yes | Yes | Partial | Partial | Core (en-GB, en-US) |
| Low-code scripting escape hatch | Apex, LWC | Custom code actions, UI extensions | Plug-ins, PCF | Deluge | Low-code | Workflow code blocks | No | Not at launch: sandboxed functions later |

Thapsus implications (beyond feature level): metadata in tenant-scoped tables under RLS; versioned
configuration with change history and rollback; configuration packages that move between a sandbox
tenant and production; custom field values stored typed (JSONB plus generated indexed columns or an
EAV side table) so reporting stays fast; the same rules engine for validation, assignment, approvals
and marketing jurisdiction checks.

---

## 6. Market and pricing (list prices, USD per user per month unless stated)

### 6.1 CRM and sales

| Vendor | Tiers and prices | Notes | Source |
| --- | --- | --- | --- |
| Salesforce Sales Cloud | Free Suite $0; Starter Suite $25 (monthly or annual); Pro Suite $100 (annual); **Core $195; Advanced $395; Max $550** (annual) | Core/Advanced/Max replaced Enterprise ($175), Unlimited ($350) and Agentforce 1 ($550) on 3 Sep 2026; Core bundles Slack Business+, Tableau Next, Premier Success and 500k Flex Credits; Advanced adds security, Backup, Archive and 1M Flex Credits; Max adds Agentforce for Sales, Spiff, Sales Planning, Maps and 2.75M Flex Credits. Legacy customers keep prices (secondary). | https://www.salesforce.com/sales/pricing/ ; https://salesforcedictionary.com/news/salesforce-news-september-3-2026-editions-repricing-core-advanced-max |
| HubSpot Sales Hub | Free (2 users); Starter $20 per seat monthly (pricing page shows $7 annual, described by reviewers as a promotion); Professional $90 annual / $100 monthly, onboarding $1,500; Enterprise $150, onboarding $3,500 | Credits included: 500, 3,000, 5,000 a month; credits $0.01 each. Revenue Hub promotion: Professional $57, Enterprise $98 (annual, new customers). | https://www.hubspot.com/pricing/sales |
| Microsoft Dynamics 365 Sales | Professional $65; Enterprise $105; Premium $150 (includes 1,000 Copilot Credits per user per month); Relationship Sales (with LinkedIn Sales Navigator) by quote, 10-seat minimum | Annual billing | https://www.microsoft.com/en-us/dynamics-365/products/sales/pricing |
| Zoho CRM | Free (3 users); Standard $14 annual / $20 monthly; Professional $23 / $35; Enterprise $40 / $50; Ultimate $52 / $65; Zoho One from $30 | Official page shows features but not prices in the fetched text; prices from secondary sources | https://www.zoho.com/crm/zohocrm-pricing.html ; https://zeeg.me/post/zoho-crm-pricing |
| Pipedrive | Lite $14 / $24; Growth $39 / $49; Premium $59 / $79 (some sources $49); Ultimate $79 / $99; no free plan | Add-ons (LeadBooster, Campaigns, Web Visitors, Smart Docs) not verified | https://zeeg.me/post/pipedrive-pricing ; https://dupple.com/pricing/pipedrive |
| Freshsales | Growth $9; Pro $39; Enterprise $59 (annual; monthly about 20% more) | Freddy AI Agent $49 per 100 sessions; branded documents $19 | https://www.freshworks.com/crm/pricing/ |
| Attio | Free (3 seats); Plus $35 annual / $44 monthly; Pro $79 / $99; Enterprise custom | Credits per seat and per workspace; SSO and SCIM on Enterprise | https://attio.com/pricing |
| monday CRM | Basic $12 / $18; Standard $17 / $25; Pro $28 / $41; Ultimate custom; minimum 3 seats; "AI" variants of each | Quotes and invoices per month capped by tier | https://monday.com/crm/pricing |
| Close | Solo $9 / $19 (1 user); Essentials $35 / $49; Growth $99 / $109; Scale $139 / $149 | Calls and SMS at cost (about $0.02 a minute); Call Assistant $50 per org plus $0.02 a minute | https://www.close.com/pricing |
| Copper | Basic $23 / $29; Professional $59 / $69; Business $99 / $134 | Google Workspace-centred | https://www.copper.com/pricing |
| SugarAI (SugarCRM) Sell | Standard+ $68; Advanced+ $98; Premier+ $155; Precision+ editions by quote; Sugar Connect+ add-on $20 | Annual; rebrand to SugarAI | https://www.sugarai.com/pricing |
| Odoo | One App Free; Standard about $24.90–31.10; Custom about $49.00–61.10 (Studio, external API, multi-company, agentic AI only on Custom) | Page labels ambiguous; first-year discount on initial users | https://www.odoo.com/pricing |
| Creatio | Growth about $40 (raised from $25 mid-2026); Enterprise $55–75; Unlimited custom; Sales, Marketing and Service products about $15 each on Growth/Enterprise; possible $10,000 minimum for new customers | All secondary; new model from 1 May 2026 | https://www.eesel.ai/blog/creatio-pricing ; https://pricingsaas.com/companies/creatio/diffs/2026Q2 |

### 6.2 Service and marketing

| Vendor | Tiers and prices | Source |
| --- | --- | --- |
| Salesforce Service Cloud | Starter Suite $25; Pro Suite $100; Core $195; Advanced $395; Max $550; Enhanced Messaging +$75; contact centre, digital channels, feedback management priced on request | https://www.salesforce.com/service/pricing/ |
| HubSpot Service Hub | Starter $20 per seat (page also shows $7); Professional $90 / $100, onboarding $1,500; Enterprise $150, onboarding $3,500; Customer Agent consumes credits | https://www.hubspot.com/pricing/service |
| HubSpot Marketing Hub | Starter from $20 per seat (also $7 shown), 1,000 marketing contacts; Professional $800 a month (also $890 shown) with 3 seats and 2,000 contacts, onboarding $3,000; Enterprise $3,600 a month with 5 seats and 10,000 contacts, onboarding $7,000; extra seats from $45 / $75 | https://www.hubspot.com/pricing/marketing |
| Dynamics 365 Customer Service | Professional $50; Enterprise $105; Premium $195; Dynamics 365 Contact Center $110; agents on Copilot Credits | https://www.microsoft.com/en-us/dynamics-365/products/customer-service/pricing |

### 6.3 Common add-ons and pricing patterns

| Pattern | Examples |
| --- | --- |
| AI as consumption credits | Salesforce Flex Credits; HubSpot Credits ($0.01); Microsoft Copilot Credits; Attio seat and workspace credits; Close AI credits; monday AI credits; Freshworks AI Agent sessions ($49 per 100) |
| AI as per-user add-on | Agentforce for Sales from $125; Revenue Intelligence $220 (Salesforce) |
| Revenue and CPQ | Salesforce Revenue Cloud from $200; Zoho CPQ inside Professional |
| Enablement and coaching | Salesforce Sales Programs from $100 |
| Partner portal | Salesforce PRM from $10 per login |
| Onboarding fees | HubSpot $1,500 to $7,000 one-off |
| Contact-based marketing pricing | HubSpot marketing contacts tiers |
| Telephony pass-through | Close (about $0.02 a minute), HubSpot calling minutes by tier |
| Minimum seats or contract value | monday (3 seats), Microsoft Relationship Sales (10 seats), Creatio minimum (secondary) |
| Platform bundles | Salesforce Core bundles Slack and Tableau; HubSpot Customer Platform bundles; Zoho One from $30 |

**Price bands (per user per month, annual):** entry $9–25 (Freshsales, Zoho, Pipedrive, monday,
Close, Salesforce Starter); mid-market $35–105 (HubSpot Professional, Zoho Enterprise, Dynamics
Professional/Enterprise, Pipedrive Ultimate, Attio Pro, Close Growth/Scale, SugarAI Advanced+);
enterprise $150–550 (Dynamics Premium, HubSpot Enterprise, SugarAI Premier+, Salesforce
Core/Advanced/Max). Thapsus should anchor in the mid-market band with residency, suite integration and
the compliance engine as differentiators, and meter AI as credits like the market does.

---

## 7. Suggested scope phasing (for the PRD)

| Phase | Scope |
| --- | --- |
| Launch (Core) | Data model with custom fields and limited custom objects; leads with forms, assignment and round robin, rule-based scoring; deals and multiple pipelines; activities; Gmail/Graph email and calendar sync; templates; meeting scheduler; products, price books, quotes with approvals; standard reports and dashboards; dedupe and merge; consent ledger, preference centre and jurisdiction rule packs; import and Data Act export; REST API, webhooks, Zapier; PWA; audit; SSO; suite integrations with HR (SCIM, teams) and MULLAH (customer, invoices, credit hold). |
| Advanced | Sequences; telephony with CTI and recording consent; SMS and WhatsApp; marketing email builder, landing pages and journeys; service module with SLAs, KB, portal and chat; forecasting and quotas; customer success health scores; migration importers; approvals; field sales with offline; AI features 1, 2, 5–8, 10–12, 17 (opt-in); MCP server. |
| Enterprise | CPQ; territories; commissions; PRM portal; sandboxes; field-level encryption with customer-managed keys; skills-based routing; account planning; warehouse export; agent builder and agentic SDR with approval gates. |

---

## Sources

Regulation (UK)
- https://www.legislation.gov.uk/uksi/2026/82/made
- https://www.legislation.gov.uk/ukpga/2025/18/section/70
- https://www.legislation.gov.uk/ukpga/2025/18/section/80
- https://www.legislation.gov.uk/ukpga/2025/18/schedule/12
- https://ico.org.uk/about-the-ico/what-we-do/legislation-we-cover/data-use-and-access-act-2025/the-data-use-and-access-act-2025-duaa-summary-of-the-changes/data-protection/
- https://ico.org.uk/about-the-ico/what-we-do/legislation-we-cover/data-use-and-access-act-2025/the-data-use-and-access-act-2025-duaa-summary-of-the-changes/privacy-and-electronic-communications/
- https://ico.org.uk/about-the-ico/media-centre/news-and-blogs/2026/04/charities-given-new-flexibility-to-contact-supporters-under-data-law-change/
- https://ico.org.uk/for-organisations/direct-marketing-and-privacy-and-electronic-communications/sending-direct-marketing-choosing-your-lawful-basis/
- https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/lawful-basis/legitimate-interests/when-can-we-rely-on-legitimate-interests/
- https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/individual-rights/individual-rights/right-to-object/
- https://ico.org.uk/for-organisations/direct-marketing-and-privacy-and-electronic-communications/guide-to-pecr/electronic-and-telephone-marketing/electronic-mail-marketing/
- https://bratby.law/data-use-and-access-act-2025-commencement/
- https://www.handleygill.co.uk/handley-gill-blog/data-use-and-access-act-2025-commencement-no-6-regulations-si-2026-82
- https://www.policypros.co.uk/cookie-consent-changes-duaa-guide/

Regulation (EU)
- https://eur-lex.europa.eu/eli/reg/2016/679/oj
- https://eur-lex.europa.eu/eli/dir/2002/58/oj
- https://www.hunton.com/privacy-and-cybersecurity-law-blog/european-commission-withdraws-eprivacy-regulation-and-ai-liability-directive-proposals
- https://www.lewissilkin.com/en/insights/2025/02/19/long-awaited-eprivacy-regulation-is-finally-dead-102k0fn
- https://www.legal500.com/intelligence/european-union/privacy/digital-omnibus-what-the-proposed-changes-mean-for-gdpr-privacy-and-cookies
- https://www.osborneclarke.com/what-our-clients-are-talking-about/digital-regulation/digital-omnibus-package
- https://artificialintelligenceact.eu/ai-act-explorer/digital-omnibus/
- https://artificialintelligenceact.eu/article/50/
- https://www.aiacto.eu/en/blog/digital-omnibus-ai-act-report-2027
- https://transition-pathways.europa.eu/adv-man/legislation/ai-act-digital-omnibus-high-risk-deadlines-postponed-machinery-carved-out
- https://datamatters.sidley.com/2026/06/22/eu-lawmakers-reach-provisional-agreement-to-delay-key-eu-ai-act-obligations/
- https://www.covingtonblogs.com/2025/06/04/european-commission-guidelines-on-prohibited-ai-practices-under-the-eu-artificial-intelligence-act/
- https://cms.law/en/gbr/legal-updates/eu-commission-issues-guidelines-on-prohibited-ai-practices-part-ii
- https://eur-lex.europa.eu/eli/reg/2023/2854/oj/eng
- https://digital-strategy.ec.europa.eu/en/policies/data-act-explained
- https://eur-lex.europa.eu/eli/dir/2019/882/oj/eng
- https://commission.europa.eu/strategy-and-policy/policies/justice-and-fundamental-rights/disability/union-equality-strategy-rights-persons-disabilities-2021-2030/european-accessibility-act_en
- https://www.cnil.fr/en/data-scraping-kaspr-fined-eu240000
- https://www.edpb.europa.eu/our-work-tools/documents/public-consultations/2024/guidelines-12024-processing-personal-data-based_en
- https://www.legislation.gov.uk/uksi/2003/2426/regulation/22
- https://www.legislation.gov.uk/uksi/2003/2426/regulation/21

Regulation (US)
- https://www.ftc.gov/business-guidance/resources/can-spam-act-compliance-guide-business
- https://media.ca11.uscourts.gov/opinions/pub/files/202410277.pdf
- https://docs.fcc.gov/public/attachments/FCC-24-17A1.pdf
- https://docs.fcc.gov/public/attachments/DA-26-12A1.pdf
- https://www.burr.com/telephone-consumer-protection-act/the-fcc-delays-effective-date-of-tcpa-revoke-all-rule-until-january-31-2027
- https://www.womblebonddickinson.com/us/node/21530
- https://www.troutman.com/insights/fcc-revises-tcpa-revocation-of-consent-rules-that-were-set-to-go-into-effect-in-january/
- https://www.leg.state.fl.us/statutes/index.cfm?App_mode=Display_Statute&URL=0500-0599/0501/Sections/0501.059.html
- https://www.bytebacklaw.com/2026/07/looking-back-on-the-last-year-of-state-level-tcpa-updates/
- https://natlawreview.com/article/texas-expands-its-mini-tcpa-text-messages-now-covered-litigation-risk-rises
- https://www.manatt.com/insights/newsletters/tcpa-connect/state-mini-tcpa-telemarketing-laws-continue-to-p
- https://cppa.ca.gov/regulations/ccpa_updates.html
- https://www.cppa.ca.gov/announcements/2025/20250923.html
- https://www.hunton.com/privacy-and-cybersecurity-law-blog/newly-approved-ccpa-regulations-have-staggered-deadlines-for-compliance
- https://assets.contentstack.io/v3/assets/bltd4dd5b2d705252bc/blt90070182553bdfe6/6a429c09099ffcd63cefaad7/State_Comp_Privacy_Law_Chart.pdf
- https://www.koleyjessen.com/insights/publications/new-state-privacy-laws-effective-january-1-2026-indiana-kentucky-and-rhode-island
- https://www.osano.com/us-data-privacy-laws
- https://viirtue.com/call-recording-consent-laws-by-state-2026-guide/

Platform policies
- https://support.google.com/a/answer/81126
- https://support.google.com/a/answer/14229414
- https://senders.yahooinc.com/best-practices/
- https://techcommunity.microsoft.com/blog/microsoftdefenderforoffice365blog/strengthening-email-ecosystem-outlook%E2%80%99s-new-requirements-for-high%E2%80%90volume-senders/4399730
- https://developers.facebook.com/docs/whatsapp/pricing/
- https://www.facebook.com/legal/Meta-Terms-for-WhatsApp-Business-Platform
- https://competition-cases.ec.europa.eu/cases/AT.41034
- https://digitalpolicyalert.org/event/40520-european-commission-adopted-interim-measures-requiring-meta-to-restore-third-party-ai-assistants-access-to-whatsapp-under-equivalent-conditions
- https://www.houthoff.com/insights/news/a-rare-show-offorce-commission-deploys-interim-measures-against-meta-to-safeguard-ai-competition/
- https://learn.microsoft.com/en-us/linkedin/sales/
- https://support.google.com/cloud/answer/13464325
- https://support.google.com/cloud/answer/13465431
- https://techcommunity.microsoft.com/blog/exchange/retirement-of-exchange-web-services-in-exchange-online/3924440

Vendors, AI and pricing
- https://www.salesforce.com/sales/pricing/
- https://www.salesforce.com/service/pricing/
- https://www.salesforce.com/agentforce/
- https://salesforcedictionary.com/news/salesforce-news-september-3-2026-editions-repricing-core-advanced-max
- https://www.subscriptioninsider.com/blog/salesforce-includes-ai-credits-in-new-195-and-395-editions
- https://www.hubspot.com/pricing/sales
- https://www.hubspot.com/pricing/service
- https://www.hubspot.com/pricing/marketing
- https://www.hubspot.com/products/artificial-intelligence
- https://resolve247.ai/blog/hubspot-agent-hub/
- https://www.microsoft.com/en-us/dynamics-365/products/sales/pricing
- https://www.microsoft.com/en-us/dynamics-365/products/customer-service/pricing
- https://learn.microsoft.com/en-us/dynamics365/release-plan/2025wave2/sales/dynamics365-sales/planned-features
- https://www.loganconsulting.com/blog/microsoft-dynamics-365-sales-ai-agents/
- https://www.zoho.com/crm/zohocrm-pricing.html
- https://zeeg.me/post/zoho-crm-pricing
- https://www.businesswire.com/news/home/20250204066995/en/Zoho-Corporation-Announces-Zia-Agents-AI-Platform-Supporting-Autonomous-Agents-Across-Organizations-Broad-Portfolio
- https://www.pipedrive.com/en/features/ai-sales-assistant
- https://zeeg.me/post/pipedrive-pricing
- https://dupple.com/pricing/pipedrive
- https://www.freshworks.com/crm/pricing/
- https://attio.com/pricing
- https://monday.com/crm/pricing
- https://www.close.com/pricing
- https://www.copper.com/pricing
- https://www.sugarai.com/pricing
- https://www.odoo.com/pricing
- https://www.eesel.ai/blog/creatio-pricing
- https://pricingsaas.com/companies/creatio/diffs/2026Q2
- https://www.mcpbundles.com/guides/mcp-servers-for-sales-and-crm
- https://chatforest.com/reviews/crm-mcp-servers/

---

## Things I could not verify

- **Search budget:** the shared web-search allowance ran out partway through. Later checks used
  direct fetches of official pages only; some items below could not be re-checked as a result.
- **ICO pages** returned HTTP 403 to direct fetches; ICO content is cited from search-result
  extracts (DUAA summaries, charity soft opt-in guidance, right to object page dated 2 Nov 2026).
- **PECR penalty banding:** that every PECR breach (not only some regulations) now carries the
  higher maximum (GBP 17.5m / 4%) is from secondary sources.
- **PECR reg 23 and the corporate-subscriber position, reg 21 TPS/CTPS details and the UK
  call-recording regulations** are stated from knowledge; not re-read this session (reg 22 itself was
  read on legislation.gov.uk).
- **TCPA revised rules:** effective date 25 Nov 2026 and Federal Register date 26 Oct 2026 come from
  law-firm alerts; the Federal Register documents (2026-21821, 2026-21820) were not opened. One
  secondary source says the revoke-all provision remains on the 31 Jan 2027 date; reconcile on the
  Federal Register text. The calling-hours rule (47 CFR 64.1200(c)) was not re-checked (eCFR blocked
  the fetcher).
- **Digital Omnibus (data/cookies):** Council compromise deleting Art 88a and trilogue status come
  from secondary commentary only.
- **ePrivacy Regulation withdrawal OJ publication** (October 2025) is secondary.
- **EDPB Guidelines 1/2024**: only the October 2024 consultation version was confirmed; whether a
  final version has since been adopted was not checked.
- **Text-only sentiment analysis outside the AI Act emotion-recognition definition** is commentary,
  not confirmed in the guidelines text.
- **CCPA ADMT scope** ("significant decision" domains; behavioural advertising excluded) not re-read
  in the final regulation text.
- **Colorado AI Act** effective status in 2026 and other state AI/bot disclosure laws: not checked.
- **State privacy laws:** employee and B2B exemptions confirmed only for Indiana; Alabama's universal
  opt-out signal requirement is disputed; Oklahoma bill number differs between sources (SB 546 vs
  HB 1012 shown in a different section of the IAPP chart).
- **All-party call-recording state list** relies on secondary guides; Michigan and Connecticut
  disputed.
- **Mini-TCPA details** for Oklahoma, Texas, Maryland are secondary.
- **CAN-SPAM penalty** for 2025/2026 after inflation adjustment not checked (FTC page shows USD 53,088).
- **EAA Art 32** transitional text not retrieved (Recital 101 used).
- **WhatsApp:** the Commission press release (IP/26/1276) could not be opened; interim-measure
  details are from secondary sources; whether Meta's EU fee for AI providers has been removed is
  unconfirmed.
- **Google Limited Use** policy text and any AI-training restrictions for Gmail data were not
  retrieved.
- **Exact HubSpot Starter price** ($7 shown alongside $20; reviewers call $7 a promotion).
- **Zoho, Pipedrive and Creatio prices** are from secondary sources (official pages did not expose
  prices to the fetcher, or returned 403/404).
- **Salesforce edition change details** beyond the pricing page (legacy-customer treatment, Service
  Cloud mapping) are secondary.
- **Microsoft Dynamics:** Sales Close Agent retirement (Sept/Oct 2026) and the end of published
  release-wave plans are single-source claims.
- **MCP servers** for HubSpot, Salesforce, Attio, Pipedrive and Zoho: GA dates and scope from
  secondary roundups, not vendor docs.
- **Freshworks Freddy, Zoho Zia and Attio AI feature pages** returned 404 or were not fetched; their
  rows rely on pricing pages and secondary sources.
- **Competitor feature cells** in section 1 and section 5 are a benchmark from vendor pages and
  product knowledge; each cell was not individually re-verified.
- **Regional availability of AI models** in europe-west2, europe-west3 and us-east5 (needed for the
  in-region AI design) was not checked.
