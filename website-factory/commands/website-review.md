---
description: Generate the Phase 3 Review stakeholder microsite from locked Phase 2 markdown — self-contained HTML with one shared CSS file, Slalom Composable DXP secondary brand
allowed-tools: Read, Write, Edit, Glob, Grep, Bash, Task
---

# /website-review

Generate the Phase 3 Review stakeholder microsite for a Website Factory project.

## What this does

Reads locked Phase 2 markdown (CX, brand, content, design, models, requirements, measurement) plus project brand tokens and the TFIC Composable DXP design system. Generates a self-contained 12-section HTML microsite that opens by double-click on `index.html`. No build step. No dependencies. One shared CSS file.

## When to run

- Phase 2 has gated (or is effectively locked, even if STATUS.md is stale).
- You need a stakeholder-friendly surface for the client review session before Phase 4 Implementation starts.
- You ran the command before and need to regenerate after Phase 2 markdown was updated.

## Skill invoked

[`website-factory-review-microsite`](../skills/website-factory-review-microsite/SKILL.md) — the full skill documentation.

## What you'll be asked

The skill uses `AskUserQuestion` for discrete decisions:

- **Which project** (if not in context — picks from `WIP/`).
- **Phase 2 lock state** (if `_ops/STATUS.md` doesn't show Phase 2 gate passed).
- **Existing microsite handling** (regenerate vs. version vs. append vs. cancel).
- **Internal review path** (open + manual review vs. automated audit first vs. ship to client).
- **Top-up higgsfield credits** if imagery generation needs run them low.

Free-text input for: signoff text, feedback notes, signoff name + email.

## Output

Lands in `WIP/[project-code]/03-review/microsite/`:

- 13 HTML files (`index.html` + 12 sections)
- 1 shared `microsite.css`
- `assets/` folder with diagrams, imagery, screens, marks

Plus, in `03-review/`:

- `feedback.md` — empty template for the review session
- `signoff.md` — empty template for written approval

## Hand-off

When the microsite is done and the operator has run the internal review, the skill prompts for the client session. After signoff, the next action is Phase 4 Implementation (Sprint 0).

## See also

- [SKILL.md](../skills/website-factory-review-microsite/SKILL.md) — full skill detail
- [`Pipeline.md`](../../../60_Digital_Manufacturing/Website_Factory/Pipeline.md) — Phase 3 Review activity
- [`Quality_Gates.md`](../../../60_Digital_Manufacturing/Website_Factory/Quality_Gates.md) §3 — Phase 3 gate
- [`Stakeholder_Microsite.md`](../../../60_Digital_Manufacturing/Website_Factory/References/Stakeholder_Microsite.md) — structure + CSS pattern
