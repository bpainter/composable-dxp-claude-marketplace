---
name: pf-brand-router
description: >
  Decides which brand a deck uses (Slalom default, Composable DXP secondary, or
  Client custom) and prepares the brand assets. For Slalom/Composable, copies
  the pre-built tokens.css and logo from resources/. For Client, ingests from
  an attached styleguide PDF or a `.com` crawl, derives tokens, saves to
  WIP/[deck]/brand/. Outputs: tokens.css + logo.svg in the WIP brand folder.
type: skill
plugin: presentation-factory
tags: [type/skill, plugin/presentation-factory, scope/specialty, topic/brand-ingestion]
status: active
---

## When to invoke

The Frame stage orchestrator calls this skill after the user has answered "which brand."

## Required context

Foundations: [`../../references/presentation-factory-foundations.md`](../../references/presentation-factory-foundations.md). Toolchain detail: [`../../references/presentation-factory-toolchain.md`](../../references/presentation-factory-toolchain.md). Brand-pattern reference: [`../../references/slalom-brand-patterns.md`](../../references/slalom-brand-patterns.md).

## What this skill does

### If brand = Slalom

1. Copy `resources/tokens/slalom.css` → `WIP/[deck]/brand/tokens.css`
2. Copy `resources/assets/slalom-logo.svg` → `WIP/[deck]/brand/logo.svg`
3. Copy `resources/assets/slalom-logo-white.svg` → `WIP/[deck]/brand/logo-white.svg`
4. Record in `00_Intake_Brief.md`: `Brand: Slalom` and `Italic-emphasis: on`

### If brand = Composable DXP

1. Copy `resources/tokens/composable-dxp.css` → `WIP/[deck]/brand/tokens.css`
2. Copy `resources/assets/composable-dxp-wordmark.svg` → `WIP/[deck]/brand/logo.svg`
3. Copy `resources/assets/composable-dxp-mark.svg` → `WIP/[deck]/brand/logo-mark.svg`
4. Record: `Brand: Composable DXP` and `Italic-emphasis: off` (DXP uses Lora 700 non-italic)

### If brand = Client

Two ingestion paths. Ask the user first via AskUserQuestion:

```
Question: "How should I pick up the client brand?"
Options:
1. I'll attach the client's brand styleguide PDF (Recommended if you have one)
2. Crawl the client's website to derive brand patterns
3. I'll provide the brand details manually (paste colors, fonts, voice notes)
```

#### Path A: PDF ingestion

1. Read the attached PDF using vision capabilities.
2. Extract:
   - **Primary color** (the most prominent / called-out hex value)
   - **Secondary color** + 2-3 **accent colors**
   - **Display font** name (the heading/feature font)
   - **Body font** name
   - **Logo** — note which page has the primary logo, extract as a high-res image if possible
   - **Voice characteristics** (3-5 words from the brand book — confident, warm, technical, etc.)
   - **Banned patterns** (any "don't" examples in the guide)
3. Copy `resources/tokens/client-template.css` → `WIP/[deck]/brand/tokens.css`
4. Replace the `/* TODO */` markers with extracted values
5. If logo can't be extracted as SVG, save raster as `WIP/[deck]/brand/logo.png` and note the limitation
6. Record `Brand: Client:[CLIENT_NAME]` and italic-emphasis flag based on whether the client uses serif-italic emphasis in their own marketing

#### Path B: `.com` crawl

1. Ask user for the client's primary URL (homepage).
2. Use `WebFetch` on: homepage, then 1-2 of (about, services, products — whichever appears in the nav).
3. Parse each page for:
   - `<style>:root { --color-...` custom properties — modern sites expose tokens
   - Heading/body font from `<link href="...fonts.googleapis.com">` or `font-family` declarations
   - Logo `<img src>` patterns (filter for "logo" in path/alt)
   - Hero colors and accent patterns observed across pages
4. Same as Path A from step 3 onwards.

#### Path C: Manual entry

Ask via AskUserQuestion (or conversation):
- Primary color (hex)
- Secondary color (hex)
- 1-3 accent colors (hex)
- Display font name + body font name
- Anything else important (voice notes, banned patterns, italic-emphasis on/off)

Apply same template-filling pattern.

## Validation before completion

After filling tokens.css, do a sanity pass:

1. Does the primary color have enough contrast against white (AA ratio ≥ 4.5)?  
   If no — flag in `00_Intake_Brief.md` under "Brand caveats" and suggest using primary-deep for text-on-light.
2. Does the gradient have enough chroma variance to read as a gradient?  
   If colors are all near-identical — fall back to single-color gradient (primary light → primary deep).
3. Is the logo present?  
   If no — record a TODO and ask the user to provide one before Compose.

## Output

| File | Path |
|---|---|
| Brand tokens (CSS) | `WIP/[deck]/brand/tokens.css` |
| Logo (light) | `WIP/[deck]/brand/logo.svg` (or `logo.png`) |
| Logo (dark / white) | `WIP/[deck]/brand/logo-white.svg` if available |

Record in `00_Intake_Brief.md` (Brand section):
```markdown
## Brand
- Name: [Slalom / Composable DXP / Client:Name]
- Tokens file: WIP/[deck]/brand/tokens.css
- Logo: WIP/[deck]/brand/logo.svg
- Italic-emphasis: [on / off]
- Brand caveats: [any limitations from ingestion]
```

## Anti-patterns

- **Don't** mix logos (e.g. Slalom logo on a client-branded deck unless explicitly co-branded)
- **Don't** force the italic-emphasis pattern on a client brand that doesn't use it
- **Don't** invent brand colors — if ingestion fails, ask the user; don't guess
- **Don't** skip the contrast check — a primary color that fails AA on white breaks every title slide
