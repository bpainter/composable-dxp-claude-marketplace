---
type: reference
project: skills-library
scope: plugin
plugin: project-management
tags: [type/reference, plugin/project-management, scope/template, topic/risk, source/josephs-rubenstein]
status: active
---

# Template — Risk Up Front (RUF)

**Source:** Josephs & Rubenstein, *Risk Up Front: Managing Projects in a Complex World* (Lioncrest, 2018) — see [`book-risk-up-front.md`](book-risk-up-front.md). Cross-references Burtonshaw-Gunn's 4Ts response and Klein's pre-mortem (familiar from Kahneman).

A structured kickoff exercise that surfaces the risks that would have killed the project — *before* the plan is built. Use it at every project kickoff, after every significant scope change, and quarterly on long-running projects.

## When to use it

- Project kickoff (before the plan is set)
- After significant scope change
- Quarterly on long-running projects
- When a project is in trouble and you suspect known risks weren't surfaced
- M&A and integration projects
- Cross-organizational projects with high coordination risk

## The RUF process

| Step | What happens | Output |
|---|---|---|
| 1. Prepare | Identify attendees, share pre-read, confirm time | Meeting invite + pre-read |
| 2. Hold the RUF meeting | Surface risks; classify; prioritize; assign mitigations | Initial risk inventory |
| 3. Output the inventory (RAP-sheet) | Document each risk with cause-effect-impact, mitigations, ownership | RAP-sheet (see structure below) |
| 4. Integrate into the plan | Bake mitigations into project plan and milestones | Updated project plan |
| 5. Manage in flight | Weekly/biweekly risk review; close, escalate, add new | Living risk log |

## The RUF meeting

**Attendees:** project sponsor, project manager, all functional leads, anyone with relevant operational knowledge. 8–12 is typical.

**Time:** 90–120 minutes. Don't compress; the surfacing time is the value.

**Facilitation:**
1. Frame the project at the start (5 min) — scope, sponsor, what success looks like
2. Brainstorm risks individually (10 min, silent) — each person writes 5–10 risks on cards/sticky notes
3. Cluster and de-dupe (15 min) — group similar risks
4. Discuss each risk (30–45 min) — the team reviews each cluster, names the cause, effect, and impact
5. Score familiarity (10 min) — for each risk, the team scores how *familiar* the risk is, not how severe
6. Prioritize and assign mitigations (15 min) — top N risks get a primary mitigation owner
7. Close with action items (5 min)

**Key facilitation moves:**
- Don't censor "small" risks early; they accumulate
- Insist on cause-effect-impact format (not "we might be late" — too shallow)
- Use the **chicken test** — when someone proposes a risk that seems too obvious or too embarrassing, ask "would we still wish we'd written it down?"
- Use a **familiarity scale** (not severity) — high familiarity = "we've seen this before and we know what to do" = low priority. Low familiarity = "we don't know how to handle this" = high priority

## Risk categories

Common categories to prompt with:

- **Scope** — ambiguity, change-control, dependencies on parallel projects
- **Schedule** — sequencing, critical path, calendar collisions, holidays
- **Resource** — availability, skill gaps, contention with other projects
- **Technical** — feasibility, integration, data quality, infra
- **Stakeholder** — alignment, communication, decision-making capacity
- **External** — vendor, regulatory, market, competitor moves
- **Organizational** — change management, capability adoption, leadership change
- **Quality** — testing coverage, acceptance criteria, post-launch support

Risks ≠ issues:
- **Risk** — something that *might* happen with negative impact
- **Issue** — something that has *already* happened with negative impact

A risk that has materialized becomes an issue. Move it to the issue log; replace with the next-level risk.

## Risk statement format

> **Cause** [event or condition] → **Effect** [what occurs as a result] → **Impact** [consequence on project outcome]

Example (good):
> "Cause: Vendor API documentation is incomplete and contradicts the contract. → Effect: Engineering build slows to documentation-by-discovery. → Impact: 4–8 week schedule slip, ~$120K in additional engineering time."

Example (bad):
> "Vendor might be slow." (No cause; vague effect; no impact.)

## Response strategies (4Ts + contingency)

For each risk, pick a primary response. Combinations are common.

| Response | Description | When |
|---|---|---|
| **Treat** | Take action to reduce probability or impact | Most common; pair with mitigation owner + due date |
| **Transfer** | Move the risk to another party (insurance, vendor, contract clause) | When you can shift accountability without losing control |
| **Terminate** | Eliminate the source — kill the dependency, change scope | When the risk is severe and the source can be removed |
| **Tolerate** | Accept the risk; do nothing | When mitigation cost > expected impact |
| **Contingency / backup plan** | What we do *if* the risk materializes | All high-impact risks should have one |

The **minimum-five-mitigations rule** (Josephs/Rubenstein): for any risk that survives the meeting, brainstorm at least 5 candidate mitigations. The first 2 are obvious; the 3rd–5th are where the good ideas live. Pick 1–2 to execute.

## The RAP-sheet (the risk register)

Each risk gets a single row in the RAP-sheet:

```markdown
# RAP-sheet — [Project Name]

| ID | Risk (cause → effect → impact) | Familiarity (1–5, 1=unfamiliar) | Severity (1–5) | Priority | Response | Primary mitigation | Owner | Due | Status |
|---|---|---|---|---|---|---|---|---|---|
| R1 | | | | | | | | | |
| R2 | | | | | | | | | |

## Risks closed (date, why)
- R3, 2026-04-12: Mitigated — vendor delivered API docs in scope. (Owner: J. Smith)

## Issues currently in flight (risks that materialized)
- I1 (was R7): [description], owner, date opened
```

Status values: `New`, `In flight`, `Mitigated`, `Closed`, `Escalated`, `Accepted`.

## Continuous risk management

After kickoff RUF:

- **Weekly stand-up review** (5 min) — any new risks? any closed? any escalations?
- **Biweekly risk review** (30 min) — full RAP-sheet walkthrough; re-score familiarity; check mitigation progress
- **Monthly leadership review** — top 3 risks escalated to the sponsor
- **Quarterly RUF refresh** — if scope has shifted significantly, run a mini-RUF

The **<80% rule** (Josephs/Rubenstein): if a risk's mitigation isn't ≥80% on track for two consecutive weeks, escalate. Don't wait for it to fail before flagging.

## Connection to the pre-mortem (Klein/Kahneman)

Gary Klein's pre-mortem is the closest analogue. The pre-mortem asks: "Imagine the project failed catastrophically. Why?" Each participant writes the failure story.

RUF and pre-mortem are complementary:
- **Pre-mortem** — works on imagination, surfaces vivid failure scenarios
- **RUF** — works on systematic enumeration, surfaces incremental risks

Run the pre-mortem first to break loose imagination, then RUF to build the structured inventory.

## Pitfalls

- **RUF theater** — going through the motions without genuine candor. The 90-min meeting is an investment; if leadership signals "don't slow us down," it'll be theater.
- **Late-RUF** — running the RUF after the plan is set. The plan should change in response to the RUF; if it can't, you skipped the integration step.
- **Shallow risks** — "we might be late" repeats across 80% of RUF outputs. Insist on cause-effect-impact.
- **Severity trap** — using severity to prioritize can hide unfamiliar risks behind familiar-but-severe ones. Familiarity is the diagnostic Josephs/Rubenstein recommend.
- **Mitigation as label** — naming a mitigation but not owning or scheduling it. Each mitigation needs an owner and a due date.
- **Risk as issue** — "the vendor was late" is an issue, not a risk. Risks are forward-looking.
- **Sponsor absent from kickoff RUF** — without the sponsor, the team can't fully name the risks (some are political; the sponsor unblocks them).
- **No re-RUF** — running RUF once and treating the inventory as static. Risk profiles shift; refresh quarterly.

## Hand-off downstream

- **Project plan integration** — every accepted mitigation becomes a milestone or task in the plan
- **RAID log** (`project-management-raid-log` skill) — RUF inventory feeds the R column of RAID
- **Sprint planning** (`project-management-sprint-planning`) — high-priority mitigations get sprint capacity
- **Stakeholder communication** — top risks become part of weekly status to the sponsor

## Skills that use this template

- `project-management-raid-log` (primary — RUF deepens the RAID R column)
- `project-management-project-manager` (engagement-level governance)
- `project-management-facilitator` (runs the RUF meeting)
- `consulting-sow-risk-advisor` (surfaces engagement-level risks for SOW change-control)
- `consulting-management-consultant` (manages the engagement-level risk view)
