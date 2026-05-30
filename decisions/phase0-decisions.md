# Catalyst Partners — Phase 0 Decision Sheet
### 9 Decisions That Gate the Build

> **Format:** Each decision has a recommendation ready to execute. Founders confirm, override, or add a constraint. The goal is to close all 9 in one working session, not circulate a questionnaire.

---

## Priority 1 — Unlock Everything Else

---

### Decision 1 — Data Residency
**The question:** Must client, lead, and CMS data physically reside in UAE infrastructure?

**Recommendation: Yes — design for UAE residency from the start.**

The cost difference is negligible. The advantage is not. Government and semi-government procurement in the UAE includes data residency as a scoring factor, and for a firm selling governance and trust to government-adjacent clients, a residency objection raised during procurement is avoidable. Design for it now; removing the constraint later is impossible.

**Practical implication:** The CMS database and lead store run on **AWS Middle East (UAE)** region (ap-southeast-3, launched in the UAE in 2022, used by major UAE government and banking clients). The front-end static layer (Next.js, CDN) runs on Vercel or Cloudflare — no PII touches it, no residency concern. Gated PDFs are served via signed S3-compatible URLs from UAE-region storage.

**What to confirm:** Does any founding partner have a conflicting constraint (cost, existing infrastructure, or a specific client requirement that changes this)?

**Unlocks:** Decisions 2 and 6.

---

### Decision 2 — CRM Choice + Segment Routing Map
**The question:** Which CRM becomes the system of record for leads, and who owns which segment?

**Recommendation: HubSpot Starter as the operational CRM, with Postgres raw lead log as the insurance layer.**

HubSpot is the de facto standard in GCC professional services advisory — it integrates cleanly with the email stack, has deal-pipeline views that match advisory sales cycles (not e-commerce), supports Arabic contact fields, and every major ESP integrates with it. Most importantly, it has a full data export at any time. The raw lead log in Postgres (written before the HubSpot write) means HubSpot is replaceable on a weekend if needed.

**If data residency is confirmed:** Switch to **Zoho CRM** (has UAE-region datacenters via AWS UAE). Comparable functionality, less ecosystem, same export capability.

**Segment → Owner routing (proposed — confirm with partners):**
| Segment | Notification recipient | Rationale |
|---|---|---|
| Family Business | Mohamed | His verifiable network + EFQM/family-governance track |
| Semi-Government | Mohamed | SKGEP/ADAEP/excellence-ecosystem relationships |
| Mid-Market / Manufacturing | Partner TBD | Confirm who holds this |
| General Contact / Newsletter | Shared inbox | Volume low at launch; review after 90 days |

**Score threshold for fast-track notification:** Score ≥70 → immediate Slack + email to the relevant partner. Any named senior person (Chairman / CEO / Director / Family Office) submitting a family-business or segment inquiry form is fast-tracked regardless of total score — the signals are definitive.

**What to confirm:** Who handles mid-market? Does Mohamed want family-business leads first, or split with another partner?

---

## Priority 2 — Content and Publishing Model

---

### Decision 3 — Root Locale + Default Language
**The question:** What does a visitor see when they land on catalystpartners.ae with no prior preference?

**Recommendation: Geo-detect → /ar for UAE/GCC/MENA, /en for everything else. Persistent manual switcher overrides and sets a preference cookie.**

The rationale: LinkedIn will drive the largest volume of early traffic, and that audience is mixed. But the firm's primary clients — UAE family businesses, semi-government, banks — are Arabic-first. A UAE visitor landing on English first creates a slight dissonance. The switcher is always visible, always accessible. No one is trapped.

Technical note: geo-detection runs at the edge (Cloudflare Worker / Vercel middleware) — it adds zero load time and requires no server.

**What to confirm:** Do the partners have a strong preference for English-default? Some UAE B2B firms default to English as a signal of international positioning. Either is defensible; this is a brand character question, not a technical one.

---

### Decision 4 — Arabic Fallback Policy
**The question:** If an Arabic version of a page or article isn't translated yet, what does an Arabic-locale visitor see?

**Recommendation: Hide from Arabic navigation entirely until translated. Never show raw English on an Arabic-locale URL.**

No alternative is acceptable for this brand. Showing untranslated English on an Arabic URL communicates that Arabic is a second-class concern — precisely the credibility gap the firm is trying to exploit in competitors. At launch, fewer, complete Arabic pages are better than more, half-built ones.

**Operational implication:** The CMS editorial workflow enforces this: an article is only added to the Arabic sitemap and AR nav once the Arabic version passes the mandatory review step.

**What to confirm:** None required — this is non-negotiable from a brand perspective. Partners to acknowledge.

---

### Decision 5 — Mandatory Arabic Review Before Publish
**The question:** Does Arabic content require a designated reviewer's approval before it goes live?

**Recommendation: Yes. Hard gate in the CMS workflow.**

A single grammatical, idiomatic, or register error in Arabic governance content shown to a UAE semi-government client is a deal-qualifier in the wrong direction. The editorial workflow: draft (EN) → translate → AR reviewer approval → publish both. The reviewer role in the CMS is a named person, not a team. It can be Mohamed or an appointed Arabic editorial lead.

**What to confirm:** Who holds the Arabic reviewer role? This person needs to be in the system on day one of CMS setup.

---

### Decision 6 — Arabic Numerals Style
**The question:** In the Arabic locale, does the site use Arabic-Indic numerals (٠١٢٣٤٥٦٧٨٩) or Western Arabic numerals (0123456789)?

**Recommendation: Western Arabic numerals (0123) in all Arabic content.**

This is the UAE corporate and government standard. Walk into any DIFC firm, any Abu Dhabi government report, any Emirates NBD document in Arabic — all use Western numerals even in fully Arabic text. Arabic-Indic numerals are associated with Egyptian Arabic print publishing and traditional Arabic text, not UAE executive-level advisory. Using Arabic-Indic in this context would feel out of register.

**What to confirm:** Mohamed to validate against what he sees in government-excellence and family-business client documents. If a specific government client segment uses Arabic-Indic, the CMS supports switching this per-locale at any point.

---

## Priority 3 — Operating Model

---

### Decision 7 — CMS Hosting Model
**The question:** Self-hosted CMS (Payload CMS on a UAE-region server) or a hosted CMS (Sanity, with export configured)?

**Recommendation: Self-hosted Payload CMS on a UAE-region VPS or managed Postgres.**

Given the data residency decision (D1), self-hosting on a UAE-region server is the cleanest answer — ownership, residency, and no monthly SaaS bill that scales with content volume. Payload CMS is the most production-ready self-hosted headless CMS available in 2025: TypeScript-native, strong localization support, role-based access, and an admin UI the team can use without developer involvement.

**Operational requirement:** Someone (the agency or a managed hosting provider) runs the server. This is a small Postgres instance + a Node.js application. Automated backups to UAE-region S3. Estimated ops overhead: ~2 hours/month once stable.

**Alternative if zero ops is a hard constraint:** Sanity with full export configured + a UAE-region Cloudflare R2 for media. Data residency is softer (Sanity is on Google Cloud US), but acceptable if the partners aren't pursuing government procurement that scores residency. This decision must be made before D1 is closed.

**What to confirm:** Is there a developer / DevOps resource on the agency side, or does this require a managed server arrangement?

---

### Decision 8 — Lead Score Thresholds + Notification Logic
**The question:** At what score does a lead become a fast-track? Who gets notified, how?

**Recommendation:** The scoring model in the tech spec (§7.3) is pre-calibrated. Proposed final thresholds:

| Score | Action |
|---|---|
| ≥70 | **Fast-track:** Slack + email to segment owner within 60 seconds |
| 40–69 | **Standard nurture:** enrolled in segment-specific email sequence |
| <40 | **Newsletter nurture:** weekly insights sequence, no partner notification |

**Automatic fast-track overrides (score-independent):**
- Role field contains: Chairman, CEO, Managing Director, Family Office, Undersecretary, Director General
- Source: Segment inquiry form (bottom-funnel intent)
- Referral source: tagged referral from a named partner (private bank, law firm, Dubai Centre)

**Notification stack:** Slack (primary) + email to partner's work address. No SMS at launch — adds complexity for marginal speed gain.

**What to confirm:** Which Slack workspace / channel? Which email address per partner?

---

### Decision 9 — Booking / Scheduler at Launch or v2
**The question:** Does the site have an online booking tool at launch (Calendly-style) or just a "Request a Conversation" form?

**Recommendation: Form only at launch. Scheduler is v2.**

This is actually the more premium choice. An online booking link signals commoditized, high-volume consulting — the same aesthetic as a freelancer's Calendly. At launch, Catalyst needs to signal high-touch, selective access. "Request a Conversation" → form → a partner calls you within 24 hours is the correct operating model for a firm pitching to family-business patriarchs and government undersecretaries. It also lets the partners qualify before committing time.

**v2 condition:** When inbound volume is high enough that manual triage becomes the bottleneck, a scheduler behind a qualification step (not a public link) makes sense. That's a good problem to have.

**What to confirm:** Partners to acknowledge. Flag immediately if either partner already uses a public booking link that clients expect — that creates a continuity expectation to manage.

---

## Decision Status Tracker

| # | Decision | Status | Owner |
|---|---|---|---|
| 1 | Data residency | **Recommended: UAE** | Partners to confirm |
| 2 | CRM + routing map | **Recommended: HubSpot/Zoho + map above** | Partners to confirm segment ownership |
| 3 | Root locale | **Recommended: Geo-detect** | Partners to confirm preference |
| 4 | Arabic fallback | **Resolved: hide until translated** | Non-negotiable |
| 5 | Arabic review gate | **Recommended: Yes — confirm reviewer name** | Partners to name the reviewer |
| 6 | Arabic numerals | **Recommended: Western (0123)** | Mohamed to validate |
| 7 | CMS hosting | **Recommended: Self-hosted Payload on UAE VPS** | Agency to confirm ops capacity |
| 8 | Lead score + notifications | **Recommended: thresholds above** | Partners to provide Slack/email |
| 9 | Booking at launch | **Resolved: v2 only** | Non-negotiable |

---

*Close D1 and D2 first. Everything else cascades.*
