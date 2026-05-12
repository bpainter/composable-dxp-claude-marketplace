---
name: website-factory-review-microsite
description: >
  Generates the Phase 3 Review stakeholder microsite for a Website Factory project.
  Takes locked Phase 2 markdown (CX, brand, content, design, models, requirements,
  measurement) plus project brand tokens and the TFIC Composable DXP design system,
  produces a self-contained 12-section HTML microsite that opens by double-click on
  index.html. No build step, no dependencies, no Vercel deploy. One shared CSS file.
  Slalom Composable DXP secondary brand. Drives the client review session and
  captures written signoff before Phase 4 Implementation starts. Calls
  design:design-web-designer, design:design-presentation, design:design-visualization,
  design:design-imagery, design:design-iconography, software-engineering:software-
  engineering-frontend-developer, software-engineering:software-engineering-a11y-audit,
  consulting:consulting-humanize. Also known as: stakeholder microsite generator,
  Phase 3 Review generator, /website-review.

type: skill
project: skills-library
plugin: website-factory
aliases: [website-factory-review-microsite, website-review-microsite, stakeholder-microsite]
tags: [type/skill, plugin/website-factory, scope/generator, phase/3-review, topic/microsite, topic/stakeholder-review]
status: active
---

## Goal

Generate the Phase 3 Review microsite that lets the client walk through Discovery + Definition deliverables and sign off in writing before Implementation begins. The microsite is plain HTML with inline JS + one shared CSS file. Opens by double-click on `index.html`. Zippable. Always Slalom Composable DXP secondary brand.

The microsite is the **client-facing gate artifact** between Definition and Implementation. Asking a non-technical stakeholder to read twenty-plus markdown files is a path to vague signoff and downstream surprise. This skill solves that.

## When to invoke

- User runs `/website-review` inside a Website Factory project.
- User says any version of: "generate the review microsite," "build the stakeholder microsite," "create the Phase 3 microsite," "prep for client review," "we're at the Phase 2 gate — let's get the microsite ready."
- The orchestrator (`website-factory-create-flow`, once built) hands off to this skill at Phase 3 entry.
- Phase 2 just gated. The next concrete action is the microsite.

**Do not invoke if:**
- Phase 2 hasn't gated yet. The microsite is meant to summarize *locked* artifacts. If Phase 2 markdown is still in flux, hold and tell the operator to lock first.
- The engagement is operator-internal (no external stakeholder). Skip Phase 3, log the deviation in `_ops/DECISIONS.md`. See `Pipeline.md` exceptions.

## Required context

For the structure, CSS pattern, and lifecycle, see [`60_Digital_Manufacturing/Website_Factory/References/Stakeholder_Microsite.md`](../../../../60_Digital_Manufacturing/Website_Factory/References/Stakeholder_Microsite.md). **Do not re-explain the microsite shape** — cite that file.

For the visual toolchain (Napkin, higgsfield, Flaticon, etc.), see [`60_Digital_Manufacturing/Website_Factory/References/Toolchain.md`](../../../../60_Digital_Manufacturing/Website_Factory/References/Toolchain.md).

For the voice rules (em-dash banned, no marketing-speak verbs), see [`60_Digital_Manufacturing/Website_Factory/References/Voice_and_Tone.md`](../../../../60_Digital_Manufacturing/Website_Factory/References/Voice_and_Tone.md).

For the TFIC brand tokens, see [`70_Templates/Composable_DXP_Brand_Assets/future-is-composable-design-system/project/colors_and_type.css`](../../../../70_Templates/Composable_DXP_Brand_Assets/future-is-composable-design-system/project/colors_and_type.css).

## Inputs (what the skill reads)

The skill reads from the active project's WIP folder. Confirm the project code first via AskUserQuestion if not in context.

### Phase 1 inputs (for the Brief + Market sections)

- `01-discovery/project-brief.md` — for Brief section
- `01-discovery/approval-matrix.md` — for Brief section signoff matrix
- `01-discovery/market-scan.md`, `competitive-analysis.md`, `trend-signals.md` — for Market & Competitive section
- `01-discovery/brand-exploration.md` (if `02-definition/brand/` not yet locked) — for Brand section

### Phase 2 inputs (the bulk)

- `02-definition/cx/personas/*.md`, `cx/jtbd.md`, `cx/journey-map.md`, `cx/service-blueprint.md`, `cx/task-flows.md`, `cx/information-architecture.md`
- `02-definition/brand/positioning.md`, `voice-and-tone.md`, `guidelines.md`, `tokens/*.json`
- `02-definition/content/sitemap.md`, `page-briefs/*.md`, `copy/*.md`
- `02-definition/design/style-direction.md`, `design-briefs/*.md`, `design-system.md`, `screens/` (images), `handoff/*.md`
- `02-definition/models/contentful-model.md`, `algolia-index.md`, `forms-decision.md`, `auth-decision.md`
- `02-definition/requirements/requirements.md`, `epics.md`, `sprint-plan.md`
- `02-definition/measurement/measurement-plan.md`, `event-spec.md`, `seo-plan.md`, `geo-patterns.md`, `schema-markup.md`, `performance-budget.md`

### Brand inputs

- Project tokens: `02-definition/brand/tokens/colors.json`, `typography.json`, `spacing.json`
- TFIC tokens: `70_Templates/Composable_DXP_Brand_Assets/future-is-composable-design-system/project/colors_and_type.css`
- TFIC marks: `70_Templates/Composable_DXP_Brand_Assets/future-is-composable-design-system/project/assets/mark.svg`, `wordmark.svg`

### Project metadata

- `PROJECT.md` at the project root — for cover slide (project name, client, version, date)

## Output (what the skill writes)

Everything lands in `WIP/[project-code]/03-review/microsite/`:

```
03-review/microsite/
├── index.html                       # Landing — top-nav links to all sections
├── 00-cover.html                    # Cover slide
├── 01-brief.html                    # Business goal + audience + success metrics + scope + approval matrix
├── 02-market-competitive.html       # Market + competitive + trend signals
├── 03-personas-jobs.html            # Personas + JTBD
├── 04-journeys-flows.html           # Current/future journey + task flows
├── 05-ia-sitemap.html               # IA + sitemap + taxonomy
├── 06-brand.html                    # Positioning + voice + color + type + imagery + applied
├── 07-content-pages.html            # Page briefs index + copy samples
├── 08-design-system-screens.html    # Tokens + primitives + composed blocks + key screens
├── 09-models.html                   # Contentful + Algolia + forms + auth decisions
├── 10-requirements-backlog.html     # Requirements + epics + sprint plan
├── 11-measurement-seo-geo.html      # KPIs + events + schema markup + perf budget
├── 12-next.html                     # Implementation timeline + what's needed from client
├── microsite.css                    # One shared stylesheet
└── assets/                          # Extracted images: mood boards, mock screens, diagrams
    ├── mark.svg                     # TFIC mark for top-nav
    ├── wordmark.svg                 # TFIC wordmark
    ├── personas/                    # Persona portraits (if generated)
    ├── journeys/                    # Journey + flow diagrams (PNG/SVG from Figma+Mermaid)
    ├── ia/                          # Sitemap tree diagram
    ├── brand/                       # Color swatches as SVG, type specimens
    ├── screens/                     # Phase 2 screen exports (PNG)
    ├── diagrams/                    # Napkin-generated infographics
    └── imagery/                     # Hero + mood imagery
```

The skill also writes:

- `03-review/feedback.md` — empty template ready to capture review session notes
- `03-review/signoff.md` — empty template ready to capture written approval

## Stages

### Stage 0 — Confirm project + Phase 2 lock

Before generating, confirm:

1. **Project code.** If not in context, AskUserQuestion: list projects in `WIP/` (excluding `_`-prefixed), pick one.
2. **Phase 2 gate passed.** Read `_ops/STATUS.md`. If Phase 2 gate isn't recorded as passed, AskUserQuestion:

   ```
   Question: "Phase 2 gate hasn't been recorded as passed in STATUS.md. Generate the microsite anyway?"
   Options:
   1. No — let me lock Phase 2 first (Recommended)
   2. Yes — Phase 2 is effectively locked but STATUS.md is stale, proceed
   3. Yes — this is a draft pass for internal review only, won't go to the client
   ```

   If option 1, hand control back. If 2 or 3, proceed; log the deviation in `_ops/DECISIONS.md`.

3. **Microsite folder.** Check `03-review/microsite/`. If it exists with content:

   ```
   Question: "A microsite already exists at 03-review/microsite/. What would you like to do?"
   Options:
   1. Regenerate from current markdown (overwrites — Recommended if Phase 2 changed)
   2. Append to existing (keep prior pages, add new sections only)
   3. Version it (rename existing to microsite-v1/, generate fresh microsite/)
   4. Cancel
   ```

### Stage 1 — Detect inputs (DEA)

Scan the expected inputs. For each file:

- **Present + non-empty** → ready, include in generation.
- **Present + nearly empty** (just headers, no content) → flag to the operator. AskUserQuestion: "Section X has sparse markdown. Generate the microsite section anyway (with placeholders) or skip the section?"
- **Missing entirely** → flag. AskUserQuestion: "Required input X is missing. Skip the related section, use the closest substitute, or pause to draft it?"

Produce a one-paragraph **input inventory** before generating. The operator should be able to see what's about to land before files are written.

### Stage 2 — Generate `microsite.css`

The shared stylesheet first. The CSS holds:

- TFIC base tokens (`@import` or inline the canonical TFIC tokens from `70_Templates/Composable_DXP_Brand_Assets/future-is-composable-design-system/project/colors_and_type.css`).
- Project-specific brand tokens (from `02-definition/brand/tokens/*.json` mapped to CSS variables).
- Layout primitives: `.slide`, `.slide--cover`, `.slide--soft`, `.slide--accent`.
- Component patterns: `.card`, `.stage`, `.quote`, `.swatch`, `.sitemap-tree`, `.journey`, `.spec-grid`, `.token-row`.
- Typography scale: display, h1–h6, lead, body, caption. Font-family declarations using TFIC defaults.
- Print stylesheet: `@media print` — page breaks at slide boundaries, `-webkit-print-color-adjust: exact`, hide `.slide-meta`.
- Accessibility behaviors: focus indicators, `@media (prefers-reduced-motion: reduce)`.
- Top-nav and section navigation styles.

Use this as the starting CSS skeleton (adapt the brand variables from the project's tokens):

```css
/* microsite.css — Phase 3 Review microsite, Composable DXP secondary brand */
/* Project: [project-code] — generated YYYY-MM-DD */

/* ============================================================
   TOKENS
   TFIC base + project-specific overrides
   ============================================================ */

:root {
  /* TFIC brand foundation — pull from 70_Templates/Composable_DXP_Brand_Assets/.../colors_and_type.css */
  --tfic-ink: #0b0c10;
  --tfic-paper: #faf8f3;
  --tfic-paper-soft: #f1eee5;
  --tfic-accent: #c8472b;       /* TFIC signature accent */
  --tfic-rule: rgba(11, 12, 16, 0.12);

  /* Project tokens — populated from 02-definition/brand/tokens/colors.json */
  --brand-primary: hsl(/* H S L from project tokens */);
  --brand-primary-deep: hsl(/* H S L */);
  --brand-secondary: hsl(/* H S L */);
  --brand-accent: hsl(/* H S L */);

  /* Type — TFIC defaults; project may override display family */
  --font-display: "Lora", "Georgia", serif;       /* bold, no italic */
  --font-body: "Inter", system-ui, sans-serif;    /* or project body family */
  --font-mono: "JetBrains Mono", "Menlo", monospace;

  /* Scale */
  --size-display-xl: clamp(2.5rem, 5vw + 1rem, 4.5rem);
  --size-display-l: clamp(2rem, 4vw + 0.75rem, 3.25rem);
  --size-h2: clamp(1.5rem, 2vw + 0.75rem, 2.25rem);
  --size-h3: 1.25rem;
  --size-lead: 1.125rem;
  --size-body: 1rem;
  --size-caption: 0.875rem;
}

/* ============================================================
   RESET + BASE
   ============================================================ */

*, *::before, *::after { box-sizing: border-box; }
html { scroll-behavior: smooth; }
body {
  margin: 0;
  font-family: var(--font-body);
  font-size: var(--size-body);
  line-height: 1.55;
  color: var(--tfic-ink);
  background: var(--tfic-paper);
}

/* Em-dash banned in TFIC copy — enforce visually via JS check at QA time */

/* ============================================================
   TOP NAV
   ============================================================ */

.topnav {
  position: sticky;
  top: 0;
  z-index: 10;
  display: flex;
  align-items: center;
  gap: 1.5rem;
  padding: 0.75rem 2rem;
  background: var(--tfic-paper);
  border-bottom: 1px solid var(--tfic-rule);
}
.topnav__mark { height: 1.5rem; width: auto; }
.topnav__sections { display: flex; gap: 1rem; flex-wrap: wrap; font-size: var(--size-caption); }
.topnav__sections a {
  color: var(--tfic-ink);
  text-decoration: none;
  padding: 0.25rem 0.5rem;
  border-radius: 3px;
}
.topnav__sections a:hover { background: var(--tfic-paper-soft); }
.topnav__sections a[aria-current="page"] {
  background: var(--tfic-ink);
  color: var(--tfic-paper);
}

/* ============================================================
   SLIDE PRIMITIVES
   ============================================================ */

.slide {
  position: relative;
  min-height: 100vh;
  padding: 4rem 6vw;
  display: flex;
  flex-direction: column;
  justify-content: center;
  border-bottom: 1px solid var(--tfic-rule);
  scroll-margin-top: 4rem;
}

.slide--cover {
  background: linear-gradient(135deg, var(--brand-primary-deep), var(--tfic-ink));
  color: var(--tfic-paper);
}
.slide--soft { background: var(--tfic-paper-soft); }
.slide--accent { background: var(--tfic-ink); color: var(--tfic-paper); }

.slide-header { margin-bottom: 2rem; }
.eyebrow {
  display: inline-block;
  font-family: var(--font-mono);
  font-size: var(--size-caption);
  text-transform: uppercase;
  letter-spacing: 0.08em;
  opacity: 0.7;
  margin-bottom: 0.5rem;
}
.slide-header__title {
  font-family: var(--font-display);
  font-weight: 700;
  font-style: normal;     /* TFIC display: bold Lora, no italic */
  font-size: var(--size-h2);
  margin: 0;
}
.display-xl {
  font-family: var(--font-display);
  font-weight: 700;
  font-style: normal;
  font-size: var(--size-display-xl);
  line-height: 1.05;
  margin: 0;
}
.lead {
  font-size: var(--size-lead);
  max-width: 50ch;
  opacity: 0.85;
}

.slide-meta {
  position: absolute;
  bottom: 1.5rem;
  left: 6vw;
  right: 6vw;
  display: flex;
  justify-content: space-between;
  font-size: var(--size-caption);
  opacity: 0.6;
}

/* ============================================================
   COMPONENT PATTERNS
   ============================================================ */

.card {
  background: var(--tfic-paper);
  border: 1px solid var(--tfic-rule);
  padding: 1.5rem;
  border-radius: 4px;
}
.slide--accent .card,
.slide--cover .card { background: rgba(255, 255, 255, 0.06); border-color: rgba(255, 255, 255, 0.18); }

.spec-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 1.5rem;
}
.swatch {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}
.swatch__chip {
  width: 100%;
  aspect-ratio: 4 / 3;
  border-radius: 4px;
  border: 1px solid var(--tfic-rule);
}
.swatch__label { font-family: var(--font-mono); font-size: var(--size-caption); }

.token-row {
  display: grid;
  grid-template-columns: 1fr 2fr 2fr;
  gap: 1rem;
  padding: 0.75rem 0;
  border-bottom: 1px solid var(--tfic-rule);
  font-family: var(--font-mono);
  font-size: var(--size-caption);
}

.sitemap-tree { font-family: var(--font-mono); font-size: var(--size-caption); }
.sitemap-tree ul { list-style: none; padding-left: 1.5rem; border-left: 1px solid var(--tfic-rule); }

.quote {
  font-family: var(--font-display);
  font-size: 1.5rem;
  font-style: normal;
  line-height: 1.35;
  border-left: 4px solid var(--brand-accent);
  padding: 0.5rem 0 0.5rem 1.5rem;
  margin: 1.5rem 0;
}

/* ============================================================
   PRINT
   ============================================================ */

@media print {
  body { background: white; }
  .topnav { display: none; }
  .slide {
    min-height: auto;
    page-break-after: always;
    padding: 1in 0.75in;
  }
  .slide-meta { position: static; margin-top: 2rem; }
  * {
    -webkit-print-color-adjust: exact;
    print-color-adjust: exact;
  }
}

/* ============================================================
   ACCESSIBILITY
   ============================================================ */

:focus-visible {
  outline: 3px solid var(--brand-accent);
  outline-offset: 2px;
}

@media (prefers-reduced-motion: reduce) {
  html { scroll-behavior: auto; }
}
```

Replace the comment placeholders with real project tokens read from `02-definition/brand/tokens/*.json`.

### Stage 3 — Generate the 12 section HTML pages

Each section is one HTML page, with a top-nav, a sequence of `.slide` sections inside, and the shared `microsite.css` linked.

**Boilerplate per page:**

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>[Section title] — [Project name] | Phase 3 Review</title>
  <link rel="stylesheet" href="microsite.css" />
</head>
<body>
  <nav class="topnav">
    <img class="topnav__mark" src="assets/mark.svg" alt="The Future Is Composable" />
    <div class="topnav__sections">
      <a href="index.html">Overview</a>
      <a href="00-cover.html">Cover</a>
      <a href="01-brief.html">Brief</a>
      <a href="02-market-competitive.html">Market</a>
      <a href="03-personas-jobs.html">Personas</a>
      <a href="04-journeys-flows.html">Journeys</a>
      <a href="05-ia-sitemap.html">IA</a>
      <a href="06-brand.html">Brand</a>
      <a href="07-content-pages.html">Content</a>
      <a href="08-design-system-screens.html">Design</a>
      <a href="09-models.html">Models</a>
      <a href="10-requirements-backlog.html">Requirements</a>
      <a href="11-measurement-seo-geo.html">Measurement</a>
      <a href="12-next.html">Next</a>
    </div>
  </nav>
  <main>
    <!-- slides go here -->
  </main>
</body>
</html>
```

The current section's nav link gets `aria-current="page"`.

**Per-section content composition.** Each section follows the same shape: an opening `.slide--cover` (eyebrow + display title + lead paragraph), 2–6 content `.slide` blocks, and a closing `.slide--accent` block for the "key takeaway" if appropriate.

#### 00 — Cover

- Single `.slide--cover` only. No content blocks.
- Pulls from `PROJECT.md`:
  - Project name → `display-xl`
  - One-line description → `lead`
  - Client + version + date → `.slide-meta`
- Optional: TFIC wordmark as a subtle full-bleed background element.

#### 01 — Brief

Source: `01-discovery/project-brief.md`, `01-discovery/approval-matrix.md`.

Slides:
1. Cover ("01 / Brief" eyebrow, "What we're building and why" display title).
2. **Business goal + audience** — two-column, business goal left, primary audience right.
3. **Success metrics** — `spec-grid` of 3–5 metric cards (metric name + target + measurement source).
4. **Scope** — in/out two-column. In-scope as a checklist; out-of-scope explicit.
5. **Constraints** — timeline + budget + regulatory + technical, four cards.
6. **Approval matrix** — table of decisions × sign-offs (the matrix verbatim).

#### 02 — Market & Competitive

Source: `01-discovery/market-scan.md`, `competitive-analysis.md`, `trend-signals.md`.

Slides:
1. Cover.
2. **Market overview** — top 3 dynamics with one-line summaries. Optional Napkin-generated landscape diagram in `assets/diagrams/market.svg`.
3. **Competitive landscape** — 3–5 competitor cards (name + positioning + key differentiator + what to avoid mimicking).
4. **Trend signals** — 3–5 signal cards (signal + evidence + implication for this project).

#### 03 — Personas & Jobs

Source: `02-definition/cx/personas/*.md`, `cx/jtbd.md`.

Slides:
1. Cover.
2. **Personas** — one card per persona. Avatar (from `assets/personas/`), name + role pattern, motivator, pressure, verbatim quote.
3. **Jobs-to-be-done** — one `.stage` block per JTBD: "When X, I want to Y, so that Z" formatted as three vertical bands.

#### 04 — Journeys & Flows

Source: `02-definition/cx/journey-map.md`, `cx/service-blueprint.md` (if present), `cx/task-flows.md`.

Slides:
1. Cover.
2. **Current-state journey** — full-bleed Figma+Mermaid-exported PNG/SVG from `assets/journeys/current-state.png`. Annotated.
3. **Future-state journey** — same, future-state.
4. **Critical task flows** — one slide per critical flow (typically 2–3): step-by-step layout with screens-or-states as the row.

#### 05 — IA & Sitemap

Source: `02-definition/cx/information-architecture.md`, `02-definition/content/sitemap.md`.

Slides:
1. Cover.
2. **Information architecture** — the IA tree as nested `<ul>` styled `.sitemap-tree`. Or as a Figma-exported tree diagram in `assets/ia/`.
3. **Sitemap** — the full page list with paths + page type + audience. Three-column table.
4. **Taxonomy** — taxonomies and their values. Card per taxonomy.

#### 06 — Brand

Source: `02-definition/brand/positioning.md`, `voice-and-tone.md`, `guidelines.md`, `tokens/*.json`.

Slides:
1. Cover (this slide uses the project's brand-primary as the cover background, not TFIC — illustrates the project's brand visually).
2. **Positioning** — the positioning statement as a `.quote` block. Plus 3 supporting bullets.
3. **Voice & tone** — voice attributes (3–4) as cards; do/don't pairs as a side-by-side.
4. **Color** — `.spec-grid` of `.swatch` blocks. Each swatch shows the color, name, HSL value, and usage note.
5. **Type** — type specimens. Display family + body family + mono. Show in-use samples.
6. **Imagery** — 2–4 reference images from `assets/brand/imagery/` (sourced via higgsfield + stock).
7. **Iconography** — Flaticon-set sample (lined OR colored — never both).
8. **Applied examples** (if `brand/applied/` has content) — favicon, social cards, email signature.

#### 07 — Content & Pages

Source: `02-definition/content/sitemap.md`, `page-briefs/*.md`, `copy/*.md` (samples).

Slides:
1. Cover.
2. **Page briefs index** — table of all pages × purpose × primary persona × primary CTA. Linkable to per-page entries.
3. **Sample page briefs** (3–5 key pages) — one slide per. Pull purpose, primary message, supporting points from the brief.
4. **Copy samples** — for the 2–3 most consequential pages, show the locked hero copy in full. Inspectable by the client.

#### 08 — Design System & Screens

Source: `02-definition/design/design-system.md`, `design/screens/*`, `02-definition/brand/tokens/*.json`.

Slides:
1. Cover.
2. **Tokens** — `token-row` table: token name, value, usage. From the project tokens JSON.
3. **Primitives** — `.spec-grid` of primitive component thumbnails (button states, input states, card variants). Either from `assets/design-system/primitives/` or rendered inline as small HTML samples.
4. **Composed blocks** — `.spec-grid` of block-level component thumbnails (hero, feature row, CTA section, testimonial, footer).
5. **Key screens** — full-bleed screen renders from `assets/screens/`. One slide per key screen (typically 3–5): home, listing, detail, form, plus a project-specific high-stakes screen.

#### 09 — Models

Source: `02-definition/models/contentful-model.md`, `algolia-index.md`, `forms-decision.md`, `auth-decision.md`.

Slides:
1. Cover.
2. **Content model** — the Topics + Assemblies summary. Two-column table: Topics on the left, Assemblies on the right. Don't dump the full schema — surface the shape.
3. **Search** (if Algolia in scope) — index name, searchable attributes, facets, ranking strategy.
4. **Forms** — table of forms × backend × rationale.
5. **Auth** (if in scope) — provider + flow summary + user model.

#### 10 — Requirements & Backlog

Source: `02-definition/requirements/requirements.md`, `epics.md`, `sprint-plan.md`.

Slides:
1. Cover.
2. **Requirements summary** — top-level requirement categories with counts. Not the full backlog.
3. **Epics** — card per epic: name, AC summary, target sprint.
4. **Sprint plan** — timeline-style horizontal view of sprints with the epics rolling up to each. Optional Figma+Mermaid Gantt-style export.

#### 11 — Measurement & SEO/GEO

Source: `02-definition/measurement/*.md`.

Slides:
1. Cover.
2. **KPIs** — `spec-grid` of KPI cards (metric, target, source).
3. **Event spec** — top-level events table (event name, trigger, properties).
4. **SEO targets** — per-page primary keyword + intent + title pattern. Top 5–10 pages.
5. **Schema markup** — per page-type Schema.org type and key properties.
6. **GEO patterns** — how the content is structured for AI assistant extraction.
7. **Performance budget** — Core Web Vitals targets per representative template.

#### 12 — Next

Source: `02-definition/requirements/sprint-plan.md`, `_ops/RAID.md` (open dependencies).

Slides:
1. Cover.
2. **Implementation timeline** — Phase 4 sprint-by-sprint expectations, plus dates for Phase 5 Quality & UAT and Phase 6 Launch.
3. **What we need from you (client)** — concrete items: domain config, Contentful editor users, brand asset deliveries, content provided, signoffs at named gates. From `_ops/RAID.md` Dependencies.
4. **Risks the client should know about** — top 3–5 risks from `_ops/RAID.md`.
5. Closing accent slide — "Ready to start Implementation. Phase 4 Sprint 0 begins on [date] pending your signoff."

### Stage 4 — Generate `index.html` (landing)

`index.html` is the entry point. Open by double-click. It mirrors the top-nav and adds:

- The cover slide rendered inline (so the landing IS the cover for fast first-load).
- A "Sections" grid of 12 cards, each linking to its section page, with the section title + a one-line summary.
- A footer with project metadata + version + generated date + "Generated by Website Factory v1."

### Stage 5 — Generate supporting assets

Run through the visual toolchain (see `Toolchain.md`):

- **Diagrams** (market overview, sitemap, journey, content model overview) → Napkin.ai with `corporate` or `clean` style. Save to `assets/diagrams/`.
- **Flow diagrams** (task flows, content-type relationships) → Figma + Mermaid. Save to `assets/journeys/` and `assets/ia/`.
- **Persona portraits** (if generated, not stock) → higgsfield (nano banana primary). Save to `assets/personas/`.
- **Hero / mood imagery** → stock-first from Slalom Creative Library; fall back to higgsfield. Save to `assets/imagery/`.
- **Color swatches** → inline SVG generated directly in HTML.
- **Marks (TFIC mark + wordmark)** → copy from `70_Templates/Composable_DXP_Brand_Assets/.../project/assets/` to `assets/`.
- **Screens** (Phase 2 design renders) → exported PNG from Figma. Save to `assets/screens/`.

### Stage 6 — Write the empty review artifacts

Create empty templates for the operator to capture review outputs:

`03-review/feedback.md`:

```markdown
---
title: Phase 3 Review feedback — [Project Name]
phase: 3
type: review-feedback
status: in-progress
review_date: YYYY-MM-DD
participants: [list]
---

# Phase 3 Review feedback

## Required changes (must address before Implementation)

| Section | Feedback | Owner | Resolution |
|---|---|---|---|
| | | | |

## Deferred changes (Phase 4+)

| Section | Feedback | Defer to | Notes |
|---|---|---|---|
| | | | |

## Explicit accepts (no change)

| Section | What the client agreed to as-is |
|---|---|
| | |

## Open questions (need follow-up)

| Question | Owner | Target answer date |
|---|---|---|
| | | |
```

`03-review/signoff.md`:

```markdown
---
title: Phase 3 Review signoff — [Project Name]
phase: 3
type: review-signoff
status: pending
signoff_date: YYYY-MM-DD
---

# Phase 3 Review signoff

## Signoff statement

**I have reviewed the Phase 3 microsite for [Project Name] and approve proceeding to Phase 4 Implementation as scoped, with the following named conditions (if any):**

- [Condition 1]
- [Condition 2]

## Signoff

| Name | Title | Email | Signed (date) |
|---|---|---|---|
| | | | YYYY-MM-DD |

## Linked artifacts

- Microsite: `03-review/microsite/index.html`
- Feedback: `03-review/feedback.md`
- Project brief: `01-discovery/project-brief.md`
- Approval matrix: `01-discovery/approval-matrix.md`
```

### Stage 7 — Internal review prompt

Once the microsite is generated, prompt the operator for internal review before the client session. AskUserQuestion:

```
Question: "Microsite generated at 03-review/microsite/. Walk through internally now?"
Options:
1. Open it now — I'll review and flag issues (Recommended)
2. Run an automated check first (a11y, broken links, voice/tone scan)
3. Ship it to the client as-is
4. Pause — I'll review later
```

If option 2: call `software-engineering:software-engineering-a11y-audit` against the microsite folder for a WCAG 2.2 AA check. Then call `consulting:consulting-humanize` for a voice/tone scan against the rendered HTML — surface any em-dashes, marketing-speak verbs, "we believe" hedges, etc., from `References/Voice_and_Tone.md`.

### Stage 8 — Client review session facilitation

When the operator confirms ready for client review, surface guidance for the session:

- **Format:** typically 60–90 min live walkthrough. Operator shares screen, navigates the microsite top-nav top-to-bottom. Client asks questions per section. Capture feedback in `03-review/feedback.md` live.
- **Decision rule:** before the session, name the signoff rule explicitly. Defaults to "leader-decides-after-consultation" — the named Phase 3 approval matrix sign-off (typically the Economic Buyer) makes the call after hearing other stakeholders. Reference `facilitation:facilitation-decision-architect` for alternatives.
- **Don't push past resistance.** If the client raises an issue, capture it; don't argue. The microsite is the artifact to debate against — it should make disagreement productive.
- **End with the decision.** Last 15 minutes: walk `03-review/signoff.md` together. Get written approval (or capture the conditions for approval).

### Stage 9 — Iterate (if required changes surfaced)

If `feedback.md` has required changes after the session:

1. Update the corresponding Phase 2 markdown sources (never the HTML directly).
2. Re-invoke this skill to regenerate the microsite from the updated markdown.
3. Send the updated microsite to the client.
4. Capture revised signoff in `signoff.md`.

### Stage 10 — Capture signoff + advance

Once `03-review/signoff.md` has a signed entry:

1. Update `_ops/STATUS.md`: Phase 3 gate passed, date, name.
2. Append to `_ops/DECISIONS.md`: "Phase 3 Review signoff received from [name] on [date]. Implementation advances per [Sprint 0 date]."
3. Hand off to `website-factory-implement` (once built) or surface "Phase 4 Sprint 0 is now the next concrete action."

## Handoffs

| Hands off to | When | What it returns |
|---|---|---|
| `design:design-web-designer` | Stage 2–3 for layout/style direction sanity check | confirmation that the visual treatment lands |
| `design:design-presentation` | Stage 3 for slide-system thinking applied to web | composition feedback |
| `design:design-imagery` | Stage 5 imagery sourcing | tagged + sourced imagery references |
| `design:design-iconography` | Stage 5 icon picks | Flaticon icon set selected |
| `design:design-visualization` | Stage 5 diagram routing | Napkin or Figma+Mermaid output |
| `software-engineering:software-engineering-frontend-developer` | Stage 2–4 for the actual HTML/CSS generation | clean HTML files |
| `software-engineering:software-engineering-a11y-audit` | Stage 7 optional audit | WCAG 2.2 AA findings |
| `consulting:consulting-humanize` | Stage 7 voice check | flagged em-dashes, marketing-speak, hedges |
| `facilitation:facilitation-decision-architect` | Stage 8 signoff rule | decision-method recommendation |
| `facilitation:facilitation-meeting-architect` | Stage 8 session agenda | review-session agenda |
| `website-factory-implement` (planned) | Stage 10 | Phase 4 Sprint 0 kickoff |

## Failure modes (and what to do)

- **Phase 2 isn't actually locked, but the operator wants the microsite anyway.** Surface the Phase 2 gate state. Confirm via AskUserQuestion. Generate, but tag the microsite version as `v0-draft-pre-phase-2-lock` and log the deviation in `_ops/DECISIONS.md`. Tell the operator clearly that the client may see content that changes.
- **Project tokens missing or incomplete.** Fall back to TFIC tokens for everything; flag the gap. AskUserQuestion: "Project brand tokens are missing for [list]. Use TFIC defaults for those tokens, or pause to define project tokens first?" If "use TFIC defaults," the Brand section explicitly shows TFIC defaults with a "to be defined" annotation.
- **A required Phase 2 input is missing entirely** (e.g., no Contentful model spec because Modeling was skipped). The Models section gets a "this engagement skipped the Modeling sub-track per phase-plan.md" callout instead of being silently empty.
- **Higgsfield credits low.** Run `mcp__79eb9064-...__balance` before Stage 5 imagery generation. If low, AskUserQuestion: "Higgsfield credits at X. Generate fewer images, top up, or use stock-only?"
- **Em-dash detected in copy during Stage 7 voice check.** This is a TFIC ban. Don't auto-fix — surface to operator with the specific line numbers and suggested rewrites. Operator decides.
- **Client signoff is verbal-only during Stage 8.** Hold the gate. AskUserQuestion: "Verbal signoff captured but signoff.md is empty. Send a follow-up email asking for written confirmation, draft the email now, or note this as the signoff anyway (logged as a deviation)?"
- **Microsite folder pre-existed and contained an older version.** Versioning per Stage 0 decision. Don't silently overwrite — preserve history.
- **Section markdown is unusually long** (e.g., 50+ personas, 200+ page briefs). Don't dump it all. AskUserQuestion: "Section X has unusually high volume. Show top N most consequential and link to the markdown for the full list?"

## How this skill collaborates with other plugins

The skill is a generator + orchestrator. It calls specialty skills rather than reimplementing their work:

| When | Plugin / Skill |
|---|---|
| Direction on visual treatment | `design:design-web-designer`, `design:design-taste`, `design:design-presentation` |
| Imagery selection + generation | `design:design-imagery` (advice), then higgsfield MCP for generation |
| Icon set selection | `design:design-iconography` (advice), then Flaticon |
| Visualization routing | `design:design-visualization` (Napkin vs. Figma+Mermaid call) |
| HTML/CSS generation | `software-engineering:software-engineering-frontend-developer` |
| Accessibility audit | `software-engineering:software-engineering-a11y-audit` |
| Voice/tone pass | `consulting:consulting-humanize` |
| Brand token consumption | `brand:brand-tokenize` (advice on token shape) |
| Session decision rule | `facilitation:facilitation-decision-architect` |
| Session agenda | `facilitation:facilitation-meeting-architect` |
| Personalization (if microsite has per-stakeholder variants — rare) | `contentful:contentful-personalization` is overkill for a static microsite; skip |

## Token discipline

- **Don't re-explain the microsite shape.** Cite `Stakeholder_Microsite.md`.
- **Don't re-explain the toolchain.** Cite `Toolchain.md`.
- **Don't re-explain voice rules.** Cite `Voice_and_Tone.md`.
- **Don't paraphrase the Phase 2 markdown.** Render it. The microsite is a presentation surface; the markdown is the source of truth.
- **Use AskUserQuestion for discrete choices** (regenerate vs. version, top up credits, ship vs. revise). Not for things the markdown answers.
- **Don't edit HTML directly when iterating.** Update markdown, regenerate. The skill is rerunnable.

## Example prompts

1. *"Generate the Phase 3 microsite for the future-is-composable-microsite project."* → Stage 0 confirms project + Phase 2 lock; Stage 1 inventories inputs; Stage 2+ generates.
2. *"Build the stakeholder microsite — we just locked Phase 2."* → Same flow; Stage 0 confirms project from WIP listing.
3. *"Regenerate the microsite — we updated the personas after last review."* → Stage 0 detects existing microsite, offers regenerate-vs-version; Stage 2+ regenerates from current markdown.
4. *"The client wants to see what's coming before we start coding."* → Recognize Phase 3 framing; route here.
5. *"Microsite is ready, what's next?"* → Stage 7 internal review → Stage 8 client review → signoff.
6. *"Skip the microsite — this is internal."* → Confirm operator-internal engagement, log skip in `_ops/DECISIONS.md`, don't generate.
7. *"Phase 2 isn't really done but the client wants to see something."* → Surface Phase 2 lock state; generate with `v0-draft` tag if operator confirms.

## See also

- [`Stakeholder_Microsite.md`](../../../../60_Digital_Manufacturing/Website_Factory/References/Stakeholder_Microsite.md) — structure + CSS pattern + lifecycle
- [`Pipeline.md`](../../../../60_Digital_Manufacturing/Website_Factory/Pipeline.md) — Phase 3 Review activity
- [`Quality_Gates.md`](../../../../60_Digital_Manufacturing/Website_Factory/Quality_Gates.md) §3 — Phase 3 gate criteria
- [`Toolchain.md`](../../../../60_Digital_Manufacturing/Website_Factory/References/Toolchain.md) — visual toolchain the skill consumes
- [`Voice_and_Tone.md`](../../../../60_Digital_Manufacturing/Website_Factory/References/Voice_and_Tone.md) — voice rules (em-dash banned, no marketing-speak)
- [`Stages/03_Review.md`](../../../../60_Digital_Manufacturing/Website_Factory/Stages/03_Review.md) — Phase 3 reference
- [`70_Templates/Composable_DXP_Brand_Assets/future-is-composable-design-system/`](../../../../70_Templates/Composable_DXP_Brand_Assets/) — TFIC tokens
- [`../README.md`](../../README.md) — website-factory plugin overview
- [`../../solution-factory/`](../../../solution-factory/) — sibling factory plugin
