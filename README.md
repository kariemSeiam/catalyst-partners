# Catalyst Partners — Agency Workspace

> **Status:** Phase 0 — Pre-code. Strategy validated, site specified, brand briefed.
> **Client:** Catalyst Partners — UAE premium strategic advisory (institutional transformation, governance, PMO, performance excellence + digital delivery)
> **Agency lead:** Kariem | **Partner:** Mohamed Abdelwahab

---

## The One Thing That Must Not Drift

**Lead with institutional transformation, governance, PMO, performance excellence + digital delivery layer.**
Do not restore financial restructuring / debt advisory to the foreground until a credentialed finance partner with a referenceable track record joins the roster. Every brand asset, every site copy block, every pitch deck must hold this line.

See `.venom/CONTEXT.md` for the full validated positioning.

---

## Project Structure

```
├── .venom/                        # AI session context & decisions
│   ├── CONTEXT.md                 # Full project anatomy for AI sessions
│   └── MEMORY.md                  # Load-bearing decisions with reversal conditions
│
├── brand/
│   └── design-brief.md            # Designer-ready brief (palette, typography, logo concept)
│                                  # [WAITING: Arabic name decision before logo design begins]
│
├── decisions/
│   └── phase0-decisions.md        # 9 decisions with recommendations; needs partner sign-off
│
├── site/
│   └── Catalyst-Partners-Website-Technical-Spec.md  # v1.0 developer-ready build doc (the spine)
│
├── client/                        # 3-page bilingual client flow for partner sign-off
│   ├── intake.html                # Page 01 — 12 foundation decisions (EN/AR)
│   ├── validate.html              # Page 02 — 10 market/positioning claims to validate (EN/AR)
│   └── next.html                  # Page 03 — 30-day PMO handoff plan (EN/AR)
│
├── content/                       # [NEXT] Content engine spec, editorial calendar, flagship report brief
│   └── .gitkeep
│
├── internal/
│   └── team-hub-ar.html           # Agency team conscience — Arabic only, no EN toggle
│
├── draft/                         # Conversation history, early strategy drafts, research
│   ├── compass_artifact_*.md      # GTM & Brand Strategy research report
│   ├── catalyst_partners_brand_identity_strategy.md  # Strategy pivot transcript
│   └── ملف_الذكاء_الاستراتيجي_محمد_عبدالوهاب_...md   # Partner intelligence (Arabic)
│
├── src/                           # [FUTURE] Website source code (Next.js App Router)
│   └── .gitkeep
│
├── README.md                      # This file
├── LICENSE                        # All Rights Reserved — proprietary
└── .gitignore
```

---

## Phase 0 — Status

| Area | Status | Owner |
|---|---|---|
| **Strategy** | ✅ Validated (GTM report complete) | Agency |
| **Technical spec** | ✅ Written (developer-ready) | Agency |
| **Brand brief** | ✅ Written (designer-ready) | Agency |
| **Phase 0 decisions (D1–D9)** | ✅ Drafted with recommendations | Agency |
| **Client pages (3)** | ✅ Built (bilingual, interactive) | Agency |
| **Team hub** | ✅ Built (Arabic-only) | Agency |
| — | — | — |
| **Decisions closed with partners** | ⏳ Pending Mohamed | Partner |
| **Domains registered** | ⏳ Pending | Partner |
| **Repos initialized** | ✅ Done | Agency |
| **CMS + environments stood up** | ⏳ Pending decision D1, D7 | Agency |
| **CRM + ESP chosen + configured** | ⏳ Pending decision D2, D9 | Agency |
| **Content engine spec** | ❌ Not yet drafted | Agency |
| **Logo / brand identity** | ⏳ Blocked on Arabic name decision | Partner |

---

## Critical Open Items

1. **Arabic name decision** — transliteration (`كاتاليست بارتنرز`) vs translated Arabic name. Blocks logo design.
2. **Data residency (D1)** — UAE-hosted required? Drives hosting and CRM choice. (Recommendation: yes.)
3. **CRM (D2)** — HubSpot Starter (recommended) or Zoho CRM if UAE residency required.
4. **Arabic reviewer name (D5)** — person who holds the mandatory review gate in the CMS.
5. **Segment ownership map (D2)** — who handles mid-market / manufacturing referrals.
6. **WhatsApp number** — client pages need a recipient number configured before deployment.

Full decision sheet: `decisions/phase0-decisions.md`

---

## Client Pages (for Partner Review)

The `client/` folder contains three private, noindex pages for Mohamed:

| Page | File | Purpose |
|---|---|---|
| **01 — Foundation** | `intake.html` | 12 decisions in 4 groups (voice / firm / rails / rhythm) |
| **02 — Validate** | `validate.html` | 10 market & positioning claims to confirm/adjust/challenge |
| **03 — Next** | `next.html` | 30-day execution timeline, ownership map, final CTA |

All three are bilingual (EN/AR) with RTL support, dark mode, copy-to-clipboard, and WhatsApp export. No external dependencies beyond Google Fonts.

> **⚠️ Pre-deployment:** Update `WA_NUMBER` in each client HTML file to the team WhatsApp number before sending links to Mohamed.

---

## Build Phases

| Phase | Scope | When |
|---|---|---|
| **0 — Foundation** | Decisions closed, domains/repos/CRM/env set up | Now |
| **1 — Core bilingual site** | Next.js + CMS + i18n + RTL + core pages | After Phase 0 closes |
| **2 — Lead system** | Gated reports, forms, scoring, CRM, owned lead log | Phase 1 + 1 |
| **3 — Content engine** | Editorial workflow, AR review gate, Arabic search | Phase 2 + 1 |
| **4 — Launch** | QA (both locales), security audit, go-live | Phase 3 |
| **5+ — Delivery layer** | Client portal, PMO dashboard (separate product) | Post-launch |

---

## Conventions

- **Bilingual:** Arabic is first-class — per-locale slugs, metadata, fonts, RTL via CSS logical properties. Hide AR nav until translated. Mandatory AR review gate. Western numerals in AR (UAE norm).
- **Ownership:** Self-hostable CMS, Git codebase, CRM-swappable via adapter + raw lead log.
- **Egypt hub:** Delivery only; never in brand/site/marketing.
- **Copy scrub:** No inflated Zegato stats on Catalyst entity. No unverified finance claims. Cite compass report for market stats.

---

## Quick Start (for future dev)

```bash
# Website source will live in src/
cd src
git init
# ... Next.js + Payload CMS setup follows in Phase 1
```

---

*This workspace is proprietary. See LICENSE for terms.*
