# Catalyst Partners — Website Technical Specification
### v1.0 — Developer-Ready Build Document

> **Scope of this document:** architecture, rendering strategy, bilingual (Arabic/English) engineering, content management, lead capture, integrations, compliance, and delivery roadmap. This is a *technical* spec — no visual design, color, or brand-aesthetic decisions are made here (those live in a separate design spec). Where a decision needs the founders' input, it is flagged **[DECISION NEEDED]**.

> **Governing principle — Ownership.** Every recommendation defaults to *you own the data and can leave any vendor*. No proprietary platform that locks content, leads, or code behind an export wall. Self-hostable or data-portable by default.

---

## 1. What This Website Must Do (Functional Mandate)

This is **not** a brochure site. It is the firm's **authority engine and lead-capture system**. It has four jobs, in priority order:

1. **Establish authority** in the family-business governance / institutional-transformation white space (host the flagship report, insights, case evidence).
2. **Capture and qualify leads** — turn anonymous readers into identified, segmented, scored contacts in a CRM you own.
3. **Publish bilingually without a developer** — the team ships Arabic + English content, reports, and insights through a CMS, not through code deploys.
4. **Feed the pipeline** — every lead routes to the right segment (family business / mid-market / semi-government) and triggers the right follow-up.

Everything below serves these four jobs. If a feature doesn't serve one of them, it's out of scope for v1.

---

## 2. Architecture Overview

**Recommended pattern: Headless + Jamstack.**
A decoupled front end (what visitors see) talking to a headless CMS (where content lives) over an API, with lead/form logic handled by serverless functions, and a CRM as the system of record for contacts.

```
                    ┌─────────────────────────┐
                    │   Headless CMS (owned)   │  ← team publishes EN/AR here
                    │   content + media + i18n │
                    └────────────┬────────────┘
                                 │ content API (REST/GraphQL)
                                 ▼
   ┌──────────────┐   build    ┌─────────────────────┐   serves   ┌──────────┐
   │   Git repo   │──────────▶ │  Front end (Next.js) │──────────▶ │ Visitor  │
   │  (codebase)  │  CI/CD     │  SSG + ISR + SSR     │   HTML/JS  │ (EN/AR)  │
   └──────────────┘            └──────────┬──────────┘            └──────────┘
                                          │ form submit (server action / API route)
                                          ▼
                               ┌─────────────────────┐
                               │  Serverless function │  validate, anti-spam,
                               │  (lead handler)      │  score, route, log
                               └──────────┬──────────┘
                          ┌───────────────┼────────────────┐
                          ▼               ▼                ▼
                    ┌──────────┐   ┌────────────┐   ┌──────────────┐
                    │   CRM    │   │ Email/ESP  │   │  Analytics    │
                    │ (record) │   │ (nurture)  │   │ (privacy-first)│
                    └──────────┘   └────────────┘   └──────────────┘
```

**Why this pattern (rationale):**
- **Speed & SEO:** pre-rendered pages load fast and rank well — critical for an authority site in a competitive consulting SEO field.
- **Security:** no live database exposed to the public internet on the front end; the attack surface is tiny. Important when government/banking clients vet you.
- **Ownership & portability:** content is in a CMS you can self-host; code is in your Git repo; leads are in your CRM. You can swap the front-end host, the CMS, or the CRM independently without rebuilding everything.
- **Bilingual-friendly:** modern front-end frameworks have first-class internationalization (i18n) routing, and headless CMSs model multilingual content cleanly.

### 2.1 Rendering strategy (per page type)
| Page type | Strategy | Why |
|---|---|---|
| Home, service, about pages | **SSG** (static, built at deploy) | Rarely change; fastest possible load |
| Insights/reports index + articles | **ISR** (incremental static regen) | New content appears without full redeploy |
| Gated report landing + thank-you | **SSG** + client-side gate logic | Fast, but form state is dynamic |
| Form submission endpoint | **Server function** (SSR/API route) | Must run server-side for security & CRM calls |
| Search results | **Client-side or edge** | Dynamic query |

---

## 3. Recommended Stack

> All choices optimize for **ownership, bilingual support, and longevity** over novelty.

| Layer | Recommendation | Why it / alternatives |
|---|---|---|
| **Front-end framework** | **Next.js (App Router)** | Best-in-class i18n routing, hybrid rendering (SSG/ISR/SSR), huge talent pool in the region, excellent RTL support via CSS logical properties. *Alt: Astro (lighter, content-first) if interactivity stays low.* |
| **Headless CMS** | **Payload CMS** (self-hosted) **or Strapi/Directus** | Self-hostable = you own the DB and content outright; strong localization (per-field EN/AR), role-based editorial access, media library. *Alt: Sanity (hosted, excellent i18n, but data lives on their cloud — acceptable if export is configured).* **[DECISION NEEDED]:** self-host (max control, more ops) vs hosted (less ops, slight ownership tradeoff). |
| **Database** (for CMS) | **PostgreSQL** | Mature, portable, supported by all the above CMS options. |
| **CRM** | **[DECISION NEEDED]** — owned/portable preferred | Options: self-hosted (e.g., a portable open CRM) for max control; or a mainstream CRM (HubSpot/Zoho/Pipedrive) for speed with a documented export path. The lead handler is CRM-agnostic by design (see §7). |
| **Email / nurture (ESP)** | Transactional + marketing provider with API | Two needs: transactional (form confirmations, report delivery) and marketing (nurture sequences). Must support Arabic content and RTL email templates. |
| **Hosting (front end)** | Edge/CDN platform (Vercel/Netlify) **or** self-managed (Cloudflare Pages / VPS + CDN) | **[DECISION NEEDED]** based on data-residency posture (see §10). |
| **Media/asset storage** | S3-compatible object storage | Portable; serve via CDN. |
| **Anti-spam** | Cloudflare Turnstile + honeypot | Privacy-respecting (no Google reCAPTCHA), works in-region. |
| **Analytics** | Privacy-first (Plausible/Matomo — self-hostable) | GDPR/PDPL-friendly, often cookieless, you own the data. *Add server-side conversion tracking for accuracy.* |

---

## 4. Bilingual (Arabic / English) Architecture — *the part most agencies fake*

This is the highest-risk, highest-credibility area. For a firm courting Arabic-first government and family-business clients, a half-built Arabic experience is a credibility killer. Treat **Arabic as a first-class language, not a translation layer**.

### 4.1 URL & routing strategy
- **Use sub-path routing:** `catalystpartners.ae/en/...` and `catalystpartners.ae/ar/...`
  - Best for SEO (single domain authority), clearest ownership, simplest hosting.
  - *Rejected alternatives:* subdomains (`ar.`) split SEO authority; query params (`?lang=ar`) are SEO-hostile.
- **[DECISION NEEDED]:** default locale + root behavior. Recommend: root `/` detects via `Accept-Language` header → redirects to `/en` or `/ar`, with a persistent manual language switcher that sets a preference cookie. Never trap a user in the wrong language.
- **Locale-aware slugs:** allow the Arabic page to have an Arabic slug (e.g., `/ar/الرؤى/...`) — the CMS must support per-locale slugs, not force the English slug onto Arabic pages.

### 4.2 RTL (right-to-left) engineering
- **`dir="rtl"`** set on `<html>` for Arabic, `dir="ltr"` for English — driven by the active locale, not hardcoded.
- **Use CSS logical properties throughout** (`margin-inline-start`, `padding-inline-end`, `inset-inline`, `text-align: start`) instead of physical `left`/`right`. This makes the entire layout mirror automatically with the `dir` attribute — no duplicate stylesheets.
- **Directionally-aware components:** icons that imply direction (arrows, chevrons, "next/back") must flip in RTL. Carousels, sliders, and progress indicators must reverse.
- **Mixed-content handling:** Arabic pages will contain LTR fragments (English brand names, numbers, email addresses, "PMO", "EFQM"). Use Unicode bidirectional isolation (`<bdi>` / `unicode-bidi: isolate`) so these don't break text flow.

### 4.3 Typography & locale formatting
- **Separate, properly-hinted Arabic typeface** with correct line-height (Arabic needs more vertical space than Latin). *(Specific fonts = design spec, not here — but the architecture must support a per-locale font stack.)*
- **Numerals:** **[DECISION NEEDED]** — Arabic-Indic (٠١٢٣) vs Western Arabic (0123) numerals in the Arabic locale. (Common modern choice in UAE business contexts is Western numerals even in Arabic copy — confirm with the founders.)
- **Dates, numbers, currency:** format per-locale using the platform's `Intl` APIs. Don't hardcode formats.

### 4.4 Content model for bilingual (see also §5)
- **Field-level localization:** every content field (title, body, slug, meta) exists in both `en` and `ar`. Editors see both side by side.
- **Independent publishing + fallback:** a page can be published in English while Arabic is still drafting. Define fallback behavior: if an Arabic version doesn't exist yet, **[DECISION NEEDED]** — show English with a notice, or hide from the Arabic site entirely. (Recommend: hide from AR nav until translated; never show raw English where Arabic is promised.)
- **Translation workflow:** content is authored, then a translation/review step before the Arabic version goes live (editorial workflow, §6).

### 4.5 Bilingual SEO
- **`hreflang` tags** on every page pointing to its counterpart locale (`hreflang="ar-AE"`, `hreflang="en"`, plus `x-default`).
- **Localized metadata:** Arabic pages need Arabic `<title>`, meta description, and Open Graph tags — not auto-translated English.
- **Localized sitemaps:** generate per-locale entries; submit both.
- **Arabic search behavior:** site search must handle Arabic (diacritics-insensitive matching, normalization of alef/hamza variants). This is non-trivial — budget for it explicitly.

---

## 5. CMS & Content Model

The CMS is the team's daily tool. Model it around the **content the strategy demands**, not generic "pages."

### 5.1 Core content types (collections)
| Collection | Purpose | Key fields (each localized EN/AR) |
|---|---|---|
| **Pages** | Static pages (Home, About, Approach) | title, slug, sections (flexible blocks), SEO meta |
| **Services** | Each offering (e.g., "Governance & Succession", "PMO & Performance") | title, slug, summary, body, related case evidence, target segment tag, CTA |
| **Insights** | Articles / thought leadership (LinkedIn-repurposed) | title, slug, body, author, publish date, topic tags, segment tags, hero asset, SEO meta |
| **Reports (gated)** | Flagship report + lead magnets | title, abstract, cover asset, gated file (PDF), gate form config, landing copy, "what you'll learn" |
| **Case Evidence** | Anonymized engagement outcomes | challenge, approach, outcome metrics, segment, sector, consent flag |
| **People** | Partners / team | name, role, bio, credentials (EFQM/PMO/etc.), photo, LinkedIn |
| **Segments** | Family Business / Mid-Market / Semi-Government landing hubs | positioning copy, mapped services, mapped insights, segment-specific CTA |
| **Globals** | Nav, footer, contact details, social links, legal text | — (localized) |

### 5.2 Flexible content blocks (page builder)
Build pages from a library of reusable, typed blocks (hero, rich text, stat band, service grid, report CTA, quote, FAQ, contact band) so the team composes pages without dev work. Each block is localizable. *(Block visual design = design spec.)*

### 5.3 Editorial roles & permissions
- **Admin** (founders/lead): full access, publish, manage users.
- **Editor:** create/edit/publish content.
- **Author/Translator:** create/edit, submit for review, *cannot* publish.
- **[DECISION NEEDED]:** does Arabic content require a mandatory review step by a designated reviewer before publish? (Recommended: yes — protects brand-critical Arabic quality.)

### 5.4 Media library
- Central, taggable, searchable. Stores report PDFs (gated, served via signed/expiring URLs — never a public static link), images, downloadable assets. Track which assets are gated vs public.

---

## 6. Editorial & Content-Production Workflow

The content engine (the next deliverable) plugs in here. The website must support this pipeline natively:

```
Idea/Calendar → Draft (EN) → Internal review → Translate (AR) → AR review
   → Schedule → Publish (both locales) → Distribute (LinkedIn/email) → Measure
```

- **Draft → Review → Publish states** on every content item, with scheduled publishing (date/time, timezone = GST/UAE).
- **Preview:** authors preview the live-rendered page (both locales, both directions) before publishing.
- **Versioning / revision history:** every change is tracked and revertible (audit trail — matters for a governance-positioned firm).
- **Content reuse:** an Insight can be flagged "LinkedIn-published" with a canonical link, supporting the repurposing strategy without duplicate-content SEO penalties.

---

## 7. Lead Capture & Conversion System — *the revenue spine*

This is where readers become pipeline. Design it as a **system you own**, CRM-agnostic, so you never lose your lead data to a platform.

### 7.1 Capture points
| Capture point | Trigger | Data captured | Lead intent signal |
|---|---|---|---|
| **Gated report download** | Wants the flagship report / lead magnet | name, work email, company, role, segment (self-select) | High — authority-stage interest |
| **Segment inquiry form** | On Family Business / Mid-Market / Semi-Gov hub | name, email, company, segment, message, optional phone | Very high — bottom-funnel |
| **General contact** | Contact page | name, email, message, segment (optional) | Medium-high |
| **Newsletter / insights subscribe** | Wants ongoing content | email, optional name, topic interest | Low-medium — nurture-stage |
| **Event / roundtable RSVP** | Governance roundtable, NextGen workshop | name, email, company, role | High — relationship-stage |

### 7.2 The lead handler (serverless function) — pipeline logic
Every submission flows through one owned server-side function that does, in order:

1. **Validate** input (server-side, never trust the client).
2. **Anti-spam:** verify Turnstile token + honeypot + rate-limit by IP.
3. **Normalize & enrich:** clean email, infer company from email domain, capture UTM/source, capture which content/locale drove the submission.
4. **Score the lead** (see §7.3).
5. **Route by segment** to the correct owner/queue (family business → Partner A; semi-gov → Partner B; etc.).
6. **Write to CRM** (system of record) via API.
7. **Trigger email:** transactional confirmation + (for gated reports) deliver the signed download link; enroll in the right nurture sequence.
8. **Log** to analytics (server-side conversion event) and to an internal owned store (so even if the CRM is swapped, you retain the raw lead log).
9. **Notify** the team (email/Slack/webhook) for high-score leads in real time.

> **Ownership guarantee:** step 8's owned raw-lead log means the CRM is replaceable. You never depend on a vendor to know who your leads are.

### 7.3 Lead scoring model (initial heuristic)
Score = sum of signals; tune over time.
- **Source weight:** gated flagship report (+30), segment inquiry (+50), contact (+40), newsletter (+10), event RSVP (+45).
- **Firmographic:** corporate email domain (+15) vs free email (−10); seniority keywords in role ("Chairman/CEO/Director/Family Office") (+20).
- **Engagement (if cookie consent given):** number of pages/reports viewed, return visits, time on key service pages (+1–20 banded).
- **Segment fit:** matches a priority segment (family business / semi-gov) (+15).
- **Threshold:** score ≥ 70 → immediate team notification + fast-track; 40–69 → standard nurture; < 40 → newsletter nurture only.

**[DECISION NEEDED]:** confirm segment-to-owner routing map and the high-priority notification threshold.

### 7.4 Form UX requirements (functional, not visual)
- Inline validation, accessible labels, fully bilingual + RTL-aware.
- Progressive disclosure for longer forms (don't ask phone until qualified).
- Clear consent checkboxes (marketing opt-in separate from privacy acknowledgment — see §10).
- Graceful failure (if CRM API is down, still capture to the owned log and queue a retry — **never lose a lead**).

---

## 8. Integrations

| System | Direction | Purpose | Notes |
|---|---|---|---|
| **CRM** | Site → CRM | Lead system of record | Via REST API; abstracted behind the lead handler so it's swappable |
| **ESP (email)** | Site → ESP | Confirmations, report delivery, nurture enrollment | Must support Arabic/RTL templates |
| **Analytics** | Site → Analytics | Behavior + conversions | Privacy-first; server-side events for accuracy |
| **LinkedIn** | Manual/assisted | Content distribution (repurpose 35k-follower audience) | No reliable public publishing API for personal profiles — workflow is "publish on site → distribute to LinkedIn"; site provides easy share/canonical-link tooling |
| **Calendar/booking** *(optional v2)* | Site → scheduler | Book a consultation | Defer to v2 unless founders want it at launch |

> **Abstraction rule:** every third-party integration sits behind an internal interface/adapter. Swapping CRM or ESP later = rewrite one adapter, not the whole site.

---

## 9. SEO, Performance & Accessibility (Non-Functional Requirements)

**SEO**
- Per-locale metadata, `hreflang`, canonical tags, structured data (Organization, Article, BreadcrumbList schema).
- Clean semantic HTML, server-rendered content (no JS-only content for crawlers).
- XML sitemaps per locale; robots.txt; 301 redirect map for any legacy URLs.

**Performance budgets (targets)**
- Largest Contentful Paint < 2.5s on 4G; total JS payload kept minimal; images served responsive + next-gen formats via CDN.
- Core Web Vitals "good" thresholds as a launch gate.

**Accessibility**
- WCAG 2.1 AA as the standard (matters for government-facing credibility and is increasingly expected in UAE public-sector procurement).
- Full keyboard nav, proper ARIA, sufficient contrast (contrast = design spec, but compliance is tested here), screen-reader testing in **both** Arabic and English.

---

## 10. Security, Privacy & Compliance

This section is disproportionately important because the firm sells **governance and trust**. The site must visibly practice what it sells.

- **UAE PDPL** (Federal Decree-Law No. 45 of 2021) compliance: lawful basis for processing, clear privacy notice (EN/AR), data-subject rights handling, consent records.
- **[DECISION NEEDED] — Data residency:** government/semi-gov clients may expect UAE-hosted data. This drives the hosting choice (§3). Decide before committing to a host. If residency is required, lean self-managed in-region; if not, edge platforms are fine.
- **Cookie consent:** granular, opt-in for non-essential cookies (analytics/marketing), with a preference center. If using privacy-first cookieless analytics, the consent burden drops significantly.
- **Egypt delivery hub note:** if the Egypt team accesses any site/CRM systems, account for cross-border data transfer (Egypt Data Protection Law No. 151/2020) and access controls — keep client/lead PII access role-restricted.
- **Transport & app security:** HTTPS everywhere (HSTS), security headers (CSP, X-Frame-Options), signed/expiring URLs for gated PDFs, secrets in a vault (never in the repo), dependency scanning in CI.
- **Audit & backups:** automated CMS/DB backups with tested restore; access logs retained; admin access via SSO/2FA.
- **Form abuse protection:** rate limiting + Turnstile + honeypot (already in §7.2).

---

## 11. Hosting, DevOps & Environments

- **Environments:** Local → Staging → Production. No content or code reaches Production unreviewed.
- **CI/CD:** Git-based. Push to main → automated build, tests, deploy. Preview deployments for every branch/PR.
- **Infrastructure as code** where practical (reproducible, portable — supports the ownership principle).
- **Monitoring:** uptime monitoring, error tracking (e.g., Sentry), CMS/DB health alerts.
- **Domains & DNS:** secure `.ae` (and `.com` defensive registration); DNS via CDN provider for performance + DDoS protection.
- **[DECISION NEEDED]:** managed hosting (faster, less ops overhead) vs self-managed VPS/in-region (max control + residency). Tied to §10 residency decision.

---

## 12. Build Roadmap (Phased)

**Phase 0 — Foundations (decisions + setup)**
Resolve all **[DECISION NEEDED]** items, register domains, stand up repos, CMS, environments, choose CRM/ESP/host.

**Phase 1 — Core bilingual site (MVP authority engine)**
Next.js + CMS wired; i18n routing + full RTL; core content types (Pages, Services, Insights, People, Segments); home + service + segment + about pages; SEO foundation; analytics. *Goal: a fast, fully bilingual, publishable authority site.*

**Phase 2 — Lead system (revenue spine)**
Gated reports + media library + signed URLs; all forms; lead handler + scoring + CRM/ESP integration + owned lead log; nurture sequences; team notifications. *Goal: every visitor can become a scored, routed lead.*

**Phase 3 — Content engine integration & polish**
Editorial workflow (draft→review→translate→publish), scheduling, versioning; site search (Arabic-aware); case-evidence collection; performance/accessibility hardening to launch gates.

**Phase 4 — Launch & post-launch**
Pre-launch QA in both locales/directions, security review, redirect map, sitemap submission, soft launch → monitored full launch. Then iterate on scoring, conversion, and content based on real data.

**Phase 5+ (v2 candidates)**
Booking/scheduler, client portal / the PMO-dashboard delivery layer (separate product spec), gated members area, advanced personalization by segment.

---

## 13. Open Decisions Summary (collect answers before Phase 1)

1. **CMS hosting:** self-host (max control) vs hosted (less ops)?
2. **CRM choice** + the segment→owner routing map.
3. **Root-locale behavior** + default language.
4. **Arabic fallback** when translation isn't ready (hide vs show-EN-with-notice).
5. **Mandatory Arabic review step** before publish?
6. **Arabic numerals** style (Arabic-Indic vs Western).
7. **Data residency** requirement (drives hosting) — especially for government clients.
8. **Lead-score notification threshold** + which leads are "fast-track."
9. **Booking/scheduler** at launch or v2?

---

## 14. Explicit Non-Goals (v1)

- No e-commerce / payments.
- No public-facing client portal (that's the separate delivery-product spec).
- No design system / visual identity (separate design spec).
- No multi-region CDN complexity beyond what performance requires.
- No speculative AI features on the public site until they serve one of the four jobs in §1.

---

*This spec is intentionally technology-opinionated but vendor-swappable: every named tool can be replaced by an equivalent without changing the architecture, because the architecture — not the vendor — is the asset you own.*
