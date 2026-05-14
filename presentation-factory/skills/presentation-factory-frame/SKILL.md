---
name: presentation-factory-frame
description: >
  Stage 1 orchestrator for the Presentation Factory — Frame. Walks the intake
  using AskUserQuestion for discrete fields (style, brand, intent, audience,
  runtime) and conversational input for free-text (purpose, context, source
  materials). Calls the pf-brand-router for brand ingestion and the
  presentation-factory-scaffolder agent to set up WIP/[deck]/. Outputs a
  filled 00_Intake_Brief.md.
type: skill
plugin: presentation-factory
tags: [type/skill, plugin/presentation-factory, scope/orchestrator, topic/frame]
status: active
---

## Goal

Capture enough intake that the Draft stage has unambiguous direction. The output is `WIP/[deck-name]/00_Intake_Brief.md` and the brand folder populated with tokens + logo.

## When to invoke

Called from `presentation-factory-create-flow` or directly via `/presentation-frame`.

## Required context

[`../../references/presentation-factory-foundations.md`](../../references/presentation-factory-foundations.md) and [`../../references/slalom-brand-patterns.md`](../../references/slalom-brand-patterns.md).

## Steps

### Step 1 — Deck name

Ask via conversation (free-text): "What's a short slug for this deck? (kebab-case, e.g. `acme-q3-kickoff` or `composable-dxp-pitch`)"

### Step 2 — Intent

```
AskUserQuestion: "What's this deck for?"
Options:
1. Pursuit / sales presentation (going into a deal)
2. Internal — playbook, training, or operating doc
3. Conference talk or external speaking
4. Solution / practice deck (reusable)
5. Client-facing one-off (specific client, specific moment)
```

### Step 3 — Style

```
AskUserQuestion: "Which style?"
Options:
1. Informational — text-heavy, designed to read on the page (proposals, training, leave-behinds)
2. Presentational — image-led, designed to back a speaker (conferences, oral pitches, kickoffs)
```

Reference the foundations file for the trade-offs. The choice locks all template selections downstream.

### Step 4 — Brand

```
AskUserQuestion: "Which brand?"
Options:
1. Slalom (default)
2. Composable DXP (Composable Platforms practice sub-brand)
3. Client — custom brand for a specific client
```

If **Client**:
1. Hand off to `pf-brand-router` with the user's choice of ingestion path (PDF / .com crawl / manual).
2. Wait for `pf-brand-router` to populate `WIP/[deck]/brand/`.
3. Verify tokens.css and logo.svg are in place.

If **Slalom** or **Composable DXP**:
- Still hand off to `pf-brand-router` — it copies the pre-built tokens and logos.

### Step 5 — Audience

Free-text: "Who's in the room? What do they know coming in? What do they need to leave with?"

A 1-3 sentence answer is enough. Probe if too thin.

### Step 6 — Runtime & target length

```
AskUserQuestion: "Target runtime?"
Options:
1. 15 minutes — short pitch / status update
2. 30 minutes — standard executive briefing
3. 45-60 minutes — full working session / pitch with discussion
4. Longer — half-day workshop, conference keynote (you'll tell me)
```

Map to target slide count:
- 15 min → ~8-12 slides
- 30 min → ~15-20 slides
- 45-60 min → ~20-28 slides
- Workshop / keynote → conversational entry

### Step 7 — Purpose

Free-text: "In one paragraph, what does this deck need to accomplish? What's the ONE thing the audience should leave knowing or deciding?"

### Step 8 — Context & source materials

```
AskUserQuestion: "Do you have source materials to draw from?"
Options:
1. Yes — I'll paste / attach links and docs now
2. Yes — but I want you to research and gather context first
3. No — work from what you know about the topic + Slalom POV
4. I have a previous deck to refresh / extend
```

For options 2: invoke web search / fetch as needed. For option 4: locate the prior deck (in `WIP/Archive/` or the canonical destinations) and load `10_Content.md` as a starting point.

### Step 9 — Scaffold the WIP

Call the `presentation-factory-scaffolder` agent with the collected answers. The agent creates `WIP/[deck]/` and writes the seeded `00_Intake_Brief.md`.

### Step 10 — Readiness check

Verify before declaring Frame complete:
- ✅ `WIP/[deck]/00_Intake_Brief.md` exists and is filled
- ✅ `WIP/[deck]/brand/tokens.css` exists
- ✅ `WIP/[deck]/brand/logo.svg` (or `.png`) exists
- ✅ `WIP/[deck]/assets/{images,infographics,icons}/` directories exist (empty)

If anything missing, surface it and re-do the relevant step.

## Output

Path to `WIP/[deck-name]/00_Intake_Brief.md`. Return to caller (the main orchestrator) so the next stage can pick it up.
