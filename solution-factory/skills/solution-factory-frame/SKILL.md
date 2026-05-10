---
name: solution-factory-frame
description: >
  Stage 1 orchestrator for the Solution Factory — Frame. Walks the user through
  the intake using AskUserQuestion for discrete decisions and conversational input
  for free-text fields. Captures the 5 required calls (Reframe / Deb Oler / Buyer
  State / Posture / Tier) plus the rest of the intake (problem, components,
  capabilities, industries, buyer personas, listen-fors). Outputs a filled
  `00_Intake_Brief.md` in `WIP/[solution-name]/`. Calls the
  `solution-factory-scaffolder` agent to set up the WIP folder structure. Routes
  to `solution-factory-draft` when the readiness check passes. Also known as:
  intake walkthrough, solution intake, frame stage runner.

type: skill
project: skills-library
plugin: solution-factory
aliases: [solution-factory-frame]
tags: [type/skill, plugin/solution-factory, scope/orchestrator, topic/intake, stage/frame]
status: active
---

## Goal

Take the user from "I want to create a solution" to a filled `00_Intake_Brief.md` that passes the 13-item readiness check. Use AskUserQuestion for everything that has discrete options. Use conversational input for everything that needs the user's words.

## When to invoke

- Stage 1 of the full `solution-factory-create-flow` pipeline (auto-routed)
- User runs `/solution-frame` directly
- User says "let's do the intake" / "I want to scope a new solution" / "walk me through the intake"

## Required context

For pipeline mechanics, see `../../references/solution-factory-foundations.md` — specifically the "Five required calls" and "Brand selection" sections. For the intake template canonical structure, see `60_Digital_Manufacturing/Solution_Factory/Intake_Brief.md`.

## The intake flow

The intake breaks into **6 prompt rounds**. Each round uses AskUserQuestion (max 4 questions per call) for discrete choices, with conversational follow-ups for free-text.

### Round 1 — Solution name + Brand

```
AskUserQuestion:
  Q1: "What's the solution name?" — text input, accept "[NAME TBD]" if undecided
  Q2: "Which brand will this solution use?"
      Options:
        - Slalom default — for solutions outside the Composable DXP line
        - Composable DXP secondary (TFIC) — for DXP-line solutions (Composable, Headless, MACH, Contentful, Vercel, Algolia, Bynder)
```

**Recommendation logic for Q2:** if the solution name contains any of {Composable, Headless, DXP, MACH, Contentful, Vercel, Algolia, Bynder, modern commerce, microservices}, recommend Composable DXP secondary as the first option. Otherwise Slalom default first.

After Round 1: scaffold the WIP folder by calling `solution-factory-scaffolder` agent with the chosen name and brand. The agent creates `WIP/[solution-name]/` and copies templates.

### Round 2 — The required calls (4 of 5)

```
AskUserQuestion:
  Q1: "What's the dominant Buyer State?"
      Options:
        - Latent — buyer unaware a problem exists, or aware but living with status quo
        - Admitted — buyer admits the problem but doesn't know how to address it
        - Vision — buyer has begun forming ideas; capabilities not yet identified
        - Evaluation — buyer is in formal review of alternatives (high caution — qualify aggressively)

  Q2: "What's the Posture you want to set in pursuit?"
      Options:
        - WITH them (Slalom default — Joint Venture mindset, 50/50 social contract) — Recommended
        - TO them (push, sell, doctor model — only if crisis or expert-decree)
        - FOR them (serve, deliver — pure execution)
        - BY them (delegate — late-engagement when client capability built)

  Q3: "What Tier is this solution?"
      Options:
        - Advisory — small team, fixed scope, 4–8 weeks. Output is a deliverable (assessment, roadmap)
        - Simple — single-platform engagement, 8–16 weeks (discover → design → build → launch)
        - Complex — multi-platform, multi-stream, 4–12+ months, full Pursuit Excellence treatment

  Q4: "What's the dominant Slalom capability area?"
      Options:
        - Cloud_Build
        - CX
        - Data_and_AI
        - Delivery
        (4 options max in AskUserQuestion — if user picks "Other," surface the remaining 3: Strategy, Transformation, Enterprise)
```

### Round 3 — The Reframe + the Deb Oler answer (free-text)

After Round 2 (the discrete calls), shift to **conversational mode** for the two most important free-text fields:

> "Now the most important part — the Reframe.
>
> The Reframe is a one-line contrarian claim. Read aloud, it should produce *'huh, I never thought of it that way before'* — not *'yes, exactly.'* It's the thing that distinguishes Slalom's perspective from what every other firm is saying.
>
> What's your Reframe headline?"

Wait for user response. If the response is generic ("we transform business," "we accelerate digital"), push back: *"That's the kind of statement most firms make. Try again — what's the contrarian thing you can defend that they can't?"* If the user struggles after 2 attempts, suggest pairing with the `consulting:consulting-management-consultant` skill or pause the intake to think.

Then:

> "And the Deb Oler answer — *why should our customers buy from us over anyone else, specifically for this solution?*
>
> One paragraph. The unique benefit you currently underappreciate."

Wait for response. Test: does this terminate at a Slalom unique strength? If not, you're teaching the client into the desert (Quality Gates anti-pattern #11). Push back if needed.

### Round 4 — The problem (free-text + AskUserQuestion for industries)

> "Tell me about the problem this solves — in the *client's* language. Not what Slalom delivers; what the client is struggling with. Examples: 'marketing teams can't prove ROI,' not 'marketing challenges.'"

Wait for response.

> "And what does Slalom actually deliver? Plain-language. As if explaining to a smart colleague who's never heard of it."

Wait for response.

> "Name 3–5 key components or pillars. Each gets one line."

Wait for response. Then:

```
AskUserQuestion:
  Q1: "Which industries is this most relevant to? (pick 1–5; or Horizontal)"
      Options:
        - Financial Services
        - Healthcare
        - Retail & Consumer Goods
        - Horizontal — applies broadly
      (Multi-select; "Other" surfaces the remaining 6)
  Q2: "Are there secondary Slalom capabilities involved?" (multi-select from the 7)
```

### Round 5 — Buyer personas + listen-fors (free-text, structured)

> "Now the buyer personas. For each role — Economic Buyer, User Buyer, Technical Buyer, Coach — give me title pattern, motivator, and pressure. I'll structure it."

Walk through each role conversationally. Capture into the Intake Brief's table.

> "Now the listen-fors. These are the highest-value content in the entire kit — specific phrases or scenarios a client might say that signal this solution is relevant. Push for 8–12. Examples: *'We have 14 different customer data sources that don't talk to each other.'* *'Our AI pilots haven't moved past the pilot stage.'*"

Wait for responses. Probe for more if fewer than 8 surface naturally.

### Round 6 — Pull-forward flag + Tracking (optional, AskUserQuestion)

```
AskUserQuestion:
  Q1: "Which optional fields can you fill in now? (Multi-select)"
      Options:
        - Common objections + responses
        - Primary competitors
        - Slalom differentiators (specific to THIS solution)
        - Case studies / proof points
        - Engagement shapes / pricing
        - Strategic partner alignments
        - Market trends / data points
        - None — defer all to Draft
```

For each item the user picks, prompt conversationally for the content.

```
AskUserQuestion:
  Q2: "Tracking metadata — do you have any of these now? (Multi-select)"
      Options:
        - Salesforce Solution tag
        - Executive sponsor (name, title, email)
        - Lead SME(s)
        - None yet
```

### Output

Write all collected fields into `WIP/[solution-name]/00_Intake_Brief.md` using the canonical template format from `60_Digital_Manufacturing/Solution_Factory/Intake_Brief.md`. Mark the readiness check at the bottom — pass or fail per the 13 items.

If the check passes (all 13), report back to the user:

> "Intake complete and saved. The 13 required fields are checked. Brand: [Brand]. Tier: [Tier]. Reframe: *'[Reframe]'*. Ready to start drafting?"

Hand back to `solution-factory-create-flow` (which asks the next AskUserQuestion about Stage 2 readiness), or invoke `solution-factory-draft` directly if the user said "go straight to drafting" earlier.

If the check fails (any of the five required calls missing), report:

> "Intake captured but [field X] is still incomplete. The kit will be incoherent without it. Want to revisit it now, or pause and come back?"

Don't proceed to Draft.

## Pause-and-resume

The user may want to pause the intake mid-stream. AskUserQuestion at any point — they can step away. The Intake Brief saves what's been captured so far. When they resume:

1. Read the existing `00_Intake_Brief.md`
2. Identify which of the 13 fields are still empty
3. Resume from the first empty field

## Failure modes

- **User can't articulate the Reframe.** Don't push them through. Suggest: *"Pause here. The Reframe is the foundation; without it, every other deliverable will be generic. Want me to pull in the consulting-management-consultant skill to help frame it, or pause the intake and revisit when the Reframe is clearer?"*
- **User picks Buyer State = Evaluation.** Surface caution: *"Buyer State = Evaluation means they have a vision and are formally reviewing vendors. The kit must run Vision Reengineering, not Vision Creation. We may also need an RFP End-Around tactic. Confirm: still want to proceed?"*
- **User picks Tier without thinking through engagement shape.** If unsure, point them at `10_Practice/Offerings/Solutions/README.md` decision tree.
- **User picks both "Composable DXP secondary" brand and a non-Composable Slalom capability** (e.g., "Strategy" with no DXP anchor). AskUserQuestion: *"You picked Composable DXP secondary brand but Strategy capability without a DXP anchor — confirm? Composable DXP brand should anchor on technical Composable DXP work."*
- **Solution name conflicts with an existing WIP folder.** AskUserQuestion as described in `solution-factory-create-flow`'s failure modes.

## Collaborates with

- `solution-factory-scaffolder` (agent) — creates the WIP folder + copies templates after Round 1
- `consulting:consulting-management-consultant` — fallback when Reframe / Deb Oler stalls
- `cx:cx-jobs-to-be-done` — for problem-statement framing if user struggles
- `solution-factory-create-flow` — parent orchestrator that called this skill
- `solution-factory-draft` — next stage if intake passes

## Token discipline

- Use AskUserQuestion for **everything** that has discrete options (max 4 options + "Other" auto-included).
- Don't re-explain the pipeline — the user is in the Solution Factory project; assume they know.
- Free-text prompts should be **specific** — provide examples, push back on generic responses.
- Capture everything into the Intake Brief file *as you go*, not in one batch at the end. The intake should survive a session crash.

## Example prompts

1. *"Start a new solution intake."* → run the 6-round flow.
2. *"Help me intake the AI Transformation kit refresh."* → look for existing intake first; if found, surface; if not, run fresh intake.
3. *"I have an idea for a Composable DXP migration solution but I'm fuzzy on the Reframe."* → start the intake; pause at Round 3 with extra patience on the Reframe.
4. *"Walk me through the intake but skip the optional fields."* → run Rounds 1–5; skip Round 6.
5. *"Intake — solution name is `cms-platform-migration`, brand is Composable DXP, tier is Simple."* → use those answers for Round 1–2 and continue with the rest.
6. *"I started an intake yesterday for `composable-dxp-migration` but didn't finish."* → resume from where left off.
7. *"My intake is done — let's start drafting."* → confirm readiness check passed; hand to `solution-factory-draft`.
8. *"Just give me the intake template."* → write the unfilled `00_Intake_Brief.md` to a WIP folder; don't run the prompt flow.

## See also

- `../../references/solution-factory-foundations.md` — pipeline + 5 required calls + brand routing
- `60_Digital_Manufacturing/Solution_Factory/Intake_Brief.md` — canonical intake template
- `../solution-factory-create-flow/SKILL.md` — main orchestrator
- `../solution-factory-draft/SKILL.md` — next stage
- `../../agents/solution-factory-scaffolder.md` — agent that creates the WIP folder
