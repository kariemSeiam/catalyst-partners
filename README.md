# Catalyst Partners — Agency Workspace

> **Status:** Phase 0 — Strategy validated. Technical specification complete. Brand brief ready. Partner decisions pending.
> **Client:** Catalyst Partners — UAE institutional advisory: transformation, governance, PMO, performance excellence, and digital delivery.
> **Agency:** Kariem · **Partner:** Mohamed Abdelwahab

---

## Positioning (Load-Bearing)

**Lead with institutional transformation, governance, PMO, performance excellence + digital delivery layer.**

Financial restructuring / debt advisory must not return to the foreground until a credentialed finance partner with a referenceable track record joins the roster. Every brand asset, every site copy block, every pitch deck holds this line.

The wedge segment is **UAE family business** (~60% GDP, ~US$1tn wealth transfer, 33% with robust succession plans). The packaged offer is the **Legacy & Institution** suite.

---

## Project Structure

```
├── .venom/                        # AI session memory and cross-session decisions
│   ├── CONTEXT.md                 # Full project anatomy for agent continuity
│   └── MEMORY.md                  # Load-bearing decisions with reversal conditions
│
├── brand/
│   └── design-brief.md            # Designer-ready identity brief (palette, typography, logo)
│                                  # Blocks: Arabic name decision
│
├── decisions/
│   └── phase0-decisions.md        # 9 decisions gating the build; D1 (residency) + D2 (CRM) unlock all
│
├── site/
│   └── Catalyst-Partners-Website-Technical-Spec.md  # v1.0 developer-ready build specification
│
├── client/                        # 3-page bilingual partner-review sequence (EN/AR)
│   ├── foundation.html            # Strategic briefing + decisions (4 groups: voice, firm, rails, rhythm)
│   ├── compass.html               # Market and positioning validation (10 claims to confirm or challenge)
│   └── handoff.html               # Delivery sequence — Phase A through Phase D (not calendar-bound)
│
├── internal/
│   └── team-hub-ar.html           # Agency team conscience — Arabic only, Gulf institutional register
│
├── drafts/                        # Research, strategy transcripts, partner intelligence
│   └── *.md
│
├── fonts/                         # Self-hosted IBM Plex Sans Arabic woff2 (300–700)
│
├── content/                       # [NEXT] Editorial calendar, flagship report brief
├── src/                           # [FUTURE] Next.js App Router site source
│
├── README.md
├── LICENSE                        # All Rights Reserved — proprietary
└── .gitignore
```

---

## Phase 0 — Status

| Area | Status | Owner |
|---|---|---|
| Strategy & positioning | ✅ Validated (GTM research complete) | Agency |
| Website technical spec | ✅ Written, developer-ready | Agency |
| Brand brief | ✅ Written, designer-ready | Agency |
| Phase 0 decisions (D1–D9) | ✅ Drafted with recommendations | Agency |
| Client review pages (3) | ✅ Built, bilingual, interactive, self-hosted fonts | Agency |
| Team hub | ✅ Built, Arabic only | Agency |
| — | — | — |
| Partner decisions signed | ⏳ Pending | Mohamed |
| Domains registered | ⏳ Pending | Partner |
| Logo / brand identity | ⏳ Blocked on Arabic name decision | Partner |
| CMS + environment | ⏳ Gates on D1 (residency) + D7 (hosting) | Agency |
| CRM + notification stack | ⏳ Gates on D2 (CRM choice) + D8 (routing) | Agency |
| Content engine | ❌ Not yet drafted | Agency |

**Critical path:** Close **D1 (data residency)** and **D2 (CRM choice + segment routing)** — everything else cascades from these two.

---

## Client Pages — Design Principles

The three `client/` pages form a private partner-review sequence. Each is a standalone HTML document — zero framework, zero build step, zero external dependencies except Latin Google Fonts.

### What was built

| Page | Purpose | Sections |
|---|---|---|
| **Foundation** | Strategic briefing + decisions | Where we are · The gap · The decisions (4 groups) · Who owns what |
| **Compass** | Market and positioning validation | 10 claims across 4 groups — confirm, adjust, or challenge |
| **Handoff** | Delivery sequence | Phase A–D delivery · Ownership map · Closing call to action |

### Technical decisions

- **Bilingual first-class:** English and Arabic rendered simultaneously via `lang` toggle. RTL via CSS logical properties (`inset-inline-start`, `margin-inline`). Language preference preserved in `localStorage`.
- **Self-hosted Arabic font:** IBM Plex Sans Arabic (300–700) stored as woff2 in `/fonts/`. No Google CDN dependency — works in networks that block Google Fonts (UAE, Egypt, corporate firewalls).
- **No numbered counters:** Header shows named pages (Foundation · Compass · Handoff), not numbered steps. Decision cards show status dots (○ / ●), not item numbers.
- **Dark mode:** Respects `prefers-color-scheme` with manual toggle override. Persisted in `localStorage`.
- **WhatsApp export:** Answers formatted and sent via WhatsApp Web to the partner's number. Routing configured in a single constant per file.
- **Phase-based timeline:** Handoff uses Phase A–D instead of calendar days. No commitment to a delivery window until scope is known.

### Visual identity

The pages follow the design brief's constraints: premium bronze accent, midnight navy, off-white paper, instrumental serif headings, mono labels, restrained visual language. All CSS is inline — no external stylesheets, no icon libraries, no JavaScript runtime.

---

## Build Phases

| Phase | Scope | Gates On |
|---|---|---|
| **0 — Foundation** | Decisions closed, domains registered, repos, brand identity | Partner review sign-off |
| **1 — Core bilingual site** | Next.js + Payload CMS + i18n + RTL + core pages | Phase 0 close |
| **2 — Lead system** | Gated reports, scoring, CRM, owned lead log, notification | Phase 1 |
| **3 — Content engine** | Editorial workflow, Arabic review gate, content search | Phase 2 |
| **4 — Launch** | QA (both locales), security audit, go-live | Phase 3 |
| **5+ — Delivery layer** | Client portal, PMO dashboard (separate product) | Post-launch |

---

## Quick Reference

- **Planned stack:** Next.js App Router · Payload CMS · Postgres (UAE region) · Serverless lead handler · HubSpot/Zoho CRM · Privacy-first analytics · Cloudflare Turnstile
- **Arabic fonts:** IBM Plex Sans Arabic — self-hosted (300, 400, 500, 600, 700)
- **Latin fonts:** Instrument Serif (headings) · Inter Tight (body) · JetBrains Mono (code/labels) — Google Fonts
- **WhatsApp number:** `201067553846` — Mohamed Abdelwahab
- **Conventions:** Arabic first-class. Egypt hub backstage. Vendor-swappable architecture. No unverified finance claims.

---

## Context for AI Sessions

This workspace is consumed by agentic coding tools. The `.venom/` directory contains cross-session memory (`CONTEXT.md` for project anatomy, `MEMORY.md` for load-bearing decisions). Read both before starting any significant work.

Key decisions that must not be contradicted:

1. **Institutional transformation leads** — not financial restructuring. Reversal condition: credentialed finance partner joins roster.
2. **Family business is the primary wedge** — site IA, content engine, and lead weighting reflect this.
3. **Egypt is backstage** — never surfaced in brand, site, or marketing.
4. **Ownership-by-architecture** — every vendor replaceable, raw lead log, adapters for third parties.
5. **Arabic is first-class** — per-locale slugs, metadata, RTL via logical properties. Arabic review gate is mandatory.

---

*This workspace is proprietary. All Rights Reserved. See LICENSE for terms.*
