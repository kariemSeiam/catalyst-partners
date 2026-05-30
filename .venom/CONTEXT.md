# Catalyst Partners — Context

## Stack
- **Phase:** Pre-code agency workspace (strategy + specs + client tools). No `package.json`, no app repo yet.
- **Planned site:** Next.js App Router (SSG/ISR/SSR), Payload CMS self-hosted on Postgres (UAE region), serverless lead handler, HubSpot or Zoho CRM, privacy-first analytics, Cloudflare Turnstile.
- **Client tools:** Standalone bilingual HTML (`client/foundation.html`, `compass.html`, `handoff.html`) — Copy/WhatsApp export. Legacy redirects: `intake.html`, `validate.html`, `next.html`.
- **Team hub:** `internal/team-hub-ar.html` — Arabic-only team briefing/conscience (Gulf institutional register; sticky index, checklist localStorage).

## Architecture
- **Agency workspace** for Kariem building brand + website + content engine + lead pipeline for Catalyst Partners (UAE advisory; Mohamed Abdelwahab is one partner).
- **Doc spine:** `site/Catalyst-Partners-Website-Technical-Spec.md` (build), `brand/design-brief.md` (identity), `decisions/phase0-decisions.md` (9 gates), `draft/compass_artifact_*.md` (GTM research), `draft/catalyst_partners_brand_identity_strategy.md` (pivot transcript).
- **Client flow (3-page mini-site, private):** 01 foundation → 02 compass → 03 handoff.
- **Lead path (planned):** forms → serverless handler → score/route → CRM + owned Postgres raw log → ESP nurture → Slack/email for fast-track.

## Conventions
- **Positioning line (load-bearing):** Institutional transformation / governance / PMO / performance excellence + digital delivery layer. **Do not** lead with financial restructuring until credentialed finance partner joins.
- **Bilingual:** Arabic first-class — per-locale slugs/metadata, RTL via CSS logical properties, hide AR nav until translated, mandatory AR review gate, Western numerals in AR (UAE norm).
- **Ownership:** Self-hostable CMS, Git codebase, CRM-swappable via adapter + raw lead log.
- **Egypt hub:** Delivery only; never in brand/site/marketing.
- **Copy scrub:** No inflated Zegato stats on Catalyst entity; no unverified finance claims; cite compass report for market stats.

## Hot Paths
- `internal/team-hub-ar.html` — Team onboarding: بوصلية الفريق · ضمير الفريق (formal AR, not Egyptian colloquial).
- `client/foundation.html` — Mohamed-facing Phase 0 decisions (maps to `decisions/phase0-decisions.md` D1–D9 + extras).
- `client/compass.html` — Partner review of 10 GTM/positioning bets before build.
- `decisions/phase0-decisions.md` — Close D1 (residency) + D2 (CRM/routing) first; unlocks rest.
- `brand/design-brief.md` — Blocks logo until Arabic name decision (transliteration vs translated).

## Gotchas
- Phase 0 checklist in README still open: partner sign-off, domains, repos, CMS/env, CRM/ESP.
- `compass.html` claim c4 softens pivot vs MEMORY: finance stays on site, sequencing not removal.
- AWS UAE region cited as ap-southeast-3 in decisions doc — verify correct region code before infra.
- Intake WhatsApp uses `wa.me/?text=` without preset number — partner must add recipient.
- Draft Arabic Zegato strategy doc exists; not primary source for Catalyst positioning.
