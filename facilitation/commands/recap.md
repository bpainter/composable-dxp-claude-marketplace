---
description: Generate a within-24h workshop recap from session notes, canvas snapshots, or live capture — sponsor-signed, decisions in body, action items with owners and dates, ready to send.

# Project context
type: command
project: skills-library
plugin: facilitation
tags: [type/command, plugin/facilitation]
status: active
---

Use the `facilitation-facilitator` persona plus [`template-workshop-recap.md`](../templates/template-workshop-recap.md) to draft the recap that decides whether the workshop's outcomes survive. Pattern 261 (*Make the solution last past Act Day*) lives here. The recap is the artifact that converts a great session into actual change.

Reference: [`template-workshop-recap.md`](../templates/template-workshop-recap.md), Pattern 261, Pattern 263 (*Rituals of closure*).

Steps:

1. **Get the inputs.** Ask the user for:
   - **Session name and date**
   - **Sponsor name** (the recap goes out under their signature, not the facilitator's)
   - **Decisions made** (raw list with rationale where available)
   - **Action items** (what / who / when)
   - **Open questions / parking lot** (with owners for follow-up — never "TBD")
   - **Canvas / artifact links** (Miro, FigJam, photos of whiteboard)
   - **Recording link** (if taken, with retention rule)
   - Optional: notable moments worth calling out (a key insight, a hard pivot, a moment of consensus)

2. **Draft the recap structure** per [`template-workshop-recap.md`](../templates/template-workshop-recap.md):
   - **Subject line** — sponsor-flavored: "{Session name} — recap, decisions, and next steps"
   - **Opening** — short paragraph, sponsor voice, thanks + purpose recap (2–3 sentences max)
   - **Decisions made** — table with #, decision, owner, next checkpoint
   - **Action items** — table with what / who / by when (no "TBD")
   - **Open questions / parking lot** — items deferred with named owners and decision dates
   - **Artifacts** — links to canvas, decision records, pre-read, recording
   - **What changes for participants this week** — short paragraph naming immediate behavioral changes
   - **Next checkpoint** — date, format, who attends, what gets reviewed
   - **Thank you** — short close in sponsor voice

3. **Enforce the discipline.** Check the draft against the recap rules:
   - Within 24 hours (past 48, the room reconstructs its own version)
   - From sponsor (sponsor authority signals decisions stick)
   - Decisions in body, not attachments (people skim)
   - Action items have owners *and* dates (no "TBD")
   - One artifact link, not five (canvas as source of truth)

4. **Generate one-pager.** Output should be ready to copy-paste into email or Slack. Markdown formatting that renders cleanly in both.

5. **Decision record(s).** For each consequential decision, also produce a populated [`template-decision-record.md`](../templates/template-decision-record.md) — these live as separate artifacts the recap links to, not embedded.

6. **Calendar follow-up.** If the user hasn't already, recommend they schedule the next checkpoint *before* sending the recap so it can be referenced. Calendar invites carry weight; promises in recaps don't.

7. **Action-item routing.** Recommend entering action items into the team's actual task system (Asana, Linear, etc.) before sending the recap — and link to those tasks from the recap. Items captured only in a recap email decay within a week.

8. **Post-event ritual.** If this was a multi-day workshop, recommend the SDT post-event debrief per [`sponsor-design-team-playbook.md`](../skills/facilitation-workshop-designer/references/sponsor-design-team-playbook.md) — separate from the participant recap.

Don't write the recap from the facilitator's perspective. Sponsor voice. Sponsor signature. Otherwise the decisions read as meeting minutes and lose force.
