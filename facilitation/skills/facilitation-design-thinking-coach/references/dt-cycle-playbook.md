---
type: reference
project: skills-library
scope: skill
plugin: facilitation
parent: "[[facilitation-design-thinking-coach]]"
tags: [type/reference, plugin/facilitation, scope/skill, topic/design-thinking]
status: active
---
# DT cycle playbook — phase by phase

Working facilitator's notes for each of the six DT phases. Pulls from Lewrick et al. (Wiley, 2018) and the Stanford d.school canon.

## 1. Understand the problem

### Goal
Reframe the problem as it was handed to you. The framing the team starts with is almost never the one they should design against.

### Time budget
Half a day to two days, depending on complexity.

### Key moves
- **5 Whys** — push past surface symptoms
- **Create the Problem / Create the Gap** — externalize current vs. desired state on a wall
- **Stakeholder Map** — who is affected, who decides, who pays
- **Boundary Conditions** — what's in, what's out, what's a constraint, what's a wish

### Common failure mode
Skipping this phase and accepting the brief as written. Symptom: ideation produces obvious ideas because the problem framing was generic.

### Facilitator's mantra
"Have we earned the right to ideate yet?"

---

## 2. Empathize with users

### Goal
Build evidence-based understanding of the user's reality — their context, their jobs, their pains, their gains, their language.

### Time budget
1–2 weeks for substantial empathy work; 2–3 days compressed.

### Methods
- **Empathic Interview** — 60–90 min, semi-structured, follow emotion not script
- **Empathic Observation** (What / How / Why) — observe the user in context; record what they do (What), how they do it (How), and why (your inference, to be tested)
- **Diary studies** — user-self-reported behavior over time
- **Shadowing** — present-but-quiet observation
- **Customer Journey audit** — walk the user's actual end-to-end experience yourself

### Synthesis
Within 24 hours of each interview, debrief:
- **What surprised us?**
- **What contradicted what we thought?**
- **What patterns are emerging?**
- **What language did they use?** (Use their words in subsequent artifacts)

After 5–10 interviews, **Affinity Mapping** to surface themes.

### Common failure mode
- Confirmation bias — interviewing only people who'll agree with the team's existing belief
- Leading questions — "wouldn't it be great if X?" instead of "tell me about the last time you Y"
- Skipping observation — relying only on stated behavior, missing the gap between what people say and what they do

### Facilitator's mantra
"What surprised us?"

---

## 3. Define the focus

### Goal
A Point of View statement specific enough to design against. The bridge from research to ideation.

### POV structure
> **User X** (specific persona) **needs Y** (specific need rooted in observation) **because Z** (insight that explains why).

### What makes a good POV
- **Specific** — not "users" but "first-time customers in their first 30 days"
- **Evidence-based** — every claim traces to an interview, observation, or artifact
- **Surprising** — if it sounds like the original brief, the empathy work didn't go deep enough
- **Actionable** — specific enough that you can design for it

### Bad POV
> "Users need a better experience because they want it to be easier."

### Good POV
> "First-time customers in their first 30 days need permission to ask basic questions because the onboarding language assumes expertise they don't have, and they would rather quietly fail than admit confusion."

### HMW (How Might We) prompts
Convert POVs to ideation kickers framed as questions:

> "How might we give first-time customers permission to ask basic questions during onboarding?"

Generate 5–10 HMWs from a strong POV; the variety unlocks different solution spaces.

### Facilitator's mantra
"Whose problem is this and why does it matter to them?"

---

## 4. Ideate

### Goal
Generate solutions at volume, then narrow with structure.

### Structure
1. **Frame** — name the HMW; remind everyone of the rules (no judgment, build on, quantity over quality)
2. **Silent generation** — 10 minutes alone, sticky notes, one idea per
3. **Share-out** — round-robin, no critique yet
4. **Build-on** — second silent round prompted by what was shared
5. **Cluster** — affinity-map the ideas
6. **Reframe and repeat** — change the prompt and go again

### Volume target
100+ ideas per HMW. Quantity is a discipline; teams default to 10–15 and stop.

### Selection
**Avoid consensus and HiPPO.** Use:
- **Dot voting** — every person, N votes (where N = total ideas / 5)
- **Criteria-based selection** — apply Design Criteria explicitly
- **NUF test** — New, Useful, Feasible (each idea scored 1–5 on each)
- **2x2 matrix** — high impact / low effort quadrant

### Common failure mode
- Stopping at the first 10 ideas
- Letting the HiPPO pull the room
- Skipping reframes — single-prompt ideation produces narrow output
- Over-criticizing during generation (kills divergence)

### Facilitator's mantra
"Build on, don't shoot down."

---

## 5. Prototype

### Goal
Build to think. The prototype is a conversation piece, not a deliverable.

### Match fidelity to learning goal

| Learning goal | Fidelity |
|---|---|
| Concept resonance — does this idea make sense? | Sketches, storyboards, paper |
| Usability — can they use it? | Wireframes, clickable prototypes |
| Technical feasibility — can we build it? | Working code, narrow scope |
| Business viability — does it make sense as a business? | Financial model, market test, smoke test |

### Time budget
- **Concept prototype**: 1–2 hours
- **Usability prototype**: 1–3 days
- **Working prototype**: 1–2 weeks
- **Pilot**: 4+ weeks

### Common failure mode
- Building too high-fidelity too early — the prototype becomes a deliverable and the team becomes attached
- Building too low-fidelity for the learning goal — paper is fine for concept, useless for usability
- Building too many — pick 2–3 concepts to prototype, not all of them

### Facilitator's mantra
"What are we trying to learn?"

---

## 6. Test

### Goal
Test for falsifiability. Design tests that can prove the prototype wrong.

### Three lenses
- **Desirability** — do users want it? Test with users.
- **Feasibility** — can we build it? Test with engineering.
- **Viability** — does it make business sense? Test with finance.

### Test design
- **What we're testing** — specific claim ("first-time users will complete onboarding without asking for help")
- **Who we're testing with** — same persona as the empathy work
- **How we'll know we're wrong** — falsifiable threshold ("if <60% complete, the prototype fails")
- **Synthesis** — what we learned, what changes, what to test next

### Loop-back rules
- Failed desirability test → loop back to define or empathy
- Failed feasibility test → loop back to ideate (different solutions)
- Failed viability test → loop back to define (wrong problem framing)

### Common failure mode
- Testing for confirmation, not falsifiability ("did they like it?")
- Testing with the wrong users
- Skipping synthesis — running tests but not deciding what they mean

### Facilitator's mantra
"How will we know we're wrong?"

---

## When to break the cycle

The cycle is not sacred. Break it when:

- The team has clear evidence the user need is solved — go to delivery (Agile)
- The team has a tested hypothesis ready to scale — go to validation (Lean)
- The problem framing is fundamentally wrong — abandon the cycle and re-scope
- The team is over-iterating and producing nothing — set a hard deadline and converge

DT is iterative, not infinite. Three loops is usually the limit before diminishing returns.

## Cross-references

- [`book-design-thinking-playbook.md`](../../../references/book-design-thinking-playbook.md) — full digest
- [`book-design-a-better-business.md`](../../../references/book-design-a-better-business.md) — canvases for empathy and ideation
- [`book-collaboration-code-tools.md`](../../../references/book-collaboration-code-tools.md) — Empathy Map, Empathic Interview, Hassle Map, Prototyping, Magic Buttons modules
- [`book-lean-agile-design-thinking.md`](../../../../project-management/references/book-lean-agile-design-thinking.md) — methodology choice
