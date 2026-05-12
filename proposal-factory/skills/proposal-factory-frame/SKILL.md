---
name: proposal-factory-frame
description: >
  Stage 1 orchestrator for the Proposal Factory — Frame. Walks the user through the
  intake using AskUserQuestion for discrete decisions and conversational input for
  free-text fields. Auto-runs the proposal-factory-rfp-parser agent if an RFP/RFQ
  PDF is attached. Captures variant + brand + companion flags + Win Theme + buyer
  state + posture + buyers + scope + pricing + team + competitors + schedule.
  Outputs a filled 00_Intake_Brief.md (and optionally 01_RFP_Parsed.md) in
  WIP/[ClientName_Descriptor]/. Calls the proposal-factory-scaffolder agent to set
  up the WIP folder structure. Routes to proposal-factory-draft when readiness
  check passes. Also known as: intake walkthrough, proposal intake, frame stage
  runner.

type: skill
project: skills-library
plugin: proposal-factory
aliases: [proposal-factory-frame]
tags: [type/skill, plugin/proposal-factory, scope/orchestrator, topic/intake, topic/frame]
status: active
---

## Goal

Capture enough about a pursuit to start drafting. Variant + brand + companion flags + Win Theme + buyer state + posture + buyers + scope + pricing + team. RFP auto-parse if applicable.

## When to invoke

- User runs `/proposal-frame`
- Called by `proposal-factory-create-flow` at Stage 1
- User says any version of: "start a new pursuit intake," "let me fill out the intake for [client]," "we got an RFP from [client]"

## Required context

See [`../../references/proposal-factory-foundations.md`](../../references/proposal-factory-foundations.md) for required intake calls, variant decision rules, companion decision rules, brand selection, RFP auto-parse, and promotion routing.

## Rounds

The intake is captured in 7 rounds. AskUserQuestion for discrete fields; conversational for free-text. Use the questions verbatim where shown.

### Round 1 — Client name + Pursuit descriptor + Variant + Brand

```
Q1: "What's the client name?"
[Free-text. Examples: AcmeCorp, GlobalBank, RetailGiant.]

Q2: "What's a short descriptor for this pursuit?"
[Free-text. Examples: DataPlatform, CloudMigration, AIStrategy.]

Q3 (AskUserQuestion): "Which output variant?"
Options:
1. SPRO — single-slide direct sell
2. Small — 12–18 slide deck
3. Large-Slides — 50–150 slide RFP-style deck
4. Large-Document — 8.5×11 brochure-style PDF

Q4 (AskUserQuestion): "Which brand?"
Options:
1. Slalom default (most pursuits)
2. Composable DXP secondary (DXP / Composable / Headless / MACH / Contentful / Vercel / Algolia / Bynder)
```

**Recommendation logic for Q3:** if the user mentions RFP/RFQ in the conversation, recommend Large. If they say "single page" or "one-pager," recommend SPRO. Otherwise, ask without recommendation.

**Recommendation logic for Q4:** if the conversation mentions DXP / Composable / Contentful / Vercel / Algolia / Bynder / Headless / MACH, recommend Composable DXP. Otherwise Slalom default.

**After Round 1:** call the `proposal-factory-scaffolder` agent to create `WIP/[ClientName_Descriptor]/` and copy the variant template + relevant section snippets + intake template.

### Round 2 — RFP attached?

```
Q5 (AskUserQuestion): "Is there an RFP / RFQ PDF for this pursuit?"
Options:
1. Yes — let's parse it now (Recommended if applicable)
2. No
3. Yes but I'll add it later (skip parsing for now)
```

**If yes:** call the `proposal-factory-rfp-parser` agent with the path to the RFP PDF. The agent produces `01_RFP_Parsed.md` with extracted requirements + draft compliance matrix + page limit + evaluation criteria + key questions.

**Pause:** show the user the parsed output summary. Confirm before continuing — sometimes the parser misses or duplicates rows.

### Round 3 — Companion flags (tentative)

```
Q6 (AskUserQuestion, multi-select): "Which companion pieces should we tentatively plan for? (You'll confirm at Polish.)"
Options:
1. POC — clickable prototype (Website Factory handoff)
2. Walkthrough video — recorded screen + voiceover
3. Microsite — static proposal companion on Vercel
4. None
```

**Recommendation logic:** use the decision matrix from [`Companions.md`](../../../../60_Digital_Manufacturing/Proposal_Factory/Companions.md) — strongest fit when variant is Large + Buyer State is Vision/Evaluation.

### Round 4 — Discrete required calls

```
Q7 (AskUserQuestion): "Dominant Buyer State (Collaborative Sale)?"
Options:
1. Latent — buyer unaware a problem exists
2. Admitted — buyer admits the problem
3. Vision — buyer is forming ideas
4. Evaluation — buyer is in formal review (high caution)

Q8 (AskUserQuestion): "Posture (Mabee)?"
Options:
1. WITH them — Slalom default joint-venture mindset (Recommended)
2. FOR them — pure execution, low ownership transfer
3. TO them — push / sell / doctor-decree
4. BY them — late-engagement delegation

Q9 (AskUserQuestion): "Primary Slalom capability?"
Options:
1. Enterprise (includes Composable DXP)
2. CX
3. Data_and_AI
4. Cloud_Build
5. (Other — Strategy / Delivery / Transformation)
```

### Round 5 — Win Theme + Deb Oler answer (free-text)

```
Q10 (conversational): "What's your Win Theme — the one-line contrarian claim that makes the buyer rethink the problem? Read it aloud; if it produces 'huh, I never thought of it that way,' it's a Win Theme. If it produces 'yes, exactly,' it's a tagline — push harder."

Q11 (conversational): "Why should this client buy from Slalom over anyone else, specifically for this pursuit? One paragraph. The unique benefit you currently underappreciate."
```

If the Win Theme is generic or the Deb Oler answer is missing, push back. Don't accept "yes, exactly" answers.

### Round 6 — Problem + scope + buyers + competitors

```
Q12 (conversational): "Client problem in their language — the pain they're living with."

Q13 (conversational): "In scope: 3–5 outcome-phrased items."

Q14 (conversational): "Out of scope: 2–3 explicit items to manage expectations."

Q15 (conversational): "Key buyers — at minimum Economic Buyer + Coach. Name + title + what makes them act + what they're under pressure on."

Q16 (conversational): "Which other firms are likely competing? Any incumbent presence?"
```

### Round 7 — Pricing + team + schedule + open questions

```
Q17 (AskUserQuestion): "Pricing model?"
Options:
1. Fixed Price
2. T&M
3. Milestone-based
4. Capped T&M
5. Hybrid

Q18 (conversational): "Indicative total + budget range if known."

Q19 (conversational): "Proposed Slalom team — Engagement Lead, Solution Architect, Tech Lead, others. Names or TBD."

Q20 (conversational): "Submission deadline + anticipated decision date + target start date."

Q21 (conversational): "Open questions you don't have answers to yet."
```

## Outputs

`WIP/[ClientName_Descriptor]/00_Intake_Brief.md` filled. Plus `01_RFP_Parsed.md` if an RFP was parsed.

The readiness check at the bottom of the intake has 11 required boxes (per the intake template). Confirm all pass before signaling readiness for Draft.

## Readiness check

After Round 7, run the readiness check:

- [ ] Variant picked
- [ ] Brand picked
- [ ] Submission deadline noted
- [ ] Win Theme written (and passes the "huh" test)
- [ ] Deb Oler answer written
- [ ] Client problem in their language
- [ ] What Slalom delivers stated
- [ ] Buyer State picked
- [ ] Posture picked
- [ ] At least Economic + Coach buyers named
- [ ] Primary capability selected

If RFP attached:
- [ ] `01_RFP_Parsed.md` reviewed
- [ ] Compliance matrix mapped

If companions flagged:
- [ ] POC: personas + journeys roughly named (full detail at Draft)
- [ ] Video: scenes roughly outlined (full detail at Draft)
- [ ] Microsite: pages roughly outlined (full detail at Draft)

## Return

When readiness check passes, return to caller (`proposal-factory-create-flow` or the user) with:

- WIP folder path
- Variant + brand + companions
- Readiness status

If failures exist, surface specifically what's missing and pause for the user to fill in.

## See also

- [`../proposal-factory-create-flow/SKILL.md`](../proposal-factory-create-flow/SKILL.md) — caller
- [`../../agents/proposal-factory-scaffolder.md`](../../agents/proposal-factory-scaffolder.md) — WIP folder setup
- [`../../agents/proposal-factory-rfp-parser.md`](../../agents/proposal-factory-rfp-parser.md) — RFP parser
- [`../../references/proposal-factory-foundations.md`](../../references/proposal-factory-foundations.md)
- [Intake_Brief.md](../../../../60_Digital_Manufacturing/Proposal_Factory/Intake_Brief.md) — intake template
