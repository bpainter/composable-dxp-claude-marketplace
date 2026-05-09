---
name: project-management-facilitator
description: >
  Pointer to the dedicated `facilitation` plugin. The PM-side facilitator skill has been promoted to its own plugin to accommodate the depth of source material (Collaboration Code, Facilitating Collaboration, Design Thinking Playbook, Meeting Design, Design a Better Business). For all workshop and meeting design work, use the facilitation plugin's specialists. This skill remains as a discovery aid for users who reach for "facilitation" inside project-management.

# Project context
type: skill
project: skills-library
plugin: project-management
aliases: [project-management-facilitator]
tags: [type/skill, plugin/project-management, topic/leadership, redirect]
status: active
---

## This skill is now a pointer

Facilitation work has moved to its own plugin: **`facilitation`**.

The `facilitation` plugin contains nine specialist skills, an agent, eight book digests, and nine ready-to-use templates — sized for the depth of canonical source material (Rob Evans' *Collaboration Code* series, Klein & Newman's *Facilitating Collaboration*, Lewrick et al.'s *Design Thinking Playbook*, Hoffman's *Meeting Design*, and van der Pijl/Lokitz/Solomon's *Design a Better Business*).

## Where to go

| If you want to... | Use |
|---|---|
| Headline facilitator persona | [`facilitation-facilitator`](../../../facilitation/skills/facilitation-facilitator/SKILL.md) |
| Design a single meeting (≤90 min) | [`facilitation-meeting-architect`](../../../facilitation/skills/facilitation-meeting-architect/SKILL.md) |
| Design a multi-day workshop with a Sponsor Design Team | [`facilitation-workshop-designer`](../../../facilitation/skills/facilitation-workshop-designer/SKILL.md) |
| Reference the Collaboration Code patterns | [`facilitation-collaboration-pattern-library`](../../../facilitation/skills/facilitation-collaboration-pattern-library/SKILL.md) |
| Facilitate design thinking work | [`facilitation-design-thinking-coach`](../../../facilitation/skills/facilitation-design-thinking-coach/SKILL.md) |
| Use a strategy / innovation canvas | [`facilitation-strategic-visualizer`](../../../facilitation/skills/facilitation-strategic-visualizer/SKILL.md) |
| Choose a decision-making method | [`facilitation-decision-architect`](../../../facilitation/skills/facilitation-decision-architect/SKILL.md) |
| Manage group dynamics, conflict, dominance | [`facilitation-group-dynamics-coach`](../../../facilitation/skills/facilitation-group-dynamics-coach/SKILL.md) |
| Run virtual or hybrid sessions | [`facilitation-virtual-hybrid-specialist`](../../../facilitation/skills/facilitation-virtual-hybrid-specialist/SKILL.md) |
| Sequence multi-step facilitation work | [`facilitation-agent`](../../../facilitation/agents/facilitation-agent.md) |

## What stays in `project-management`

These remain because they are PM-adjacent more than facilitation-adjacent:

- [`project-management-retro-facilitator`](../project-management-retro-facilitator/SKILL.md) — retros are PM cadence work; the retro-specific patterns and formats live with the PM plugin
- [`project-management-sprint-planning`](../project-management-sprint-planning/SKILL.md) — agile-cadence facilitation
- [`project-management-requirements-discovery`](../project-management-requirements-discovery/SKILL.md) — discovery-as-PM-work

For any other facilitation request, route to the `facilitation` plugin.

## Why the move

The previous version of this skill did decent work on facilitation principles, but the source material — eight canonical books across patterns, tools, models, casebook, design thinking, meeting design, strategy canvases, and facilitation craft — exceeded what fits comfortably as a single SKILL.md. The depth deserved its own plugin, with specialists per facet of the craft.

See the [`facilitation` plugin README](../../../facilitation/README.md) for the full structure.
