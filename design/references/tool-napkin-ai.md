---
type: reference
project: skills-library
scope: plugin
plugin: design
tags: [type/reference, plugin/design, scope/foundational, topic/design, topic/visualization, topic/tool]
status: active
---

# Tool: napkin.ai

The workflow protocol for [napkin.ai](https://www.napkin.ai). Loaded by [[design-visualization]] when the project needs generative concept diagrams.

napkin.ai is an AI tool that generates polished diagrams from text descriptions. **Web-based — no MCP, no API integration.** The workflow is: write a prompt, paste into napkin, evaluate output, iterate, export. This document captures the prompt patterns and quality gates that make napkin output land instead of getting close.

## What napkin.ai is good at

- **Concept diagrams** — frameworks, value chains, ecosystems.
- **Process diagrams** — non-technical workflow visuals.
- **Architecture sketches** — high-level system overviews for executive audiences.
- **2×2 matrices, pyramids, funnels** — common consulting visual archetypes.
- **Polish at speed** — usable visual in 5–10 minutes.

## What napkin.ai is NOT good at

- **Code-versioned diagrams** — use Mermaid.
- **Live-data charts** — use shadcn / Recharts.
- **Strict technical precision** — diagrams have visual polish but may lose technical fidelity. Verify before shipping.
- **Brand-statement marquee visuals** — commission custom.

## Account and access

napkin.ai is freemium. Free tier covers most consulting use; pro tier adds higher-res exports and custom themes. Check team subscription before assuming pro features.

## Prompt patterns

napkin.ai's prompt-to-diagram quality is highly sensitive to prompt structure. Vague prompts produce vague diagrams.

### Pattern 1: List with structure
For frameworks, hierarchies, sequences:

```
A {framework name} with {N} {elements}: 

1. {Element 1} — {short description}
2. {Element 2} — {short description}
3. {Element 3} — {short description}

Show as a {pyramid / hierarchy / sequence / value chain}.
```

Example:
```
A composable DXP value chain with 5 stages:

1. Discovery — understand customer journey
2. Architecture — design the composable platform
3. Build — implement the headless components
4. Activate — integrate, launch, measure
5. Optimize — iterate based on data

Show as a horizontal value chain with arrows.
```

### Pattern 2: Relationship map
For systems and ecosystems:

```
A diagram showing how {N} elements relate:

- {A} connects to {B} via {relationship}
- {B} connects to {C} via {relationship}
- {C} feeds back to {A} via {relationship}

Show as an {ecosystem map / relationship diagram / architecture overview}.
```

### Pattern 3: 2×2 matrix
For positioning, prioritization, frameworks:

```
A 2×2 matrix with axes:

X axis: {label} — from {low end} to {high end}
Y axis: {label} — from {low end} to {high end}

Quadrants:
- Top-right: {name} — {description}
- Top-left: {name} — {description}
- Bottom-right: {name} — {description}
- Bottom-left: {name} — {description}

Show as a 2×2 framework.
```

### Pattern 4: Process flow
For workflows, sequences:

```
A process flow with {N} steps:

1. {Step name} — {what happens}
2. {Step name} — {what happens}
3. {Step name} — {what happens}
{...}

Decision points at: {step N where branching happens}

Show as a flowchart with the decision branch.
```

### Pattern 5: Architecture overview (executive)
For high-level system diagrams:

```
An architecture diagram showing:

- {System component 1}
- {System component 2}
- {System component 3}
- {External integration 1}
- {External integration 2}

Connections:
- {Component 1} integrates with {Component 2}
- {External 1} feeds {Component 1}

Audience: executive (not engineering). Show as a clean overview, not detailed schematic.
```

## Style and theme

napkin.ai offers built-in themes (visual styles for output). Pick one per deliverable; hold across all napkin diagrams in the same artifact.

Common themes that fit Slalom-house registers:
- **Minimal flat** — fits premium-restrained, swiss-modern.
- **Hand-drawn** — fits anti-design, maximalist (rare for consulting).
- **Editorial** — fits editorial-tech.

Test 2–3 themes per project; commit to one before producing more diagrams.

## Iteration workflow

1. **Write the prompt** following one of the patterns above.
2. **Generate.** napkin produces 3–5 diagram variations.
3. **Evaluate against the brief.** Not "does this look cool" but "does this say what the audience needs to understand?"
4. **Iterate the prompt** — most "didn't quite land" outputs are a prompt problem, not a tool problem. Refine the structure or descriptions.
5. **Refine in napkin's editor** — once a generation is close, napkin lets you edit labels, colors, and structure. Use this for final tuning.
6. **Export.**

## Export discipline

napkin.ai exports to:
- **PNG** — raster, default. Use for slides and social.
- **SVG** — vector, scales cleanly. Use for documents and high-res deliverables.
- **PDF** — for inclusion in larger PDF documents.

For deck embedding: PNG at 2× the slide-display size (so 4K-equivalent quality if shown on a 4K display).

For document embedding: SVG preferred — scales cleanly and respects print resolution.

## Quality gates before shipping

Before any napkin output ships:

- [ ] **Caption added** — figure number, title, source ("Figure 3.2 — Composable DXP value chain. Source: Slalom").
- [ ] **Verified for accuracy** — napkin can produce visually polished but factually incorrect diagrams. Read every label.
- [ ] **Style consistency** — all napkin diagrams in this deliverable use the same theme.
- [ ] **Color tokens match brief** — napkin's default colors may not match the project palette. Edit if needed.
- [ ] **Type readable at deliverable's display size** — test at deck thumbnail or document zoom level.
- [ ] **Accessibility** — alt text drafted; color is not the only encoding.

## Common napkin failure modes

| Failure | Symptom | Fix |
|---|---|---|
| **Vague prompt produces vague diagram** | Output has generic labels, weak structure | Rewrite prompt with specific structure pattern |
| **Visually polished but inaccurate** | Diagram looks great but a label is wrong | Always verify labels against the source content |
| **Mixed themes across diagrams** | One napkin diagram in theme A, another in theme B | Pick one theme; regenerate |
| **Default colors clash with brand** | napkin colors fight brand palette | Edit in napkin's editor or in post-export |
| **Scale issues at deck thumbnail** | Type unreadable when slide is small | Re-export at higher resolution |
| **Used napkin when Mermaid was right** | Code-versioned source-of-truth diagram in napkin | Switch to Mermaid for engineering-grade work |

## When to abandon napkin

If the prompt has been iterated 3+ times without landing, the visual probably isn't a napkin job. Either:
- **Step down** to napkin's free-form editing (build the diagram by hand using napkin's primitives).
- **Switch to Mermaid** if the diagram is technical and structured.
- **Switch to commission** if the diagram is brand-statement.
- **Build by hand** in Figma or Keynote if no other tool is the right fit.

## Cross-references

- **Skill:** [[design-visualization]] (the workflow that uses this tool).
- **Surface contexts:** [[design-presentation]], [[design-document]] for embedding diagrams.
- **Alternative tools:** Mermaid (code-versioned), shadcn charts (data-driven), commissioned (custom marquee).
- **Pre-delivery:** [[pre-delivery-checklist]].
