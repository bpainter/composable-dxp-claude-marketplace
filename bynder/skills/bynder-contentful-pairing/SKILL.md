---
name: bynder-contentful-pairing
description: The Bynder + Contentful integration playbook — the most common Slalom Composable DXP pattern. Covers the OOTB Bynder app for Contentful, when to extend it with custom Compact View embeds, the asset-reference data shape persisted in Contentful entries, derivative selection at the CMS field level, webhook-driven cache invalidation between the two systems, multi-environment / multi-brand integration patterns, and the optimization-engagement signals that point to a broken pairing. Use this skill any time the engagement involves Bynder and Contentful together — greenfield stand-up, integration repair, or upgrading an existing OOTB-only deployment to a custom-embed pattern.

# Project context
type: skill
project: skills-library
plugin: bynder
aliases: [bynder-contentful-pairing]
tags: [type/skill, plugin/bynder, plugin/contentful, topic/integration, topic/pairing]
status: active
---

# Bynder + Contentful pairing (the Slalom default pattern)

This is the bridge skill. Most Slalom Composable DXP engagements pair Bynder (DAM source of truth) with Contentful (CMS source of truth for everything else). The OOTB Bynder app for Contentful is the starting point; almost every production deployment extends it with custom configuration, custom Compact View embeds for advanced flows, or webhook-driven sync between the two systems.

This skill puts you in the role of the engineer who owns the integration end-to-end. Default posture: install OOTB, identify gaps against the editorial flow, layer custom UCV embeds for the gaps, wire webhooks for downstream sync.

Pair with `bynder-marketplace-connectors` (the OOTB starting point), `bynder-compact-view` (the custom embed substrate), `bynder-asset-model` (the metaproperty schema CMS field controls filter on), `bynder-derivatives` (what URL flows from DAM to CMS), `bynder-webhooks-events` (the bidirectional sync), and the `contentful` plugin's `contentful-content-model` (how Contentful entries reference Bynder assets) and `contentful-react-wrapper` (how the front end consumes them).

## When to use this skill

- Standing up Bynder + Contentful from scratch.
- Adding Bynder to an existing Contentful deployment (or vice versa).
- The OOTB Bynder app for Contentful is installed but isn't fitting the editorial flow.
- Editors are downloading from Bynder and re-uploading to Contentful (a sign the integration isn't working).
- Asset updates in Bynder aren't reflecting in Contentful-driven sites.
- Multi-environment / multi-brand split between the two systems needs design.
- Optimization engagement where the pairing is one of the failing components.

## Core posture

- **Install the OOTB Contentful Bynder app first.** It's free, supported, uses UCV under the hood, and gets you to a working baseline in an hour. Marketplace listing: https://marketplace.bynder.com/contentful/.
- **Configure the OOTB app at both ends.** Half the install lives in Contentful (app installation, field assignment, configuration), half in Bynder (return-derivative selection, allowed asset types, default filters). Don't skip the Bynder side.
- **Custom UCV embed for advanced flows.** When the OOTB app's editorial flow falls short — bulk-pick, advanced filtering, custom validation, custom preview — build a custom field control via Contentful App Framework that wraps UCV.
- **Asset references store small.** Persist asset ID + the canonical derivative URLs the front end needs + alt + width/height. Don't store the full Bynder asset object.
- **Webhook-driven invalidation.** Bynder `asset.updated` → invalidate Contentful entries that reference it. Contentful `Entry.publish` → optionally tag/categorize the asset back in Bynder.
- **One environment per side.** Contentful's `master` should reference the same Bynder portal that production reads from. Non-prod Contentful → non-prod Bynder portal or sandbox. See the multi-environment notes below.

## The OOTB Bynder app for Contentful

Available at the [Bynder Marketplace](https://marketplace.bynder.com/contentful/) and the [Contentful Marketplace](https://www.contentful.com/marketplace/).

### What it does

- Installs as a Contentful app definition.
- Provides a custom field control of type "Bynder Asset" — set on a JSON-object field type in Contentful.
- Editors click the field; UCV opens; they search/filter/select; the field saves a JSON payload with asset ID + derivative URLs.
- Configurable: allowed asset types, default filters, return-derivative selection, theme.

### What it doesn't do

- It doesn't keep stored references in sync with later Bynder updates. If an asset is renamed, recropped, or replaced in Bynder, the Contentful field still has the old derivative URL. Wire webhooks for that.
- It doesn't enforce metaproperty validation on selection — an editor can pick an asset that hasn't been rights-cleared. Use Bynder portal-level filters (default-filter the picker to `RightsStatus=Cleared`) to constrain choices.
- It doesn't bulk-pick. One asset at a time per field. For galleries / batches, layer a custom embed.
- It doesn't co-author — if multiple editors are picking simultaneously into the same field, the last-write-wins semantics of the Contentful field apply.

### Setting it up

```
Contentful side:
1. Install the Bynder app definition (Apps → marketplace → Bynder).
2. Configure the app: Bynder portal URL, default asset types, default derivative.
3. Create a field on your content type:
   - Type: JSON object
   - Field control: Bynder asset (provided by the app)
4. Optionally constrain with field-level config (specific filters per content type).

Bynder side:
1. Set up the Compact View configuration in the portal (return-derivative,
   default filters per integration).
2. Confirm the API user / token used by the app has scope to read assets and
   metaproperties.
3. Set focal points on photographic assets so derivative crops behave.
```

Walk an editor through picking an asset before declaring done.

## Custom field controls when OOTB falls short

Build a Contentful App Framework app that hosts a custom field control wrapping UCV. Patterns:

### Pattern: hard brand filter

When the entry's content type implies a single brand context (a single-brand client's `Article` type), pre-filter the picker:

```tsx
// in your Contentful App Framework custom field control
BynderCompactView.open({
  defaultMetapropertyOptions: [BRAND_OPTION_IDS[entry.fields.brand['en-US']]],
  // ...
});
```

Editors only see assets from the right brand. Removes a class of editorial mistakes.

### Pattern: derivative selection UI

Some flows need the editor to choose which derivative variant goes into the field — e.g., the entry has a `heroAsset` field where the editor picks an image and then specifies whether to use the wide-crop, square-crop, or portrait-crop derivative.

```tsx
function HeroAssetField() {
  const [value, setValue] = useFieldValue<HeroAssetFieldValue>();
  // ...
  return (
    <>
      <PreviewCard asset={value?.asset} />
      <CropSelector
        options={['wide', 'square', 'portrait']}
        value={value?.crop}
        onChange={(crop) => setValue({ ...value, crop, derivativeUrl: value.asset.thumbnails[`hero_${crop}`] })}
      />
    </>
  );
}
```

### Pattern: gallery / multi-pick

Single-field-multi-asset using `MultiSelect` mode:

```tsx
BynderCompactView.open({
  mode: 'MultiSelect',
  // ...
  onSuccess: (assets) => {
    setValue(assets.map((a) => ({ assetId: a.id, derivativeUrl: a.thumbnails.web_card_md, alt: a.description })));
  },
});
```

The Contentful field type is JSON object with an array shape.

### Pattern: validation

Refuse selections that don't meet requirements (e.g., dimensions too small for the design slot):

```tsx
onSuccess: (assets) => {
  const a = assets[0];
  if (a.width < 1920) {
    sdk.dialogs.openAlert({
      title: 'Asset too small',
      message: 'This slot needs ≥ 1920px wide. Pick a higher-resolution asset.',
    });
    return;
  }
  setValue({ /* ... */ });
};
```

## The asset-reference data shape

Standardize across content types. A `bynderAsset` JSON shape:

```jsonc
{
  "assetId": "5KsDBWseXY6QegucYAoacS",
  "name": "Hero — Composable 2026",
  "alt": "Two engineers reviewing a composable architecture diagram",
  "type": "image",
  "width": 4000,
  "height": 2250,
  "derivatives": {
    "hero": "https://...?...&w=2560&fm=webp",
    "card": "https://...?...&w=1024&fm=webp",
    "thumbnail": "https://...?...&w=320&fm=webp"
  },
  "version": "2026-04-08T12:00:00Z"  // Bynder's dateModified at pick time
}
```

The `version` field powers staleness detection on the front end and the webhook-invalidation logic.

For multi-asset fields (galleries), an array of the same shape.

## Webhook-driven invalidation

The OOTB app doesn't sync; you wire it.

### Bynder asset updated → Contentful cache invalidation

```
Bynder webhook: asset.updated  →  webhook receiver
  → look up Contentful entries that reference this asset (by Bynder assetId in JSON field values — query against Contentful's CMA)
  → for each entry, optionally re-fetch the asset from Bynder, update the cached derivative URLs, save (don't publish — let editors see the change in preview)
  → trigger on-demand revalidation on the front end (Vercel/Next.js revalidateTag for the affected entries)
```

The Contentful CMA query is annoying — JSON-field contents aren't indexed for query. Two practical strategies:

1. **Maintain an external index.** Receive Bynder webhooks, look up affected entries in a small Postgres / Redis index keyed by `assetId → entryId[]`. Build the index from a one-time scan + maintain incrementally on Contentful publish events.
2. **Tag Contentful entries with the asset IDs they reference.** When an editor picks an asset, also write the asset ID to a (hidden) `Symbol`-array field. Now CMA can query by tag value.

Strategy 2 is simpler if you control the content model; strategy 1 if the model is fixed.

### Contentful entry published → optionally update Bynder

Less common, but high-value where the brand team wants to know "this asset is in production" — fire a Contentful `Entry.publish` webhook, look up the referenced Bynder assets, and stamp them with a `LastUsedInProduction` metaproperty or add them to a "currently in use" Bynder collection. Powers downstream analytics ("which assets are actually live?").

See `bynder-webhooks-events` for the receiver patterns.

## Multi-environment patterns

Contentful and Bynder both have environment models, but they don't quite line up.

| Contentful concept | Bynder equivalent | Reality |
|---|---|---|
| `master` environment | Production portal | One-to-one |
| `staging` / `dev` environment | Sandbox portal (if plan supports) or shared production portal with restricted folders / sub-portal | Often shared — the practical effect is staging Contentful references production Bynder |
| Environment alias | n/a | Contentful concept only |
| Sub-portal | n/a | Bynder concept only |

Slalom default:

- Production Contentful (`master` alias) → Production Bynder portal.
- Non-prod Contentful (`staging`, `dev`) → Same Bynder production portal, *but* with a permission profile that restricts non-prod tokens to a subset of assets (e.g., a `Sandbox` collection) so non-prod CMS doesn't accidentally publish unreleased assets.
- For Complex engagements with a Bynder sandbox portal: separate production / non-prod portals with a one-way prod-to-sandbox replication (rare but exists).

Document the environment mapping in the integration plan and walk the editor team through it. The number-one cause of "the staging site shows the wrong image" is environment mismatch.

## Multi-brand patterns

When the client has multiple brands:

- Bynder side: sub-portals per brand, *or* a single portal with `Brand` metaproperty driving partition.
- Contentful side: spaces per brand, *or* a single space with `brand` reference field.

Pick the same partition strategy on both sides where possible.

| Bynder | Contentful | Use |
|---|---|---|
| Sub-portal per brand | Space per brand | Strong isolation; multi-brand-as-multi-business-unit |
| Single portal + Brand metaproperty | Single space + brand field | Brand-aware shared assets; faster to build, more cross-brand surface |
| Single portal | Space per brand | Centralized asset pool, brand-isolated content |
| Sub-portal per brand | Single space | Brand-isolated assets, shared content scaffold (rare and brittle) |

## Output formats

### Pairing integration plan

```
# Bynder + Contentful Integration Plan: [Project]

## Engagement shape
[Greenfield + Contentful | Optimization | Adding Bynder to existing Contentful | Adding Contentful to existing Bynder]

## Baseline (OOTB)
- Contentful app installed: [yes / planned]
- Bynder portal config done: [yes / planned]
- Default asset types, derivatives, filters: [...]

## Custom embeds (if any)
- [Field name on content type]: [why custom; configuration]

## Asset-reference shape
[The JSON envelope persisted in Contentful entries.]

## Webhook strategy
- Bynder → Contentful invalidation: [strategy 1 (external index) | strategy 2 (tag on entry)]
- Contentful → Bynder (if any): [LastUsedInProduction stamp | collection membership]

## Multi-environment mapping
| Contentful env | Bynder portal/profile |
| ... | ... |

## Multi-brand mapping (if applicable)
[Bynder partition × Contentful partition]

## Test plan
- Editor picks asset, saves entry, publishes — front end renders correct derivative
- Bynder asset updated — front end reflects within [N] minutes
- Bynder asset deleted — Contentful entry surfaces clear error
- Multi-env: pick from non-prod Bynder, never bleeds into prod Contentful
```

## Optimization signals (when the pairing is broken)

These are the prompts that point to a broken-pairing optimization engagement:

| Signal | Diagnosis | Fix |
|---|---|---|
| "Editors download from Bynder, edit, re-upload" | Compact View not exposing the right filters / derivatives | Custom embed with editorial-flow-aware config |
| "Asset updates in Bynder don't show on the site" | No webhook-driven invalidation | Wire webhooks → external index or entry-tag strategy |
| "We can't tell which assets are actually live" | No Contentful → Bynder reverse signal | Stamp assets on entry publish |
| "Picker shows internal assets that legal hasn't cleared" | OOTB without portal-level default filter | Add `RightsStatus=Cleared` default filter on portal Compact View config |
| "Wrong brand's assets in the picker" | Single-portal multi-brand without filtering | Pre-filter UCV by Brand on entry / content type |
| "Different image variants in different places" | No standardized derivative selection | Standardize asset-reference shape; rebuild custom field control |
| "Staging site shows production assets" | Environment mismatch | Document and enforce env mapping |

## Common anti-patterns to call out

- **Building custom UCV first, OOTB never.** The OOTB app does 80% of the work and is supported. Use it.
- **OOTB without Bynder-side configuration.** Half the install is in the Bynder portal; skipping it leaves the picker generic and editorial flow loose.
- **Storing the full Bynder asset object in Contentful.** Goes stale; bloats the entry payload. Store the curated envelope.
- **No webhook strategy.** Asset updates in Bynder never reflect downstream; editors lose trust within a quarter.
- **Hard-coded derivative URLs in components.** Centralize. Store derivative URLs in the asset reference; render from there.
- **Different environments pointing at the same Bynder, no permission isolation.** Non-prod CMS pulls unreleased assets into staging.
- **Multi-brand split done one way on Bynder, the other way on Contentful.** Misery. Pick one strategy.
- **Editors authenticated via Permanent Tokens for OOTB picker.** The OOTB app uses OAuth flow. Don't fight that.

## Constraints

- Do not propose custom UCV when the OOTB app fits without explicit gap analysis.
- Do not propose Bynder-Contentful integration without a webhook strategy — invalidation is part of the design, not an afterthought.
- Do not assume single-environment; map non-prod explicitly.
- Do not store full Bynder asset objects in Contentful entries — use the curated envelope.
- Do not skip walkthroughs with editors and brand-team; pairing problems show up in editorial flow first.

## How this skill relates to others

- The Marketplace catalog (always check first) → `bynder-marketplace-connectors`.
- The custom embed substrate → `bynder-compact-view`.
- The metaproperty filters that pre-narrow the picker → `bynder-asset-model`.
- The derivative URLs that flow back → `bynder-derivatives`.
- The webhook strategy → `bynder-webhooks-events`.
- The Contentful side of the integration → `contentful-content-model` (entry shape), `contentful-react-wrapper` (front-end consumption), `contentful-app-framework` (the custom field control hosting).

## Source material

- Bynder app for Contentful (Bynder Marketplace listing): https://marketplace.bynder.com/contentful/
- Bynder app for Contentful (Contentful Marketplace listing): https://www.contentful.com/marketplace/
- Universal Compact View docs: https://developers.bynder.com/universal-compact-view
- Contentful App Framework: https://www.contentful.com/developers/docs/extensibility/app-framework/
- Plugin reference: `../../references/marketplace-connectors.md`
- Plugin reference: `../../references/bynder-foundations.md`
- Sister plugin: `../contentful/skills/contentful-app-framework/SKILL.md`
