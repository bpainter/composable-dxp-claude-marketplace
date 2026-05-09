---
description: Audit an existing brand application across surfaces — strategy fit, identity consistency, voice and tone, surface application, cross-surface, governance — produce prioritized fix list

# Project context
type: command
project: skills-library
plugin: brand
tags: [type/command, plugin/brand]
status: active
---

Audit an existing brand application against guidelines. Read-only — produces a prioritized remediation plan; doesn't fix issues directly.

Steps:

1. **Confirm scope** ([[brand-audit]] step 1):
   - Surfaces audited (web, decks, documents, social, email, etc.).
   - Time window for samples (past N months).
   - Reviewer + brand owner.

2. **Pull samples.** 5–10 deliverables per surface from the time window.

3. **Run the 6-dimension audit** ([[brand-audit]] checklist):
   - **Strategy fit** — does brand application match positioning?
   - **Identity consistency** — logo, color, type, iconography, photography across surfaces.
   - **Voice and tone** — voice attributes recognizable; banned filler words absent; AI-writing tics absent.
   - **Surface application** — web, decks, documents, social, email all on-brand.
   - **Cross-surface consistency** — voice / treatment / iconography consistent across surfaces.
   - **Governance** — guidelines accessible, brand owner active, approval flows working.

4. **Cite specific bans** ([[ai-tells-forbidden-patterns]]) when finding violations:
   - Color: LILA ban, pure-black ban, neon-glow ban.
   - Typography: Inter ban (for premium briefs), oversized H1, modular-scale absence.
   - Layout: centered-hero, 3-column card row, anti-card-overuse.
   - Voice: filler-words bans (elevate, leverage, seamless, unleash, etc.), AI-tic bans (em-dash overuse, "not X but Y").
   - Surface-specific: deck five-pillar slide, document executive-summary-that-summarizes, social teal-quote-card.

5. **Categorize findings by severity:**
   - **CRITICAL** — block ship. Trademark drift, logo misuse on public surface, major voice violation in widely-distributed copy.
   - **HIGH** — fix this quarter. Off-palette colors in production, mixed icon families, banned filler-words in marketing copy.
   - **MEDIUM** — fix this year. Inconsistent type scale, outdated photography, signature inconsistency.
   - **LOW** — track but de-prioritize. Minor naming inconsistencies, slight color drift in internal docs.

6. **Output prioritized fix list** — top 20 items maximum, not 87. The audit's value is the prioritization.

7. **Add Recommendations.** Structural changes — governance, tooling, process — to prevent future drift.

8. **Recommend re-audit cadence.** Typically annual; semi-annual for fast-evolving brands.

Output format:

```
# Brand audit: {Brand} — {Date}

## Scope
- Surfaces audited: {list}
- Time window: {past N months}
- Reviewer: {name}

## Summary
- Overall brand-health score: {A/B/C/D/F}
- Critical: {N} | High: {N} | Medium: {N} | Low: {N}

## CRITICAL (block ship; fix immediately)
[Each: what, where, why critical, fix path → which skill or command resolves it]

## HIGH (fix this quarter)
[...]

## MEDIUM (fix this year)
[...]

## LOW (track)
[...]

## Recommendations
- {Structural changes}
- {Governance changes}
- {Tooling / process changes}

## Methodology
- Audit checklist: brand-audit standard 6-dimension
- Samples reviewed: {N}
- References: brand guidelines version {X.X}, dated {YYYY-MM-DD}

## Next audit
- Recommended date: {YYYY-MM-DD}
- Recommended owner: {role}
```

Findings → fix routing:
- Identity drift → [[brand-identity-system]] or `/brand:identity`.
- Voice drift → [[brand-voice-tone]] or `/brand:voice` + `consulting-humanize`.
- Strategy gap → [[brand-strategist]].
- Surface-specific drift → `/design:audit-system` for the surface.
- Governance gap → [[brand-guidelines-composer]] (governance chapter).
