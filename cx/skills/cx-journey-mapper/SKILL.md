---
name: cx-journey-mapper
description: Build current-state and future-state journey maps that show what the user does, thinks, and feels across the stages of an experience — with pain points, moments of truth, opportunities, and the channels and touchpoints that matter. Use this skill any time the user is mapping an experience, debugging a stuck flow, identifying where to invest, or aligning a team on how the audience actually moves through a process — "journey map," "experience map," "where are users dropping off," "show me the funnel," even when the words aren't used. Trigger whenever the conversation crosses from "who is the user" to "how do they move through this" so the team has a shared picture of the experience instead of disconnected screens.

# Project context
type: skill
project: skills-library
plugin: cx
aliases: [cx-journey-mapper]
tags: [type/skill, plugin/cx topic/customer-experience, topic/ux-research, topic/service-design]
status: active
---

# Journey Map

A journey map is a visual model of how a user moves through an experience, stage by stage, with what they do, think, and feel at each stage. The point is not the artifact; it is the conversation the artifact produces — about gaps, drop-offs, moments of truth, and where investment lands.

Pair with [[cx-persona-developer]] (who is making the journey), [[cx-jobs-to-be-done]] (what job they're hiring the experience to do), [[cx-customer-research]] (the evidence the journey rests on), `cx-behavioral-design` (specific decision moments inside the journey), and `strategy-digital-strategist` (when journey insights feed strategy).

## When to use this skill

- Mapping the current-state experience for a key user (founder onboarding, formation, etc.).
- Designing the future-state experience for a feature, flow, or product.
- Diagnosing where a flow loses users (drop-off, hesitation, churn).
- Aligning a cross-functional team on how the audience actually behaves vs. how the team thinks they do.
- Identifying highest-leverage opportunities for investment.
- Briefing a design team before they start composing screens.

If the work is about a single decision moment, use `cx-behavioral-design`. If the work is about who the user is, use `cx-persona`. If the work is about what they're trying to get done, use `cx-jobs-to-be-done`. The journey map sits across all three.

## Two kinds of map

**Current state.** What the experience is today — based on research and behavioral evidence. The point is to surface gaps, pain, and friction. Honest current-state maps are uncomfortable; that is the signal they're real.

**Future state.** What the experience should be — based on the desired outcome. The point is to align the team on what to build.

Build current-state first whenever possible. A future-state map without a current-state foundation is a wishlist.

## Structure of a journey map

Most journey maps share a common shape:

```
┌─────────────────────────────────────────────────────────────────┐
│ Persona: [name]    Job: [JTBD reference]   Scope: [start → end] │
├─────────────────────────────────────────────────────────────────┤
│ Stage 1   │ Stage 2   │ Stage 3   │ Stage 4   │ Stage 5         │
├───────────┼───────────┼───────────┼───────────┼─────────────────┤
│ DOING     │           │           │           │                 │
├───────────┼───────────┼───────────┼───────────┼─────────────────┤
│ THINKING  │           │           │           │                 │
├───────────┼───────────┼───────────┼───────────┼─────────────────┤
│ FEELING   │           │           │           │                 │
├───────────┼───────────┼───────────┼───────────┼─────────────────┤
│ TOUCH-    │           │           │           │                 │
│ POINTS    │           │           │           │                 │
├───────────┼───────────┼───────────┼───────────┼─────────────────┤
│ PAIN      │           │           │           │                 │
├───────────┼───────────┼───────────┼───────────┼─────────────────┤
│ OPP-      │           │           │           │                 │
│ ORTUNITY  │           │           │           │                 │
└───────────┴───────────┴───────────┴───────────┴─────────────────┘
```

### The rows

- **Doing** — observable actions. "Searches Google for 'Delaware C-corp.'" Not "thinks about whether to incorporate." That's the next row.
- **Thinking** — what the user is asking themselves, considering, weighing. Phrased as user thoughts: "Is Delaware really worth it for me?"
- **Feeling** — emotional state, sometimes plotted as a line graph across stages. Not optional; emotional dips often predict drop-offs better than functional friction.
- **Touchpoints** — the channels and surfaces the user encounters at this stage. Search results, articles, friend recommendations, the formation site, a lawyer's office, etc.
- **Pain points** — specific frustrations or barriers, with severity if research supports it.
- **Opportunities** — what the team could do at this stage to remove pain, reduce friction, or create delight.

Some journeys also include rows for backstage actions (what the firm or system is doing on the other side, often called a service blueprint), KPIs (success metrics per stage), or moments of truth (the few moments that disproportionately shape the user's overall experience).

### The columns (stages)

Stages are the chronological steps of the experience. For a founder-formation journey, stages might be:

```
Awareness → Research → Decision → Action → Onboarding → Ongoing
```

Or for a more bounded flow:

```
Land on site → Take questionnaire → Receive results → Take next step
```

The stage list should match how the user *experiences* the journey, not how the firm operates internally. Test by reading the stages aloud; if they sound like the firm's phases (e.g., "lead capture, lead qualification, lead conversion"), reframe.

## Common stage patterns

Two typical shapes — broad lifecycle and bounded flow.

**Broad lifecycle (e.g., a B2B buyer's full path with a vendor):**
1. Trigger event (need surfaces; budget unlocks; old solution breaks).
2. First research (what do I even need to do? what's out there?).
3. Comparison and decision (vendor / approach selection).
4. Execution (procurement, kickoff, initial implementation).
5. Confirmation and follow-on tasks (rollout to broader org, integration with adjacent systems).
6. Ongoing operation, expansion, surprise costs.

**Bounded flow (e.g., a single-feature evaluation or onboarding):**
1. Awareness of the flow.
2. Land on the entry surface.
3. First few steps.
4. Mid-flow.
5. Submit / save progress.
6. Receive output.
7. Next step.

The right stages depend on scope. Be explicit about scope in the map header.

## How to build a journey map

```
1. Define scope. Persona, job, and start/end of the journey. Write
   these into the header. Tight scope produces a useful map; broad
   scope produces a poster nobody reads.

2. Gather research. Interviews, behavioral data, support tickets,
   analytics. The journey needs evidence, not assumption.

3. Identify stages. From the research, identify the chronological
   stages the user actually moves through. Use their language where
   possible.

4. Fill the rows for each stage. Doing, thinking, feeling, touchpoints,
   pain, opportunities. Tie each cell to evidence (interview ID,
   data source, ticket).

5. Identify moments of truth. The 2-4 stages that disproportionately
   shape the overall experience. Mark them.

6. Pressure-test. Does the journey match what the research team would
   describe? Is anything cherry-picked or missing?

7. Publish, then use it. The map is a tool; if it's not driving
   conversations and decisions in the next month, it's not earning
   its keep.

8. Re-validate. Journey maps drift. Refresh quarterly or after major
   experience changes.
```

## Future-state mapping

When mapping the future state:

- Start from the same persona and job.
- Use the same stages where possible (so current and future can be compared side by side).
- For each stage, define the *desired* user experience: what they should be doing, thinking, feeling. What touchpoints they encounter. What's been removed and what's been added.
- For each stage where current and future differ, note the *change* explicitly: what we're removing, what we're adding, what we're keeping.
- Tie changes to specific roadmap initiatives.

A future-state map is a design brief; treat it as such. It should be the source of truth for what gets built.

## Moments of truth

Most journeys have 2-4 stages that punch above their weight. Identifying them is one of the highest-value outputs of the mapping exercise.

Moments of truth tend to be:

- The first time the user encounters the brand or product.
- The moment a high-stakes decision is made.
- The moment a commitment is locked (form submitted, money paid, account created).
- The first use after committing.
- A failure mode or recovery moment.

Investment at moments of truth produces disproportionate experience improvement. Investment elsewhere is often necessary but rarely transformative.

## Common failure modes

- **Internally-framed stages.** "Lead capture, qualification, conversion." Stages should match the user's experience, not the funnel team's view.
- **Vibes-only maps.** Built from team intuition. Look thorough; don't predict behavior.
- **Decorative maps.** Heavy on icons and gradients, light on insight. The goal is decisions, not posters.
- **One map for everyone.** Different personas have different journeys. If a single map covers "all users," it's probably too generic to be useful.
- **No emotional layer.** Functional pain alone doesn't predict drop-off. Where the user feels confused, anxious, or skeptical is where they leave.
- **No opportunities row.** If the map doesn't generate ideas for what to do, it's stopped short.
- **Stages too granular or too broad.** A 14-stage journey is a checklist. A 3-stage journey is too coarse to act on. Five to seven is usually right.
- **Future state without research.** Designing the experience from the inside out. The future state still needs to honor real user behavior.

## Outputs

### Journey map artifact

The grid above, populated. For deliverable polish, render in a design tool or slide. For collaboration, a Miro / FigJam board with sticky-note rows works well.

### Pain-and-opportunity ranked list

Pulled from the map. Each pain has a stage, a severity, an evidence anchor, and a proposed opportunity. Useful as input to backlog prioritization (`pm-user-story`).

### Service blueprint companion (optional)

When the experience involves backstage actions (delivery staff, content authors, internal systems, partner integrations), pair the journey with a service blueprint that shows the backstage layer. Same column structure; additional rows for backstage actions and support processes. See [[service-blueprint]] for the format and method.

## Tone

When journey-map content gets written into briefs, decks, or shareable artifacts, run through [[consulting-humanize]] for the prose pass. Customers and end users read these maps too sometimes; condescension toward the user is the most common own-goal.

## How this skill relates to others

- Who is making the journey → [[cx-persona-developer]].
- What job they're hiring the experience to do → [[cx-jobs-to-be-done]].
- The research underneath the map → [[cx-customer-research]].
- Specific decision moments inside the journey → `cx-behavioral-design`.
- Service-blueprint extension (frontstage + backstage) → [[cx-service-designer]].
- Briefing screen design from the map → `brand-design-screen` and `brand-design-process`.
- Translating opportunities into backlog items → `pm-user-story`.

## References

- [[journey-map-template]] — reusable header, stages, rows, validation checklist. The scaffold for any new map.
- [[experience-map-vs-journey-map]] — picking the right artifact (journey vs. experience vs. blueprint vs. job map).
- [[ecosystem-mapping|Ecosystem mapping]] — system view of all actors; pairs with journey map.
- [[managing-complexity|Managing complexity]] — Norman's six tools (esp. wait design and start/end-strong rule).

## Source material

- Polaine, A., Løvlie, L., Reason, B. *Service Design: From Insight to Implementation*.
- Stickdorn, M., et al. *This Is Service Design Doing*. (See [[journey-map-template]].)
- Kalbach, J. *Mapping Experiences*. (See [[experience-map-vs-journey-map]].)
- Risdon, C. & Quattlebaum, P. *Orchestrating Experiences*. (See [[experience-map-vs-journey-map]].)
- Norman, D. *Living with Complexity*. (See [[managing-complexity]].)
- Norman, D. *The Design of Everyday Things*.
