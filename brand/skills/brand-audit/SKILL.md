---
name: brand-audit
description: >
  Read-only audit of an existing brand application — surfaces drift between brand guidelines and actual deliverables, named bans being violated, identity inconsistency across surfaces, voice drift in copy. Outputs a prioritized fix list (CRITICAL / HIGH / MEDIUM / LOW) ready for the team to remediate. Use this skill any time the user wants to assess brand-application quality, before a brand refresh, or as part of periodic brand-health reviews.

# Project context
type: skill
project: skills-library
plugin: brand
aliases: [brand-audit, brand-review, brand-consistency-audit]
tags: [type/skill, plugin/brand, topic/brand, topic/audit]
status: active
---

# Brand Audit

A brand audit is the gap analysis between what the brand says it is (guidelines) and what the brand actually shows up as (in real deliverables across surfaces). Most brands look great in their guidelines and inconsistent everywhere else. The audit surfaces the gap so it can be fixed.

This skill is **read-only** — it identifies issues, prioritizes them, and outputs a remediation plan. The actual fixes happen in the relevant authoring skills ([[brand-identity-system]] for identity drift, [[brand-voice-tone]] for voice drift, [[design-document]] / [[design-presentation]] / [[design-social-asset]] for surface-specific cleanup).

Pair with [[brand-strategist]] (for the strategic context the audit is testing against), [[brand-guidelines-composer]] (the source-of-truth document), and the design plugin's audit skill ([[design-audit]]) for product UI specifically.

## When to use this skill

- Before a brand refresh — establishing the baseline.
- Periodic brand-health reviews (annual, biannual).
- Onboarding a new brand owner — surfacing what they've inherited.
- Pre-launch audits before a major brand application.
- Investigating "why does our brand feel inconsistent?"
- Vendor / agency review — evaluating partner deliverables for brand fit.

## What this skill is NOT

- Not the fix skill. Fixes happen in the relevant authoring skills.
- Not the design audit (product UI drift) — see [[design-audit]] for that.
- Not the writing audit — see `consulting-humanize` for AI-tic cleanup on copy.
- Not the brand strategy skill — see [[brand-strategist]] for positioning evaluation.

## What the audit checks

A complete brand audit covers six dimensions. Match the audit to the brand's actual surfaces; skip dimensions the brand doesn't have.

### 1. Strategy fit
- Does the brand application match the positioning?
- Are messaging pillars consistent across surfaces?
- Is the audience focus visible in the work?

### 2. Identity consistency
- **Logo:** correct file, correct lockup, correct minimum size, correct clearspace.
- **Color:** palette matches guidelines; functional colors used correctly.
- **Type:** correct families, correct weights, correct scale.
- **Iconography:** one family across surfaces, consistent stroke weight.
- **Photography / imagery:** consistent treatment, lighting, palette.

### 3. Voice and tone
- Voice attributes recognizable in the writing?
- Banned filler words present?
- AI-writing tics (em-dash overuse, "not X but Y," summary closers) present?
- Tone register matches situation?

### 4. Surface application
- **Web:** brand applied correctly to the website?
- **Decks:** master slides used, no per-slide creative drift?
- **Documents:** type scale consistent, citation style consistent, cover treatments on-brand?
- **Social:** platform-specific best practices? Brand mark present? Aspect ratios correct?
- **Email:** signatures, header, footer, template usage?

### 5. Cross-surface consistency
- Same brand voice across web, deck, document, social?
- Same color application logic?
- Same imagery treatment?
- Same iconography family?

### 6. Governance
- Are guidelines accessible to the team?
- Is the brand owner active?
- Are approval flows working?
- Are assets centralized?

## The audit deliverable

The audit produces a markdown document with this structure:

```
# Brand audit: {Brand} — {Date}

## Scope
- Surfaces audited: {web, decks, documents, social, etc.}
- Time window: {what samples were reviewed — past N months}
- Reviewer: {who ran the audit}

## Summary
- Overall brand-health score: {A/B/C/D/F}
- Critical issues: {count}
- High-priority issues: {count}
- Medium / low issues: {count}

## CRITICAL issues (block ship; fix before next launch)
[Each issue: what, where (with link/screenshot), why critical, fix]

## HIGH-priority issues (fix this quarter)
[...]

## MEDIUM-priority issues (fix this year)
[...]

## LOW-priority issues (track but de-prioritize)
[...]

## Recommendations
- {Prioritized list of structural changes}
- {Suggested governance changes}
- {Tooling / process changes to prevent future drift}

## Methodology
- Audit checklist used: {brand-audit standard 6-dimension}
- Samples reviewed: {N}
- References: brand guidelines version {N}, dated {date}
```

## Audit checklist

Run this checklist against the brand's actual surfaces.

### Identity gates (CRITICAL violations block ship)

- [ ] **Logo file integrity** — correct format, correct version, no recoloring.
- [ ] **Logo clearspace and minimum size** — respected on every surface.
- [ ] **Logo on-light / on-dark / on-color** — correct variants used.
- [ ] **Color palette tokens** — no off-palette colors in production deliverables.
- [ ] **Type families** — only sanctioned families in use.
- [ ] **Type weights** — only sanctioned weights in use.
- [ ] **Iconography** — one family across surfaces; no mixed stroke weights.

### Voice gates

- [ ] **Voice attributes recognizable** in current marketing copy.
- [ ] **Banned filler words absent** ("elevate," "leverage," "seamless," "unleash," "next-gen," etc.).
- [ ] **AI-writing tics absent** (em-dash overuse, "not X but Y," summary closers).
- [ ] **Tone register appropriate** for each surface (sales follow-up isn't crisis-response tone).

### Surface gates

- [ ] **Web** applies brand correctly.
- [ ] **Decks** use master slides; no per-slide drift.
- [ ] **Documents** apply type scale consistently.
- [ ] **Social** assets follow brand-approved templates.
- [ ] **Email signatures** consistent across team.

### Cross-surface gates

- [ ] **Voice consistent** across web, deck, document, social.
- [ ] **Imagery treatment consistent** across surfaces.
- [ ] **Iconography family consistent.**
- [ ] **Color application logic consistent** (e.g., accent always reserved for emphasis).

### Governance gates

- [ ] **Guidelines accessible** to team and partners.
- [ ] **Brand owner active and named.**
- [ ] **Approval workflow works** — team uses it.
- [ ] **Assets centralized** — clear "where does the team find logos / colors / templates?"

## Severity rubric

| Severity | Examples |
|---|---|
| **CRITICAL** | Logo misuse on public-facing surface. Trademark drift. Major voice violation in widely-distributed copy. Wrong brand entirely (using an old brand version). |
| **HIGH** | Off-palette colors in production decks. Mixed icon families. Missing dark-mode treatment. Banned filler-words in marketing copy. AI-writing tics throughout copy. |
| **MEDIUM** | Inconsistent type scale across documents. Outdated photography style. Drifted social aspect ratios. Inconsistent signature in team emails. |
| **LOW** | Minor naming inconsistencies. Imperfect logo clearspace in non-public deliverables. Slight color drift in internal docs. |

## Common audit findings (Slalom Composable DXP context)

When auditing Slalom-house brand application, common findings:

- **Slide-deck drift** — proposal decks across the practice using inconsistent type scales, color tokens, and slide archetypes.
- **Voice drift** — marketing copy with "leverage," "transform," "elevate" still appearing despite brand-voice rules.
- **AI-writing tics** — "not just X but Y" patterns and em-dash overuse in posts and POVs.
- **Imagery inconsistency** — some pieces using commissioned photography, others using stock, no clear rule.
- **Iconography drift** — Lucide in product UI, Material in some decks, Phosphor in others.
- **Sub-brand drift** — when a sub-brand or codename has evolved without a guidelines update.

## Audit workflow

When asked to run a brand audit:

1. **Confirm scope** — which surfaces, what time window.
2. **Pull samples** — 5–10 deliverables per surface from the time window.
3. **Run the checklist** against each sample.
4. **Categorize findings** by severity.
5. **Prioritize fixes.** Don't list 200 items. The audit's value is the prioritized top 20.
6. **Recommend structural changes** — governance, tooling, process — to prevent future drift.
7. **Hand off the deliverable** — the audit document goes to the brand owner.
8. **Recommend re-audit cadence** — typically annual; semi-annual for fast-evolving brands.

## How to engage

Ask:
- What surfaces should the audit cover?
- What's the time window for samples?
- Who is the brand owner who'll receive the audit?
- Is there a recent brand refresh I should benchmark against, or a long-running brand?
- What's the goal — pre-launch QA, periodic health-check, pre-refresh baseline?

## Common audit failure modes

| Failure | Symptom | Fix |
|---|---|---|
| **Audit lists 200 items** | Comprehensive but unactionable | Top 20 prioritized; categorize by severity |
| **Audit without prioritization** | "Here are 87 things wrong" — team can't action | Severity rubric; clear "fix first" list |
| **Audit without root-cause analysis** | Finds the symptoms but not the structural cause | Add Recommendations section with governance/process changes |
| **Audit without source-of-truth** | Auditing against memory or vague "brand feel" | Reference guidelines document with version + date |
| **Audit that judges old work harshly** | Penalizing pre-refresh deliverables | Set time window appropriate to current brand era |
| **Audit handed to too-junior owner** | Findings ignored | Audit goes to brand owner with authority to approve fixes |

## Hand-offs

- **Findings → fix:**
  - Identity drift → [[brand-identity-system]].
  - Voice drift → [[brand-voice-tone]] and `consulting-humanize`.
  - Strategy gap → [[brand-strategist]].
  - Surface-specific drift → [[design-presentation]], [[design-document]], [[design-social-asset]], [[design-screen]] in design plugin.
  - Governance gap → [[brand-guidelines-composer]] (governance chapter).
- **Source-of-truth:** [[brand-guidelines-composer]] output (the guidelines document being audited against).
- **Product UI specifically:** [[design-audit]] in design plugin.

## Constraints

- Don't audit against a guidelines version older than 12 months — re-audit the guidelines themselves first.
- Don't list more than 30 issues per audit. Prioritization is the value.
- Don't hand the audit to anyone without authority to approve fixes.
- Don't rerun the audit until the team has had time to address prior findings.
- For Slalom client work, brand audits typically go through the brand-owner role on the engagement, not the practice level.
