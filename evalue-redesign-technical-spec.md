# eValue.cz — Technical Redesign Specification
**Version 2.5 · Updated June 2026**
*Based on full audit report + iterative prototype sessions (evalue-prototype.html)*

---

## PROTOTYPE vs. PRODUCTION — KEY DISTINCTION

This spec describes **two parallel realities** that must be reconciled before launch:

| Dimension | Prototype (`evalue-prototype.html`) | Production target |
|-----------|--------------------------------------|-------------------|
| Language | English throughout (`lang="en"`) | Czech default, English at `/en/`, German `/de/` phase 2 |
| H1 copy | English marketing headlines | Czech SEO-optimised headlines (see per-page specs) |
| Client logos | Rohlik, Creditas, Alza, Škoda, KB, GitHub, Metronom | Carvago, Finshape, Košík.cz, Nano Energies, Inventi (real portfolio) |
| Case study clients | Rohlik Group, Komerční banka, FireAlarm, Škoda, Bonami, etc. | Finshape, Carvago, Košík.cz, Nano Energies, Protank, UPC (real projects) |
| Case study pattern | Modal overlay (no URL per case) | Individual slug pages `/case-studies/[slug]/` with full 7-section layout |
| Counter values | 150 projects · 40 engineers · **14 years** · 98% NPS (initial values pre-rendered in HTML; JS animates from those values, not from 0) | 15+ let · 120+ klientů · 2M+ uživatelů produktů · 14 dní onboarding |
| Chat implementation | Custom bespoke widget ("Jíťa" avatar, quick replies, Web Audio) | Crisp.chat with live agent + AI fallback (DC-05) |
| Cal.com | Placeholder div only | Live iframe embed on all service pages |
| Cost calculator | Not implemented | DC-07 — Nearshore Cost Calculator (Phase 1) |
| Pricing currency | EUR one-off prices | EUR and CZK (websites tier: 149 CZK/mo) |
| Blog language | English, 9 articles | Czech, 6 articles at launch (see content plan) |

**Everything documented below reflects the production target unless explicitly marked `[BUILT]` (exists in prototype) or `[PLANNED]` (not yet implemented).**

---

## SITE ARCHITECTURE — Page Map

The current site is a single-page "brochure" mixing 4 audience types. The redesign splits into **6 distinct URL paths** with shared nav + footer shell.

```
evalue.cz/                        → Homepage (conversion hub)
evalue.cz/agile-team/             → Dedicated Agile Team (Enterprise / CTO audience)
evalue.cz/body-shopping/          → Body Shopping (HR / staffing managers)
evalue.cz/vyvoj-aplikaci/         → Custom App Development (startups / scaleups)
evalue.cz/weby/                   → Websites & E-shops (SMBs)
evalue.cz/case-studies/           → Case Studies index
evalue.cz/case-studies/[slug]/    → Individual case study
evalue.cz/ai-automatizace/        → AI Automation service (new)
evalue.cz/produkty/               → Own Products (9 SaaS products)
evalue.cz/o-nas/                  → About / Team / Culture
evalue.cz/blog/                   → Blog / Insights
evalue.cz/blog/[slug]/            → Individual article
evalue.cz/kontakt/                → Contact (with segmented forms)
evalue.cz/kariéra/                → Careers (open roles + apply modal)
evalue.cz/deals/                  → Seasonal Promotions hub (Summer Special + upcoming deals)
```

**Languages:** `/` = Czech (default), `/en/` = English, `/de/` = German (phase 2), `/bg/` = Bulgarian (phase 2).
All pages ship with `hreflang` tags for cs / en / de / bg.

**Bulgarian language strategy:** BG launches as machine-translated from CZ/EN (DeepL API or similar). A `data-translated="auto"` attribute on `<html>` triggers a subtle "Auto-translated · Help us improve →" banner so BG visitors understand the context. The Sofia team reviews and replaces machine-translated sections over time, at which point the banner is removed page by page.

---

## PAGE 1 — Homepage (`/`)

### Purpose
Top-of-funnel conversion hub. Routes each visitor type into the correct sub-page within 3 seconds.

### Content Sections (top → bottom)

| # | Section | Prototype state | Production target |
|---|---------|----------------|-------------------|
| 1 | **Sticky Nav** | `[BUILT]` Logo · Services ▾ · Case Studies · Products · 🌴 Summer Deal · **Company ▾** (About / Blog / Contact / **Careers**) · 🔍 Search · **Compact lang switcher** (active lang only, dropdown on click) · Theme toggle · "Let's Talk" CTA. Standalone About/Blog/Contact links moved into Company dropdown. Careers added in v2.4 with work icon and "Join our team" subtitle. Search opens full-screen overlay; supports ⌘K/Ctrl+K. | "Nezávazná konzultace" CTA; CZ default; language switcher functional; Services dropdown marks Agile as "Nejpopulárnější", AI Automation as "Novinka" |
| 2 | **Hero** | `[BUILT]` Two badge pills above headline: "14+ years building software in CEE" + "★ 4.9/5 on Clutch · 38 reviews" (amber pill — Clutch social proof visible above the fold from first paint) · H1: "We build software that grows your business" · Subline · CTAs: **"Book a Free 30-min Call →"** + **"Contact Us"** (navigates to contact page; replaced "See Case Studies" which was low-conversion for the secondary position) · Jan Procházka BDM card removed from hero | H1: "Vývojové týmy, které opravdu dodají váš software" · Availability badge "🟢 Volné kapacity od [date]" (DC-11) · CTAs: "Získat nabídku do 24 h →" + "Prohlédnout reference" · Trust micro-copy: "Bez závazku · NDA standardně · Reakce do 4 hodin" |
| 3 | **Scrolling Logo Strip** | `[BUILT]` CSS marquee with: Carvago, Creditas, Alza, GitHub, Rohlik, Škoda, KB, Metronom | Replace with real portfolio clients: Carvago, Finshape, Košík.cz, Nano Energies, Inventi, Summit |
| 4 | **Stats counters** | `[BUILT]` 4 animated JS counters: 150 projects · 40 engineers · 12 years · 98% NPS. Appear in a dedicated section below logo strip | Update values to: 15+ let · 120+ klientů · 2M+ uživatelů produktů · 14 dní onboarding. Move directly below logo strip. Add `<noscript>` fallback (currently absent) |
| 5 | **4-Path Service Navigator** | `[BUILT]` 4 cards: Agile Team · Body Shopping · Custom App Development · **AI Automation** (not Weby) | Swap AI Automation for Weby in the 4th slot, or use a 5-card layout if Websites is also a significant revenue line. AI Automation should have its own nav entry regardless |
| 6 | **Featured Case Study** | `[NOT BUILT]` Does not exist in prototype | Add full-width highlight card: Finshape — Java + React · −62% load time · +38% login conversion · 14-day onboarding |
| 7 | **Case Study Grid** | `[BUILT]` Filter tabs: All · Fintech · E-commerce · SaaS · AI · IoT. Shows 6 of 15 cases with pagination. Cases are placeholder (Rohlik, KB, FireAlarm) | Filter tabs: Vše / FinTech / E-commerce / SaaS / Energetika. Replace placeholder clients with real portfolio |
| 8 | **Testimonials** | `[BUILT]` 6 testimonials in a 2-slide carousel, 6s auto-play interval. No company logos — only generated avatars. No star ratings. Placeholder clients | Reduce to 3 real testimonials with real name + title + company logo + star rating + year. Carousel interval: 4s (spec DC-08). |
| 9 | **Mid-page CTA strip** | `[BUILT]` "Not sure which model fits?" + "Book 30-min call →" + "Get cost estimate" — both navigate to contact page, no Cal.com embed | Add Cal.com inline embed to the "Book 30-min call" CTA (DC-04) |
| 10 | **Tech Stack carousel** | `[BUILT]` Horizontal scroll carousel with devicon SVG logos: React/Next.js, TypeScript, Node.js, Python, Java/Spring, Go, Docker, K8s, AWS, GCP, PostgreSQL, MongoDB, Redis, Flutter, iOS, Android | Add PHP, .NET, Angular. Add AI/LLM row: LangChain, Semantic Kernel, OpenAI, RAG. Remove or keep Go/Flutter/mobile depending on service focus |
| 11 | **Products teaser** | `[BUILT]` 3 cards: FireAlarm.cloud · EvHard · **PropTrack CZ** (replaced vague "9 more products" placeholder with real product name and description). No "Vyzkoušet zdarma" links | Add "Vyzkoušet zdarma" / "Zjistit více" links. CTA: "Zobrazit všechny produkty →" |
| 12 | **About teaser** | `[BUILT]` 2-column: text (with Praha/Znojmo/Sofia office badge tags) + photo (placeholder Picsum) | Add 3-column layout: HQ Praha · Znojmo Hub · Sofia nearshore. Use real Marešova vila photo |
| 13 | **Footer** | `[BUILT]` **Single shared footer defined once** (`<footer id="site-footer">`) outside all `.page` divs. 4 columns: brand+tagline · Services · Company · Contact (includes WhatsApp link). Company column (v2.4): Case Studies · Products · About Us · Blog · **Contact** · **Summer Deal** · **Careers** — Contact and Summer Deal links added in v2.4; Careers now correctly routes to `showPage('careers')`. Bottom bar: copyright + Privacy + Terms · social icons (LinkedIn · GitHub · X/Twitter · **Facebook**, v2.5) · Clutch 4.9★ + GDPR. | Replace Instagram with Facebook. Add Products links column. Add language switcher in footer bottom bar. Add live Clutch badge embed |

---

## PAGE 2 — Agile Team (`/agile-team/`)

### Purpose
Converts enterprise CTOs and product managers evaluating a nearshore development partner.

### Content Sections

| Section | Status | Notes |
|---------|--------|-------|
| **Hero** H1 "Dedikovaný agile tým — váš vlastní vývojový oddíl" | `[BUILT — EN]` Prototype: "Your dedicated engineering squad, ready in 2 weeks" · CTAs: "Start now →" + "Download service brief" | Czech copy + "Sestavit tým →" CTA for production |
| **Pain → Solution block** 3 pain cards (slow hiring / agency churn / no ownership) | `[BUILT]` Implemented ✓ | — |
| **How it works** 3-step timeline | `[NOT BUILT]` Missing from prototype | Discovery call → Team proposal (72h) → Sprint 1 starts |
| **Team composition visual** roles available (FE, BE, PM, QA, DevOps, AI eng.) | `[PARTIALLY BUILT]` A 10-person "Meet the team" grid exists — but spec places team grid on About page, not Agile | Decide: keep here as "available roles" showcase, or move to About |
| **Pricing transparency block** day rates + T&M vs Fixed Price table | `[BUILT — DIFFERENT]` Prototype has monthly package tiers (€10k/mo · €18k/mo · Custom), not daily blended rates or T&M table | Add daily rate anchors alongside monthly packages. Add T&M vs Fixed Price comparison |
| **Case study** Finshape or Carvago deep-dive | `[NOT BUILT]` | Add Finshape: −62% load time · +38% login · 14-day onboarding |
| **Testimonials** 2 enterprise-specific quotes | `[NOT BUILT]` | — |
| **FAQ accordion** 8–10 questions | `[NOT BUILT]` | NDA, IP, timezone, languages, rate range, team size |
| **Conversion close** Cal.com 30-min embed | `[NOT BUILT]` Placeholder div only | Wire live Cal.com iframe |
| **Agile delivery guarantee** | `[BUILT — UNDOCUMENTED]` "If we miss sprint goals twice in a row, you get the next sprint free." | Keep — strong conversion signal for CTOs burned by waterfall agencies |

---

## PAGE 3 — Hire Engineers / Body Shopping (`/body-shopping/`)

### Purpose
Converts HR managers and technical leads who need 1–3 senior specialists fast.

**Naming note:** The page URL and internal ID remain `body-shopping` for continuity, but all user-facing labels (nav dropdown, nav-card, footer links, page eyebrow) now read **"Hire Engineers"** with "Body Shopping" retained only as a secondary eyebrow subtitle. Rationale: "Body Shopping" is industry jargon understood by procurement/HR buyers but opaque to engineering managers and CTOs. "Hire Engineers" is self-explanatory to all audiences and performs better on direct-intent searches.

### Content Sections

| Section | Status | Notes |
|---------|--------|-------|
| **Hero** H1 "Senior vývojáři do vašeho týmu — do 14 dní" | `[BUILT — EN]` Prototype: "Senior engineers embedded in your team within 72 hours" · CTA: "Find my engineer →". "Browse engineer profiles" secondary CTA **removed in v2.3** (eValue does not maintain a public profile directory — the button set a false expectation) | Note: prototype says 72 hours, spec says 14 days — confirm which SLA is accurate |
| **Stack filter** React / Java / .NET / PHP / Node.js / Python / DevOps / QA / PM / AI Eng. | `[PARTIALLY BUILT]` Prototype has: All stacks · React/Next.js · Python · Java/Spring · DevOps/K8s · Mobile (iOS/Android) | Add: .NET, PHP, QA, PM, AI Eng. filter tabs |
| **Engineer profile cards** (3 named profiles: Jan K., Martin P., Tomáš N.) | `[BUILT — UNDOCUMENTED]` Real-looking anonymized cards with tech tags, availability, and "Request profile" button | Not in original spec — keep as a strong conversion element |
| **"How fast?" block** 3-step process | `[NOT BUILT]` | Role brief → CV shortlist (48h) → Trial week |
| **Pricing block** day-rate ranges by seniority | `[NOT BUILT]` | Junior / Mid / Senior / Lead tiers |
| **Client logos + Finshape quote** | `[NOT BUILT]` | — |
| **Risk-free offer** "Vyzkoušejte 1 vývojáře 2 týdny" | `[NOT BUILT]` | — |
| **FAQ** | `[NOT BUILT]` | timezone, English level, notice period, trial extension, IP ownership |
| **Get matched CTA + Jan Procházka card** | `[BUILT]` Side-by-side flex row ✓ | — |

---

## PAGE 4 — Custom App Development (`/vyvoj-aplikaci/`)

### Purpose
Converts startups and scaleups with a defined product idea who want a Fixed Price or MVP engagement.

### Content Sections

- **Hero:** H1 "Webové a mobilní aplikace na míru — od MVP po enterprise" · Stack breadth · Fixed Price offer · CTA: "Probrat projekt →"
- **Service cards:** Web app / Mobile app / Legacy modernisation / AI-augmented app / Integration / SaaS product build
- **AI Engineering highlight box:** RAG · LangChain · LangGraph · Semantic Kernel — "AI pro firmy, které s AI teprve začínají"
- **Process timeline:** Discovery → Architecture → Sprint delivery → Launch → Hyper-care
- **Case studies:** Carvago MVP / Košík.cz speed refactor / Nano Energies legacy replacement
- **Tech stack detail:** full visual grid by layer (Frontend / Backend / Cloud / AI)
- **Pricing anchor:** "Projekty od €X — odhad do 72 hodin"
- **CTA:** project brief form (Name, Company, Budget range, Stack, Timeline, Describe project)

---

## PAGE 5 — Websites & E-shops (`/weby/`)

### Purpose
SMBs and solo founders. Completely separate audience — should not bleed into enterprise pages.

### Content Sections

- **Hero:** H1 "Moderní web nebo e-shop — rychlý, krásný, měřitelný" · 149 CZK/mo offer · Core Web Vitals A badge · CTA: "Zobrazit balíčky →"
- **Pricing table:** 3 tiers — Starter (149 CZK/mo) / Pro / Custom. Feature comparison grid.
- **Examples gallery:** 6–8 screenshots of built sites (real or placeholder)
- **Performance badges:** PageSpeed score, Core Web Vitals, mobile-first
- **CTA:** short contact form — Name, Email, "Popis webu v 2 větách", Budget

---

## PAGE 6 — Case Studies Index (`/case-studies/`)

### Purpose
SEO landing page + trust accelerator. Every CTO Googles a dev shop's work before calling.

### Content Sections

- **H1:** "Výsledky, které mluví za nás — reálná čísla z reálných projektů"
- **Filter bar:** All / FinTech / E-commerce / Energy / SaaS / Public sector / Nearshore
- **Case study cards grid:** Each card — client logo + industry tag + 1-line headline + 2–3 metric badges + "Číst studii →"
- **Minimum 6 at launch:** Finshape · Carvago · Košík.cz · Nano Energies · Protank · UPC
- **"Similar cases" / "More like this" strip removed** — was `#similar-cases-section` rendered below the grid. Removed in v2.3: adds no conversion value, dilutes the clean paginated grid, and the filter bar already serves the "find related work" need.

### Individual Case Study (`/case-studies/[slug]/`)

Structure per case study:
1. Client logo + tag + headline
2. Metrics bar (3–4 KPIs with numbers)
3. Challenge paragraph
4. Solution paragraph (stack, team size, timeline)
5. Quote from client (name + photo + title)
6. Technical breakdown (optional for CTOs)
7. "Next case study →" + "Discuss a similar project →" CTA

---

## PAGE 6b — AI Automation (`/ai-automatizace/`)

### Purpose
New dedicated service page. Target audience: Czech and DACH mid-market companies who want to automate workflows, integrate AI into existing software, or build AI agents — but don't have an internal AI team.

**Positioning angle:** "AI pro firmy, které s AI teprve začínají" — we do the AI engineering, you get the results. This page directly converts the AI Automation opportunity identified in the audit and roadmap.

### Hero

- H1: "Automatizujte, co vás zdržuje — my postavíme AI za vás"
- Subheadline: Custom AI integrations, agents, RAG systems and workflow automation for Czech and DACH companies. No AI team required on your side.
- Primary CTA: "Získat AI audit zdarma →" (opens Cal.com)
- Secondary CTA: "Prohlédnout AI projekty →" (scrolls to case studies)
- Trust bar: LangChain · LangGraph · RAG · OpenAI · Semantic Kernel · AWS / Azure

### Service Cards (3 tiers — priced to qualify)

| Tier | Name | Description | Price anchor | CTA |
|------|------|-------------|-------------|-----|
| 1 | AI Audit | We assess your current processes and identify what to automate first. Delivered in 2 weeks. | od €3 000 | "Objednat audit" |
| 2 | RAG / AI asistent | We build a knowledge-base AI assistant trained on your data (docs, CRM, support tickets). | od €15 000 | "Probrat projekt" |
| 3 | Agentic systém | Fully autonomous AI agent pipeline integrated into your systems — approvals, routing, reporting. | od €50 000 | "Domluvit schůzku" |

### "What we automate" section

Grid of 6 use-case tiles (real examples, not generic):
- Document processing & extraction (contracts, invoices, reports)
- Customer support AI (chat + email auto-response)
- Internal knowledge assistant (RAG over company docs)
- Lead qualification & routing (AI + CRM integration)
- Workflow approvals & notifications (Slack + email + ERP)
- Data reporting & anomaly detection

### How it works — 3 steps

1. **AI Audit** (2 weeks) — we map your processes, identify highest-ROI automation candidates, deliver prioritised report
2. **Build & integrate** (4–12 weeks) — we build the solution on your existing stack, no rip-and-replace
3. **Hyper-care** (ongoing) — we monitor, retrain, and iterate as your data grows

### Proof section

- Case study: "Ask eValue" RAG assistant (dogfooding — eValue's own AI chatbot as proof of capability)
- Any AI project from client work (Nokia Bell Labs–style case if available)
- Tech logos: LangChain, OpenAI, Mistral, AWS Bedrock, Azure OpenAI, Pinecone, Weaviate

### FAQ

- "Musíme mít vlastní AI tým?" → Ne. Celý vývoj zajišťujeme my.
- "Kde budou naše data?" → EU-only hosting, GDPR compliant.
- "Jak dlouho trvá první výsledek?" → Audit do 2 týdnů, MVP do 6 týdnů.
- "Integrujete se s naším ERP/CRM?" → Ano — SAP, HubSpot, Jira, custom.
- "Co je RAG?" → Short plain-language explanation + link to blog article.

### SEO targets for this page

Primary: `AI automatizace pro firmy`, `AI agent vývoj Czech Republic`
Secondary: `RAG systémy`, `LangChain vývoj`, `AI integrace do firemního softwaru`
EN: `AI automation services Europe`, `custom AI agent development Czech`

---

## PAGE 7 — Products (`/produkty/`)

### Purpose
Showcase 9 own products; seed SaaS revenue funnel; demonstrate product-thinking DNA to enterprise clients.

### Content Sections

- **H1:** "Naše vlastní produkty — budujeme software, který sami používáme"
- **Product grid (9 cards):** Logo + name + 1-sentence description + category tag + "Vyzkoušet / Zjistit více" link
- **Highlighted products (2–3):** Full cards with screenshots, pricing badge, "Start free trial" CTA — priority: FireAlarm + EvHard (recommended for SaaS monetisation)
- **"Built by the same team" bridge block:** cross-sell hook to services pages

---

## PAGE 8 — About (`/o-nas/`)

### Content Sections

- **Mission statement** (single H1 line)
- **Story timeline:** 2015 → now (key milestones)
- **3 locations block:** Praha HQ · Znojmo Hub (Marešova vila photo) · Sofia/Plovdiv nearshore
- **Team grid:** photos + names + roles (as many real photos as available)
- **Values block:** 3–5 principles, short
- **Clients logos full list** (all verified clients)
- **Awards / certifications** (Clutch badge placeholder, ISO placeholder)
- **"Join us" teaser:** 2–3 open roles + "Kariéra →" link — routes to `showPage('careers')` (Careers page, v2.4)

---

## PAGE 9 — Blog (`/blog/`)

### Structure

- **H1:** "Insights z eValue — technologie, teambuilding, byznys"
- **Category filters:** All / Dev / AI & Automation / Nearshoring / Product / Case Studies
- **Card grid:** Thumbnail + category tag + title + 2-line excerpt + author + date + read time
- **Sidebar / featured:** Latest article + newsletter sign-up widget (Beehiiv embed)

### Initial content plan (first 6 articles based on audit keywords)
1. "Nearshore vývoj softwaru: průvodce pro CTO" — targets `nearshore vývoj softwaru`
2. "Body shopping vs. outsourcing — co je levnější?" — targets `body shopping Praha`
3. "Jak zkrátit onboarding vývojáře na 14 dní" — expertise + SEO
4. "RAG a LangChain pro firmy bez AI týmu" — targets `AI/RAG pro firmy`
5. "Carvago: jak jsme postavili MVP do Series A" — case study as blog post
6. "Czech dev rates 2026: co platí DACH firmy" — DACH lead magnet

---

## PAGE 10 — Contact (`/kontakt/`)

### Content Sections

- **H1:** "Začněme spolu — odpovíme do 4 hodin"
- **Layout:** 2-column grid. Left column: form. Right column: BD person card + live chat card + WhatsApp card (sticky on scroll).
- **Form structure:** Single `contact-form` wrapper. First field: **"How can we help you?" `<select>` dropdown** (replaces the previous 4-button tab bar — cleaner, less visual noise, universally understood). Options: Start a project / Hire an engineer / AI Automation / Book a call. Selecting an option swaps the fields below it without reloading.
  - Start a project: Name, Company & role, Email, Project brief, optional promo code
  - Hire an engineer: Name & company, Email, Tech stack, Seniority, Start date
  - AI Automation: Name & role, Email, Company size, "What would you like to achieve?" (open textarea — not AI-specific framing since eValue offers more than AI services)
  - Book a call: Cal.com embed placeholder
- **Promo code field** (Start a project): optional text input + "Apply" button. Valid codes auto-populate an amber banner. Codes: `SUMMER26`, `AUTUMN26`, `AIAUDIT26`.
- **Named BDM card** (right column, sticky): Jan Procházka — gradient header with avatar, name, role, email, phone, "Book a Free 30-min Call" button. See DC-16.
- **"Send brief" CTA button:** solid accent blue, no gradient animation, `justify-content:center` — centred text+icon when full-width.
- **Offices section** (full-width, below the form/BD grid): 3-column grid — Prague HQ · Znojmo Dev Centre · Sofia AI & ML. Each card: real city photo (Unsplash) as background with dark gradient overlay · small flag pin (flagcdn.com) in top-left corner · role badge top-right · city name + country overlaid · live local time (JS, updates every 30s) · timezone label · address · contact info · "Open in Maps" link · hover lift animation. See DC-20.

---

## PAGE 10b — Careers (`/kariéra/`)

### Purpose
Attract and qualify engineering candidates across all three offices. Secondary purpose: humanise the brand for enterprise clients who evaluate a potential partner's team culture and hiring standards.

### Content Sections (prototype state `[BUILT]`)

| Section | Status | Notes |
|---------|--------|-------|
| **Hero** — H1 "Build real things with people who care about craft" + eyebrow "Careers at evalue.cz" + subline about offices | `[BUILT — EN]` | Czech: "Budujte skutečné věci s lidmi, pro které řemeslo znamená vše" |
| **Why join us** — 6-card grid (Greenfield projects / Ownership not tickets / Small teams / Remote-friendly / Learning budget €800/yr / Competitive pay) | `[BUILT]` | Real perks, not vague "great culture" copy |
| **Open roles list** — 4 role cards (Senior Backend Node/Python · AI/ML Engineer · React/TypeScript FE · DevOps/Platform) each with location, type, department, and Apply button | `[BUILT]` | Apply button calls `prefillRole(role)` — pre-fills the modal and opens it |
| **Speculative application row** — dashed-border card "Don't see your role?" with "Get in touch" button | `[BUILT]` | Calls `prefillRole('Speculative — Open Application')` |

### Apply Modal (`#careersModalOverlay`) — `[BUILT]` — DC-21

The apply form lives in a modal overlay, not inline on the page. This eliminates the UX anti-pattern of having CTAs + a form simultaneously visible.

- **Trigger:** Any `prefillRole(role)` call. If already on `page-careers`, modal opens instantly. If navigating from another page, `showPage('careers')` fires first, modal opens after 350ms delay (allows page transition + scroll to settle).
- **Modal class:** `.case-modal-overlay` (same overlay pattern as case study modal — `position:fixed;inset:0;z-index:10000;backdrop-filter:blur(8px)`). Shown/hidden via `.open` class toggle — **not** `display:none/flex` inline style. (Lesson from v2.4: using the wrong class name `.case-modal__overlay` caused the modal to render in document flow with no fixed positioning, blocking scroll with no way to close.)
- **Title:** Dynamically set by `prefillRole()` — "Apply for: [Role Name]" or "Get in touch" for speculative.
- **Form fields:** Full name · Email · LinkedIn/portfolio URL · CV link (Google Drive, Dropbox, etc.) · Tell us about yourself (textarea)
- **"What happens next" strip:** Full-bleed footer inside the modal — negative margin `-32px` on each side to bleed to modal edges, matching the modal's 32px padding. No border. Background `rgba(79,126,255,.06)` with `border-top`. Content: "We reply within 3 business days → 30-min call (no puzzles) → Paid take-home task (~3h) → Offer within 1 week."
- **Close:** Backdrop click (`e.target === overlay`) or Escape key (wired to global `keydown` handler alongside `closeCaseModal`). Close button calls `closeCareersModal()` with no args.
- **Submit:** `submitCareersForm()` — validates name + email (alert if missing), closes modal, resets all fields.

### Production targets

- Replace English copy with Czech
- Wire form to HubSpot CRM (careers pipeline) or email to `careers@evalue.cz`
- Add real open roles from current hiring pipeline
- Add company culture photos (Znojmo vila, team at work)
- SEO: H1 "Práce v eValue.cz — přidejte se k týmu", meta description targeting `práce IT Praha`, `backend developer Praha`, `AI engineer Česká republika`

---

## PAGE 10c — Seasonal Promotions (`/deals/`)

### Purpose
Dedicated hub for time-limited offers that converts fence-sitters into leads with urgency, social proof, and a frictionless claim flow. Secondary purpose: email list growth via "Notify me" for upcoming deals.

### Why a separate promotions page exists

Embedding promotions inside service pages dilutes the primary conversion message — a CTO evaluating a nearshore team should not be distracted by "Summer Special" messaging. A dedicated `/deals/` page lets:

- Search traffic find "eValue special offer" or "software development discount" directly
- Marketing campaigns link to one clean URL
- The offer live and die without touching service page copy
- A/B testing the offer independently of the core funnel

### Hero Section

Full-bleed image with strong atmospheric framing (beach, ocean, seasonal backdrop). The visual contrast with standard B2B software agency pages is intentional — it creates a pattern interrupt that stops scroll and earns attention. The offer headline "You chill. We work." communicates the value proposition in 4 words and lands the emotional hook: you get to relax because we handle the complexity.

**Key hero elements:**
- Animated countdown timer (days / hours / mins / secs to offer expiry) — scarcity is a proven conversion driver; the ticking clock communicates that inaction has a cost
- Offer badge with icon (e.g. "Limited Summer Offer · Ends Aug 31")
- Primary CTA: "Claim the offer" — orange (`#f97316`) not the standard gradient button. Orange is used deliberately: it matches the seasonal warmth and is visually distinct from every other CTA on the site, reducing confusion about which button is the main deal action
- Trust line: "Only 4 spots remaining this summer" — real scarcity, not manufactured. If this is accurate, it must stay. If not, remove it — false scarcity damages trust more than no scarcity
- Optional ambient audio toggle (ocean sound) — positioned centrally with wave-pulse animation so it's discoverable; off by default (autoplay audio violates browser policies and user expectations); the waves animation functions as a teaser that invites interaction

### Deals Carousel

Horizontal scrollable carousel showing all deals across time: active, upcoming (Soon), date-windowed, and finished (greyed out). This multi-state carousel serves three purposes:

1. **Transparency** — showing past finished deals proves eValue runs recurring promotions, building anticipation for future ones
2. **Email capture** — "Notify me" on upcoming deals captures emails from high-intent visitors who are not ready to buy today but want to be first when the deal opens
3. **SEO** — each deal card contains its own structured text (title, date, description) that contributes to page content density for seasonal keyword searches

**Card states:**
- `Active` — orange accent, "Claim now" CTA, full-colour image
- `Soon` — indigo badge, "Notify me" flow (shows email input inline), full-colour image
- `Dated` — teal badge with date range, "Notify me", full-colour image
- `Finished` — greyscale image, disabled "Expired" button, still shows "Read all details" for reference

**Why deal cards must not clip shadows/outlines:**
The selected card state uses CSS `outline` (not `border` or `box-shadow`) because `overflow:hidden` on the card clips `box-shadow`. `outline` renders outside the element's layout bounds and is unaffected by `overflow:hidden`. Removing `overflow:hidden` from the card itself and moving it to the image container only preserves the border-radius on the photo while allowing the selection indicator to display correctly on all sides including the left edge.

### Deal Details Modal

Each card includes a "Read all details" text link (underlined, low-visual-weight so it doesn't compete with the primary CTA). Clicking it opens a modal overlay with:

- Deal header: emoji icon + title + date/status line
- Highlighted summary panel (blue-tinted gradient box) — the one-sentence pitch in slightly larger type
- Full terms and conditions list with icon indicators: green `check_circle` for qualifying conditions, amber `warning` for restrictions, grey `info` for fine print
- Footer CTA: "Claim this offer" (active deals) · "Got it" (upcoming) · "Close" (expired)

**Why a modal instead of an expanded card or a new page:** The user is mid-scroll in the carousel. A modal maintains context (they can see the page behind it through the blur), feels lightweight (no navigation), and can be dismissed instantly. A new page would break the browsing flow and increase the navigation cost of getting back to the carousel.

**Why separate terms matter:** Showing full conditions on the card is not feasible (cards are 220px wide). Hiding conditions entirely damages trust — a visitor who claims an offer and later discovers a 3-month minimum they didn't expect feels deceived. The "Read all details" link gives every visitor access to full transparency at zero friction cost.

### Subscribe Strip

Warm amber-tinted strip between the carousel and the footer. Captures emails from visitors who are interested but not ready to act. Used to:
- Build a promotional email list separate from the main newsletter
- Notify subscribers when a new deal goes live
- Measure interest in future deal types before running them

---

## DYNAMIC COMPONENTS SPECIFICATION

### Technical SEO (all pages)

| Element | Spec |
|---------|------|
| `<title>` | 55–60 chars, keyword-first + brand. **Prototype (v2.2):** `"Custom Software Development Prague \| evalue.cz — Czech Dev Partner"` `[BUILT]` |
| `<meta description>` | 150–160 chars, includes primary keyword + CTA phrase. **Prototype (v2.2):** `"Czech software development company with 14+ years in CEE. Custom apps, dedicated engineering teams, and AI automation for European businesses. 150+ projects delivered, 98% client satisfaction."` `[BUILT]` |
| `canonical` | Self-referencing. **Prototype:** `<link rel="canonical" href="https://evalue.cz/">` `[BUILT]` |
| `og:title / og:description` | Open Graph tags for social sharing `[BUILT]` |
| H1 | Exactly 1 per page, contains primary target keyword |
| H2–H4 | Semantic hierarchy, supporting keywords in H2s |
| `hreflang` | `cs`, `en`, `de` on every page |
| `canonical` | Self-referencing canonical on every page |
| Structured data | `Organization` (all pages) · `Service` (service pages) · `Review` + `AggregateRating` (homepage, about) · `BlogPosting` (blog) · `FAQPage` (FAQ sections) · `BreadcrumbList` (all inner pages) |
| Sitemap | XML sitemap auto-generated, submitted to GSC |
| Robots.txt | Allow all, disallow /admin, /staging |
| Core Web Vitals | LCP < 2.5s · FID < 100ms · CLS < 0.1 |
| Image format | WebP with lazy loading; `alt` text on every image |
| Internal linking | Each page links to at least 2 other pages (service cross-links, blog → case studies) |
| Google My Business | Verified for Praha + Znojmo (separate listings) |

### Primary keyword targets per page

| Page | Primary keyword | Secondary |
|------|----------------|-----------|
| Homepage | `vývoj softwaru na míru Praha` | `software development company Czech Republic` |
| Agile Team | `nearshore agile team Czech Republic` | `dedikovaný vývojový tým` |
| Body Shopping | `body shopping Praha` · `body shopping IT specialisté` | `hire developers Czech Republic` |
| Custom Dev | `vývoj aplikací na míru` · `React vývojář Praha` | `AI engineering services Europe` |
| Weby | `tvorba webových stránek Praha` | `weby na míru SEO` |
| Case Studies | `případové studie vývoj softwaru` | `nearshore development results` |
| Blog (cornerstone) | `nearshore vývoj softwaru průvodce` | `Czech developer rates 2026` |
| About | `eValue.cz Praha softwarová firma` | `software development company Prague` |

### Backlink targets (first 90 days)

- Clutch.co profile (+ 5 reviews minimum at launch)
- GoodFirms profile
- Zdroják.cz guest post
- Lupa.cz press mention
- IT directories: Firmáreň, Firmy.cz, HiHoneyBee
- LinkedIn articles linking back to case studies

---

## DYNAMIC COMPONENTS SPECIFICATION

### DC-01 — Animated Trust Counters
**Location:** Homepage (dedicated stats section below logo strip), About page
**Status:** `[BUILT]` — values and animation fixed
**Behaviour:** On scroll-into-view, count up from final value to final value (no jarring zero-start flash) over 1.5s using easing. IntersectionObserver + requestAnimationFrame. `data-target` and `data-suffix` attributes drive the animation; initial HTML content pre-renders the final value so users without JS or with slow connections see correct numbers immediately.

| | Prototype (current) | Production target |
|-|--------------------|--------------------|
| Counter 1 | **150** Projects delivered | 15+ let zkušeností |
| Counter 2 | **40+** Senior engineers | 120+ spokojených klientů |
| Counter 3 | **14** Years of experience (corrected from 12) | 2 000 000+ uživatelů produktů |
| Counter 4 | **98%** client satisfaction (NPS) | 14 dní průměrný onboarding |

**SEO note:** `<noscript>` fallback block with static counter values `[BUILT]` — added in v2.2. All four numbers render as plain HTML for crawlers and non-JS users.

---

### DC-02 — 4-Path Service Navigator
**Location:** Homepage, sticky nav dropdown
**Behaviour:** 4 cards in a 2×2 grid (desktop) / vertical stack (mobile). Hover state: card lifts with box-shadow, arrow icon translates right. Click → routes to dedicated service page.
**Variants:** Homepage (full cards) · Nav mega-menu (compact 4-column layout)

---

### DC-03 — Case Study Filter Tabs
**Location:** Homepage (preview grid), `/case-studies/` index
**Status:** `[BUILT — PARTIAL]` Client-side filtering implemented ✓. Pagination (15 cases, 6 per page) implemented ✓ (undocumented). URL hash update `[NOT BUILT]` — filter clicks do not update `window.location.hash`.
**Behaviour:** Client-side JS filter. Tab click → re-renders cards matching category. No page reload. **Add for production:** URL hash update (`/case-studies/#fintech`) so filtered views are shareable.
**Categories (prototype):** All · Fintech · E-commerce · SaaS · AI · IoT · Healthcare · Manufacturing
**Categories (production target):** Vše / FinTech / E-commerce / SaaS / Energetika / Nearshore
**Tech:** Vanilla JS (implemented)

---

### DC-04 — Cal.com Booking Widget
**Location:** All service pages (inline embed), Homepage mid-page strip, `/kontakt/`
**Status:** `[NOT BUILT]` All Cal.com placements are placeholder divs with `calendar_month` icon and the text "In production this loads the Cal.com embed." No actual iframe exists anywhere in the prototype.
**Behaviour (production):** Inline iframe embed of Cal.com 30-min slot. Matches site colors via Cal.com theme API (dark/light toggle support). On mobile: opens as bottom sheet.
**Fallback:** If Cal.com unavailable, shows a simple "send us email" link.
**Action required:** Confirm Cal.com account, get embed script, replace all placeholder divs.

---

### DC-05 — Live Chat Widget
**Location:** All pages (bottom-right corner)
**Status:** `[BUILT — CUSTOM]` Prototype implements a fully custom bespoke chat widget, NOT Crisp. The custom widget includes: "Jíťa" named avatar, quick-reply chip buttons ("Start a project / Hire a developer / AI automation / Pricing"), keyword-matched bot responses, `addChatBubble()` / `addTyping()` JS engine. It intentionally showcases eValue's own AI capability.

**Production decision required:** Keep the custom widget (demonstrates AI competence, brand-consistent) or replace with Crisp (live agent handoff, real business hours logic, CRM integration).

**If Crisp (production):** Crisp JavaScript snippet in `<head>`. During business hours (Mon–Fri 9:00–18:00 CET): live agent. After hours: AI bot trained on services + FAQ + case studies. Chat icon shows availability status indicator (green dot = live, grey = bot).

**If custom widget:** Wire keyword responses to a real backend (LangChain RAG over eValue docs). Add live-agent handoff when keyword = "speak to human".

**Auto-open after 5 minutes:** `[BUILT]` — see DC-14.

**New client popup (promo card):** `[BUILT]` — see DC-15. Fires after **90 seconds** (changed from 2 minutes). Offer reframed from "Save 10% on your first project" to **"Free Discovery Workshop — €2,500 value"**. Rationale: a discount on an unknown price feels vague and potentially cheapens the brand. A free workshop with a named € value is a concrete deliverable the visitor can evaluate — it removes price risk without implying eValue is discounting its rates.

---

### DC-06 — "Ask eValue" AI Assistant (Phase 2)
**Location:** Floating button on `/agile-team/`, `/body-shopping/`, `/vyvoj-aplikaci/` pages
**Behaviour:** RAG chatbot trained on: case studies, service descriptions, FAQ, pricing ranges. Answers questions like "how fast can you onboard a React team?" or "do you do fixed price?". Qualifies lead, ends with "Want me to book you a call?" → triggers DC-04.
**Tech:** LangChain RAG on eValue's own infra (showcase of internal AI capability). OpenAI or Mistral backend.

---

### DC-07 — Nearshore Cost Calculator
**Location:** Standalone section on `/agile-team/` + `/blog/` lead magnet
**Status:** `[NOT BUILT]` Not implemented anywhere in the prototype. Confirmed as must-have (see Competitor Features section).
**Behaviour:** Multi-step form — Step 1: Team size (slider 1–20). Step 2: Role mix (checkboxes). Step 3: Duration (dropdown). Output: monthly cost range (€X–€Y) vs estimated in-house cost. "Get exact quote" → email capture form.
**Why this is the highest-priority missing feature:** Boldare's estimator is their #1 lead magnet. A visitor who self-qualifies via a calculator is already invested before they hit the contact form — they know the rough budget. Conversion rates on pre-qualified leads from calculators are 3–5× higher than cold contact form submissions.
**Tech:** Vanilla JS. No backend required — all calculation is client-side with hardcoded rate ranges.

---

### DC-08 — Testimonial Carousel
**Location:** Homepage, service pages
**Status:** `[BUILT — VALUES DIFFER]`
**Behaviour:** Auto-play, pause on hover. Each slide: quote text + author name + company.
**Prototype vs. target:**

| | Prototype | Production target |
|-|-----------|-------------------|
| Auto-play interval | 6 seconds (`setInterval 6000`) | 4 seconds |
| Slides | 2 slides × 3 testimonials per slide (6 total) | 3 testimonials, 1 per slide |
| Author photos | **Real Unsplash portraits (v2.5)** — `ui-avatars.com` initials used as `onerror` fallback only | Replace with actual client photos |
| Company logos | Absent (no company logo next to testimonials) | Real company logos (Carvago, Finshape, UPC) |
| Star ratings | Absent | Include + Schema `AggregateRating` markup |
| Year | Absent | Include (e.g. "— CTO, Carvago · 2024") |
| Clients | Rohlik, Alza, Metronom, KB, Bonami, Woltair (placeholder) | Carvago / Finshape / UPC (real testimonials) |

**Tech:** CSS transitions + minimal JS (no jQuery). Touch/swipe on mobile.

---

### DC-09 — Blog / Article Engine
**Location:** `/blog/` + `/blog/[slug]/`
**Status:** `[BUILT — PROTOTYPE ONLY]` Fully functional in the prototype as a client-side JS engine. Not production-ready for SEO (content is JS-rendered, not static HTML).
**What's built:**
- `BLOG_CARDS` array (9 articles in English, placeholder content)
- Category filter tabs: All · AI & Automation · Engineering · Product · Company
- Pagination: 6 posts per page with `blogGoTo()` engine `[UNDOCUMENTED]`
- Blog detail page: table of contents sidebar, share buttons (LinkedIn/Twitter/copy-link) `[UNDOCUMENTED]`, reading progress bar ✓, author bio block `[UNDOCUMENTED]`, related posts grid `[UNDOCUMENTED]`
- `showPage()` monkey-patched by blog engine (fragile chain — fix before production)

**What's missing:**
- Newsletter CTA inject after paragraph 3 `[NOT BUILT]`
- Read time calculated from word count (currently hardcoded in data object) `[PARTIAL]`
- Beehiiv newsletter sidebar widget `[NOT BUILT]`

**Production target:** SSG via Next.js or Astro — each article pre-rendered to static HTML. 6 Czech articles at launch (see content plan). Blog sidebar: latest article + Beehiiv newsletter sign-up.
**Tech:** Must output clean HTML for SEO. JS-rendered blog = delayed indexing = lost organic traffic.

---

### DC-10 — Language Switcher
**Location:** Sticky nav (all pages)
**Status:** `[BUILT — COMPACT UI]` Redesigned in v2.3. Now shows only the active language (e.g. "EN ▾") as a single compact button. Clicking toggles a small dropdown showing all 4 options (EN / CZ / DE / BG). Selecting an option updates the displayed label via `setLang(code)`. Click-outside closes the dropdown. Design matches the nav's card/border design system.
**Behaviour (current prototype):** UI-only — switching language does not route to translated pages yet. Purely visual state change.
**Production behaviour:** Selecting a language routes to the equivalent translated page (`/agile-team/` → `/en/agile-team/`). Sets `lang` attribute on `<html>`.
**SEO:** Each language version needs its own canonical + hreflang set pointing to all 4 variants. Without this, multi-language pages get penalised as duplicate content.

---

### DC-11 — Availability Badge
**Location:** Hero section (homepage + service pages)
**Behaviour:** Displays "🟢 Volné kapacity od [month year]" or "🟡 Kapacity obsazeny — přihlaste se na čekatele". Value comes from a CMS field or simple JSON config file updated manually each month. No API needed.

---

### DC-12 — Clutch / GoodFirms Badge Widget
**Location:** **Hero section (above the fold — amber pill badge "★ 4.9/5 on Clutch · 38 reviews")**, footer bottom bar. Removed from testimonials section (was below "Words from our partners" — redundant with hero placement).
**Rationale for moving to hero:** Clutch rating is the strongest third-party trust signal eValue has. Showing it above the fold in the first screen — before any scrolling — means 100% of visitors see it, not just those who scroll past the testimonials section. The amber pill contrasts with the blue "14+ years" pill while remaining visually consistent through the design system's badge component.
**Behaviour:** Static badge in hero (no embed needed for the pill). Footer text "Clutch 4.9★ · GDPR compliant" in the bottom bar. About page: add live Clutch embed script when available.

---

### DC-13 — Seasonal Promotions System
**Location:** `/deals/` page
**Components:**
- Hero with countdown timer (pure JS, `setInterval` 1s tick targeting expiry date `2026-08-31T23:59:59`)
- Carousel with `selectDeal(index)` function: crossfades hero image, swaps badge/title/subtitle/CTA, shows/hides countdown and sound button per active deal, applies greyscale overlay for finished deals
- Deal Details Modal (`openDealModal(index)`) driven by a `DEAL_DETAILS` JS object — one entry per deal with `icon`, `title`, `sub`, `highlight`, `terms[]`, and `cta`. Modal HTML is a single reusable shell; JS populates it on open
- Subscribe strip with inline email form
**Behaviour:** Clicking any deal card calls `selectDeal(index)` which updates the hero section above to reflect that deal's content. The selected card gets an `outline` highlight (not `box-shadow` — see carousel clipping note in page spec above). Non-summer deals hide the ambient sound button (music is contextually tied to the summer/beach aesthetic only).
**Scalability:** Adding a new deal requires: (1) adding one object to the `DEALS` JS array for the hero sync, (2) adding one object to `DEAL_DETAILS` for the modal, (3) adding one card HTML block to the carousel. No structural changes needed.

**Current deal states (v2.2):**
| # | Deal | State | Notes |
|---|------|-------|-------|
| 0 | Summer Sprint '26 | Active | Countdown to Aug 31, 2026 |
| 1 | Autumn Scale Pack | Soon | Starts Sep 1, 2026 |
| 2 | AI Audit Pack | Soon | Oct 1 – Nov 30, 2026 |
| 3 | Black Friday '25 | **Finished** | Restored (was briefly removed). Shows as greyed-out expired card — proves recurring promotional calendar, builds anticipation |
| 4 | Holiday Sprint | Soon | Starts Dec 1, 2026 |

**Why the expired Black Friday card stays visible:** A carousel showing only active/upcoming deals looks thin. The expired card signals to visitors "we run promotions regularly — come back" and creates FOMO for the next deal. It also answers the implicit question "have you done this before?" with a concrete "yes, last November."

---

### DC-14 — Engagement-Triggered Chat Auto-Open
**Location:** All pages (chat widget)
**Behaviour:** After 5 continuous minutes on the site, the chat panel opens automatically if the visitor has not already opened it manually. On open, a proactive bot message appears: "Hey! You've been exploring for a while. Can I help you find the right service or connect you with our team?" — followed by quick-reply chips: "Start a project / Hire a developer / Pricing."
**Why 5 minutes:** Visitors who remain on the site for 5+ minutes are highly engaged — they are reading, comparing, considering. This is peak intent. A proactive message at this moment mirrors what a salesperson would do in a physical showroom. Earlier (e.g. 30 seconds) would feel intrusive on a first visit; later would miss the window.
**Implementation:** `setTimeout` of 300,000ms. If the visitor opens chat manually before the timer fires, `clearTimeout` cancels the auto-open to prevent a double-trigger. The override wrapper pattern ensures `toggleChat` is only wrapped once regardless of how many times the page is re-rendered.
**Note:** The auto-open respects the visitor's intent — if they have already opened and closed the chat, the `autoOpened` flag prevents a second trigger. Auto-open fires once per session.

---

### DC-15 — Promo Code & Discount Redemption Flow
**Location:** Contact page — "Start a project" tab
**Behaviour:** Optional promo code input field with inline "Apply" button. On submit, the code is validated client-side against `VALID_PROMOS` object. Valid code: shows an amber highlighted banner above the form with the deal name + discount description. Invalid code: input border flashes red for 2s, placeholder resets to "e.g. SUMMER26". The banner has a × dismiss button that clears both the visual banner and the input field.
**Deep-link integration:** `claimDeal(code, label, desc)` — called from any "Claim the offer" button across the site — navigates to the contact page, switches to the "Start a project" tab, waits 120ms for the DOM transition to complete, calls `applyPromo()` to pre-fill the code and show the banner, then scrolls the banner into view.
**Current valid codes:**
| Code | Deal | Discount |
|------|------|----------|
| `SUMMER26` | Summer Sprint '26 | 1 week of development free (Jul–Aug 2026) |
| `AUTUMN26` | Autumn Scale Pack | 10% off team daily rate |
| `AIAUDIT26` | AI Audit Pack | Free 2-day AI audit (€2,500 value) |
**Why client-side validation:** At the prototype stage, server-side validation would require a backend. Client-side validation is sufficient for a demo/prototype — in production, the form submission handler should re-validate server-side before applying any discount. The client-side check improves UX (instant feedback) but should not be the sole gate.

---

### DC-17 — WhatsApp Contact Channel
**Location:** Contact page sidebar (card block matching the "Live chat" card style), all page footers (footer link in "Contact" column)
**Status:** `[BUILT]`
**Behaviour:** Opens `https://wa.me/420123456789` with a pre-filled message. Renders as a blue-tinted card (matching the live chat card design language — same `rgba(79,126,255,.08)` background + `rgba(79,126,255,.25)` border) in the contact sidebar. In footers: plain `footer__link` style link with small monochrome WhatsApp icon.
**Why not green:** Using WhatsApp brand green (#25d366) for the entire card background breaks the site's blue-accent design system. The icon itself stays green (brand recognition), but the card inherits the same tint as every other info block on the page.
**Production:** Replace placeholder number with real WhatsApp Business number.

---

### DC-18 — Tech Stack Clickable Navigation
**Location:** Homepage tech stack carousel
**Status:** `[BUILT]`
**Behaviour:** Each tech item (`cursor:pointer`) routes to the most relevant page on click:
- Frontend/backend/mobile stacks (React, TypeScript, Node.js, Python, Java, Go, Flutter, Swift, Android) → `showPage('body')` (Hire Engineers)
- Infrastructure/cloud/database stacks (Docker, Kubernetes, AWS, GCP, PostgreSQL, MongoDB, Redis) → `showPage('cases')` (Case Studies)
**Rationale:** A visitor clicking a tech logo has a clear intent — either "I want someone who knows this stack" (hire) or "show me what you built with it" (proof). The two-path routing covers both. `title` attribute on each item explains the destination on hover.

---

### DC-19 — Site Search Overlay
**Location:** Nav right area (🔍 icon button, all pages)
**Status:** `[BUILT]`
**Behaviour:** Clicking the search icon (or pressing ⌘K / Ctrl+K) opens a full-screen backdrop blur overlay with a centred search input. Typing filters a `SEARCH_PAGES` array of **13 pages** (label + description). If the query matches exactly one page, `showPage()` navigates there and the overlay closes automatically. Escape or clicking outside closes without navigating. The overlay uses `pointer-events:none` when closed (no interaction cost when idle).
**Current SEARCH_PAGES entries (v2.4):** Home · Agile Team · Hire Engineers · Custom App Dev · Websites · AI Automation · Case Studies · Products · About · Blog · Contact · **Careers** · Summer Deal.
**Production:** Replace client-side array filtering with a real full-text search (Algolia or native browser search API). Index blog articles, case study titles and descriptions, service page content. Add keyboard navigation (↑↓ to select results, Enter to navigate).

---

### DC-20 — Office Location Cards
**Location:** Contact page — full-width section below the form/BD grid
**Status:** `[BUILT]`
**Behaviour:** 3-column grid of `.office-card` components. Each card:
- **Header (190px):** Real city photo (Unsplash) as CSS `background-image` with `background-size:cover`. Dark gradient overlay (`linear-gradient to top, rgba(5,8,20,.92) → transparent`) ensures text is always readable regardless of photo. Small flag image (`flagcdn.com/w40/[cc].png`) pinned top-left as a corner identifier. Role badge top-right. City name (26px bold) + country name overlaid at bottom.
- **Live local time:** `js-time` span updated every 30s via `updateOfficeTimes()` using `Date.toLocaleTimeString()` with the office's IANA timezone (`Europe/Prague` for Prague + Znojmo, `Europe/Sofia` for Sofia). Shows HH:MM format. Timezone label (CET / EET) displayed alongside.
- **Body:** Address · contact info (email + phone + specialty — **all three cards now consistent, v2.4**) · "Open in Maps" external link.
- **Hover:** `translateY(-4px)` lift + enhanced box-shadow.

**Contact info consistency (v2.4):** All three cards follow the same structure: email row → phone row → specialty/team row. Previously Prague had email+phone, Znojmo had phone only, Sofia had specialty only — fixed in v2.4. Sofia phone uses Bulgarian prefix (+359).

**Photo sources (prototype):** Unsplash CDN — Prague Old Town (`photo-1541849546-216549ae216d`), European town square / Znojmo (`photo-1555881400-74d7acaacd8b`, updated v2.4), Alexander Nevsky Cathedral Sofia (`photo-1558618666-fcd25c85cd64`). **Replace with owned/licensed photography before production.**

---

### DC-21 — Careers Apply Modal
**Location:** `#careersModalOverlay` — global (rendered outside `.page` divs like footer and nav)
**Status:** `[BUILT]`
**Trigger:** `prefillRole(role)` called from role card Apply buttons and speculative "Get in touch" button on the Careers page.
**Behaviour:**
- If visitor is already on `page-careers`: modal opens instantly.
- If visitor is on another page: `showPage('careers')` navigates first, then a 350ms `setTimeout` fires `openApplyModal()` after scroll animation settles.
- Modal title is dynamically set: "Apply for: [Role Name]" for specific roles, "Get in touch" for speculative.
- Overlay shown/hidden via `.open` class on `.case-modal-overlay` element (same pattern as case study modal DC-03). Never uses `display:none/flex` inline — this caused a critical bug in early implementation where the modal rendered in-document with no backdrop, blocking page scroll with no dismiss affordance.
- Close paths: (1) backdrop click — `closeCareersModal(event)` with guard `e.target === overlay`; (2) Escape key — wired into existing global `keydown` handler; (3) close button — `closeCareersModal()` with no args (guard skipped when `e` is undefined).
- Submit: `submitCareersForm()` — validates name + email, closes modal, resets all 5 form fields.
- "What happens next" info strip: full-bleed inside modal, `margin: 0 -32px`, `border-top` only (no surrounding border), `border-radius:0 0 var(--radius-lg) var(--radius-lg)` to follow modal's bottom corners. Positioned below the Submit button.

---

### DC-16 — Named BDM Conversion Card
**Location:** Contact page (alongside the form), Body Shopping engineers page, Agile Team page footer
**Content:** Jan Procházka avatar image · name · role (BDM) · email · "Usually replies in 40m"
**Why "Usually replies in 40m" not "Replies in 4h":** The 40-minute claim is more specific and therefore more credible. "4 hours" is a standard B2B reply promise that buyers have learned to discount. "40 minutes" implies it has been measured, is taken seriously, and differentiates eValue from every competitor. The word "Usually" adds honesty — it prevents the claim from feeling like marketing copy and makes it feel like a real person's commitment.
**Why a named person, not a generic form CTA:** Anonymity kills trust in B2B. A named face with a specific response commitment converts significantly better than "Submit your enquiry." The visitor is not filling in a form into the void — they are contacting Jan specifically. This is borrowed from Moravio (confirmed high-conversion pattern) and reinforced by research showing that adding a real face to a CTA can lift conversion rates by 30–40%.

---

## COMPETITOR FEATURES TO BORROW

Reviewed: STRV · Boldare · Netguru · Ackee · Moravio · Altamira (site down)

### 🔴 High Priority — implement from day 1

**Named person on contact CTA** *(from Moravio)*
Instead of a generic "Contact Us" form, show a real photo + name + title + direct email + phone. "Jakub Bílý, Head of BDM — odpovíme do 8 hodin." This is the single highest-trust conversion move on a B2B site. Apply to: Contact page, AI Automation page, service page footers.

**App / Project Cost Estimator widget** *(from Boldare)*
Boldare's "Get estimate" calculator is their #1 lead magnet. Multi-step: project type → team size → duration → output: cost range + "Get exact quote" (email capture). For eValue: "Kolik stojí váš projekt?" — calculates nearshore vs in-house for body shopping, or ballpark for custom dev. Maps directly to DC-07 in the spec but now confirmed as a must-have, not optional.

**Role-tagged case study cards** *(from STRV)*
Each project card shows the disciplines involved: PM · DESIGN · FE · BE · QA · iOS etc. as small chips. This lets CTOs immediately assess team capability and scope. Apply to all case study cards.

**Service tier navigation by company size** *(from Moravio)*
Secondary nav under Services: "For Startups / For Medium Businesses / For Enterprise." Helps self-routing visitors jump straight to the right offer. Add as a secondary filter on the Services mega-menu.

**"★ TOP" / "New" service labels** *(from Moravio)*
Moravio marks their best-seller service as "★ TOP" and new services as "New" directly in the nav. Apply: mark "Nearshore Agile Team" as "★ Nejpopulárnější" and "AI Automatizace" as "Novinka" in the nav dropdown.

**Testimonial-first hero** *(from Boldare)*
Boldare opens with a client quote rather than a headline. Option: make eValue's hero alternate between the headline and a featured quote from Carvago or Finshape CTO on a 6s loop. The quote is the strongest conversion signal available.

---

### 🟡 Medium Priority — phase 2

**Numbered competitive advantages with expand/collapse** *(from Boldare)*
Boldare lists 6 advantages numbered 1–6. Each is 2 lines visible + "Read more" expand. Clean, scannable. Apply on About page and Agile Team page: "6 reasons eValue teams deliver differently."

**Named BDM face on contact + "respond in X hours" SLA** *(from Moravio)*
"Odpovíme do 4 hodin" is already in the spec. Moravio makes it more credible by attaching it to a specific named person with photo. Add: founder or BDM photo + name + "Garantuji odpověď do 4 hodin."

**NPS score as trust metric** *(from Netguru)*
Netguru displays their NPS score (73) prominently in the hero stats. Once eValue collects NPS data, add as a 5th stat counter alongside the existing 4.

**"Working increment every 2 weeks" agile guarantee** *(from Boldare)*
A concrete delivery promise: "Working increment every sprint — change course any time." Converts CTOs who've been burned by waterfall agencies. Add to Agile Team page and hero micro-copy.

**Community newsletter** *(from Boldare, Netguru)*
Boldare has "Learn from Agile Product Builders — monthly digest." Netguru has "Next in Commerce" newsletter. eValue's equivalent: "eValue Insights — jednou měsíčně, co jsme se naučili." Beehiiv embed in blog sidebar + footer.

**Podcast / "Made in eValue" series** *(from Netguru, STRV)*
Netguru has a commerce podcast. STRV has a Substack. Content = credibility = organic leads. Already in the SEO roadmap — confirm as a Phase 2 deliverable.

---

### 🟢 Nice to have — phase 3

**Hero showreel video** *(from STRV, Netguru, Moravio)*
All 3 use a looping background video in the hero. Moravio uses real team photos cycling as a slideshow. STRV uses a product reel. For eValue: a 30-second clip of Marešova vila in Znojmo + team at work would humanise the brand more than any copy. Keep muted, autoplay, with a pause button.

**Pronunciation / brand moment** *(from Boldare)*
Boldare opens with "Bold•dare / bəʊldeə(r) / noun." A small creative moment that makes visitors pause. For eValue: could display "eValue / iː-ˈvæl.juː / přídavné jméno" as a subtle Easter egg or brand statement on the About page.

**Country flags on case study cards** *(from Boldare)*
Each case study card shows the client's country flag. Makes geographic reach immediately visible without text. Add to case study cards for DACH/UK/US clients.

**"8 business hours" form response SLA badge** *(from Moravio)*
Small text under the contact form submit button: "Odpovíme do 4 hodin v pracovní době." Already in spec, but make it a visual badge, not plain text.

---

## DESIGN SYSTEM NOTES

### CSS Custom Properties (tokens)

| Token | Value | Usage |
|-------|-------|-------|
| `--accent` | `#4f7eff` | Primary blue — links, active states, focus rings |
| `--card` | surface variant | Card backgrounds |
| `--border` | subtle line color | All borders |
| `--surface` | slightly elevated bg | Input backgrounds, secondary surfaces |
| `--text-muted` | dark `#8e99b8` / light `#64748b` | Supporting text, labels — readable. Dark value updated v2.3 (was `#7a85a3`, contrast ~4.6:1 — marginal). New value ~5.5:1. |
| `--text-faint` | dark `#5e6d94` / light `#cbd5e1` | Tertiary info (addresses, timestamps). Dark value updated v2.3 (was `#3d4a6b`, contrast ~1.9:1 on card bg — nearly invisible). New value ~3.5:1. Never use for body copy. |
| `--radius` | `12px` | Buttons, inputs, small cards |
| `--radius-lg` | `20px` | Large cards, modals, hero panels |
| `--transition` | `0.2s ease` | All hover/state transitions |

### Button System

All buttons share a single `.btn` base class: `display:inline-flex; align-items:center; justify-content:center; gap:8px; padding:12px 24px; font-size:15px; font-weight:600`. **No variant may override these** — this ensures uniform button height and centred content across the entire interface. `justify-content:center` added in v2.3 — previously full-width buttons (`width:100%`) left-aligned their text+icon because `inline-flex` defaults to `flex-start`.

| Class | Use case | Animation |
|-------|----------|-----------|
| `.btn--primary` | Top-of-funnel CTAs (hero, mid-page) | `gradientShift` 4s infinite — signals "click me first" |
| `.btn--outline` | Secondary actions, filter chips | None — calmer, subordinate |
| `.btn--ghost` | Tertiary, nav-level actions | None |
| `.btn--summer-cta` | Summer deal actions only | `translateY` on hover only — seasonal warmth, orange `#f97316` |
| `.btn--cta-footer` | Footer CTA | `gradientShift` same as primary |

**Why the "Send brief" form button is solid blue with no animation:** The visitor who reaches the contact form has already decided to engage. The animated gradient is a "look at me" signal designed for visitors still browsing. At the form stage, visual noise increases cognitive load and can interrupt the submitting momentum. Solid `var(--accent)` communicates "press this, you're done."

### Spacing System

The global CSS reset zeroes all margins (`*{margin:0;padding:0}`). All spacing is therefore explicit and rule-driven:

| Context | CSS rule | Value |
|---------|----------|-------|
| Eyebrow label → H2/H1 | `.eyebrow + h2, .eyebrow + h1` | `margin-top: 8px` |
| H2 → filter bar | `.cases__filters` | `margin-top: 24px` |
| H2 → card grid | `.skeleton-grid` | `margin-top: 32px` |
| H2 → subtitle paragraph | `h2 + p` | `margin-top: 16px` |
| Filter bar bottom gap | `.cases__filters` | `margin-bottom: 40px` |
| Section padding | `.section` | `padding: 96px 0` (64px mobile) |

Using CSS sibling selectors (`.eyebrow + h2`) rather than inline `margin-top` on every element means new sections automatically inherit correct spacing without the developer needing to remember the convention.

### Icon System

All icons use **Material Symbols Rounded** (Google variable icon font). Usage: `<span class="material-symbols-rounded">icon_name</span>`. Replaces all Unicode-glyph icons (✕, ♪, ☒) used in earlier drafts. Unicode glyphs render inconsistently across OS/browser; Material Symbols render identically everywhere and can be sized/coloured via CSS.

### Gradient Headline

Inner-page `<h1>` headings use a `.highlight` span with animated gradient text (`background-clip:text; animation:gradientShift`). The same `gradientShift` keyframe is reused by `.btn--primary` and `.btn--cta-footer` — one animation definition, three visual uses, zero extra CSS weight.

### Footer Architecture (DRY)

The footer HTML is **defined exactly once** as `<footer id="site-footer">` placed in the DOM after all `.page` divs and before the first `<script>` tag. Since all pages use `display:none` except the active one, the single footer always renders below whichever page is currently visible — no JavaScript injection, no template engine, no duplication.

**Before v2.2:** 13 identical footer blocks, one embedded inside each `.page` div. Any change (e.g. adding a WhatsApp link, fixing a copyright line) had to be applied 13 times, inevitably diverging.

**After v2.2:** One footer. Change it once, it updates everywhere.

This pattern applies to the nav too — the `<nav>` is already defined once outside all `.page` divs. Footer now follows the same principle.

### Badge / Tag System

Three badge variants exist. Do not mix them:

| Class | Shape | Color | Usage |
|-------|-------|-------|-------|
| `.tag` | Pill (`border-radius:20px`) | Blue fill (`rgba(79,126,255,.12)`) | Feature highlights, flagship labels (Products page "★ Flagship") |
| `.case-card__tag` | Pill (`border-radius:20px`) | Blue (`var(--accent)`, `rgba(79,126,255,.08)` bg) | Case card category AND role tags — **unified in v2.3**. Previously: category was grey rect, role was blue rect — two-tone inconsistency. Now both use the same blue pill, differentiated only by semantic position (first tag = category, second = role). |
| `.case-card__tag` inside `.case-modal__tags` | Pill | White translucent (`rgba(255,255,255,.15)`) | Auto-applied via CSS descendant selector when tags appear on the dark modal hero overlay. No inline styles needed in JS. |
| `.hero__badge` | Pill | Context-dependent (blue or amber) | Hero trust badges only |
| `.office-card__badge` | Pill | White translucent on dark photo | Office role labels (HQ / Dev Centre / AI & ML) |

**Rule:** All pill-shaped tags use `border-radius:20px`. No rectangular tags (`border-radius:6px`) remain in the UI.

### Button Arrow Alignment

All `.btn` variants use `display:inline-flex; align-items:center; gap:8px` from the base class, ensuring icon and label are always vertically centred. The `.case-card__read-more` button (custom style, not inheriting from `.btn`) was missing `display:inline-flex; align-items:center` — this caused the `arrow_forward` Material Symbol to sit on the text baseline rather than being vertically centred. Fixed in v2.2 by adding those properties to the `.case-card__read-more` rule. **Rule:** any button-like element that contains a Material Symbol icon must declare `display:inline-flex; align-items:center` or extend `.btn`.

### General

| Token | Spec |
|-------|------|
| Typography | Body font system-safe or `font-display: swap` |
| Grid | Max-width 1200px, 24px gutters. Mobile breakpoint 768px, compact 640px |
| Card design | `border:1px solid var(--border)` + shadow on hover. `border-radius: var(--radius-lg)`. `padding:32px`. Flex column with `> button:last-child { margin-top:auto }` pushes CTA to card bottom |
| Illustrations | Avoid stock "developers at laptops". Prefer: real team photos (Znojmo vila), client logos, screenshot mockups, contextual Unsplash |
| Dark mode | Fully implemented via `[data-theme="dark"]` on `<html>`. All colors are CSS custom properties. Dark `.btn--outline` uses `border-color:rgba(255,255,255,.22)` |
| Animation | Subtle fade-in on scroll (IntersectionObserver). Ambient audio off by default, user-initiated only. Performance budget: no transition > 200ms |

---

## WHY A CMS?

Short answer: **without a CMS, every content update requires a developer and a code deployment**. For a site that needs to grow through content (blog, case studies, availability badge) that's a bottleneck that kills momentum.

Here's the concrete case for eValue specifically:

### What changes frequently on this site

| Content | How often | Who should update it |
|---------|-----------|----------------------|
| Blog articles | 2× per week (SEO plan target) | Writer / marketer — not a dev |
| Case studies | Monthly (as projects close) | PM or account manager |
| Availability badge ("Volné kapacity od…") | Monthly | Whoever manages sales pipeline |
| Client logos + testimonials | Quarterly | Marketing |
| Team member photos / bios | As people join/leave | HR / founder |
| Service descriptions, pricing anchors | Occasionally | Founder / sales |

If every one of these requires a pull request + deployment, they simply won't get done at pace. The SEO content plan calls for **2 blog posts per week** — that's 8+ deploys per month just for content. That's not sustainable.

### What a CMS actually gives you

A CMS (like Sanity.io) adds a simple browser-based editor — think Notion but connected to your website. The developer builds the site once with the CMS wired in. After that:

- A writer logs into the CMS, writes a blog post, clicks Publish — the article goes live in ~30 seconds with no dev involved.
- The founder updates the availability badge (one field, one click) every month.
- A PM uploads a new case study with metrics, photos, client quote — structured form, no HTML knowledge needed.
- The site stays SEO-healthy because content is consistent, structured, and properly tagged — the CMS enforces the schema (H1, meta description, tags, hreflang) via form fields.

### Why Sanity specifically

- **Free tier** covers eValue's needs at launch (up to 3 users, unlimited content)
- Outputs clean JSON that Next.js or Astro fetches at build time → pages are still static HTML for SEO and speed (best of both worlds)
- Supports **multilingual content** natively — one entry, 4 language versions (CZ/EN/DE/BG) side by side
- eValue's own dev team can customise it — it's code-first, not locked to a vendor UI
- Alternative if you want zero monthly cost: **Markdown files in Git** — writers edit `.md` files directly in GitHub. Slightly more technical but completely free.

### The alternative (no CMS)

Hardcode all content in HTML/JSX files. Fine for a 5-page brochure site that never changes. Not fine for a site targeting 10× organic traffic growth via content. You'd be paying a developer to copy-paste blog posts into code files.

**Verdict:** CMS is not a nice-to-have — it's the infrastructure that makes the SEO and content plan executable by non-devs.

---

## TECH STACK RECOMMENDATION

| Layer | Recommendation | Rationale |
|-------|---------------|-----------|
| Framework | **Next.js** (SSG + ISR) or **Astro** | SEO-first, fast builds, easy blog/CMS integration |
| CMS | **Sanity.io** or Markdown files | Non-dev content updates (blog, case studies, availability badge) |
| Hosting | **Vercel** or **Netlify** | CDN-native, free SSL, easy deploys |
| Analytics | GA4 + Microsoft Clarity | Free, GDPR-compliant with Cookiebot |
| Chat | **Crisp** (free/€25/mo) | Live + AI fallback, integrates with HubSpot |
| Booking | **Cal.com** (~€15/mo) | Self-serve discovery calls |
| CRM | **HubSpot Starter** (€20/mo) | Form submissions → CRM pipeline |
| Email capture | **Beehiiv** or HubSpot | Newsletter for blog |
| Cookie consent | **Cookiebot** or **Iubenda** (€9–14/mo) | GDPR required before analytics |

---

## UNDOCUMENTED FEATURES IN PROTOTYPE

These components exist in `evalue-prototype.html` but were not in the original spec. Each needs a decision: keep, improve, or remove before production.

| Feature | Location | Decision needed |
|---------|----------|----------------|
| **Cursor glow** — `#cursor-glow` div that follows mouse with radial gradient | Global (all pages) | Keep as premium feel signal, or remove for performance |
| **Card tilt** — `.tilt` class 3D perspective transform on hover | Service cards, case cards | Keep (depth illusion) or remove (accessibility — can cause vestibular issues) |
| **New client discount popup** `#promo-card` — "🎁 New client offer — Save 10% on your first project", appears after 2 min or 30s on pricing section, has session-storage dismiss | Global | **Important:** This is undocumented marketing copy that makes a public discount commitment. Confirm: (a) is 10% accurate? (b) should this popup appear across all service pages, or only on Pricing/Websites? (c) add to DC-15 as a second promo channel |
| **Case studies pagination** — 15 cases in `ALL_CASES` array, 6 per page, `casesGoTo()` with ellipsis page buttons | Case Studies page | Extend spec DC-03 to document pagination. Update case data with real portfolio |
| **Case study modal** — `#case-modal-overlay` with hero image, 3-metric strip, challenge/solution text, CTAs | All pages with case cards | **Critical mismatch with spec.** Spec calls for individual slug pages (`/case-studies/finshape/`). Modal is fast/lightweight but not SEO-indexable. Decision: keep modal as a preview + add "Read full case study" link to slug page, or go modal-only and use Server-Side Rendering for meta tags |
| **"Similar cases" strip** — `#similar-cases-section`, JS renders tag-matched cases after a case modal is opened | Case Studies page | Good UX. Document in DC-03. |
| **Blog table of contents** — `.blog-toc` sidebar widget auto-generated from article H2s | Blog detail page | Keep — improves long-form article UX |
| **Blog share buttons** — `.blog-share` with LinkedIn/Twitter/copy-link | Blog detail page | Keep. Add Facebook share for CZ market |
| **Blog author bio** — `.blog-author` block below article body | Blog detail page | Keep. Wire to real author data |
| **Blog related posts grid** — 3 tag-matched articles below author bio | Blog detail page | Keep. Wire to real article data |
| **`showPage()` monkey-patch chain** — cases pagination wraps `showPage` once, blog engine wraps it again | Global JS | **Technical debt.** Before production, consolidate into a single router function. The double-wrap is fragile and makes debugging difficult |
| **Web Audio API ocean sound** — synthesized with `AudioContext`, `createBuffer`, `createBiquadFilter`, LFO oscillator; no audio file | Summer/Deals page | Keep (impressive, no HTTP cost, no audio CDN needed). Document in DC-13 |
| **Products page ARR figures** — "€120k ARR", "€80k ARR" on product cards | Products page | **Decision required:** Are these real figures? Displaying revenue publicly is unusual. If real, confirm. If placeholder, remove before launch |
| **Footer copyright year inconsistency** — Homepage footer: © 2024, all inner-page footers: © 2025 | Footer | Fix to © 2026 across all pages before launch |
| **Blog `showPage()` double-wrap** | Global JS | Fix — see monkey-patch chain above |

---

## CHANGELOG

### v2.5 (June 2026) — Real avatars, Facebook social

**Avatars:**
- All `ui-avatars.com` generated initials replaced with real Unsplash portrait photos across: 6 testimonial cards, Jan Procházka BDM card (hero + Agile Team + Hire Engineers + Contact pages), 3 engineer profile cards (Jan K., Martin P., Tomáš N.). All images use `?w=…&h=…&fit=crop&crop=faces` for consistent face-centred crops.
- `onerror` fallback on every avatar `<img>` pointing back to the original `ui-avatars.com` URL — nothing breaks if Unsplash is unavailable.
- `object-fit:cover` added to all avatar `<img>` elements to prevent distortion on non-square source crops.

**Footer:**
- Facebook icon added to social row (LinkedIn · GitHub · X/Twitter · **Facebook**). Links to `https://facebook.com/evalue.cz`.

---

### v2.4 (June 2026) — Careers page, footer fixes, office card consistency

**New page — Careers (`page-careers`):**
- Full careers page added: inner hero · 6-card "Why join us" grid · 4 open role listings (Senior Backend, AI/ML, React FE, DevOps) · speculative application option.
- Apply buttons do not scroll to an inline form — they open a modal overlay (`#careersModalOverlay`, DC-21). This eliminates the anti-pattern of having both a CTA and a visible form simultaneously on the same page.
- Apply modal uses `.case-modal-overlay .open` pattern (same as case study modal). Early implementation used `.case-modal__overlay` (wrong class, no CSS definition) causing the modal to render in document flow and block scroll with no dismiss — fixed by switching to the correct class name.
- `prefillRole(role)` handles two states: immediate open if already on careers page, 350ms delayed open if navigating from another page.
- Escape key added to global `keydown` handler for modal close.

**Navigation:**
- Careers added to Company ▾ dropdown (work icon · "Join our team" subtitle).

**Footer:**
- Contact link added to Company column (`showPage('contact')`).
- Summer Deal link added to Company column (`showPage('summer')`).
- Careers link corrected: was `showPage('contact')` (wrong) → now `showPage('careers')`.

**Search (DC-19):**
- SEARCH_PAGES expanded from 12 to 13 entries — Careers added.

**Office cards (DC-20):**
- All three office cards now have consistent contact structure: email → phone → specialty. Previously Prague had email+phone, Znojmo had phone only, Sofia had specialty only — now uniform.
- Sofia phone number added (+359 XXX XXX XXX).
- Znojmo Unsplash photo updated to a more fitting European town square image.

---

### v2.3 (June 2026) — Contact page overhaul, nav expansion, design system refinements

**Navigation:**
- Company dropdown added (About / Blog / Contact grouped under one ▾ trigger). Standalone About/Blog/Contact nav links removed — nav is now: Services ▾ · Case Studies · Products · 🌴 Summer Deal · Company ▾
- Language switcher redesigned: compact single button showing active language only ("EN ▾"), opens dropdown on click, closes on outside click. Previously showed all 4 languages simultaneously.
- Site search added (DC-19): 🔍 icon in nav right, full-screen overlay, ⌘K/Ctrl+K shortcut, auto-navigate on single match.
- "Contact" standalone nav link added (accessible from both nav and Company dropdown).

**Hero:**
- Secondary CTA changed: "See Case Studies" → "Contact Us". Rationale: "See Case Studies" is a passive browsing action; "Contact Us" is a conversion action that mirrors the primary CTA's intent.

**Contact page — full restructure (DC-20 added):**
- Layout changed from 2-col (form left / offices+chat right) to: 2-col (form left / BD person card right) + full-width offices section below.
- Form navigation changed from 4-button tab bar to a single `<select>` dropdown ("How can we help you?") as the first form field. Tabs were confusing navigation inside a form; a select is the standard web pattern for "choose a path."
- AI Automation form field renamed from "Start your AI journey" to "What would you like to achieve?" — eValue offers more than AI services; the generic framing applies to all engagement types.
- Jan Procházka BD card: now a prominent visual card (gradient header, avatar, email, phone, Calendly CTA) in the right column, sticky on scroll.
- Offices section: 3-column grid of `.office-card` components with real Unsplash city photos, flag pin (flagcdn.com), live local time (DC-20), timezone labels, "Open in Maps" links, hover lift animation.

**Case Studies page:**
- "Similar cases / More like this" strip (`#similar-cases-section`) removed. It added no conversion value — visitors who want to find related work already have the filter bar.

**Hire Engineers page:**
- "Browse engineer profiles" button removed. eValue does not maintain a public profile directory; the button created an expectation that couldn't be fulfilled.

**Design system:**
- `--text-faint` (dark): `#3d4a6b` → `#5e6d94`. Previous value had ~1.9:1 contrast against card background — nearly invisible. New value: ~3.5:1.
- `--text-muted` (dark): `#7a85a3` → `#8e99b8`. Improved from ~4.6:1 to ~5.5:1 contrast.
- `.btn` base: `justify-content:center` added. Fixes left-aligned text+icon in full-width buttons.
- Badge system unified: `.case-card__tag` changed from grey rectangular (`border-radius:6px`) to blue pill (`border-radius:20px`). The grey/blue two-tone inconsistency within each case card is resolved — both category and role tags now use the same blue pill style.

---

### v2.1 (June 2026) — Full prototype audit and reconciliation

Compared `evalue-prototype.html` line-by-line against the v2.0 spec. 84 gaps identified (28 undocumented prototype features, 56 spec↔prototype mismatches). Major updates in this version:
- Added "PROTOTYPE vs. PRODUCTION" distinction table at top of document
- Updated all homepage section descriptions with `[BUILT]` / `[NOT BUILT]` / `[BUILT — DIFFERENT]` status
- Updated Agile Team and Body Shopping page specs with table format showing built vs. target
- Fixed DC-01 counter values table (prototype vs. production)
- Updated DC-05 to reflect custom bespoke chat widget (not Crisp), with production decision required
- Updated DC-08 with exact testimonial mismatch details (6s interval, placeholder clients, no logos)
- Added `[NOT BUILT]` status to DC-04 (Cal.com), DC-07 (calculator), DC-10 (language switcher)
- Added `[BUILT — PARTIAL]` status to DC-03, DC-09 (blog engine)
- Added full "Undocumented Features" section documenting 14 prototype components not in prior spec
- Added "Production Readiness Checklist" with 20 items across content, technical, and decisions
- Expanded Open Questions from 10 to 13

### v2.0 (June 2026) — Prototype iteration updates

**New pages / features:**
- `/deals/` Seasonal Promotions page (DC-13): beach hero, countdown timer, deal cards carousel (active/soon/dated/finished states), deal details modal with full T&C, subscribe strip, ambient ocean sound toggle
- Promo code & discount redemption flow (DC-15): `SUMMER26 / AUTUMN26 / AIAUDIT26` codes pre-fill contact form via `claimDeal()` deep-link
- Engagement-triggered chat auto-open (DC-14): opens after 5 minutes idle with proactive message + quick-reply chips

**Navigation:**
- Summer deals nav link (palm tree emoji) between Products and About
- Services dropdown rebuilt as JS-driven with 220ms hide delay — fixes the "menu disappears before I can click it" UX issue caused by the gap between trigger element and menu
- "AI Automation" tagged as "Novinka" and "Agile Team" as "Nejpopulárnější" in nav

**Contact form:**
- Segmented tabs: Start a project / Hire an engineer / AI Automation / Book a call
- Optional promo code field on "Start a project" tab
- "Send brief" button: solid accent blue, no gradient animation (rationale in button system section)
- Named BDM card: Jan Procházka · "Usually replies in 40m"

**Design system updates:**
- Button system unified: all variants inherit `padding:12px 24px; font-size:15px` from `.btn` base — no variant overrides (fixes historical button height inconsistency)
- Spacing system formalised: CSS sibling selectors replace ad-hoc inline margin-top values
- Icon system: all Unicode glyphs replaced with Material Symbols Rounded
- `.highlight` gradient headline applied to all inner-page `<h1>` titles
- Dark mode `.btn--outline` border color fixed: `rgba(255,255,255,.22)`
- Deal card `overflow:hidden` moved from card root to image container only — fixes outline/shadow clipping on selected state

**Footer:**
- Social icons added: LinkedIn, GitHub, Twitter/X, Facebook
- Social icon hover: removed blue border artefact, now uses `var(--text)` color + `var(--surface)` background
- Products `<h4>` heading styling fixed (was unstyled due to CSS selector mismatch)

---

## PRODUCTION READINESS CHECKLIST

Before going live, every item below must be resolved:

### Content & Copy
- [ ] Translate all English copy to Czech (default language)
- [ ] Replace all placeholder clients (Rohlik, Creditas, etc.) with real portfolio clients
- [ ] Replace all placeholder testimonials with real named quotes from Carvago, Finshape, UPC
- [ ] Update counter values: 15+ years · 120+ clients · 2M+ users · 14-day onboarding
- [ ] Write 6 launch blog articles in Czech (content plan above)
- [ ] Fix copyright year to © 2026 on all pages
- [ ] Confirm "Usually replies in 40m" SLA is accurate — this is a public commitment
- [ ] Confirm whether displaying ARR figures on Products page is intentional

### Technical
- [ ] Wire Cal.com iframe embeds on all service pages + contact page (replace placeholder divs)
- [ ] Add `<noscript>` fallback for all animated counters
- [ ] Implement URL hash update on case study filter clicks
- [ ] Add `hreflang` tags for CZ/EN/DE on all pages
- [ ] Fix language switcher — add real routing (currently decorative)
- [ ] Consolidate `showPage()` monkey-patch chain into a single router
- [ ] Fix border-color reset on invalid promo code (use `var(--border)` not empty string)
- [ ] Decide: individual case study slug pages (SEO) vs modal-only (prototype). Recommend: both — modal as preview, slug page as full article
- [ ] Add individual case study slug pages with full 7-section layout for SEO
- [ ] Switch blog from client-side JS-rendered to SSG (Next.js/Astro) for SEO

### Decisions Required
- [ ] Custom chat widget vs. Crisp (affects CRM integration and live agent handoff)
- [ ] Confirm 10% new client discount popup — is this the right offer? Which pages should it appear on?
- [ ] Language scope at launch: CZ only or CZ+EN?
- [ ] Phase 1 scope: full site or homepage + 4 service pages first?
- [ ] Promo codes configurable via CMS or hardcoded (current: hardcoded in JS `VALID_PROMOS` object)

---

## OPEN QUESTIONS FOR SERHII TO CONFIRM

1. **Dark or light theme?** Prototype is dark — keep or offer a light mode toggle?
2. **CMS needed?** Should non-devs update blog posts, case studies, and availability badge?
3. **Languages at launch:** CZ only, CZ+EN, or CZ+EN+DE+BG?
4. **Real team photos** available for About page, hero humanisation, and BDM card (Jan Procházka)?
5. **Cal.com / booking tool** — existing account or start fresh?
6. **Which 6 case studies to write first?** Finshape + Carvago confirmed — which 4 others?
7. **Framework preference:** Next.js / Astro / plain HTML+JS?
8. **Phase 1 scope:** Full site or homepage + 4 service pages first?
9. **Promo codes:** `SUMMER26 / AUTUMN26 / AIAUDIT26` final or CMS-configurable?
10. **Custom chat vs. Crisp:** Keep the bespoke widget (showcases AI) or switch to Crisp (live agent)?
11. **10% new client popup:** Confirm the discount is real and intentional.
12. **ARR figures on Products page:** Are the €120k / €80k ARR figures real and public-ready?
13. **Case study pattern:** Modal preview + slug page, or modal-only?

---

*Ready for your review. Edit or approve any section — then I'll move to wireframes or HTML build.*

---

## SEO & UX RATIONALE — Why Every Decision in This Spec Works

This section defends every structural and design decision against one question: *does it help us get more organic leads, or does it help visitors convert once they arrive?* Written in plain language, no jargon.

---

### 1. Why we split into separate pages per service

**The problem with one page:** Google ranks pages, not websites. A single page trying to talk about agile teams, body shopping, custom apps, and websites all at once sends a confused signal — "what is this page actually about?" Google responds by ranking it weakly for everything.

**The fix:** Each service gets its own URL (`/agile-team/`, `/body-shopping/`, etc.) with its own H1, meta title, and content built around a single keyword cluster. Now `/agile-team/` can compete specifically for "nearshore agile team Czech Republic" and win — because the entire page is about exactly that.

**The UX argument:** A CTO evaluating a €200k nearshore engagement and an HR manager looking for one React developer are two completely different people with different questions, fears, and decision timelines. Sending them to the same page forces both to wade through irrelevant content. Separate pages let each visitor feel spoken to directly — which reduces bounce rate and increases time on page, both of which are indirect Google ranking signals.

**Proof it works:** This is exactly how STRV, Netguru, and Moravio are structured. Each competitor has dedicated service landing pages. They rank. eValue currently doesn't.

---

### 2. Why the sticky navigation matters for SEO

Navigation that disappears when you scroll means every time a visitor wants to go somewhere else, they have to scroll back to the top. The friction causes exits. Exits increase bounce rate.

**Bounce rate** is the percentage of visitors who leave after seeing only one page. Google uses it (indirectly, via engagement signals) to judge whether a page satisfied the visitor's search intent. High bounce rate = "this page didn't help" = lower ranking over time.

A sticky nav keeps conversion points (the "Nezávazná konzultace" button) visible at all times. Every extra second a visitor spends on the site — clicking to another page, reading a case study — improves the engagement signals Google sees.

---

### 3. Why the H1 headline must contain the primary keyword

Google's algorithm reads the H1 as the single most important signal for what a page is about. It's the equivalent of a chapter title in a book — if the chapter is about "nearshore agile teams" and the title says "We deliver your software," the reader (and Google) has to guess what the chapter covers.

**Current eValue hero:** "Dáváme život vašemu podnikání" — no keyword, no outcome, no proof. A visitor who searched "nearshore development Czech Republic" sees this and immediately wonders if they landed on the right page. They bounce.

**Proposed H1:** "Vývojové týmy, které opravdu dodají váš software" — contains the word "týmy" (teams), implies delivery, implies software. Not perfect keyword stuffing, but Google understands the topic. The subheadline then carries the longer keyword phrases ("nearshore", "agile", "15+ let").

**Rule:** Every page in this spec has exactly one H1 that contains the primary target keyword for that page. This is non-negotiable for basic on-page SEO.

---

### 4. Why the trust bar (counters + client logos) sits above the fold

"Above the fold" means what a visitor sees before they scroll — the first screenful. Studies consistently show that 57% of page-viewing time is spent above the fold. If the only thing a visitor sees before deciding to scroll is a headline and a button, most will leave.

Adding "15+ let · 120+ klientů · 2M+ uživatelů · 14 dní onboarding" + client logos (Carvago, Finshape, Košík) immediately answers the visitor's first subconscious question: *"Can I trust these people?"*

**SEO link:** When visitors scroll further (instead of bouncing), Google's systems record longer sessions and more scroll depth. Pages with higher average session duration tend to rank better for competitive keywords over time. The trust bar is the single cheapest way to reduce the bounce rate of the hero section.

---

### 5. Why we have 4 separate service cards instead of one "Services" link

This is audience routing. The moment a visitor arrives on the homepage, they're one of four people: a CTO looking for a full team, an HR manager who needs one developer, a startup founder with a product idea, or an SMB owner who needs a website.

If we give them one "Services" link, they click it, land on a list, have to figure out which thing applies to them, and many leave without clicking further.

**4 cards with direct links = one click to the right page.** This reduces the average path to conversion from 3–4 clicks to 2 clicks. Every extra click in a journey loses roughly 20–30% of visitors.

**SEO bonus:** Each service card links to a dedicated internal page. Internal links pass "PageRank" — a measure of authority — from the homepage (your most-visited page) to the service pages. This helps Google discover and rank those pages faster.

---

### 6. Why the featured case study is full-width on the homepage

Case studies are the #1 trust signal for B2B software buyers. They answer: "Have you done this before, for someone like me, and did it work?" The Finshape case study (−62% load time, +38% login conversion, 14-day onboarding) answers all three.

**UX argument:** A full-width case study card stops the scroll. It's visually heavier than the surrounding content, which naturally draws the eye. Visitors who were passively scanning start reading. Reading increases dwell time.

**SEO argument:** Case studies with real metrics give Google rich textual content to index. "−62% load time" + "internet banking" + "Java + React" + "Czech Republic" = a dense cluster of long-tail keywords that no competitor has in exactly this combination. These long-tail phrases are easy to rank for because almost nobody else has written them.

---

### 7. Why client-side filtering on the case study grid (no page reload)

The alternative — linking each filter to a separate URL like `/case-studies/?filter=fintech` — creates a duplicate content problem. Google sees the same case study cards appearing on multiple URLs and gets confused about which URL to rank. It often ranks none of them, or splits the authority between them.

Client-side filtering (JavaScript shows/hides cards without loading a new page) means there is only one URL, one canonical page, and all authority stays concentrated there. The filter is purely a UX feature — it helps visitors find relevant work faster — but has zero SEO cost.

The URL hash update (`/case-studies/#fintech`) lets users share filtered views and bookmark them, which is a UX nicety that costs nothing.

---

### 8. Why testimonials need real names, company logos, and Schema markup

Anonymous testimonials ("A satisfied client says...") are worth nothing to a B2B buyer. They are assumed to be fabricated. A testimonial from Jakub N., CTO of Carvago, with the Carvago logo, is verifiable — the buyer can look up Jakub on LinkedIn.

**Schema markup** (`Review` + `AggregateRating` in JSON-LD) tells Google that this page contains verified reviews. Google may then display a star rating directly in the search result — called a "rich snippet." Rich snippets increase click-through rate from search results by an average of 20–30%, even without moving up in ranking position. You get more clicks for free from the same position.

---

### 9. Why we put a Cal.com booking embed mid-page (not just at the bottom)

Most websites put their contact CTA at the bottom of the page and nowhere else. This means visitors who are convinced at the 40% scroll mark (after reading the hero and case study) have to keep scrolling to find a way to act. Many don't — they leave and intend to "come back later." They rarely do.

A mid-page CTA strip with an inline booking embed ("Rezervovat 30min konzultaci →") catches the visitor at peak intent — right after they've seen the proof. It removes the question "what do I do now?" at exactly the moment they're asking it.

**UX rule:** Every time you make a visitor think "where do I go next?" you lose them. The booking embed is the answer before they finish asking the question.

---

### 10. Why the tech stack section matters for SEO (not just for looks)

Listing PHP · .NET · Java · React · Node.js · TypeScript etc. visually establishes capability. But for SEO, these technology names are **latent semantic keywords** — words that Google expects to see on a page about software development. Their presence tells Google's algorithm that the page is genuinely about software development, not just using that phrase in the title.

More practically: a DACH CTO searching "hire TypeScript developer Czech Republic" will not necessarily land on the homepage — but if TypeScript appears prominently on the page and in the internal links, Google may surface it. Each technology name is a potential search query from a real buyer.

---

### 11. Why each service page needs a Pain → Solution block

Google's core ranking philosophy since 2022 is **search intent matching** — does this page answer what the person was actually looking for when they searched?

A CTO searching "how to scale my dev team quickly" has a pain (can't hire fast enough), not a question about eValue's services. If the page immediately talks about eValue's features, it mismatches the intent. The visitor bounces.

If the page opens with: "Hiring takes 3 months. Your competitors ship every 2 weeks." — the visitor recognises their problem, feels understood, and reads on. Pain → Solution is not copywriting fluff — it's the mechanism by which a page earns a low bounce rate for a high-intent search query.

---

### 12. Why FAQ sections (with FAQ Schema) are on every service page

FAQs serve two purposes simultaneously.

First, they reduce buyer anxiety. A CTO's objections before booking a call are predictable: "What's the NDA situation?" "Do your developers speak English?" "What happens if the team doesn't work out?" Answering these on the page removes the friction that stops people from clicking the CTA.

Second, **FAQPage Schema** tells Google that this page has question-and-answer content. Google may display individual FAQ answers as expandable dropdowns directly in the search results — without the user ever clicking through to the site. This sounds counterintuitive, but it increases brand visibility and signals expertise. Users who see your FAQ in Google's results before clicking arrive with pre-answered objections, making them more likely to convert.

---

### 13. Why pricing anchors ("Projekty od €X") are critical for lead quality

Hiding pricing feels safe — "let's get them on a call and then reveal the price." In B2B software, this strategy backfires. Visitors who have no price expectation either assume you're too expensive and leave, or book a discovery call only to discover the budget mismatch. Both outcomes waste everyone's time.

A pricing anchor ("Teams from €X/day", "Projects from €Y") does not commit you to a price — it sets a floor. Visitors who see it and continue are pre-qualified. Visitors who see it and leave would have left after the discovery call anyway — you've saved 30 minutes.

**SEO angle:** Pricing pages rank for **commercial intent keywords** like "nearshore developer rates Czech Republic" or "custom software development cost." These queries come from buyers who are actively comparing options and close to a decision. They are the highest-value search traffic available.

---

### 14. Why individual case study pages (`/case-studies/finshape/`) are better than a portfolio section

A portfolio section is one page with many client names listed. Google indexes it as one page and ranks it for very generic terms at best.

Individual case study pages each have their own URL, their own H1, their own content. `/case-studies/finshape/` can rank for "Finshape development partner," "internet banking redesign Czech Republic," "Java React banking app agency." These are ultra-specific, low-competition queries — exactly the kind of search a CTO makes when they're already evaluating vendors.

Each individual case study page is also a natural destination for backlinks. Finshape themselves might link to it. Industry blogs writing about Czech fintech might reference it. Every backlink to a case study page increases the domain authority of the entire eValue.cz site.

---

### 15. Why the AI Automation page needs its own URL instead of a section on the homepage

`/ai-automatizace/` can rank for "AI automatizace pro firmy" and "AI agent vývoj Czech Republic." A section on the homepage cannot — the homepage is already competing for its own keyword set, and diluting it with AI-specific content weakens both.

This is called **topical authority** — Google ranks pages higher when the page is entirely, deeply focused on one topic. A dedicated AI Automation page with its own H1, FAQs, use-case tiles, pricing tiers, and case studies sends a clear signal: "this page is the authoritative source on AI automation services from Czech Republic."

The free AI audit CTA ("Získat AI audit zdarma →") specifically targets visitors who are curious but not yet committed. A free, low-risk entry point converts browsers into leads. "Free audit" is also a high-intent keyword — people searching "free AI audit" are actively looking to start a project.

---

### 16. Why the blog must use static generation (SSG), not a JavaScript SPA

This is the most important technical SEO decision in the entire spec.

When a blog article is rendered by JavaScript in the browser (typical React/Vue SPA), Google's crawler has to run the JavaScript to see the content. Google's crawler is slow at this — it can take days to weeks for JS-rendered content to be fully indexed. Some content never gets indexed properly.

When a blog article is **pre-rendered to static HTML** (SSG via Next.js or Astro), Google's crawler sees the full text immediately on the first visit, with zero JavaScript required. The article is indexed within hours. Every word is searchable from day one.

For an SEO content strategy that targets 2 blog articles per week, the difference between "indexed in hours" and "indexed in weeks (maybe)" is enormous. SSG is not a tech preference — it is a prerequisite for the blog to perform as an organic lead engine.

---

### 17. Why the newsletter CTA appears after paragraph 3 of every blog article

A visitor who has read three paragraphs of an article is already engaged — they found the content useful enough to keep reading. This is the moment of highest receptiveness to a low-friction offer ("Get monthly insights from eValue → subscribe free").

Placing the CTA after paragraph 3 (rather than at the end of the article) captures readers who will not reach the bottom — which is most readers. Studies on blog reading behaviour show that fewer than 20% of visitors scroll to the bottom of a long article. Waiting until the end to capture emails means missing 80% of your opportunity.

---

### 18. Why the animated counters (DC-01) must have a `<noscript>` fallback

The counters — "15+ let · 120+ klientů · 2M+ uživatelů" — are animated with JavaScript. If a visitor has JavaScript disabled (rare but possible), or if the JS fails to load, they see nothing where the numbers should be.

More importantly: **Google's crawler does not always execute JavaScript.** If the counter numbers only exist in JS, Google may index the page with blank spaces where the trust metrics are. The `<noscript>` fallback ensures Google always sees "15+ let zkušeností" as static HTML text — which it can read, index, and use as a relevance signal.

This is a small technical detail with a disproportionate SEO impact.

---

### 19. Why the Nearshore Cost Calculator (DC-07) is the single best lead magnet on the site

A lead magnet is content or a tool that's valuable enough to exchange an email address for. Most lead magnets (PDFs, whitepapers) require the visitor to trust your content before they give you their contact details. A calculator is different — it gives value instantly, before asking for anything.

A visitor who uses the calculator ("Kolik stojí váš projekt?") is self-qualifying: they're actively exploring whether eValue fits their budget. The email capture at the end ("Get your exact quote") feels like a natural next step, not a form to fill in.

**SEO bonus:** Calculators are highly linkable content. Journalists, bloggers, and other agencies writing about nearshore development costs will link to a useful calculator. Each of those links is a backlink — which increases domain authority and improves the ranking of every page on eValue.cz, not just the calculator page.

---

### 20. Why the availability badge (DC-11) is a freshness signal for Google

Google's algorithm rewards pages that are **regularly updated**. A page that hasn't changed since 2023 is treated with lower freshness score than a page updated this month.

The availability badge ("🟢 Volné kapacity od července 2026") is updated monthly. Every time it changes, the page content changes, which triggers a re-crawl. More frequent re-crawling means Google notices new content (new blog posts, updated case studies, new testimonials) faster.

This is a free way to tell Google: "this site is actively maintained." It costs one CMS field update per month.

---

### 21. Why hreflang tags are required on every page (not optional)

Without hreflang tags, Google doesn't know which language version of a page to show which user. A Czech CTO searching in Czech might be served the English version. A German buyer might be served the Czech version. Both experiences cause an immediate bounce.

Worse, without hreflang, Google may treat `/en/agile-team/` and `/agile-team/` as **duplicate content** — two pages saying the same things — and penalise both by splitting authority between them. With hreflang, Google understands they are translations of each other and ranks each in its appropriate language market.

For eValue, which is targeting CZ + EN + DE + BG markets, this is the difference between "we have an international site" and "our international site actually works."

---

### 22. Why Schema.org structured data on every page is worth the development effort

Schema markup is JSON code added to a page's `<head>` that describes the page's content to Google in a structured, unambiguous way. It doesn't change how the page looks. It changes how Google presents it in search results.

Here's what each Schema type in the spec does:

**`Organization`** (all pages) — tells Google eValue is a registered company with a verified address, phone, and website. Enables the Knowledge Panel (the box that appears on the right side of Google when someone searches "eValue.cz"). This is free brand real estate.

**`Service`** (service pages) — tells Google "this page is about a specific service, not just general information." Helps Google match the page to commercial-intent searches.

**`Review` + `AggregateRating`** (homepage, about) — enables star ratings in search results. Higher CTR from same ranking position. A result showing ★★★★★ 4.9 (23 reviews) gets clicked significantly more than one without it.

**`BlogPosting`** (blog articles) — enables rich results for blog posts: author name, date, featured image shown in Google News and Discover. Opens the site to Google Discover traffic — a separate, high-volume traffic source that most B2B sites ignore.

**`FAQPage`** (FAQ sections) — enables expandable Q&A dropdowns in search results. Takes up more SERP real estate, reducing space for competitors.

**`BreadcrumbList`** (inner pages) — shows the page path in search results (eValue.cz › Case Studies › Finshape). Improves click-through and helps Google understand site structure.

Total development time: 2–3 hours. SEO impact: permanent, compounding.

---

### 23. Why Core Web Vitals (LCP < 2.5s, CLS < 0.1) are non-negotiable

Since 2021, Google uses Core Web Vitals as a direct ranking factor — meaning a slow site literally ranks lower than a fast site, all else being equal.

**LCP (Largest Contentful Paint) < 2.5s** means the main content of the page appears within 2.5 seconds of the first request. The hero headline and image must load fast. WebP images, preloaded fonts, and a CDN are how we get there.

**CLS (Cumulative Layout Shift) < 0.1** means the page doesn't jump around as it loads — images popping in, fonts swapping, banners appearing and pushing content down. Every layout shift frustrates users and is measured and penalised by Google.

For eValue, which is targeting DACH buyers (German internet speeds) and UK buyers, slow load times are doubly punished: Google ranks the site lower, and international buyers with high expectations leave before the page finishes loading.

---

### 24. Why internal linking (every page links to 2+ others) is a compounding SEO investment

Google's crawlers discover pages by following links. A page that no other page links to may never be indexed, or may be indexed very slowly.

Internal links also pass **PageRank** — a measure of authority — between pages. The homepage receives the most direct traffic and external backlinks, making it the most authoritative page on the site. When the homepage links to `/agile-team/`, some of that authority flows there. When `/agile-team/` links to a specific case study, authority flows further.

The result: pages deep in the site structure (individual blog posts, case studies) inherit authority from the homepage and rank better than they would in isolation. This is why every page in the spec cross-links to at least 2 others — it creates an authority distribution network, not a dead-end structure.

---

### 25. Why Google My Business listings for Praha AND Znojmo separately

Google My Business (now Google Business Profile) is the source of the "local pack" — the map and 3 business listings that appear at the top of local searches like "software development company Praha."

Two separate listings for two separate physical locations means eValue can appear in local searches from both Prague and Znojmo. Clients in Znojmo or South Moravia searching for local IT partners will find a local result, not a Prague-only company. This opens a small but zero-cost lead channel that no competitor has bothered to claim.

---

*Every decision in this spec exists for a reason. If something is removed or simplified, re-read this section to understand what organic traffic or conversion rate is being sacrificed in exchange.*
