---
name: solution-factory-ship
description: >
  Stage 4 orchestrator for the Solution Factory — Ship. Runs the Quality Gates
  checklist (15 unified anti-patterns + ~10 SF-specific gates), runs the
  cross-document consistency check, then promotes via the dual routing logic
  (Composable DXP secondary → 10_Practice/Offerings/Solutions/{Tier}/[Name]; Slalom
  default → 40_Library/Solution_Briefs/[Name]). Creates the library reference stub
  for DXP solutions. Updates the practice catalog. Calls
  `solution-factory-promoter` agent for the file-system work. Output: kit at
  canonical home + (optional) reference stub + WIP archived. Also known as:
  quality gates + promote, ship runner, solution finalization.

type: skill
project: skills-library
plugin: solution-factory
aliases: [solution-factory-ship]
tags: [type/skill, plugin/solution-factory, scope/orchestrator, topic/ship, stage/ship]
status: active
---

## Goal

Take a polished kit through Quality Gates and promote it to its canonical home — single source of truth, with reference stubs only for discoverability.

## When to invoke

- Stage 4 of the full `solution-factory-create-flow` pipeline (auto-routed)
- User runs `/solution-ship` directly
- User says "ship the kit" / "promote the solution" / "run quality gates"

## Required context

For dual routing logic, see `../../references/solution-factory-foundations.md` → "Promotion routing." For the canonical Quality Gates checklist, see `60_Digital_Manufacturing/Solution_Factory/Quality_Gates.md` — 15 anti-patterns + ~10 SF-specific gates. **Don't reimplement** — walk the user through the existing checklist.

## The ship sequence

### Step 1 — Quality Gates checklist

Walk the full checklist from `60_Digital_Manufacturing/Solution_Factory/Quality_Gates.md`. For each item, mark pass / fail / n/a.

The 15 unified anti-patterns:
1. Lead with Slalom?
2. Generic "we transform" framing?
3. Capability brochure case studies?
4. Single-stakeholder assumption?
5. Optimistic forecasting?
6. Verbal commitments without artifacts?
7. Open-ended discovery?
8. Pitching Collins's eleven companies as proof?
9. Selling against EOS instead of inside it?
10. Win without Win?
11. Free consulting?
12. Boundary failure?
13. Failure to disqualify?
14. Splitting the difference?
15. Inconsistent posture across the boundary?

Plus the SF-specific gates:
- Deb Oler question answered in one sentence
- Reframe produces "huh," not "yes, exactly"
- Dominant Buyer State named, kit routes accordingly
- Four buying roles named with specific people (where known)
- Pain Chain is real (top-of-chain in client's language)
- Each deliverable points at the right framework
- Cross-document consistency
- Speaker notes complete on First Call Deck
- Handoff Packet (Template 07) complete with all 10 items
- Brand applied correctly
- Visuals are real (no placeholders)
- Tier selected
- Promotion routing correct
- Promoted folder README is hybrid format
- Practice catalog updated (Composable DXP only)
- Visual toolchain followed
- Output formats correct
- Marketing Handoff complete
- Microsite call documented (Composable DXP only)

**Rule:** any kit failing 2 or more is rejected for revision. AskUserQuestion if borderline:

```
Question: "Quality Gates: X failures. Proceed anyway, revise, or document?"
Options:
1. Revise — these are blockers (Recommended if 2+ failures)
2. Proceed and document failures (only for non-blocking — e.g., 1 minor gate)
3. Show me which gates failed and why
```

If failures: write `WIP/[solution-name]/QUALITY_GATES_FAILED.md` with the failures, then route back to the right earlier stage (Draft for content failures, Polish for visual / brand failures).

### Step 2 — Cross-document consistency check

Walk the cross-document table from `Quality_Gates.md`. Verify across all 8 deliverables:

- Solution name matches everywhere
- Tagline (Reframe headline) matches between One-Pager / First Call Deck Act 1 / Marketing Handoff
- Key components match between One-Pager / Sales Enablement Kit / First Call Deck Act 3
- Proof points match between One-Pager / Sales Enablement Kit Battlecard / First Call Deck Act 2 / Case Study
- Differentiators match between One-Pager / Sales Enablement Kit Win Themes / First Call Deck Act 3
- Stakeholders consistent (4 buying roles named the same way + Mobilizer/Talker types)

Surface mismatches; fix or accept (with documentation).

### Step 3 — Determine promotion target

Pull from `00_Intake_Brief.md`:
- Brand selected at intake
- Tier selected at intake
- Solution name (slugified for the folder name)
- Date (YYYY-MM)

```
If Brand = Composable DXP secondary:
  Canonical: 10_Practice/Offerings/Solutions/{Tier}/[Solution-Name]-[YYYY-MM]-Slalom/
  Reference stub: 40_Library/Solution_Briefs/[Solution-Name]-[YYYY-MM]-Slalom.md

If Brand = Slalom default:
  Canonical: 40_Library/Solution_Briefs/[Solution-Name]-[YYYY-MM]-Slalom/
  No stub.
```

AskUserQuestion to confirm:

```
Question: "Ready to promote [Solution Name] to {target}?"
Options:
1. Yes, promote (Recommended)
2. Show me the promotion target details first
3. Pause — I want to revise once more
```

### Step 4 — Promote via the agent

Hand off to **`solution-factory-promoter`** agent. The agent:

1. Creates the canonical destination folder
2. Copies all WIP top-level files (markdown sources + PPTX/PDF/HTML final outputs + assets/) to the destination
3. Generates the hybrid promoted folder `README.md` from One-Pager content + practice metadata frontmatter (per the format in `60_Digital_Manufacturing/Solution_Factory/Pipeline.md`)
4. **For Composable DXP solutions only:** also creates the single-file reference stub at `40_Library/Solution_Briefs/[Solution-Name]-[YYYY-MM]-Slalom.md` (delegates to `solution-factory-stub-generator` agent)
5. Returns the canonical path and (if applicable) stub path to this orchestrator

### Step 5 — Update practice catalog (Composable DXP only)

If the solution promoted to `10_Practice/Offerings/Solutions/{Tier}/`, edit the catalog at `10_Practice/Offerings/Solutions/README.md`:

- Find the relevant table row (e.g., the row matching this solution's name in the right tier section)
- Replace `_to be authored_` with a link to the new folder
- If the solution name doesn't match an existing catalog entry, AskUserQuestion: *"This solution doesn't match an existing catalog entry. Add as new, or pick the closest existing entry to associate with?"*

### Step 6 — Tag in Salesforce + final wrap

- Remind user to tag the Salesforce Solution per `50_Knowledge/Frameworks/Go_to_Market/`. Surface the SF Solution tag from `00_Intake_Brief.md` if captured at intake.
- AskUserQuestion: *"Solution promoted. Archive or delete the WIP folder?"* with Archive / Delete / Keep options. Default Archive (move to a local archive subfolder).

### Step 7 — Final report

Produce a single-message summary:

```
✓ Solution promoted: [Solution Name]
✓ Canonical home: {path}
✓ Reference stub (if DXP): {path}
✓ Practice catalog updated (if DXP): {row}
✓ Salesforce Solution tag: {tag — reminder to apply}
✓ WIP archived: {action}
```

## Failure modes

- **Quality Gates fail at 2+.** Don't proceed. Write `QUALITY_GATES_FAILED.md`; route back to earlier stage.
- **Cross-document consistency mismatches.** Show the user the table; offer to auto-propagate from the One-Pager (the source of truth) or fix manually.
- **Target folder already exists.** AskUserQuestion: versioned suffix (`-v2`), replace, or pick a new name?
- **Practice catalog has no matching entry.** Surface to user. Offer to add as new entry.
- **Filesystem permission error during promotion** (OneDrive, etc.). Surface clearly; suggest manual move with the canonical path printed.
- **Reference stub creation fails for DXP solution.** Don't proceed silently — DXP solutions MUST have a stub for discoverability. Re-attempt or prompt manual creation.

## Collaborates with

- `solution-factory-promoter` (agent) — does the file-system work
- `solution-factory-stub-generator` (agent) — emits the reference stub for DXP solutions
- `solution-factory-create-flow` — parent orchestrator that called this skill
- `consulting:consulting-humanize` — fallback if any deliverable fails Quality Gate "AI prose"

## Token discipline

- Don't re-walk the templates' Quality Gates inline. Reference them.
- The promotion logic lives in foundations. Don't restate it.
- Use AskUserQuestion ONCE per major confirmation; don't re-ask after the first decision.

## Example prompts

1. *"Ship the kit."* → run all 7 steps.
2. *"Run Quality Gates only — don't promote yet."* → Step 1 + Step 2 only.
3. *"Promote `composable-dxp-migration` — gates already passed."* → skip to Step 3.
4. *"What's my promotion target?"* → Step 3 only, surface the answer without action.
5. *"This is a Slalom default solution — promote to 40_Library."* → confirm intake brand, run all steps for the Slalom default route.
6. *"The catalog row for CMS Migration says `_to be authored_` — link to my new folder."* → run Step 5 in catalog-update-only mode.
7. *"Quality Gates failed on anti-pattern #4 — fix the stakeholder map."* → route back to Stage 2 Draft for the Sales Enablement Kit stakeholder section.
8. *"Ship done — anything else I should do?"* → surface SF tag reminder + WIP archive action.

## See also

- `../../references/solution-factory-foundations.md` — promotion routing
- `60_Digital_Manufacturing/Solution_Factory/Quality_Gates.md` — the canonical checklist
- `60_Digital_Manufacturing/Solution_Factory/Pipeline.md` — Stage 4 + promoted folder README format + library stub format
- `../solution-factory-create-flow/SKILL.md`
- `../../agents/solution-factory-promoter.md`
- `../../agents/solution-factory-stub-generator.md`
