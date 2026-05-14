---
name: presentation-factory-status
description: >
  Advice skill — inspects WIP/ and Archive/ folders and reports on the
  status of decks in progress. Read-only. Shows which stage each deck is at,
  what's complete, and where artifacts live. Optionally inspects canonical
  destinations to report on recently shipped decks.
type: skill
plugin: presentation-factory
tags: [type/skill, plugin/presentation-factory, scope/advice, topic/status]
status: active
---

## When to invoke

User runs `/presentation-status` or asks any version of "where are we on the [X] deck," "what decks are in progress," "show me my drafts."

## What this skill does

Read-only. Does not modify any files.

### Step 1 — Inspect WIP/

```bash
ls -la 60_Digital_Manufacturing/Presentation_Factory/WIP/
```

For each deck folder:

| Artifact | What it means |
|---|---|
| `00_Intake_Brief.md` only | At end of Frame stage |
| `+ brand/tokens.css` | Brand ingested |
| `+ 10_Content.md` | At end of Draft stage |
| `+ assets/images/*.png` | Assets generated |
| `+ slides/*.html` | At end of Compose stage |
| `+ deck.pdf` | PDF rendered |
| `+ deck.pptx` | PPTX ready |

Map the most-advanced artifact to the stage:
- Has only intake → "Stage 1 (Frame) complete · ready to Draft"
- Has content → "Stage 2 (Draft) complete · ready to Compose"
- Has slides → "Stage 3 (Compose) complete · ready to Ship"
- Has deck.pptx → "Stage 4 (Ship) artifacts ready · awaiting promotion"

Report as a table:

```
| Deck | Stage | Last modified | Notes |
|---|---|---|---|
| acme-q3-kickoff      | Draft complete    | 3h ago | 22 slides drafted, awaiting compose |
| composable-dxp-pitch | Frame complete    | 1d ago | Brand: DXP, runtime: 30 min |
| stage-conf-keynote   | Compose complete  | 5h ago | Has all slides + deck.pdf |
```

### Step 2 — Stale check

For each WIP deck, calculate days since last modification. Flag any deck >7 days untouched as "stale — consider archiving or completing."

### Step 3 — Optionally inspect Archive/

If the user asks for recent ship history:

```bash
ls -la 60_Digital_Manufacturing/Presentation_Factory/Archive/ | head -10
```

List the 5-10 most-recently archived decks with their ship date and destination (read from `00_Intake_Brief.md` if it has the destination recorded).

### Step 4 — Optionally inspect canonical destinations

If the user asks "where's my [name] deck" and it's not in WIP, search:
- `40_Library/Decks/[name]/`
- `20_Proposals/Active/*/Final/`
- `30_Talks/*/*/`
- `10_Practice/Offerings/*/Decks/`

Return the path with a computer:// link.

## Output format

Concise table-based status. No prose preamble. Example:

```
WIP — 3 decks in progress:

| Deck                  | Stage             | Last touched | Detail |
|-----------------------|-------------------|--------------|--------|
| acme-q3-kickoff       | Draft complete    | 3h ago       | 22 slides ready to compose |
| composable-dxp-pitch  | Frame complete    | 1d ago       | DXP brand · 30 min runtime |
| stage-conf-keynote    | Compose complete  | 5h ago       | All slides + PDF rendered |

Recent ships (last 5):
- acme-discovery.pptx → 20_Proposals/Active/ACME/Final/ · 2 days ago
- dxp-2026-overview.pptx → 10_Practice/Offerings/Composable_DXP/Decks/ · 5 days ago
- …
```

## Anti-patterns

- **Don't** modify any files — this skill is strictly read-only
- **Don't** promote stale WIPs automatically — flag them and ask the user
- **Don't** descend into archive folders deeper than needed — stop at top-level metadata
