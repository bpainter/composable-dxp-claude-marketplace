---
name: design-visualization
description: >
  Pick the right tool and format for any visualization need — diagrams (architecture, flow, process, system), data visualizations (charts, graphs, dashboards), conceptual visuals (frameworks, models, value chains). Routes by job: napkin.ai for generative concept diagrams, Mermaid for code-versioned flow diagrams, recharts/Chart.js/shadcn charts for live data, hand-built SVG for marquee custom visuals. Owns the "is this the right kind of visual?" decision before any tool gets opened. Use this skill any time a deliverable needs a diagram, chart, or conceptual visual.

# Project context
type: skill
project: skills-library
plugin: design
aliases: [design-visualization, visualization, diagrams, dataviz]
tags: [type/skill, plugin/design, topic/design, topic/visualization, topic/dataviz]
status: active
---

# Design Visualization

Most visualizations get reached for too quickly: a Slack DM mentions "we need a diagram" and someone opens PowerPoint shapes. The result is a boxes-and-arrows mess that takes 90 minutes and looks like every consulting deck.

This skill is the routing layer. Given a visualization need, pick the right tool — napkin.ai, Mermaid, shadcn/Recharts, or commissioned SVG — based on what the visual has to do, who reads it, and how often it'll change.

Pair with [[design-taste]] (load first), [[shadcn-chart-inventory]] (when chart selection is the question), the surface skills for context.

## When to use this skill

- Architecture diagrams (system topology, data flow, integration patterns).
- Process diagrams (workflow, sequence, RACI, swim lane).
- Conceptual frameworks (2×2 matrix, pyramid, value chain, ecosystem map).
- Data visualizations (charts, dashboards, KPI displays).
- Hero visuals for decks, documents, social assets.
- Reviewing existing visualizations for clarity and craft.

## What this skill is NOT

- Not the dataviz technique skill — for chart-type selection within shadcn, see [[shadcn-chart-inventory]].
- Not the chart implementation skill — for code, see `software-engineering-frontend-developer`.
- Not the imagery skill — for photography and generated imagery, see [[design-imagery]].
- Not the brand identity skill — visualizations apply identity but don't define it.

## The four tools (decision tree)

### 1. napkin.ai (generative concept diagrams)
**[napkin.ai](https://www.napkin.ai)** is an AI tool that generates diagrams from text descriptions. Strong for **concept diagrams** — frameworks, value chains, ecosystems, simple architecture sketches.

**Reach for napkin.ai when:**
- The diagram is a one-off (won't be re-rendered with different data).
- The audience values visual polish over technical precision.
- A concept needs to feel approachable rather than schematic.
- Time-to-good is short (a usable visual in 5 minutes).

**Don't use napkin.ai when:**
- The diagram has to track a code-versioned source of truth (use Mermaid).
- The data behind the visual is dynamic (use a chart library).
- The visual needs strict technical precision (commission custom).

**Workflow:** see [[tool-napkin-ai]].

### 2. Mermaid (code-versioned flow diagrams)
**[Mermaid](https://mermaid.js.org)** is markdown-style syntax for diagrams. Diagrams live in `.md` files, version-controlled, rendered to SVG.

**Reach for Mermaid when:**
- The diagram is engineering-grade and tracks a versioned source of truth.
- The diagram lives in technical documentation.
- The audience is technical and trusts the precision.
- The diagram needs to be updated as the code/architecture changes.

**Mermaid diagram types:**
- **Flowchart** — process, decision flows.
- **Sequence diagram** — interactions over time (API calls, user flows).
- **Class diagram** — object models.
- **Gantt** — timelines.
- **State diagram** — state machines.
- **Entity-relationship** — data models.
- **User journey** — UX flows.
- **C4** — software architecture (via plugin).

**Don't use Mermaid when:**
- The audience is executive and values visual polish over precision.
- The diagram is a one-off concept visual (use napkin.ai or commission).

### 3. shadcn / Recharts / Chart.js (live data visualizations)
For **chart-driven** visualizations backed by data. See [[shadcn-chart-inventory]] for the catalog.

**Reach for shadcn charts when:**
- The visual is a chart (line, bar, area, pie, radar, radial).
- The data is dynamic or might update.
- The deliverable is a product UI.

**For static deliverables (decks, documents):**
- Generate the chart in code or in a chart tool.
- Export as PNG or SVG.
- Embed in the deliverable.

### 4. Commissioned SVG (marquee custom visuals)
For **flagship deliverables** where the visualization is a brand statement.

**Reach for custom commission when:**
- The visual is the centerpiece of a flagship deliverable (a whitepaper hero, a deck cover).
- The brief calls for a unique visual style that no tool produces by default.
- The visual will be reused across multiple deliverables.

**Don't reach for custom when:**
- Time and budget aren't there. Custom commission is real designer time.
- A napkin.ai or Mermaid diagram would land just as well.

## Decision tree

```
What does the visualization show?
├─ Architecture, system, integration topology
│   ├─ Engineering audience, code-versioned → Mermaid (C4 or flowchart)
│   ├─ Executive audience, one-off → napkin.ai or commissioned
│   └─ Marquee deliverable → commissioned SVG
├─ Process / workflow / sequence
│   ├─ Engineering precision needed → Mermaid (sequence or flowchart)
│   └─ Executive concept → napkin.ai or commissioned
├─ Conceptual framework (2×2, pyramid, value chain, ecosystem)
│   ├─ One-off in a deck/doc → napkin.ai
│   └─ Marquee → commissioned
├─ Data visualization (chart driven by data)
│   ├─ Live in product UI → shadcn/Recharts (see shadcn-chart-inventory)
│   ├─ Static in deck/doc → render with shadcn/Recharts → export PNG/SVG
│   └─ Custom data viz needing unusual format → commissioned
└─ Hero visual / brand statement
    └─ Always commissioned (or higgsfield-generated — see design-imagery)
```

## Diagram archetype guide

| Archetype | Best tool |
|---|---|
| Composable architecture (system topology) | Mermaid C4 (technical) or napkin.ai (executive) |
| Data flow / API call sequence | Mermaid sequence diagram |
| Customer journey | Mermaid user-journey or commissioned |
| Process / workflow | Mermaid flowchart |
| Decision tree | Mermaid flowchart |
| 2×2 matrix | napkin.ai or hand-built (Figma/Keynote) |
| Pyramid | napkin.ai or hand-built |
| Value chain | napkin.ai or commissioned |
| Org chart | Mermaid or hand-built |
| Roadmap / Gantt | Mermaid Gantt or specialized tool |
| Capability map | napkin.ai or hand-built |
| Ecosystem map | napkin.ai or commissioned |
| RACI matrix | hand-built table (not a diagram) |
| Bento grid (composed metrics) | shadcn (built from primitives) |

## How to engage

When asked to make a visualization:

1. **Calibrate** ([[design-taste]]): audience, intent, deliverable surface.
2. **Pick the tool** using the decision tree above. Don't open a tool before this is settled.
3. **For napkin.ai:** see [[tool-napkin-ai]] for the prompt protocol and workflow.
4. **For Mermaid:** write the syntax in a `.md` or `.mmd` file. Version-control. Render via VSCode preview or Mermaid Live Editor.
5. **For shadcn charts:** pick a chart type from [[shadcn-chart-inventory]]. Implement in code or pre-render to PNG/SVG for static deliverables.
6. **For commission:** brief the designer following [[brand-identity-system]] brief template if relevant.
7. **Verify clarity.** Show the visual to one person who isn't on the project. Can they understand it in 30 seconds? If not, simplify.
8. **Caption it.** Every visual gets figure number, title, source — see [[surface-document-letter]] for caption format.

## Common visualization failure modes

| Failure | Symptom | Fix |
|---|---|---|
| **Boxes-and-arrows mess** | 12 boxes, 18 arrows, no clear flow | Simplify — pick one focal pathway, fade or remove the rest |
| **AWS-cloud-explosion cliché** | "Standard architecture diagram" with all the AWS service icons | Custom diagram earns more |
| **Pie chart with 8 slices** | Unreadable proportional comparison | Switch to horizontal bar |
| **Vertical bar chart with cramped labels** | Category labels rotated 45°, illegible | Switch to horizontal bar |
| **Diagram without caption** | Visual hangs in the page with no figure number / source | Caption with figure number, title, source |
| **Visualization that needs a key** | "What does the orange triangle mean?" | Embed labels directly; eliminate keys when possible |
| **Mismatched diagram styles in one deliverable** | One Mermaid + one PowerPoint shapes diagram + one napkin.ai diagram all on adjacent pages | Pick one style; redo the others |
| **Chart for the sake of chart** | Three numbers presented as a chart instead of a sentence | Use prose ("Engagement rose 42% over Q3") instead of a chart |
| **Overdesigned data viz** | 3D bars, drop shadows, gradient fills | Strip decorations; data over decoration |
| **Underspecified colors** | Color palette random across visualizations in same deliverable | Define color tokens once; apply across all visualizations |

## Hand-offs

- **`tool-napkin-ai`** in `design/references/` — for the napkin.ai workflow protocol.
- **[[shadcn-chart-inventory]]** — for chart type selection.
- **[[design-imagery]]** — for hero imagery and generated visuals (separate from data visualization).
- **`software-engineering-frontend-developer`** — for chart implementation in code.
- **[[design-presentation]]** / [[design-document]] — for embedding visualizations in surface deliverables.

## Constraints

- Don't open a tool before the right tool is decided.
- Don't ship a visualization without a caption (figure number, title, source).
- Don't mix visualization tool styles within one deliverable.
- Don't reach for a chart when prose would say it more clearly.
- Don't use AWS/cloud icon explosions as architecture diagrams — they look like every other consulting deck.
- For Slalom Composable DXP work, default to **Mermaid** for technical engineering audiences, **napkin.ai** for executive/one-off visuals, **shadcn/Recharts** for product UI charts, **commission** for marquee brand moments.
