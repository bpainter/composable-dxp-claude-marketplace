---
name: design-imagery
description: >
  The taste layer over higgsfield image and video generation — and the broader photography / illustration direction layer. Defaults to nano banana 2 / chatgpt image 2 for stills, seedance 2 for video. Specifies art direction language, lighting, mood, composition, brand consistency across an imagery family — not just prompt engineering. Owns the decision of "what kind of image is this?" before any generation tool gets opened. Use this skill any time a deliverable needs commissioned, generated, or stock imagery.

# Project context
type: skill
project: skills-library
plugin: design
aliases: [design-imagery, imagery, photo-direction, art-direction]
tags: [type/skill, plugin/design, topic/design, topic/imagery, topic/art-direction]
status: active
---

# Design Imagery

Imagery is one of the highest-leverage decisions in a deliverable. A wrong cover image undoes a good document. A right hero image elevates an average deck. AI-generated imagery is fast and cheap — and it's the easiest place for a deliverable to default to AI-default aesthetics: hands-on-keyboard, abstract-tech-blob, sunset-over-mountains-as-metaphor.

This skill is the art-direction layer. Given an imagery need, decide: stock, commissioned, generated, or photographed. For generated work, route to higgsfield with art-direction language that produces output worth shipping.

Pair with [[design-taste]] (load first), [[ai-tells-forbidden-patterns]] (the Unsplash-defaults ban, the egg-avatar ban), and the surface skills.

## When to use this skill

- Cover imagery for decks, documents, or social assets.
- Hero visuals for landing pages or marquee deliverables.
- Section dividers needing visual punctuation.
- Product / lifestyle imagery for client-facing decks.
- Avatar imagery for case studies or team pages.
- Video for higgsfield / seedance generation.

## What this skill is NOT

- Not the photography skill (no actual camera work).
- Not the prompt-engineering skill — prompts are part of the workflow but not the value here.
- Not the imagery-generation tool itself — the tool runs externally; this skill governs taste and direction.
- Not the iconography skill — see [[design-iconography]].
- Not the data visualization skill — see [[design-visualization]].

## The four imagery sources (decision tree)

### 1. Generated via higgsfield (default for original imagery)
**[higgsfield.ai](https://higgsfield.ai/mcp)** — image and video generation. MCP available for direct integration.

**Models:**
- **Stills default:** nano banana 2 (fast, photoreal, brand-safe).
- **Stills alternative:** chatgpt image 2 (better at complex compositions and abstract concepts).
- **Video default:** seedance 2 (motion, lighting, scene composition).

**When to use:**
- Cover imagery, hero visuals, section dividers.
- Brand-statement visuals for marquee deliverables.
- When commissioned photography isn't budgeted.
- When stock isn't on-brand.

**Workflow:** see [[tool-higgsfield]] for the prompt protocol and quality gates.

### 2. Stock libraries (for utility imagery only, with discipline)
- **When:** illustrative imagery for non-marquee surfaces, utility photography (a real product photo, a real venue).
- **Banned defaults:** Unsplash hands-on-keyboard, abstract-tech-blob, sunset-over-mountains. The "AI Unsplash aesthetic." See [[ai-tells-forbidden-patterns]].
- **Acceptable stock sources:** brand photo libraries (Slalom internal), Getty (paid), specific photo collections from working photographers.
- **License verification:** required.

### 3. Commissioned photography (for flagship work)
- **When:** flagship whitepapers, brand identity launches, executive-audience marquee deliverables.
- **Budget impact:** real. Day rates for B2B-quality photography vary widely.
- **Workflow:** brief carefully — shot list, location, talent (if any), mood references, technical specs.
- **Lead time:** 2–6 weeks typical.

### 4. Existing photography (curated reuse)
- **When:** Slalom has internal photo libraries from prior engagements, events, employee photography.
- **Discipline:** verify rights, attribution, and that the photo fits the current brief (not "we used this in a deck two years ago, why not now").

## Art direction language

For any imagery — generated, commissioned, stock-curated — direction is specified in the same language. This is not just prompt engineering; it's the brief.

### The 7-element art direction brief

Every imagery decision answers these 7 elements. Skip them and the result drifts to AI defaults.

1. **Subject** — what's in the frame? (people, place, object, abstract). Be specific. "A retail merchandiser walking through aisles" not "a person working."
2. **Setting** — where is this? Time of day? Specific environment? "Late-afternoon light in a back-of-house warehouse, fluorescent overhead and natural side-light from a loading-dock door."
3. **Mood** — what should the viewer feel? Use sensory adjectives. "Quiet authority, late-shift focus, slightly tired but capable."
4. **Lighting** — natural / studio / dramatic / soft / harsh / golden hour / blue hour / overcast. "Side-lit by warm fluorescent, slight shadow detail, no flash."
5. **Composition** — wide, medium, tight; symmetric, asymmetric; rule-of-thirds, centered, off-axis. "Wide medium, subject right-third, environment left-two-thirds."
6. **Palette** — colors that should appear. "Warm amber from fluorescent, cool blue from outside, deep charcoal from machinery."
7. **What it's NOT** — actively reject AI defaults. "Not the AI Unsplash aesthetic. Not stock-pose. No hands-on-keyboard. No back-of-laptop."

### Stylistic register matters

Match imagery treatment to the [[stylistic-vocabulary]] register of the deliverable:

| Register | Imagery treatment |
|---|---|
| Editorial | Documentary photography style; quiet observation; muted palette |
| Editorial-tech | Documentary + abstract technical motifs; restrained color |
| Swiss-modern | Crisp, geometric, restrained; product / object photography |
| Premium-restrained | Soft lighting, generous negative space, single subject; quiet luxury |
| Brutalist | High contrast, raw, unedited feel; rule-breaking compositions |
| Minimal-soft | Soft focus, warm tones, friendly subjects; the AI default — use rarely |
| Maximalist | Saturated, layered, energy-packed, lots of subjects |
| Anti-design | Disposable-camera aesthetic, harsh flash, intentionally rough |
| Neo-skeu / textured-realism | Material focus — fabric, glass, metal, leather; deep depth-of-field |
| Editorial-tech | Hybrid — technical subject matter shot with editorial restraint |

For Slalom Composable DXP work, default registers: **editorial-tech** for technical thought-leadership, **premium-restrained** for executive-audience pieces.

## Imagery families (consistency across a deliverable)

A deck or document has multiple images. They need to look like they belong together — like one photographer or one art director shot them.

Define an imagery system at the start:

```
# Imagery system: {Project}

## Register: {editorial-tech / premium-restrained / etc.}
## Subject category: {people in environment / objects / abstract / mixed}
## Lighting signature: {warm late-afternoon / cool overcast / studio-soft / etc.}
## Composition signature: {wide / medium / mixed; centered / asymmetric}
## Palette: {3–5 colors that thread through all imagery}
## Treatment: {color / black-and-white / duotone / muted color}
## What we're NOT doing: {AI defaults to actively reject}
```

When all imagery is generated via higgsfield, this system informs every prompt. When commissioned, it's the brief.

## How to engage

1. **Calibrate** ([[design-taste]]): audience, register, intent.
2. **Inventory the imagery need.** How many images? Cover, hero, section dividers, supporting?
3. **Define the imagery system** if 3+ images are needed.
4. **Pick the source** per image: generated, commissioned, stock, existing.
5. **For generated:** see [[tool-higgsfield]] for the workflow.
6. **For commissioned / stock:** brief in the same 7-element art direction language.
7. **Curate** the outputs against the system. Reject anything that drifts.
8. **Run the four checks** ([[design-taste]] — swap, squint, signature, token).
9. **Pre-delivery checklist:** alt text, license verified, captions written.

## Common imagery failure modes

| Failure | Symptom | Fix |
|---|---|---|
| **Hands-on-keyboard** | Generic stock photo of someone typing | Generate with specific subject; commission; or skip imagery |
| **Back-of-laptop hero** | Person from behind looking at code | Different angle, different subject |
| **Sunset-over-mountains-as-metaphor** | Generic landscape used as metaphor for "growth" | Cut metaphor imagery; use concrete subject |
| **Abstract-tech-blob** | Generic generated abstract pattern | Subject-specific imagery |
| **Mixed lighting across imagery family** | One image golden-hour, next image overcast, next studio-flash | Define lighting signature; hold across all |
| **Mixed composition signatures** | Some wide, some macro, no system | Pick one composition register and hold |
| **Stock-pose smiling** | "Diverse team smiling at camera" | Documentary photography — capturing moments, not poses |
| **Egg-avatar / Lucide-user-icon** | Default placeholder avatars | Use real photos or themed initials |
| **Unattributed stock** | Image used without license verification | License every image; track in `imagery-attributions.md` |
| **Imagery without alt text** | Accessible PDF / web fails screen-reader | Draft alt text for every image |

## Hand-offs

- **`tool-higgsfield`** in `design/references/` — for the higgsfield workflow.
- **[[design-iconography]]** — for icon work (separate from imagery).
- **[[design-visualization]]** — for diagrams (separate from imagery).
- **Surface skills** — [[design-presentation]], [[design-document]], [[design-social-asset]].
- **[[brand-identity-system]]** (brand plugin) — for imagery as part of brand identity.

## Constraints

- Don't reach for stock photos as default. Generated or commissioned earns more.
- Don't ship the AI Unsplash aesthetic.
- Don't mix imagery treatments within a deliverable. Define a system.
- Don't ship imagery without alt text.
- Don't use imagery as filler. If the deliverable doesn't need imagery, don't add it.
- Don't accept higgsfield's first generation. Iterate the prompt 3–5 times before committing.
- For Slalom Composable DXP work, default register: **editorial-tech**. Default model: **nano banana 2** for stills, **seedance 2** for video.
