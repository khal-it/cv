# CV Update: Add Personal & Client Projects — Design

**Date:** 2026-07-24
**Status:** Approved by Khalit (sections 1–3 approved in conversation)

## Context

The CV (LaTeX, AltaCV template, EN + DE variants) is missing recent work:
three personal projects (khal.it portfolio, TessellAI, Norlin) and one client
project (Grip King). The existing Tessellai entry describes a retired AWS/SST
architecture, and the page-2 sidebar lists outdated plugin projects.

## Goals

1. Add Grip King as a client engagement in Experience.
2. Refresh the Personal Projects section with current, accurate entries.
3. Reposition the CV from "Flutter Expert" toward Fullstack + AI.
4. Final PDFs must be properly formatted and easily readable (explicit user
   requirement) — verified visually, not just by successful compilation.

## Decisions

- **Positioning:** Broaden to Fullstack + AI. Tagline changes:
  - EN: `Freelance Fullstack Engineer — Mobile, Web & AI`
  - DE: `Freelance Fullstack Engineer — Mobile, Web & KI`
- **Structure:** Two-tier. Medium-detail entries (2–3 bullets + trimmed tags)
  in the main-flow Personal Projects section; short link + tag entries in the
  page-2 sidebar.
- **Old content:** Keep the web-scrapers sidebar entry; remove the ARCore
  plugin and Facebook Analytics plugin entries; replace the stale AWS-era
  Tessellai content everywhere.
- **Grip King naming:** Named entry "Grip King", dated from git history.
- **TessellAI dates:** 12.2024 – 03.2026 (no longer actively developed).

## Section 1 — Grip King experience entry

Placement: top of Experience, above Purelei (ongoing engagement).

> **\cvevent:** Freelance Fullstack Engineer — Grip King — 01.05.2026 – Present
>
> **Task:** Architecting and building a multi-channel tire & rim e-commerce
> platform for the German market, replacing a manual spreadsheet-based retail
> operation.
>
> **Achievements:**
> - Designed and built the platform end-to-end: NestJS API, SvelteKit
>   storefront, and admin dashboard in a pnpm monorepo.
> - Modeled EU tire-label compliance (EU 2020/740 / EPREL), DOT tracking, and
>   ledger-based inventory with immutable stock lots.
> - Implemented 15-minute stock reservations preventing oversell across eBay,
>   Kleinanzeigen, Tyre24/ALZURA, and the shop's own storefront.
> - Integrated Billbee OMS (orders, stock, invoicing, DATEV), lexoffice,
>   Stripe, and PayPal.
> - Set up security-scanned CI/CD (Gitleaks, Semgrep, dependency audit) with
>   auto-deployed staging on Hetzner.
> - Spec-driven development with 23 ADRs and a full testing pyramid (Vitest,
>   Playwright).
>
> **Tags:** E-Commerce, TypeScript, NestJS, SvelteKit, Svelte 5, Prisma,
> PostgreSQL, Redis, BullMQ, Tailwind CSS, Stripe, PayPal, Billbee, Docker,
> Caddy, GitHub Actions, CI/CD, Sentry, TDD

## Section 2 — Personal Projects (main flow)

Replaces the current single stale Tessellai entry.

### TessellAI · 12.2024 – 03.2026 · https://tessellai.com

- Built a real-time photo-mosaic platform for live events: guests upload
  photos via QR code and watch them assemble into mosaic art live on screen.
- Engineered a computer-vision pipeline (OpenCV, NumPy, Jonker-Volgenant
  linear-assignment solver) with incremental per-tile updates streamed over
  WebSockets + Redis Pub/Sub.
- Migrated the platform from AWS serverless (SST/TypeScript) to a
  FastAPI/Python monolith with Celery workers and typed OpenAPI client
  generation.

Tags: Python, FastAPI, Celery, Redis, PostgreSQL, React, TypeScript, PixiJS,
OpenCV, WebSockets, Docker, Vercel

### khal.it · 06.2026 – Present · https://khal.it

- Designed and shipped a bilingual (DE/EN) portfolio and content platform with
  SvelteKit/Svelte 5, Tailwind v4, and Paraglide i18n.
- Built an SEO content engine: 21 keyword-researched articles with automated
  weekly republishing via GitHub Actions.
- Hardened the delivery pipeline: Gitleaks + Semgrep security scans,
  Lighthouse CI performance budgets, strict CSP; self-hosted on Hetzner with
  Docker/Caddy.

Tags: SvelteKit, Svelte 5, TypeScript, Tailwind CSS, i18n, SEO, Vitest,
Playwright, Lighthouse CI, Docker, Caddy, Hetzner

### Norlin · 07.2026 – Present · https://norlin.ai

- Building an AI-powered lead-outreach SaaS for marketing agencies: ad-funnel
  leads are engaged instantly by an AI conversation flow
  (Mirror → Qualify → Convert).
- Multi-tenant from day one: PostgreSQL row-level security via Prisma Client
  Extensions, queue-driven processing with BullMQ.
- Provider-agnostic LLM layer (Vercel AI SDK, Anthropic/OpenAI) with
  tool-calling; HubSpot OAuth + HMAC-verified webhooks; GDPR-by-design with EU
  data residency.

Tags: AI/LLM, NestJS, SvelteKit, TypeScript, Prisma, PostgreSQL RLS, BullMQ,
Redis, WebSockets, HubSpot, GDPR

## Section 3 — Page-2 sidebar, tagline, mechanics

### Sidebar ("My Projects", page2sidebar-en.tex / page2sidebar-de.tex)

- TessellAI → tessellai.com — tags: Python, FastAPI, React, OpenCV, WebSockets
- khal.it → khal.it — tags: SvelteKit, TypeScript, Tailwind, i18n, SEO
- Norlin → norlin.ai — tags: NestJS, LLM, Multi-tenant SaaS
- Web scrapers — kept exactly as-is (Python, Scrapy, Selenium, Beautiful Soup)
- Removed: ARCore plugin entry, Facebook Analytics plugin entry.

### Mechanics & verification

- All changes applied to both EN and DE `.tex` files; DE content properly
  translated, not copied.
- Compile both PDFs with `pdflatex`; visually inspect every rendered page:
  page breaks around the new Grip King entry and the longer Personal Projects
  section, tag-line wrapping, sidebar alignment. Adjust `\newpage`/`\vspace`
  until clean.
- Stale AWS-era tags (SST, ECS, DynamoDB, Pulumi) are removed with the
  Tessellai refresh.

## Out of scope

- Rotating the exposed Cloudflare API token in the Tessellai repo
  (`.env.production` is tracked in git) — flagged to Khalit separately;
  handled outside this CV work.
- Any restructuring of existing client experience entries.
