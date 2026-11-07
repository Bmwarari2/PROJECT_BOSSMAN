# Steps only a person can take — Thapsus CRM

Suite-wide steps (repository, Google Cloud, domains, legal, certification, design partners, SES) are in
`suite/HUMAN_TODO_SUITE.md`. These are CRM's own. Status on 2026-11-07: none done. Build against mocks and sandboxes
until each is done.

## B-1 Approve the build brief

1. Read `docs/BUILD_BRIEF.md`, `docs/PLAN.md` and `docs/FEATURES.md`.
2. Approve, or note changes, in the lead session. The build starts only after approval.

## B-2 Design partners (needed by the C1 gate)

1. Find at least two UK pilot companies (one B2B sales-led, one with a service desk and newsletters).
2. Sign pilot agreements and DPAs before any real data; until then pilots use fictional data.

## B-3 Google Workspace sync (start in Stage 1; it takes weeks)

1. Create the Google Cloud OAuth client for Gmail and Calendar in the suite project, with the production consent
   screen (Thapsus name, privacy policy, domain).
2. Submit for verification of the restricted Gmail scopes.
3. Book the annual CASA security assessment with an approved assessor; repeat every year.
4. Record the verification and assessment dates here.

## B-4 Microsoft 365 sync

1. Register a multi-tenant Microsoft Entra application for Microsoft Graph (mail, calendars, and later Teams).
2. Complete Microsoft publisher verification (Microsoft Partner Network id) so customers' admins see a verified
   publisher when consenting.
3. Write the admin-consent guide for customers (the lead session drafts it).

## B-5 Marketing email reputation

1. In the suite's SES account (`suite/HUMAN_TODO_SUITE.md` H-S10), request production access and dedicated IP pools for
   CRM marketing in each region the CRM sends from; plan the warm-up with the lead session.
2. Register for mailbox-provider feedback and reputation tools: Google Postmaster Tools, Yahoo Complaint Feedback Loop,
   Microsoft SNDS and JMRP.

## B-6 Telephony and messaging partners (before Phase 7)

1. Twilio account (or another provider) with UK number regulatory bundles; later EU and US numbers and US 10DLC brand
   and campaign registration.
2. WhatsApp Business Platform: Meta business verification and a tech-provider or solution-partner route (directly
   or through Twilio); accept Meta's terms, including the AI providers clause.
3. Record each provider in SUBPROCESSORS with region before integration.

## B-7 Do-not-call data (before Phase 5 and Phase 11)

1. UK: licence TPS and CTPS data for screening (through the Data & Marketing Association's licensing).
2. US: decide whether customers bring their own National DNC subscription or Thapsus screens as their agent; register
   accordingly.

## B-8 E-signature and meeting partners (before Phase 2 and Phase 5)

1. DocuSign and Adobe Acrobat Sign developer and partner programmes; production API keys.
2. Zoom Marketplace app review; Slack app directory review.

## B-9 Marketing-law review

1. Ask a UK data-protection solicitor to review the UK marketing rule pack (PECR regulations 21–23 as amended, the
   charity soft opt-in, the cookie exemptions) before release C1, and EU and US counsel before those packs.

## B-10 Pricing

1. Approve the CRM price configuration (seats by edition, light users, service and marketing add-ons, contact tiers,
   message pass-through pricing for SMS and WhatsApp, AI credits). Prices are configuration in the suite back office,
   never code.
