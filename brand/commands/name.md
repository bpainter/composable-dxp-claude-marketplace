---
description: Run a naming sprint — brief, generate widely, screen, recommend three finalists with full rationale and screening notes

# Project context
type: command
project: skills-library
plugin: brand
tags: [type/command, plugin/brand]
status: active
---

Run a naming sprint for a product, company, feature, or offering.

Steps:

1. **Confirm strategic context.** If [[brand-strategist]] hasn't established positioning, pause and route there first. Naming without strategy is rationalization.

2. **Build the brief** ([[brand-naming]] Step 1):
   - What it is.
   - Who it serves.
   - What it must convey (2–3 strategic attributes).
   - Must include / must avoid.
   - Adjacent brands to avoid colliding with.
   - Geographic markets and trademark territory.
   - Domain expectations (.com required, willing to accept .ai/.co/brand-prefix).

3. **Generate widely** (Step 2): 30–50 names across all five categories (descriptive, suggestive, coined, acronym, founder/proper-name). Bias toward suggestive and coined.

4. **Group and shortlist** (Step 3): cluster by feel; trim to 8–12 with rationale per name.

5. **Screen** (Step 4): SERP collisions, exact-match domain, USPTO TESS quick search, translation hazards, pronunciation test. Note results next to each shortlist name.

6. **Recommend three finalists** (Step 5):
   - Name + category.
   - Rationale (what it earns the brand).
   - Pros.
   - Cons / risks.
   - Screening notes.
   - Tagline pairing example.

7. **Hand off** (Step 6): note that final trademark clearance requires real attorney. Recommend the next step.

Output format follows the [[brand-naming]] template:

```
# Naming exploration: {Project}

## Brief
- What it is:
- Who it serves:
- What it must convey:
- Must include / must avoid:
- Adjacent brands to avoid colliding with:
- Geographic markets:
- Domain expectations:

## Long list (30–50)
[Grouped by category]

## Shortlist (8–12)
[Each with: name, category, rationale, initial screen notes]

## Finalists (3)
For each:
- Name
- Category
- Rationale
- Pros
- Cons / risks
- SERP / domain / trademark screen notes
- Tagline pairing example
```

Reject any output that includes Acme / Nexus / SmartFlow / Cloudify / Streamline AI ([[ai-tells-forbidden-patterns]] — startup-slop names ban).
