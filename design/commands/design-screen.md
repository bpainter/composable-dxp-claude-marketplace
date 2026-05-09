---
description: Compose a new screen — calibrate via design-taste, define from design-system primitives, validate against pre-delivery checklist, hand off

# Project context
type: command
project: skills-library
plugin: design
tags: [type/command, plugin/design]
status: active
---

Compose a new screen using the design system and the anti-bland gates.

Steps:

1. **Calibrate via [[design-taste]].**
   - Set the three dials (DESIGN_VARIANCE, MOTION_INTENSITY, VISUAL_DENSITY) — read the brief or default to Slalom-house (6/4/5).
   - Run the intent-first protocol: who is this human, what must they accomplish, what should this feel like (in specific words).
   - Produce the four required outputs: Domain (5+ concepts), Color world (5+ colors from this brief's actual world), Signature (one element unique), Defaults to reject (3 obvious choices we won't make).
   - Pick a stylistic register from [[stylistic-vocabulary]].

2. **Inventory available primitives** via [[shadcn-component-inventory]] and [[shadcn-block-inventory]] (if shadcn project) or the project's own design system. Don't build from scratch what already exists.

3. **Compose using [[design-screen]]** — pull from system primitives, apply tokens, cover all required states (default, empty, error, loading, success).

4. **Run the four checks** ([[design-taste]] — swap, squint, signature, token). If any fail, iterate before showing.

5. **Run [[pre-delivery-checklist]]** — CRITICAL accessibility gates must clear; HIGH gates clear or explicitly waived.

6. **Hand off via [[design-handoff]]** — annotations explain decisions, not visuals.

7. **Surface metadata:** name, status, version, linked story, designer, last updated.

Output format:

```
# {Screen Name} — v{X.X} — {Status}
Linked: {Ticket ID} | Updated: {Date} | Designer: {Name}

## Calibration
- Dials: variance {N} / motion {N} / density {N}
- Stylistic register: {register}
- Domain: {5+ concepts}
- Signature: {unique element}
- Rejecting: {default 1 → alt}, {default 2 → alt}, {default 3 → alt}

## Screens included
- Default state
- Empty state
- Error state(s)
- Loading state
- Success / confirmation state

## Component inventory
- {list of design-system components used}

## Annotations
- {decision notes — why, not what}
- {open questions flagged}
- {deferred scope}
```
