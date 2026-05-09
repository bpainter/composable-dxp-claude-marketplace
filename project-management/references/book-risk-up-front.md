---
type: reference
project: skills-library
scope: plugin
plugin: project-management
tags: [type/reference, plugin/project-management, scope/foundational, topic/risk-management, source/josephs-rubenstein, source/gilbert]
status: active
---

# Risk Up Front: Managing Projects in a Complex World — Adam Josephs and Brad Rubenstein (2018)

**Source digest** for the project-management plugin. Page numbers refer to the Lioncrest 2018 edition. Cite as "(Josephs & Rubenstein, *RUF*, p. X)". Note: the prompt referred to "Adam Gilbert"; the actual authors are Adam Josephs and Brad Rubenstein of Celerity Consulting Group.

## Thesis

Most project failure traces to risks knowable at the project's beginning but never surfaced — they sit in the team's "blind spot," the DKDK ("don't know you don't know") quadrant (pp. 65–69). The cost of making changes rises exponentially across the project life (the Boehm curve, p. 31), but teams are "optimistic procrastinators" whose felt urgency *also* rises with time, so they meet the rising-cost curve at the worst moment (pp. 32–37). RUF engineers urgency to the *front* — before any plan is built — by inventorying risks at kickoff with the full cross-functional team, then driving them down to a level the team can credibly *commit* to (pp. 39–46, 88–94). Four principles hold it together: **accountability**, **transparency**, **integrity**, **commitment** (p. 41). "RUF makes teams constructively paranoid about their ignorance" (p. 71). The Mars Climate Orbiter (prologue, pp. 11–14) is the canonical case: brilliant teams crash because norms didn't force early elevation of risks. RUF replaces "we'll do our best" with "it will be so, even in the face of circumstances" (p. 124).

## The Risk Up Front (RUF) process

RUF splits the lifecycle into two phases separated by a hard commitment moment (pp. 88–95, 173–179):

1. **Decide to begin** (Decision #1, pp. 174–176). Management funds a *commitment phase*, not a delivery, based on an opportunity and a named project leader. The team starts from zero: no accountability yet, no transparency yet (p. 102).
2. **Identify a cross-functional core team** that can speak to "90 percent of the deliverables and 90 percent of the risks and issues" (pp. 92, 184). Named from day one.
3. **Hold the first definition meeting** within days of start (pp. 92–94, 224–235). DKDK meeting; output is action items and risks, not a polished plan.
4. **Initiate weekly accountability meetings (WAMs)** (pp. 94, 250–270).
5. **Iterate definition meetings** (3–5 over the first 25–30% of timeline, p. 95) until the team can answer "yes" to "Is there anything that stands in the way of our commitment?"
6. **The moment of commitment** (Decision #2, pp. 176–177). Project statement signed; senior management authorizes the delivery phase.
7. **Manage in flight**: WAMs continue, RAP sheet stays live, top-two risks polled weekly (pp. 95–96).
8. **Decide to end** (Decision #3, p. 178). "Done" is binary.

## The RUF meeting structure

### Definition meeting (pp. 224–248)

- **Attendees**: entire core team mandatory; full project team midpoint and last.
- **Length**: 4 hours under ~12 people; full workday for larger.
- **Setup**: one room (or full video, never audio-only); U-shaped seating facing shared screen; designated recorder; strong facilitator with explicit permission to interrupt (pp. 220–223).
- **Core activity**: line-by-line read-through of the draft project statement. For each item, the facilitator asks three questions (p. 228): **Is it clear?** **Is it accurate?** **Does it belong here?**
- **Output**: updated project statement plus action items on the weekly schedule. Target a count ("10 action items on the board before lunch," p. 233).
- **Discipline**: don't solve issues in the room — record and move on. "Moths to a flame" tendency must be resisted (p. 223).

### Weekly Accountability Meeting / WAM (pp. 250–270)

- **Cadence**: 1 hour, mid-week, every week.
- **Agenda — exactly 4 items, in order** (p. 251):
  1. **Establish done/not done** for last week (project leader pre-collects, p. 256).
  2. **Report Weekly Percent Complete** = done/committed. Don't weight by importance (p. 259).
  3. **Confirm commitment** for next week, owner by owner.
  4. **Risk transparency**: every member names "top two risks that will cause this project to fail" (p. 261). Mandatory, not optional.
- **Escalation**: <60% one week, or <80% two weeks running, triggers senior-mgmt mini-review. "Below 80% two weeks running, shame on the team. Four weeks running, shame on management" (p. 259).

## Risk categories and what makes a "good" risk statement

RUF doesn't enumerate fixed risk categories like PMI does — every risk lives on the **RAP sheet** categorized by **track** (engineering, marketing, legal, ops, p. 188) and bound to a single accountable owner.

A good risk is written in **CEI form** (pp. 144–151):

- **Cause**: an existing, observable condition — a fact already true today
- **Effect**: a possible future bad outcome that hasn't happened yet
- **Impact**: why management should care — delay, lost feature, missed measure, dollars

Example (p. 148):
- Cause: "We have no previous experience with the selected database back-end system."
- Effect: "We may have to rebuild the database using the old back end."
- Impact: "Schedule would be delayed two weeks; we'd miss the date we promised our target customer."

**Risk vs. issue** (pp. 149–150): if the bad outcome has already happened, it's an issue, not a risk. "When the house is already on fire, you no longer have a fire risk."

The **specificity of the cause** dial (pp. 268–269): vague causes ("we don't have enough resources") fragment into specific independently-mitigable risks ("no database developer on board… nobody has JavaScript experience… one person has left").

## Risk response strategies

Mitigation is reframed from "as much as possible" to "just enough that the owner can robustly commit" (p. 154). Rule of thumb: every risk gets **at least five proposed mitigations** (p. 158). Three structural categories (pp. 156–157):

1. **Attack the cause** — change the present-tense fact (send Hervé to JavaScript camp).
2. **Attack the effect** — reduce probability of the bad outcome (vendor design review; fund a backup plan).
3. **Attack the impact** — change what would matter if it happened (shift target customer; renegotiate delivery date; drop a feature).

Two named species (pp. 273–274):

- **Contingency plan**: plan B funded *if and only if* plan A fails.
- **Backup plan**: plan A and plan B funded in parallel. Used when COBL massively exceeds duplicate-work cost.

The **chicken test** (p. 272): for risky components, invent an absurdly early simulated test — like the chicken-gun used to certify jet engines against bird strikes.

Every axis of the 5W tradeoff is a possible response — mitigate by changing target customer, dropping a feature, adding a teammate, or buying time, not just direct technical mitigation (p. 274).

## The risk register / RAP sheet structure (pp. 264–271)

A spreadsheet with two tabs: **active risks** and **closed risks**. Each row: Risk ID (Track-NN, e.g. "Database-2"), Submitted On/By, Cause, Effect, Impact, Priority, Owner (named, with consent), Mitigations (minimum five proposed), Notes.

**Priority scale — familiarity, not severity** (p. 266):

1. **Super-high** — no one anywhere has ever mitigated this before
2. **High** — our team has never mitigated this before
3. **Medium** — we've mitigated this, but not always
4. **Low** — we regularly mitigate this

Only bump priority up one level for unusual severity. Familiarity-based prioritization exists because teams are systematically too optimistic about unfamiliar risks; probability-weighted models (FMEA, FTA, CMM) "create a chicken-and-egg problem" (pp. 152–153).

A risk moves to closed only with joint agreement of project leader, submitter, and owner; any member can resubmit (p. 265).

## Continuous risk management

RUF is not a one-time kickoff exercise (pp. 95–96, 250–270):

- **Weekly** — every WAM closes with the top-two-risks round-robin. New risks immediately enter the RAP sheet.
- **Open submission** — any member, any time, "submits a RAP" when they notice a worry. Bar kept comically low ("rows in Excel are cheap; you can make more," p. 270).
- **Repeat-risk red flag** — same risk in multiple WAMs means the owner hasn't mitigated; escalate (p. 263).
- **Track-level cadence** — no track goes more than two weeks without a tracked deliverable, even QA or Legal (p. 295). "What must be true at week two, without which you'd fail at week six?"

The four RUF documents (project statement, team list with individual accountabilities, weekly schedule, RAP sheet) are kept current as "wall-ware" (p. 199), under revision control, marked DRAFT / FINAL / CURRENT-AS-OF (pp. 277–279).

## The pre-mortem connection

Klein's pre-mortem (Kahneman *TFS* pp. 313–315) is the closest cousin: assemble the team, imagine the project has failed two years from now, have each member silently write down why. RUF is structurally similar but pushes three steps further:

1. **Continuous, not one-shot.** Pre-mortem is one meeting; RUF's risk-transparency round runs every WAM for the project's life.
2. **Structural language (CEI form).** Every entry must be present-tense cause + future effect + business impact — immediately actionable, not a complaint vent.
3. **Bound to a register with named owners and 5 mitigations.** RUF binds each risk to a singular accountable owner, demands at least 5 mitigations, and routes the chosen ones onto the weekly schedule with deadlines.

RUF is what you get when you take Klein's pre-mortem, run it weekly, give it a vocabulary, and weld it to a register. Cite Klein/Kahneman for the cognitive justification; cite RUF for the operational mechanism.

## Frameworks worth pulling out

1. **5W tradeoff** (pp. 84–88) — Why / What / Who / When / Why Not. Adds customer (Why) and risk inventory (Why Not) to the classic triple constraint. Every axis change ripples; commitment requires clarity on all five.
2. **DKDK quadrant** (pp. 65–70) — KK / KDK / DKDK. Project killers live in DKDK. RUF exists to drag items from DKDK into KDK.
3. **Four principles** (pp. 41, 102–137) — Accountability ("singular ownership of a result"), Transparency ("team-wide clarity of what is so"), Integrity ("do what you say"), Commitment ("it will be so, even in the face of circumstances").
4. **Four levers of culture change** (pp. 73–80) — Language, Metrics, Structures, Practices. The dials you turn to change behavior/results/narrative.
5. **Cost of Being Late (COBL)** (pp. 49–54) — Narrative, not a number. Linear costs (lost profit/week, delayed savings, project expense) plus nonlinear (breach of contract, lost market share, business-process failure, valuation). Makes mitigation spend rational.
6. **CEI form** (pp. 144–151) — Cause + Effect + Impact, the mandatory grammar for every risk.
7. **Familiarity-based prioritization** (p. 266) — Super-high / High / Medium / Low keyed to prior mitigation experience.
8. **Action Equation** (p. 132) — Action = Clear Measurable Result + Committed Owner + Deadline + Written Down.
9. **Chicken test** (p. 272) — Absurdly early simulated test for high-risk components.

## Cross-references

**vs. Burtonshaw-Gunn (4Ts):** Tolerate ≈ Low priority + tracking; Treat ≈ attack-cause/effect; Transfer ≈ backup plan with vendor; Terminate ≈ change 5W tradeoff to remove the risk. RUF is more granular: five mitigations per risk, familiarity-based prioritization. Burtonshaw for the decision tree; RUF for the operational discipline.

**vs. Reinertsen (variability = risk):** Reinertsen prescribes WIP limits, small batches, decentralized control to buffer variability. RUF attacks upstream — most projects don't have a variability problem but a *visibility* problem. Complementary: RUF surfaces risk early; Reinertsen's flow practices buffer what remains. Both reject "as much rigor as possible" in favor of "just enough."

**vs. Perri (build-trap risks):** Perri warns about the WHY axis — shipping features without confirming customer/outcome connection. RUF's customer-measures-of-success (pp. 285–290) and primary-vs-secondary target customer distinction (pp. 287–288) are the counter-discipline. The "ice machine ICAB" story (p. 293) — Ice Chewability Approval Board — is the canonical anti-build-trap parable.

**vs. Klein/Kahneman pre-mortem:** Same cognitive bet; RUF is the operational/repeating version (see "The pre-mortem connection").

## How `project-management-raid-log` should use it

1. **CEI form as the mandatory risk schema** (pp. 144–151) — replace free-text descriptions with cause/effect/impact fields; refuse to log without all three.
2. **Familiarity-based four-point priority** (p. 266) — supplement or replace prob × impact, especially at kickoff when probability calibration is unreliable.
3. **Minimum five mitigations rule** (pp. 158, 274) — prompt for five candidates across cause/effect/impact. Breaks the "we'll just monitor it" anti-pattern.
4. **Risk vs. issue distinction** (pp. 149–150) — if effect has materialized, it belongs in the issue log. Detect tense slip ("the design isn't working") and convert.
5. **Weekly top-two risks round-robin** (p. 261) — recurring prompt for each named team member; new entries flow into the RAID.
6. **Track-tagged Risk IDs** (p. 264) — "Track-NN" so risks roll up by functional area.
7. **Resubmit-with-rebuttal mechanic** (p. 265) — any team member can reopen a closed risk with a stated reason.

## How `project-management-project-manager` should reference it

1. **5W tradeoff at engagement start** (pp. 84–88) — force explicit clarity on Why, What, Who, When, and especially **Why Not** (named risks) before any plan.
2. **Four documents discipline** (Ch. 8) — project statement (≤3 pages, signed at commitment), team list with individual accountabilities, weekly schedule, RAP sheet. Everything else is supplementary.
3. **COBL as a stakeholder conversation** (pp. 49–54) — make the sponsor articulate linear and nonlinear COBL up front. Rational basis for any later mitigation spend or scope cut.
4. **The WAM as project heartbeat** (pp. 250–270) — same hour weekly, four agenda items, 100% weekly percent complete as integrity proxy, <80%-two-weeks-running as structured escalation trigger.

## Pitfalls

- **RUF theater** — running the agenda but never recording action items, never resisting in-room problem-solving, never policing CEI form. "Excellent paperwork is the booby prize" (p. 166).
- **Late-RUF** — running the risk inventory after the plan is set and the date promised. Team is anchored, 5W tradeoffs that would have been cheap are now expensive. The whole curve argument (p. 31) is about the futility of late-RUF.
- **Shallow risks** — "we might be late," "the design might not work." Vague causes are unactionable; the specificity-of-cause practice (pp. 268–269) demands fragmentation into multiple CEI-form risks.
- **Severity-only prioritization** — prob × impact when no one has risk-type experience. Probability estimates are unreliable (p. 153). Default to familiarity.
- **Co-leadership** — "When multiple people are accountable, none are accountable" (p. 181). Partition accountabilities or designate explicit backup.
- **Risk list as excuse list** — PM privately collects worries, files them, produces post-failure as foresight. The RAP sheet is shared, weekly-reviewed, owned, tied to schedule.
- **DKDK derailed into problem solving** (p. 223) — strong facilitation must record-and-move-on.
- **"We'll do our best" culture** (pp. 121–124) — accepting "I'll try" produces ready-made excuses at the deadline.

## Pull-quotes worth keeping verbatim

- "It will be so, even in the face of circumstances." (commitment, p. 41)
- "Excellent paperwork is the booby prize." (p. 166)
- "Rows in Excel are cheap; you can make more." (p. 270)
- "When the house is already on fire, you no longer have a fire risk." (p. 150)
- "RUF makes teams constructively paranoid about their ignorance." (p. 71)
- "If there's no room for a 'no,' then a 'yes' is meaningless." (p. 134)
- "Below 80% two weeks in a row, shame on the team. Four weeks in a row, shame on management." (p. 259)
