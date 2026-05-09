---
name: brand-designer
description: >
  RETIRED — split into four specialty skills. This skill no longer holds content. Use the specialty skill that matches the task. Aliases preserved for backwards compatibility — references to "brand-designer" will route here, and this redirect points users to the right new skill.

# Project context
type: skill
project: skills-library
plugin: brand
aliases: [brand-designer]
tags: [type/skill, plugin/brand, topic/brand, status/retired]
status: retired
retired-date: 2026-05-09
---

# brand-designer (retired)

This skill was previously a 313-line monolith covering naming, logo direction, color, typography, voice, and guidelines structure all in one place. **It has been split into four specialty skills plus a new audit skill** for sharper routing and easier maintenance.

## Where the content went

| Original section | New skill |
|---|---|
| Naming framework, criteria, process | [[brand-naming]] |
| Logo direction, evaluation, brief template | [[brand-identity-system]] |
| Color systems (palette building, contrast, dark mode) | [[brand-identity-system]] |
| Typography systems (display/text/mono, pairing, licensing) | [[brand-identity-system]] |
| Voice and tone framework | [[brand-voice-tone]] |
| Brand guideline document structure | [[brand-guidelines-composer]] |
| (New, didn't exist before) | [[brand-audit]] |

## How to use the new skills

For most brand work, start with [[brand-strategist]] (strategy and orchestration), then route to:

- **Naming a product, company, or feature** → [[brand-naming]].
- **Building or refreshing visual identity** (logo, color, type, photography, iconography) → [[brand-identity-system]].
- **Defining or applying brand voice** → [[brand-voice-tone]].
- **Compiling a brand guidelines document** → [[brand-guidelines-composer]].
- **Auditing existing brand application** → [[brand-audit]].

## Why this was split

The monolithic skill had four problems:

1. **Routing ambiguity.** "Brand designer" could mean naming, identity, voice, or guidelines. Specialty skills make the routing clear.
2. **Maintenance burden.** Updating one section meant scrolling 313 lines.
3. **Cross-cutting concerns missing.** Voice, identity, naming each have substantial methodologies that deserve full skill space.
4. **Stale references.** The original referenced outdated skill names. Fresh skills start clean.

## Stale references cleaned up

The original brand-designer referenced:
- `brand-design-process`, `brand-design-screen`, `brand-design-handoff`, `brand-design-audit` (renamed to current `design-*` skills in design plugin)
- `dev-design-implementation`, `dev-shadcn-component`, `dev-tailwind-tokens` (renamed to current `software-engineering-*` skills)

The new specialty skills use the current naming throughout.

## If you specifically need the old content

The pre-split version is preserved in git history (commit prior to 2026-05-09). For most uses, the new specialty skills are stricter, sharper, and better-routed.
