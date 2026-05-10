---
name: solution-factory-draft
description: >
  Stage 2 orchestrator for the Solution Factory — Draft. Sequences the
  markdown-first drafting of all 8 deliverables (One-Pager, Sales Enablement Kit,
  Discovery Workshop, First Call Deck, Case Study, Email Templates, Handoff Packet,
  Marketing Handoff). Calls specialty skills from consulting, marketing, brand, cx,
  facilitation, and design plugins as needed. Pauses for review at the end of each
  deliverable (or batches review of the trio One-Pager + Sales Enablement Kit +
  First Call Deck per user preference). Output: 8 markdown files in WIP/.
  Also known as: solution drafting, deliverable authoring, draft stage runner.

type: skill
project: skills-library
plugin: solution-factory
aliases: [solution-factory-draft]
tags: [type/skill, plugin/solution-factory, scope/orchestrator, topic/draft, stage/draft]
status: active
---

## Goal

Take the user from a completed `00_Intake_Brief.md` to 8 markdown deliverables — substance right, formatting deferred. This is where most of the kit's actual work lives.

## When to invoke

- Stage 2 of the full `solution-factory-create-flow` pipeline (auto-routed)
- User runs `/solution-draft` directly
- User says "let's start drafting" / "draft the kit" / "write the deliverables"

## Required context

Read `WIP/[solution-name]/00_Intake_Brief.md` first — every draft is grounded in it. For the canonical templates and authoring rules, see `60_Digital_Manufacturing/Solution_Factory/Templates/01_*` through `08_*`. For pipeline mechanics and anti-patterns, cite `../../references/solution-factory-foundations.md`.

## The drafting sequence

Drafts in this order. The order is deliberate — each downstream deliverable propagates from upstream ones:

1. **`01_One_Pager.md`** — anchors the kit. Reframe + problem + components + Win-Results. Other deliverables reference this. **AskUserQuestion at the end:** *"One-Pager drafted. Want to review now, or continue with the Sales Enablement Kit and review them together?"*

2. **`02_Sales_Enablement_Kit.md`** — depth lives here. Battlecard, Strike Zone, Competitive Analysis, Industry Field Guides, Lead Gen Email, GTM Brief, Objection Handling, Value Map, Pricing, Engagement Sizing, Buyer Personas, Qualification Scorecard, Pursuit Health Checklist, **Engagement Handoff** section.

3. **`04_First_Call_Deck.md`** — the most important client-facing artifact. Three-act Commercial Teaching pitch (Warmer → Reframe → Rational Drowning → Emotional Impact → A New Way → Slalom). Three Reframe variants (Growth / Trouble / Even Keel). Speaker notes per slide. **Pause for review** of the trio (01 + 02 + 04) before continuing — these are the most consequential.

4. **`03_Discovery_Workshop.md`** — workshop facilitator's guide. Two outputs: sales-side (Sponsor Letter, Win-Results, draft Collaboration Plan) AND delivery-side prep packet (Block 5-layer, Kubr 5-dimension, RACI starter).

5. **`05_Case_Study.md`** — Reference Story format (Maersk pattern: Brutal Truth → Bold Vision → Seller Enablement → Quantified Result). Multiple stories OK.

6. **`06_Email_Templates.md`** — 14 templates (cold outreach × 3, lead-gen, trigger-event nurture, LinkedIn-to-1:1, post-discovery recap, post-RFP recap, milestone update, stalled-deal nudge × 2, Coach ask, Economic-Buyer access, reactivation, kickoff confirmation).

7. **`07_Handoff_Packet.md`** — 10-item kit bridging sales close → engagement kickoff. Sponsor Letter, Win-Results Chart, Stakeholder Map (4 layers), Pain Chain, PPVVCC, Reframe + Commercial Insight, Engagement shape, Open Red Flags, Coach inventory, Collaboration Plan v1.

8. **`08_Marketing_Handoff.md`** — LinkedIn post angles (3–5), thought-leadership angles (1–2), pull quotes, audience targeting, suggested cadence, hashtag strategy. Marketing_Factory handoff fields (forward-looking).

## Calling specialty skills

The Solution Factory orchestrator pulls from other plugins. Don't reimplement their expertise.

| When drafting… | Call (in order) |
|---|---|
| One-Pager (esp. Reframe + differentiators) | `consulting:consulting-management-consultant` to sharpen positioning, then `consulting:consulting-humanize` for prose pass |
| Sales Enablement Kit Battlecard | `consulting:consulting-management-consultant` (engagement scoping + stakeholder map) |
| Sales Enablement Kit Industry Field Guides | `cx:cx-product-trend-researcher` (current trend research with sources) |
| Sales Enablement Kit Discovery Questions | `cx:cx-jobs-to-be-done` (Pain Chain framing) |
| Sales Enablement Kit Pricing & Engagement Sizing | `consulting:consulting-management-consultant` (engagement-shape and pricing tiers) |
| Sales Enablement Kit Objection Handling | `consulting:consulting-negotiation-coach` (Voss + Challenger DuPont scripts) |
| First Call Deck | `consulting:consulting-sales-bd-strategist` (Challenger 6-step), then `design:design-presentation` (deck system + slide masters), `brand:brand-voice-tone` (voice per brand) |
| Discovery Workshop | `facilitation:facilitation-workshop-designer` (workshop arc), `cx:cx-jobs-to-be-done` (9-Block / Pain Chain) |
| Case Study | `consulting:consulting-management-consultant` (positioning), `consulting:consulting-humanize` (prose pass) |
| Email Templates | `marketing:marketing-copywriter` (especially for cold outreach + lead-gen variants) |
| Marketing Handoff | `marketing:marketing-social-media-marketer` (LinkedIn post shapes), `marketing:marketing-copywriter` (post copy), `marketing:marketing-geo-content` (LLM-extractable structure) |
| Anywhere prose feels AI-flavored | `consulting:consulting-humanize` |
| Anywhere voice is off-brand | `brand:brand-voice-tone` |

## Pause-and-review pattern

After drafting the trio (01 + 02 + 04), AskUserQuestion:

```
Question: "Trio drafted (One-Pager + Sales Enablement Kit + First Call Deck). Review now?"
Options:
1. Yes, review the trio before drafting the rest (Recommended)
2. Continue drafting all 8 — review at the end
3. Pause here — I want to revise the trio first
```

After all 8 are drafted, AskUserQuestion:

```
Question: "All 8 deliverables drafted. Ready to Polish?"
Options:
1. Yes, ready (Recommended)
2. Show me the cross-document consistency check first
3. Pause — revisions needed
```

If the user picks option 2, run a quick consistency sweep:

- Solution name matches across all 8?
- Reframe headline matches between One-Pager + First Call Deck + Marketing Handoff?
- Key components match between One-Pager + Sales Enablement Kit + First Call Deck?
- Proof points match between One-Pager + Sales Enablement Kit + Case Study?
- Stakeholders consistent (4 buying roles named the same way)?

Surface any mismatches as a list before handing off to Polish.

## Failure modes

- **User skips the One-Pager review.** It's the source of truth — if the One-Pager is wrong, everything downstream propagates wrong. Push back: *"The One-Pager anchors the kit. Sure you don't want to review it before we draft the others?"*
- **A draft section reads as AI-flavored prose.** Auto-route to `consulting:consulting-humanize` for a humanization pass.
- **Listen-fors didn't yield enough variants for the Sales Enablement Kit Industry Field Guides.** Surface the gap. Suggest: *"Industry Field Guides need at least 3 listen-fors per industry. We have only X. Pause to gather more, or proceed with placeholders?"*
- **Pricing tier in the Sales Enablement Kit doesn't match the Tier picked at intake.** Surface the mismatch — likely the user thought through the engagement shape after the intake. Update the intake or reconcile.
- **First Call Deck fails the "lead with vs. lead to" test.** Slalom logo / services bar appearing in slides 2–3. Auto-flag, route back through `consulting:consulting-sales-bd-strategist` for restructure.

## Token discipline

- The templates ARE the canonical content guide. Read them; don't paraphrase them. Cite section by section as you draft.
- Specialty skills (consulting, marketing, etc.) provide the substance. The Draft orchestrator's job is to call them, gather their output, and write it into the right deliverable.
- Pause early and often when content quality is questionable. Don't write 8 deliverables on a shaky One-Pager.

## Example prompts

1. *"Draft the kit for `composable-dxp-migration`."* → run the full sequence.
2. *"Just draft the One-Pager — I want to review the framing first."* → draft 01 only, pause for review.
3. *"My intake is done — let's go."* → run the full sequence.
4. *"The One-Pager looks good but the First Call Deck feels generic — redraft Acts 1–2."* → re-invoke `consulting:consulting-sales-bd-strategist` for First Call Deck Acts 1–2.
5. *"Skip the Marketing Handoff for now."* → draft 01–07, leave 08 unfilled, flag in WIP.
6. *"The Email Templates feel AI-flavored."* → re-invoke `consulting:consulting-humanize` on 06.
7. *"Run the cross-document consistency check."* → run the sweep, surface mismatches.
8. *"Drafts done — what's next?"* → AskUserQuestion about Polish readiness.

## See also

- `../../references/solution-factory-foundations.md`
- `60_Digital_Manufacturing/Solution_Factory/Templates/` — the 8 templates this stage drafts from
- `60_Digital_Manufacturing/Solution_Factory/Pipeline.md` — full pipeline doc
- `../solution-factory-create-flow/SKILL.md`
- `../solution-factory-polish/SKILL.md` — next stage
