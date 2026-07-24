# CV Projects Update Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add the Grip King client engagement and three refreshed personal projects (TessellAI, khal.it, Norlin) to the EN and DE LaTeX CVs, reposition the tagline to Fullstack + AI, delete dead sidebar files, and deliver cleanly formatted PDFs.

**Architecture:** The CV is two independent LaTeX documents (`cv-khalit-hartmann-EN.tex`, `cv-khalit-hartmann-DE.tex`) on the AltaCV class; content changes are applied to both in parallel (DE properly translated). Verification is `pdflatex` compilation plus visual inspection of the rendered PDF pages.

**Tech Stack:** LaTeX (pdflatex, altacv.cls), git.

**Spec:** `docs/superpowers/specs/2026-07-24-cv-projects-design.md` (including the 2026-07-24 sidebar amendment).

**Working directory for all commands:** `/Users/khalithartmann/Desktop/Projects/My Resume`

**Conventions used below:**
- Dates DD.MM.YYYY / MM.YYYY, European style, matching existing entries.
- "Present" (EN) / "Gegenwart" (DE) for ongoing.
- `&` must be escaped as `\&` in LaTeX text. Literal `—` (em dash) is fine — the files already use it.
- Every task ends with a compile check and a commit. Commit only the named `.tex`/`.md` files — never `.aux`/`.log`/`.out`. PDFs are committed once, in Task 8.
- Line numbers refer to the files as of commit `7f7c674`; after each task earlier line numbers shift, so always locate edits by the quoted text, not the line number.

---

### Task 1: EN tagline

**Files:**
- Modify: `cv-khalit-hartmann-EN.tex:73`

- [ ] **Step 1: Replace the tagline**

Find:

```latex
\tagline{Freelance Fullstack Engineer / Flutter Expert}
```

Replace with:

```latex
\tagline{Freelance Fullstack Engineer — Mobile, Web \& AI}
```

- [ ] **Step 2: Compile to verify**

Run: `pdflatex -interaction=nonstopmode cv-khalit-hartmann-EN.tex | tail -3`
Expected: a line starting `Output written on cv-khalit-hartmann-EN.pdf` (no `!` error lines).

- [ ] **Step 3: Commit**

```bash
git add cv-khalit-hartmann-EN.tex
git commit -m "Update EN tagline to Fullstack — Mobile, Web & AI

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>"
```

---

### Task 2: EN — insert Grip King entry, move Brack into fullwidth

Page 1's narrow column fits two experience entries. Grip King goes on top, so Brack moves from the narrow column into the `fullwidth` section that starts page 2 (chronological order is preserved: Grip King 05.2026 > Purelei 12.2025 > Brack 05.2025 > SnapNext 08.2024).

**Files:**
- Modify: `cv-khalit-hartmann-EN.tex:107-189`

- [ ] **Step 1: Insert the Grip King block**

Directly after the line `\cvsection[page1sidebar-en]{Experience}` and before the line `% Protofy GmbH & Co. KG - Purelei`, insert:

```latex
% Grip King
\cvevent{Freelance Fullstack Engineer}{Grip King}{01.05.2026 - Present}{}
\textbf{Personnel responsibility:} —\\
\textbf{Task:} Architecting and building a multi-channel tire \& rim e-commerce platform for the German market, replacing a manual spreadsheet-based retail operation.\\
\textbf{Achievements:}
\begin{itemize}
    \item Designed and built the platform end-to-end: NestJS API, SvelteKit storefront, and admin dashboard in a pnpm monorepo.
    \item Modeled EU tire-label compliance (EU 2020/740 / EPREL), DOT tracking, and ledger-based inventory with immutable stock lots.
    \item Implemented 15-minute stock reservations preventing oversell across eBay, Kleinanzeigen, Tyre24/ALZURA, and the shop's own storefront.
    \item Integrated Billbee OMS (orders, stock, invoicing, DATEV), lexoffice, Stripe, and PayPal.
    \item Set up security-scanned CI/CD (Gitleaks, Semgrep, dependency audit) with auto-deployed staging on Hetzner.
    \item Spec-driven development with 23 ADRs and a full testing pyramid (Vitest, Playwright).
\end{itemize}

\cvtag{E-Commerce}
\cvtag{TypeScript}
\cvtag{NestJS}
\cvtag{SvelteKit}
\cvtag{Svelte 5}
\cvtag{Prisma}
\cvtag{PostgreSQL}
\cvtag{Redis}
\cvtag{BullMQ}
\cvtag{Tailwind CSS}
\cvtag{Stripe}
\cvtag{PayPal}
\cvtag{Billbee}
\cvtag{Docker}
\cvtag{Caddy}
\cvtag{GitHub Actions}
\cvtag{CI/CD}
\cvtag{Sentry}
\cvtag{TDD}

\vspace{2em}

```

- [ ] **Step 2: Move the Brack block after `\begin{fullwidth}`**

Cut the entire Brack block — from the line `% Protofy GmbH & Co. KG - Brack` through its closing `\cvtag{Android}` plus the following `\vspace{2em}` (in the pre-task file this is lines 150–185). The block starts:

```latex
% Protofy GmbH & Co. KG - Brack
\cvevent{Freelance Mobile App Developer}{\href{https://protofy.com/}{Protofy GmbH \& Co. KG}}{01.05.2025 - 01.06.2026}{}
```

and ends:

```latex
\cvtag{iOS}
\cvtag{Android}

\vspace{2em}
```

Paste it (unchanged, followed by a blank line) immediately after the line `\begin{fullwidth}` and before the line `% SnapNext GmbH`. The result around the page break must read:

```latex
\vspace{2em}

\newpage
\begin{fullwidth}
% Protofy GmbH & Co. KG - Brack
```

- [ ] **Step 3: Compile to verify**

Run: `pdflatex -interaction=nonstopmode cv-khalit-hartmann-EN.tex | tail -3`
Expected: `Output written on cv-khalit-hartmann-EN.pdf`. Page count may grow by one — fine; layout is finalized in Task 8.

- [ ] **Step 4: Commit**

```bash
git add cv-khalit-hartmann-EN.tex
git commit -m "Add Grip King engagement to EN CV, move Brack to fullwidth section

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>"
```

---

### Task 3: EN — replace Personal Projects section

**Files:**
- Modify: `cv-khalit-hartmann-EN.tex:534-555` (pre-task numbering)

- [ ] **Step 1: Replace the section**

Delete everything from the line `\cvsection{Personal Projects}` down to and including the line `\cvtag{Pulumi}` (the old Tessellai entry with its `\vspace{1em}` and AWS-era tags). Do NOT delete the following `\cvsection{References}`. Insert in its place:

```latex
\cvsection{Personal Projects}

% TessellAI
\cvevent{\href{https://tessellai.com}{TessellAI}}{Personal Project}{12.2024 - 03.2026}{}
\begin{itemize}
    \item Built a real-time photo-mosaic platform for live events: guests upload photos via QR code and watch them assemble into mosaic art live on screen.
    \item Engineered a computer-vision pipeline (OpenCV, NumPy, Jonker-Volgenant linear-assignment solver) with incremental per-tile updates streamed over WebSockets + Redis Pub/Sub.
    \item Migrated the platform from AWS serverless (SST/TypeScript) to a FastAPI/Python monolith with Celery workers and typed OpenAPI client generation.
\end{itemize}

\cvtag{Python}
\cvtag{FastAPI}
\cvtag{Celery}
\cvtag{Redis}
\cvtag{PostgreSQL}
\cvtag{React}
\cvtag{TypeScript}
\cvtag{PixiJS}
\cvtag{OpenCV}
\cvtag{WebSockets}
\cvtag{Docker}
\cvtag{Vercel}

\vspace{1.5em}

% khal.it
\cvevent{\href{https://khal.it}{khal.it}}{Personal Project}{06.2026 - Present}{}
\begin{itemize}
    \item Designed and shipped a bilingual (DE/EN) portfolio and content platform with SvelteKit/Svelte 5, Tailwind v4, and Paraglide i18n.
    \item Built an SEO content engine: 21 keyword-researched articles with automated weekly republishing via GitHub Actions.
    \item Hardened the delivery pipeline: Gitleaks + Semgrep security scans, Lighthouse CI performance budgets, strict CSP; self-hosted on Hetzner with Docker/Caddy.
\end{itemize}

\cvtag{SvelteKit}
\cvtag{Svelte 5}
\cvtag{TypeScript}
\cvtag{Tailwind CSS}
\cvtag{i18n}
\cvtag{SEO}
\cvtag{Vitest}
\cvtag{Playwright}
\cvtag{Lighthouse CI}
\cvtag{Docker}
\cvtag{Caddy}
\cvtag{Hetzner}

\vspace{1.5em}

% Norlin
\cvevent{\href{https://norlin.ai}{Norlin}}{Personal Project}{07.2026 - Present}{}
\begin{itemize}
    \item Building an AI-powered lead-outreach SaaS for marketing agencies: ad-funnel leads are engaged instantly by an AI conversation flow (Mirror, Qualify, Convert).
    \item Multi-tenant from day one: PostgreSQL row-level security via Prisma Client Extensions, queue-driven processing with BullMQ.
    \item Provider-agnostic LLM layer (Vercel AI SDK, Anthropic/OpenAI) with tool-calling; HubSpot OAuth and HMAC-verified webhooks; GDPR-by-design with EU data residency.
\end{itemize}

\cvtag{AI/LLM}
\cvtag{NestJS}
\cvtag{SvelteKit}
\cvtag{TypeScript}
\cvtag{Prisma}
\cvtag{PostgreSQL RLS}
\cvtag{BullMQ}
\cvtag{Redis}
\cvtag{WebSockets}
\cvtag{HubSpot}
\cvtag{GDPR}

\vspace{1.5em}

% Web Scrapers
\cvevent{Web Scrapers}{Personal Project}{Spare Time}{}
\begin{itemize}
    \item Implemented multiple web scrapers.
\end{itemize}

\cvtag{Python}
\cvtag{Scrapy}
\cvtag{Selenium}
\cvtag{Beautiful Soup}

\vspace{1em}
```

- [ ] **Step 2: Compile to verify**

Run: `pdflatex -interaction=nonstopmode cv-khalit-hartmann-EN.tex | tail -3`
Expected: `Output written on cv-khalit-hartmann-EN.pdf`.

- [ ] **Step 3: Commit**

```bash
git add cv-khalit-hartmann-EN.tex
git commit -m "Refresh EN Personal Projects: TessellAI, khal.it, Norlin, scrapers

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>"
```

---

### Task 4: DE tagline

**Files:**
- Modify: `cv-khalit-hartmann-DE.tex:75`

- [ ] **Step 1: Replace the tagline**

Find:

```latex
\tagline{Freiberuflicher Fullstack Engineer / Flutter Experte}
```

Replace with:

```latex
\tagline{Freiberuflicher Fullstack Engineer — Mobile, Web \& KI}
```

- [ ] **Step 2: Compile to verify**

Run: `pdflatex -interaction=nonstopmode cv-khalit-hartmann-DE.tex | tail -3`
Expected: `Output written on cv-khalit-hartmann-DE.pdf`.

- [ ] **Step 3: Commit**

```bash
git add cv-khalit-hartmann-DE.tex
git commit -m "Update DE tagline to Fullstack — Mobile, Web & KI

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>"
```

---

### Task 5: DE — insert Grip King entry, move Brack into fullwidth

Mirror of Task 2 for the German document.

**Files:**
- Modify: `cv-khalit-hartmann-DE.tex:109-191`

- [ ] **Step 1: Insert the Grip King block**

Directly after the line `\cvsection[page1sidebar-de]{Erfahrung}` and before the line `% Protofy GmbH & Co. KG - Purelei`, insert:

```latex
% Grip King
\cvevent{Freiberuflicher Fullstack Engineer}{Grip King}{01.05.2026 - Gegenwart}{}
\textbf{Personalverantwortung:} —\\
\textbf{Aufgabe:} Architektur und Entwicklung einer Multi-Channel-E-Commerce-Plattform für Reifen \& Felgen für den deutschen Markt, als Ablösung eines manuellen, tabellenbasierten Handelsbetriebs.\\
\textbf{Erfolge:}
\begin{itemize}
    \item Entwurf und Entwicklung der Plattform von Ende zu Ende: NestJS-API, SvelteKit-Storefront und Admin-Dashboard in einem pnpm-Monorepo.
    \item Modellierung von EU-Reifenlabel-Compliance (EU 2020/740 / EPREL), DOT-Tracking und Ledger-basierter Bestandsführung mit unveränderlichen Stock-Lots.
    \item Implementierung von 15-minütigen Bestandsreservierungen gegen Überverkauf über eBay, Kleinanzeigen, Tyre24/ALZURA und den eigenen Shop.
    \item Integration von Billbee OMS (Bestellungen, Bestand, Rechnungen, DATEV), lexoffice, Stripe und PayPal.
    \item Aufbau einer CI/CD-Pipeline mit Security-Scans (Gitleaks, Semgrep, Dependency Audit) und automatischem Staging-Deployment auf Hetzner.
    \item Spezifikationsgetriebene Entwicklung mit 23 ADRs und vollständiger Test-Pyramide (Vitest, Playwright).
\end{itemize}

\cvtag{E-Commerce}
\cvtag{TypeScript}
\cvtag{NestJS}
\cvtag{SvelteKit}
\cvtag{Svelte 5}
\cvtag{Prisma}
\cvtag{PostgreSQL}
\cvtag{Redis}
\cvtag{BullMQ}
\cvtag{Tailwind CSS}
\cvtag{Stripe}
\cvtag{PayPal}
\cvtag{Billbee}
\cvtag{Docker}
\cvtag{Caddy}
\cvtag{GitHub Actions}
\cvtag{CI/CD}
\cvtag{Sentry}
\cvtag{TDD}

\vspace{2em}

```

- [ ] **Step 2: Move the Brack block after `\begin{fullwidth}`**

Cut the entire Brack block — from the line `% Protofy GmbH & Co. KG - Brack` through its closing `\cvtag{Android}` plus the following `\vspace{2em}` (pre-task lines 152–187). The block starts:

```latex
% Protofy GmbH & Co. KG - Brack
\cvevent{Freiberuflicher Mobile App Entwickler}{\href{https://protofy.com/}{Protofy GmbH \& Co. KG}}{01.05.2025 - 01.06.2026}{}
```

Paste it (unchanged, followed by a blank line) immediately after the line `\begin{fullwidth}` and before the line `% SnapNext GmbH`. The result around the page break must read:

```latex
\vspace{2em}

\newpage
\begin{fullwidth}
% Protofy GmbH & Co. KG - Brack
```

- [ ] **Step 3: Compile to verify**

Run: `pdflatex -interaction=nonstopmode cv-khalit-hartmann-DE.tex | tail -3`
Expected: `Output written on cv-khalit-hartmann-DE.pdf`.

- [ ] **Step 4: Commit**

```bash
git add cv-khalit-hartmann-DE.tex
git commit -m "Add Grip King engagement to DE CV, move Brack to fullwidth section

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>"
```

---

### Task 6: DE — replace Persönliche Projekte section

**Files:**
- Modify: `cv-khalit-hartmann-DE.tex:536-557` (pre-task numbering)

- [ ] **Step 1: Replace the section**

Delete everything from the line `\cvsection{Persönliche Projekte}` down to and including the line `\cvtag{Pulumi}`. Do NOT delete the following `\newpage` / `\cvsection{Referenzen}` lines. Insert in its place:

```latex
\cvsection{Persönliche Projekte}

% TessellAI
\cvevent{\href{https://tessellai.com}{TessellAI}}{Eigenes Projekt}{12.2024 - 03.2026}{}
\begin{itemize}
    \item Entwicklung einer Echtzeit-Fotomosaik-Plattform für Live-Events: Gäste laden Fotos per QR-Code hoch und sehen live zu, wie daraus ein Mosaik-Kunstwerk entsteht.
    \item Aufbau einer Computer-Vision-Pipeline (OpenCV, NumPy, Jonker-Volgenant-Assignment-Solver) mit inkrementellen Kachel-Updates über WebSockets + Redis Pub/Sub.
    \item Migration der Plattform von AWS Serverless (SST/TypeScript) zu einem FastAPI/Python-Monolithen mit Celery-Workern und typisierter OpenAPI-Client-Generierung.
\end{itemize}

\cvtag{Python}
\cvtag{FastAPI}
\cvtag{Celery}
\cvtag{Redis}
\cvtag{PostgreSQL}
\cvtag{React}
\cvtag{TypeScript}
\cvtag{PixiJS}
\cvtag{OpenCV}
\cvtag{WebSockets}
\cvtag{Docker}
\cvtag{Vercel}

\vspace{1.5em}

% khal.it
\cvevent{\href{https://khal.it}{khal.it}}{Eigenes Projekt}{06.2026 - Gegenwart}{}
\begin{itemize}
    \item Konzeption und Umsetzung einer zweisprachigen (DE/EN) Portfolio- und Content-Plattform mit SvelteKit/Svelte 5, Tailwind v4 und Paraglide i18n.
    \item Aufbau einer SEO-Content-Engine: 21 Keyword-recherchierte Artikel mit automatisiertem wöchentlichem Republishing über GitHub Actions.
    \item Härtung der Delivery-Pipeline: Gitleaks- und Semgrep-Security-Scans, Lighthouse-CI-Performance-Budgets, strikte CSP; Self-Hosting auf Hetzner mit Docker/Caddy.
\end{itemize}

\cvtag{SvelteKit}
\cvtag{Svelte 5}
\cvtag{TypeScript}
\cvtag{Tailwind CSS}
\cvtag{i18n}
\cvtag{SEO}
\cvtag{Vitest}
\cvtag{Playwright}
\cvtag{Lighthouse CI}
\cvtag{Docker}
\cvtag{Caddy}
\cvtag{Hetzner}

\vspace{1.5em}

% Norlin
\cvevent{\href{https://norlin.ai}{Norlin}}{Eigenes Projekt}{07.2026 - Gegenwart}{}
\begin{itemize}
    \item Entwicklung einer KI-gestützten Lead-Outreach-SaaS für Marketing-Agenturen: Leads aus Ad-Funnels werden sofort in einem KI-Gesprächsflow betreut (Mirror, Qualify, Convert).
    \item Multi-Tenant von Tag eins: PostgreSQL Row-Level Security über Prisma Client Extensions, Queue-basierte Verarbeitung mit BullMQ.
    \item Provider-agnostische LLM-Schicht (Vercel AI SDK, Anthropic/OpenAI) mit Tool-Calling; HubSpot OAuth und HMAC-verifizierte Webhooks; GDPR-by-Design mit EU-Datenresidenz.
\end{itemize}

\cvtag{AI/LLM}
\cvtag{NestJS}
\cvtag{SvelteKit}
\cvtag{TypeScript}
\cvtag{Prisma}
\cvtag{PostgreSQL RLS}
\cvtag{BullMQ}
\cvtag{Redis}
\cvtag{WebSockets}
\cvtag{HubSpot}
\cvtag{GDPR}

\vspace{1.5em}

% Web Scraper
\cvevent{Web Scraper}{Eigenes Projekt}{Freizeit}{}
\begin{itemize}
    \item Implementierung mehrerer Web Scraper.
\end{itemize}

\cvtag{Python}
\cvtag{Scrapy}
\cvtag{Selenium}
\cvtag{Beautiful Soup}

\vspace{1em}
```

- [ ] **Step 2: Compile to verify**

Run: `pdflatex -interaction=nonstopmode cv-khalit-hartmann-DE.tex | tail -3`
Expected: `Output written on cv-khalit-hartmann-DE.pdf`.

- [ ] **Step 3: Commit**

```bash
git add cv-khalit-hartmann-DE.tex
git commit -m "Refresh DE Persönliche Projekte: TessellAI, khal.it, Norlin, Scraper

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>"
```

---

### Task 7: Delete dead sidebar files

`page2sidebar-en.tex` / `page2sidebar-de.tex` are referenced nowhere (verified: no `\cvsection[page2...]` or `\addnextpagesidebar` call in any `.tex`).

**Files:**
- Delete: `page2sidebar-en.tex`
- Delete: `page2sidebar-de.tex`

- [ ] **Step 1: Delete the files**

```bash
git rm page2sidebar-en.tex page2sidebar-de.tex
```

- [ ] **Step 2: Compile both to prove nothing referenced them**

Run: `pdflatex -interaction=nonstopmode cv-khalit-hartmann-EN.tex | tail -3 && pdflatex -interaction=nonstopmode cv-khalit-hartmann-DE.tex | tail -3`
Expected: both end with `Output written on ...` and no `! LaTeX Error: File ... not found`.

- [ ] **Step 3: Commit**

```bash
git commit -m "Remove unused page2 sidebar files

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>"
```

---

### Task 8: Layout verification and final PDFs

The explicit user requirement: the final PDFs must be properly formatted and easily readable — verified by looking at the rendered pages, not just by a clean compile.

**Files:**
- Modify (only if fixes needed): `cv-khalit-hartmann-EN.tex`, `cv-khalit-hartmann-DE.tex`
- Regenerate + commit: `cv-khalit-hartmann-EN.pdf`, `cv-khalit-hartmann-DE.pdf`

- [ ] **Step 1: Compile each document twice** (marginpar/sidebar and cross-references stabilize on the second run)

```bash
pdflatex -interaction=nonstopmode cv-khalit-hartmann-EN.tex >/dev/null && pdflatex -interaction=nonstopmode cv-khalit-hartmann-EN.tex | tail -3
pdflatex -interaction=nonstopmode cv-khalit-hartmann-DE.tex >/dev/null && pdflatex -interaction=nonstopmode cv-khalit-hartmann-DE.tex | tail -3
```

Expected: `Output written on ...` for both. Note the page counts.

- [ ] **Step 2: Visually inspect every page of the EN PDF**

Use the Read tool on `cv-khalit-hartmann-EN.pdf` (pages 1 through the last page). Checklist:

1. Page 1: header + tagline render; Grip King and Purelei both fit in the narrow column; page-1 sidebar (languages/education/skills) aligns beside them; no text runs into the sidebar.
2. Page 2: starts with Brack in full width; no half-empty page between page 1 and Brack.
3. No `\cvsection` heading is orphaned at the very bottom of a page (heading with no content under it).
4. Personal Projects: all four entries (TessellAI, khal.it, Norlin, Web Scrapers) render with title, date line, bullets, tags; no entry is split so its title sits alone at a page bottom.
5. Tags wrap as tidy rounded chips, no overfull lines sticking into the margin (also check the compile log: `grep "Overfull" cv-khalit-hartmann-EN.log` — badness under ~10pt is acceptable, big overfulls are not).
6. References section and the company-logos image still render on the final page.

- [ ] **Step 3: Fix EN layout issues found in Step 2**

Apply the matching contingency, then recompile twice and re-inspect (repeat until the checklist passes):

- Orphaned section heading or split entry → insert `\newpage` before the affected `\cvsection{...}` or `\cvevent{...}`.
- Large awkward gap before a section → remove an existing manual `\newpage` or shrink the preceding `\vspace`.
- Personal Projects overflowing badly mid-entry → add `\newpage` before `\cvsection{Personal Projects}` so the section starts on a fresh page.

- [ ] **Step 4: Repeat Steps 2–3 for the DE PDF**

Same checklist against `cv-khalit-hartmann-DE.pdf` (section names: Erfahrung, Persönliche Projekte, Referenzen). Note: the DE file has a manual `\newpage` before `\cvsection{Referenzen}` — keep it only if References would otherwise start mid-page awkwardly.

- [ ] **Step 5: Commit the .tex fixes and final PDFs**

```bash
git add cv-khalit-hartmann-EN.tex cv-khalit-hartmann-DE.tex cv-khalit-hartmann-EN.pdf cv-khalit-hartmann-DE.pdf
git commit -m "Rebuild CV PDFs with Grip King and refreshed projects

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>"
```

(If Step 3/4 required no `.tex` changes, the commit simply contains the two PDFs.)
