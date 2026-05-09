---
name: vercel-v0
description: Use v0.app (Vercel's design-to-code AI) effectively in Slalom delivery — prompt patterns that produce shadcn-aligned output, the v0 API for pipeline integration, when to use it vs. hand-coding, when to use it vs. the design-implementation skill, and how to clean up the output to design-system standards. Use this skill when v0 enters the conversation, when a screen needs to be bootstrapped from a Figma export or a description, when an internal tool needs UI quickly, or when a stakeholder wants to see something interactive in hours rather than days. Trigger on any "use v0" or "design-to-code" question.

# Project context
type: skill
project: skills-library
plugin: vercel
aliases: [vercel-v0]
tags: [type/skill, plugin/vercel, topic/vercel, topic/v0, topic/design-to-code]
status: active
---

# v0.app — design-to-code on Vercel

[v0.app](https://v0.app/) is Vercel's AI design-to-code product. Prompt + optional image in; React + Tailwind + shadcn-shaped output. This skill owns when and how Slalom uses it.

Pair with `vercel-ai-sdk` (v0 is a model behind the scenes; the API uses similar patterns), `software-engineering-shadcn-component` (v0 outputs shadcn primitives — clean them up to design-system standards), `software-engineering-design-implementation` (the rigorous design-to-code skill that v0 outputs feed into), and `design-handoff` (Figma exports as v0 input).

## When to use this skill

- Bootstrapping a new screen from a Figma export.
- Internal-tool UI that needs to ship in hours, not weeks.
- Prototype for stakeholder review before Build picks it up.
- Generating boilerplate for shadcn-based pages.
- Ideation — quickly producing variants of a page idea.
- Integrating v0 into a content pipeline (programmatic generation).

## Core posture

- **v0 outputs are starting points, not endings.** Always run output through `software-engineering-shadcn-component` cleanup and design-system token alignment.
- **Prompt + Figma export beats prompt alone.** Image inputs anchor v0 in real designs.
- **Use shadcn explicitly.** v0 is shadcn-aware; mention `shadcn` in prompts to bias the output.
- **Don't ship v0 output raw.** It misses design-system tokens, accessibility detail, and project conventions. The polish is what makes it production-grade.
- **The v0 API is for pipelines, not interactive use.** Interactive iteration happens in v0.app; programmatic generation uses the API.

## The two surfaces

### v0.app (interactive)

The web UI at https://v0.app. Prompt-based, conversational. Iterate by:

1. Describing a screen.
2. Optionally uploading a Figma export, screenshot, or sketch.
3. Receiving generated React + Tailwind + shadcn output.
4. Refining: "make the hero darker," "add a CTA below the headline," "use a sidebar layout instead."
5. Copying the output to your codebase or deploying directly to Vercel.

Use for:

- Bootstrapping new screens.
- Stakeholder-facing prototypes.
- Internal tools.

### v0 API

Programmatic access to the v0 model. Use for pipelines:

- Generate UI from CMS-stored prompts.
- Build a tool that converts Figma exports to React in batch.
- Embed v0 generation into an internal admin where non-engineers compose UIs.

Auth via API key. Endpoints accept prompts + optional images; return code blocks.

Reach for the API when the same workflow happens repeatedly. Otherwise, the interactive surface is faster.

Docs: https://v0.app/docs/api

## Prompt patterns that work

### Anchor with the design system

```
Generate a pricing page using shadcn/ui components. Use Card for tiers, Badge
for the "Popular" label, Button for CTAs. Tailwind design tokens. Mobile-first
layout. Three pricing tiers stacked vertically on mobile, three columns desktop.
```

Bad prompt: "make a pricing page." v0 generates *something*; might not match your stack.

### Anchor with a Figma export

Upload the export and prompt:

```
Match this design exactly. Use shadcn/ui primitives where they map. The card
borders use Tailwind border-slate-200. Use the Inter font (already imported in
the project).
```

v0 reads the image and produces matching output. Faster than describing.

### Constrain the output

```
Output a single React functional component using TypeScript. No "use client"
needed — this is a server component. Use only shadcn/ui primitives and Tailwind
classes. No external image URLs (use placeholders). No hardcoded data — accept
props.
```

Constraints make the output drop-in. Without constraints, v0 may add `"use client"` for everything, hardcode data, or pull in unrequested libraries.

### Iterate concretely

```
Round 1: "Generate a dashboard layout with sidebar navigation and a content area."
Round 2: "Make the sidebar collapsible. Add a search input at the top of the
sidebar above the nav links."
Round 3: "Use Lucide icons for nav items. The icons should be next to the labels."
```

Each round is a focused diff. Big multi-change rounds usually produce regressions in earlier work.

### Specify accessibility

```
The CTA buttons must have accessible names (aria-label if the text alone isn't
descriptive). Form inputs must have labels. Color contrast must meet WCAG AA.
Keyboard navigation must work — focus indicators visible.
```

v0 will produce A11y-aware output if you ask. By default, it sometimes omits.

## What v0 outputs

A typical v0 output for a screen:

- One or more React components (default export named `Page` or similar).
- TypeScript.
- shadcn/ui imports (`@/components/ui/button`, etc.).
- Tailwind classes.
- Sometimes: example data, types, helper functions inline.
- Sometimes: a `"use client"` directive, even when not needed.

What v0 *doesn't* output by default:

- Project-specific design tokens (your brand colors, spacing scale).
- Accessibility detail (skip links, ARIA labels, keyboard handlers).
- Server-side data fetching (`async` server components with real APIs).
- Authentication / authorization gates.
- Error / loading / empty states (sometimes; depends on prompt).
- Tests.

The polish step turns "v0 output" into "production code."

## The Slalom polish workflow

Once v0 produces a component, walk it through:

```
1. Move into the project.
   - Place under app/ or components/blocks/ as appropriate.
   - Rename to match project conventions.

2. Run software-engineering-shadcn-component cleanup.
   - Verify shadcn primitives are installed, not duplicated.
   - Replace v0's inline duplicates with project's shadcn imports.

3. Apply design-system tokens.
   - Replace literal colors with semantic Tailwind classes (bg-primary, etc.).
   - Replace literal spacing with the project's spacing scale.
   - Apply font tokens.

4. Wire data.
   - Replace hardcoded example data with real fetches.
   - Server component → fetch on server; pass typed props.
   - Client component → useQuery / useChat / etc.

5. Add accessibility.
   - aria-label on icon-only buttons.
   - Form inputs with labels.
   - Skip links if it's a page.
   - Keyboard handlers tested.

6. Add states.
   - Loading skeleton.
   - Empty state.
   - Error state.

7. Add a Storybook story.
   - Mock props.
   - Document the variants.

8. Add tests if it's a critical path.
```

For pixel-precise design-to-code, see `software-engineering-design-implementation` — it owns the rigor; v0 owns the speed.

## When to use v0 vs. hand-coding vs. design-implementation skill

| Situation | Reach for |
|---|---|
| New screen, design exists, production-bound | `software-engineering-design-implementation` (with v0 as a starter) |
| New screen, no design, need fast iteration | v0 + polish workflow |
| Internal tool UI, ship today | v0 + light polish |
| Stakeholder demo / prototype | v0 raw |
| Single component you've built 10 times | hand-code (faster than prompt iteration) |
| Standard shadcn primitive | install via shadcn CLI, hand-code (no v0 needed) |
| Composed page from existing blocks | `software-engineering-pro-blocks-composer` (faster than v0 for known blocks) |

The trap: using v0 for everything because it's fast. For routine work, hand-coding or the existing skills are faster *and* produce better-aligned output.

## v0 API integration

For pipelines (generating UI from CMS prompts, batch conversion of designs):

```ts
// Example shape — verify against current v0 API docs
async function generateFromV0(prompt: string, imageUrl?: string) {
  const res = await fetch("https://api.v0.app/v1/generate", {
    method: "POST",
    headers: {
      "Authorization": `Bearer ${process.env.V0_API_KEY}`,
      "Content-Type": "application/json",
    },
    body: JSON.stringify({ prompt, imageUrl }),
  });
  return res.json();
}
```

Pipeline patterns:

- **CMS-driven landing pages.** Editor writes a prompt → API generates → CI deploys. Useful for fast-launching campaign pages.
- **Figma → code batch.** Upload many exports; generate one component each.
- **Internal admin UI generation.** Non-engineers compose admin UIs by prompt.

Cost: per-generation. For high-volume pipelines, evaluate cost against the polish-time savings.

Verify pricing and API shape at https://v0.app/docs/api — both move.

## Common failure modes

- **Shipping v0 output raw.** Inconsistent with the rest of the design system; misses A11y; doesn't match project conventions. Always polish.
- **Prompt too vague.** "Make a dashboard" produces generic SaaS dashboard, not the one you wanted. Be specific.
- **Too many changes per round.** v0 may regress earlier work. One concept per iteration.
- **Hardcoded data left in.** Production page ships with placeholder text. Always wire real data before review.
- **v0 imports duplicated shadcn.** Output reinstalls components your project already has. Audit imports and dedupe.
- **`"use client"` on everything.** v0 defaults to client components; your server components don't need it. Audit and remove unnecessary directives.
- **API rate limits.** Pipelines hammering v0 API. Batch with backoff.
- **Mixing v0 and Pro Blocks awkwardly.** When you have shadcn Pro Blocks, prefer those for known patterns; use v0 for novel layouts.

## Output formats

### v0 polish review

```
# v0 Output Review: [Component]

## Source
- Prompt: ...
- Image input: {Figma export | none}

## Generated
- Files: {list}
- Imports: {list — flag duplicates with project's shadcn}

## Polish applied
- [✓] Moved into project structure
- [✓] shadcn primitives reconciled
- [✓] Design tokens applied
- [✓] Data wired (server / client)
- [✓] Accessibility (aria, keyboard, contrast)
- [✓] States (loading, empty, error)
- [✓] Storybook story
- [✓] Tests (if critical path)

## Outstanding issues
- {known gaps; tracking}
```

## How this skill relates to others

- v0 outputs feed into → `software-engineering-shadcn-component` (clean up the imports).
- Design-system tokens to apply during polish → `software-engineering-tailwind-tokens`.
- Pixel-accurate design-to-code rigor → `software-engineering-design-implementation`.
- Composing page from existing blocks (often faster than v0) → `software-engineering-pro-blocks-composer`.
- Figma export upstream → `design-handoff`.
- Component documentation post-polish → `software-engineering-component-spec`.
- Programmatic v0 API integrations → `vercel-rest-api` (for the deploy side) + `vercel-ai-sdk` (similar SDK shape).

## Source material

- v0.app: https://v0.app/
- v0 API docs: https://v0.app/docs/api
- Plugin reference: `../../references/api-surface.md`
