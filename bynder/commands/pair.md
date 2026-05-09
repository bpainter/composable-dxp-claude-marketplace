---
description: Wire up Bynder + Contentful pairing — install OOTB app, configure both sides, design the asset-reference shape, plan webhook-driven sync, layer custom Compact View where needed

# Project context
type: command
project: skills-library
plugin: bynder
tags: [type/command, plugin/bynder, plugin/contentful]
status: active
---

Use the `bynder-contentful-pairing` skill to integrate Bynder DAM with Contentful CMS — the most common Slalom Composable DXP pattern.

Detect engagement shape first:

- **Greenfield + Contentful** — full pairing build from scratch.
- **Adding Bynder to existing Contentful** — start with content-type review for asset reference fields.
- **Adding Contentful to existing Bynder** — start with metaproperty audit before designing Contentful entry shape.
- **Optimization (broken pairing)** — run `/audit` first; route findings here.

The flow:

1. **Install OOTB Bynder app for Contentful** — both sides. Half lives in Contentful (app installation, field assignment), half in Bynder (return-derivative, default filters, allowed asset types, API user). Skipping either side is the most common install failure.
2. **Confirm OOTB fits ≥ 80% of editorial flow.** Walk an editor through. If gaps exist, route to `bynder-compact-view` for a custom field control layered on top.
3. **Standardize the asset-reference shape** stored in Contentful entries: asset ID + curated derivative URLs + alt + width/height + version pointer. Don't store the full Bynder asset object.
4. **Wire webhooks** via `bynder-webhooks-events` for asset-update → Contentful-entry invalidation. Pick the strategy (external index, entry-tag, or CMS update + revalidate) and document.
5. **Map environments** — Contentful `master` ↔ Bynder production; non-prod Contentful ↔ Bynder sandbox or `Sandbox` collection with permission isolation.
6. **Map brands** if multi-brand — pick the same partition strategy on both sides (sub-portal × space, or single-portal × single-space + brand metaproperty/field).

Default filters on the Bynder side are the leverage point — `RightsStatus=Cleared` + brand pre-filter prevents a class of editorial mistakes.

Don't propose custom UCV before installing OOTB. Don't ship without a webhook strategy — invalidation is part of the design, not an afterthought.
