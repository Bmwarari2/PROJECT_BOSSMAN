# Feature catalogue — Thapsus CRM

Researched against Salesforce (Sales, Service, Marketing, Agentforce), HubSpot (all hubs and Agent Hub), Microsoft
Dynamics 365 Sales and Customer Service, Zoho CRM and Zoho One, Pipedrive, Freshworks, Attio, monday CRM, Close,
Creatio, SugarAI, Odoo and Copper (`RESEARCH.md` §1, with which leaders have each feature). **Tier**: Core (launch:
what a buyer needs to leave spreadsheets, Pipedrive, HubSpot Starter or Zoho Standard), Advanced (parity with HubSpot
Professional, Zoho Enterprise, Dynamics Sales Enterprise), Enterprise (parity with Salesforce and Dynamics Premium for
large customers). **Phase**: see `PLAN.md`. Everything configurable is set in Studio (`suite/STUDIO.md`) without code.

## 1. Data model

| Feature | Tier | Phase |
| --- | --- | --- |
| Accounts with parent and child hierarchy and roll-ups | Core | 1 |
| Contacts linked to several accounts with role labels | Advanced | 1 |
| Typed many-to-many associations between any records | Advanced | 1 |
| Person accounts (B2C mode) per tenant | Advanced | 1 |
| Households | Enterprise | Later |
| Custom objects and custom fields (formula, roll-up, lookup, multi-select, money, date-time with zone) | Core / Advanced | 0, 1 |
| 360-degree activity timeline that respects field-level permissions | Core | 1 |
| Files with preview and versions | Core | 1 |
| Saved views, filters, tags, static and dynamic lists | Core | 1 |
| Multi-currency with dated rates (from Finance when linked) | Advanced | 1 |
| Field history into the audit log | Core | 1 |

## 2. Leads, capture, routing and scoring

| Feature | Tier | Phase |
| --- | --- | --- |
| Lead object or lifecycle stages per tenant | Core | 2 |
| Accessible web forms with consent capture; email-to-lead; chat-to-lead | Core | 2, 4 |
| Criteria-based assignment rules with "which rule fired" | Core | 2 |
| Round-robin, weighted and capacity assignment skipping absent reps | Advanced | 2 |
| Lead conversion with duplicate check | Core | 2 |
| Rule-based lead and account scoring with decay and explanations | Core / Advanced | 2 |
| Speed-to-lead SLA timers and escalation | Advanced | 2 |
| Website visitor identification (consent required in the EU and for sales profiling in the UK) | Enterprise, optional | Later |

## 3. Pipelines, quotes and sales performance

| Feature | Tier | Phase |
| --- | --- | --- |
| Deals with amount, currency, stage, probability, close date, next step | Core | 2 |
| Multiple pipelines; stages with required fields and guidance | Core / Advanced | 2 |
| Stalled-deal alerts | Core | 2 |
| Line items from products and price books (from ERP when linked) | Core | 2 |
| Opportunity teams and revenue splits | Advanced | 2 |
| Board, list, calendar and forecast views; win and loss reasons; competitors | Core | 2 |
| Quotes with templates and versions; discount approvals | Core / Advanced | 2 |
| E-signature through DocuSign or Adobe Acrobat Sign | Advanced | 2 |
| CPQ: bundles, options, rules, guided selling | Enterprise | 6 |
| Contracts and clause library; subscriptions, renewals, ARR and MRR | Advanced / Enterprise | 6 |
| Forecasting with categories, roll-ups, adjustments, snapshots; quotas | Advanced | 6 |
| Territories (rule-based, hierarchical, several models, preview) | Enterprise | 6 |
| Commissions with statements and approvals; results to HR as pay inputs | Enterprise | 6 |
| Playbooks and guided selling; account planning; mutual action plans | Advanced / Enterprise | 6 |
| Coaching and call scorecards (never emotion inference about staff) | Enterprise | Later |

## 4. Email, calendar, meetings and sequences

| Feature | Tier | Phase |
| --- | --- | --- |
| Two-way calendar sync (Google, Microsoft 365) | Core | 3 |
| Two-way email sync (Gmail; Microsoft Graph) with logging rules and private exclusions | Core | 3 |
| Open and click tracking, switchable per tenant | Core | 3 |
| Templates, snippets, merge fields, attachments library | Core | 3 |
| Shared team inboxes | Advanced | 3 |
| Gmail add-on, Outlook add-in, browser extension | Core | 3 |
| Scheduled send and per-mailbox limits | Core | 3 |
| Sequences with email, call, task and manual LinkedIn steps; auto-unenrol; quiet hours; throttles | Advanced | 3 |
| Meeting scheduler: personal, round-robin and team pages | Core | 3 |

## 5. Telephony and messaging

| Feature | Tier | Phase |
| --- | --- | --- |
| Click-to-call through a CTI adapter framework (Twilio, Aircall, RingCentral) | Advanced | 7 |
| Power dialer (no predictive dialler) | Enterprise | 7 |
| Call recording with a jurisdiction consent engine; off by default | Advanced | 7 |
| Transcription and summaries (optional AI, consent first) | Advanced | 12 |
| Two-way SMS | Advanced | 7 |
| WhatsApp Business Platform with cost estimates | Advanced | 7 |
| Social inbox and publishing | Advanced / Enterprise, optional | 7 |

## 6. Marketing

| Feature | Tier | Phase |
| --- | --- | --- |
| Segments and lists (static, dynamic, cross-object) | Core | 5 |
| Accessible email builder with dynamic content and preview | Core / Advanced | 5 |
| Forms and landing pages on region-pinned hosting | Core / Advanced | 5 |
| Journeys with branches, waits, goals and re-entry rules | Advanced | 5 |
| A/B and multivariate tests | Advanced | 5 |
| Events and webinars (Zoom, Teams) | Advanced | 5 |
| Ad-audience sync (hashed, consent and opt-out aware) | Advanced | Later |
| Attribution (first, last, multi-touch; campaign influence) | Advanced / Enterprise | 5 |
| SMS and WhatsApp campaigns | Advanced | 7 |
| Website tracking under the consent rules | Advanced | 5 |

## 7. Consent, preferences and deliverability

| Feature | Tier | Phase |
| --- | --- | --- |
| Lawful basis per person, purpose and channel, with evidence and wording versions | Core | 0, 5 |
| Subscription types and preference centre per brand and locale; double opt-in | Core | 5 |
| Global and per-channel suppression; instant STOP and unsubscribe | Core | 0, 5 |
| Jurisdiction rules engine as rule-pack data (UK PECR; EU national ePrivacy; CAN-SPAM; TCPA and state laws) | Core | 5, 10, 11 |
| Subscriber type (individual, sole trader, corporate) and recipient country on every contact point | Core | 0 |
| Do-not-call screening (UK TPS and CTPS; US national and state lists) | Advanced | 5, 11 |
| Global Privacy Control and CCPA opt-out of sale and sharing | Core (US tenants) | 11 |
| Authenticated sending domains (SPF, DKIM 2048, DMARC) with guided DNS and checks | Core | 5 |
| RFC 8058 one-click unsubscribe; complaint and bounce handling with automatic pause and stop | Core | 5 |
| Article 14 notice workflow for imported or enriched people; field provenance; non-resetting retention | Core | 1 |
| Subject requests and a privacy complaint case type with the UK 30-day acknowledgement | Core | 1 |

## 8. Customer service

| Feature | Tier | Phase |
| --- | --- | --- |
| Tickets and cases with queues, priorities, statuses, parent and child | Core | 4 |
| Omnichannel inbox (email, web form, chat; WhatsApp, SMS and voice later) | Advanced | 4, 7 |
| SLAs, entitlements, business hours and holidays per time zone | Advanced | 4 |
| Skills- and capacity-based routing | Enterprise | Later |
| Macros and canned responses | Core | 4 |
| Knowledge base (internal and public, versions, approvals, translations) | Core / Advanced | 4 |
| Customer self-service portal (WCAG 2.2 AA) | Advanced | 4 |
| Live chat widget and rule-based bot | Core / Advanced | 4 |
| CSAT, CES and NPS surveys | Advanced | 4 |
| Contact centre (IVR, workforce management) | Partner | Integrate |
| Field service | — | Thapsus ERP Phase 12 |

## 9. Customer success, partners and field sales

| Feature | Tier | Phase |
| --- | --- | --- |
| Rule-based health scores with visible weights | Advanced | 8 |
| Success playbooks and onboarding plans | Advanced | 8 |
| Renewal and expansion pipelines | Advanced | 6, 8 |
| Product-usage events API | Enterprise | 8 |
| Churn-risk alerts (rules; AI optional) | Advanced | 8 |
| Partner portal: deal registration, lead distribution, tiers, MDF | Enterprise | 8 |
| Field sales: maps, nearby accounts, routes, check-ins (location only at check-in), offline | Advanced | 8 |

## 10. Reporting, data quality and platform

| Feature | Tier | Phase |
| --- | --- | --- |
| Standard reports (pipeline, funnel, revenue, activity, leaderboards) and dashboards | Core | 2 |
| Cross-object report builder; scheduled delivery that respects permissions | Core / Advanced | 9 |
| Historical snapshots, pipeline waterfall, forecast against quota | Advanced | 9 |
| Export to a regional BigQuery dataset | Enterprise | 9 |
| Duplicate rules and reversible merge with survivorship | Core | 1 |
| Normalisation and bulk clean-up | Core | 1 |
| First-party and company-registry enrichment | Advanced | 1 |
| Third-party person enrichment (off by default; sub-processor and notices first) | Enterprise, optional | Later |
| Imports with dry run and rollback; importers from HubSpot, Pipedrive, Salesforce, Zoho, Dynamics, Mailchimp | Core / Advanced | 1 |
| REST API (OpenAPI 3.1), bulk API, webhooks, idempotency keys | Core | 0, 9 |
| Developer sandboxes and configuration promotion | Enterprise | 9 |
| UI extensions; app marketplace | Advanced / Enterprise | 9 |
| Zapier, Make and n8n connectors | Core | 9 |
| Installable PWA with offline | Advanced | 8 |
| Roles, permission sets, field-level security, sharing rules, teams | Core / Advanced | 0 |
| SSO, MFA, session policies, IP allow-lists; SCIM | Core / Advanced | 0 |
| Retention policies and legal hold | Advanced | 1 |
| Data residency choice (UK, EU, US) | Core | 0 |

## 11. Optional AI features (off by default, drafts only)

| Feature | Human step | Phase |
| --- | --- | --- |
| Email and reply drafts | User sends; "AI-assisted" flag on the activity | 12 |
| Call and meeting summaries with action items | Only with captured recording consent; editable; tasks suggested | 12 |
| Conversation insights (topics, competitors, objections) | Aggregate coaching; no scoring of individuals | 12 |
| Text sentiment hints on emails and tickets | Hint only, never drives outcomes; never about employees | 12 |
| Deal-risk signals and next-step suggestions | Rules first; suggestions with reasons | 12 |
| AI forecast column beside the rep's commit | People own the submitted number | 12 |
| Record updates proposed from emails and calls | Approval queue with provenance | 12 |
| Company research from licensed and public sources | Writes to suggested fields with citations | 12 |
| Plain-language reports and questions | Runs as the user under the sharing model | 12 |
| Service chatbot from published articles with citations | Discloses it is AI; hands over to a person | 12 |
| Case summaries, classification and routing suggestions | Agents decide | 12 |
| Knowledge-article drafts from resolved cases | Article approval | 12 |
| Segments from plain language; marketing content drafts | Approval; generated content labelled | 12 |
| Predictive scoring beside the rule-based score | Tenant can disable; top factors shown | 12 |
| Duplicate and merge suggestions | Reversible | 12 |
| MCP server with read and draft-only tools | Drafts approved in CRM | 12 |
| **Never**: emotion recognition of employees; AI scoring of reps; autonomous first messages to prospects; outbound AI voice calls | — | — |

## 12. Configuration without code (Studio)

Custom objects and fields; pipelines and stages per team; record types; layouts and dynamic forms per role; validation
rules (CEL); workflows (triggers, conditions, actions, delays, branches, schedules, cross-object); approval processes
(multi-step, delegation, escalation); assignment rules and queues; email templates with merge fields and conditional
blocks; custom buttons and actions; quote templates; consent purposes and subscription types; SLA policies;
labels ("deal" or "opportunity"); sandboxes and configuration promotion; solution packs (B2B software, professional
services, wholesale, charities).

## 13. Integrations customers expect

Google Workspace and Microsoft 365 (Graph); Zoom; Slack and Teams notifications; telephony (Twilio, Aircall,
RingCentral, Dialpad, Vonage); e-signature (DocuSign, Adobe Acrobat Sign, Dropbox Sign); Stripe payment links (with
Finance as billing owner); migration from Mailchimp, Brevo, Klaviyo, HubSpot, Pipedrive, Salesforce, Zoho; Zapier,
Make, n8n; ad platforms (consent-aware); accounting for tenants without Thapsus Finance (Xero, QuickBooks, Sage);
company registries (Companies House first). LinkedIn Sales Navigator API is closed to new partners: profile URLs and
manual tasks only, never scraping.
