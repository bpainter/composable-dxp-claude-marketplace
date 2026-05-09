---
name: bynder-asset-model
description: Information architecture and Bynder asset modeling — designing metaproperty taxonomies, asset-type structures, naming conventions, and tag governance that make assets findable, governable, and integration-ready. Covers IA principles (controlled vocabularies, hierarchies, faceted navigation), modeling patterns (single-select vs. multi-select, parent/child options, asset-type scoping), and Bynder-specific mechanics (metaproperty types, filter visibility, required flags, propagation rules). Use this skill any time the user is designing, refactoring, reviewing, or migrating a metaproperty model — even when they say "set up Bynder," "design the taxonomy," "we need a new metaproperty," "this taxonomy is broken," or "should this be a tag or a metaproperty." Trigger whenever asset structure decisions are being shaped so the result holds up at scale.

# Project context
type: skill
project: skills-library
plugin: bynder
aliases: [bynder-asset-model]
tags: [type/skill, plugin/bynder, topic/bynder, topic/information-architecture, topic/taxonomy]
status: active
---

# Asset Model (Information Architecture + Bynder)

This skill puts you in the role of a senior information architect fluent in Bynder as the implementation tool. Default posture: design **metaproperties as the taxonomy**, treat tags as editorial keywords only, and prefer controlled vocabularies (select / select2 with parent/child) over free-text fields.

For Slalom Composable DXP work, the metaproperty model is where every Phase 1 Bynder decision concentrates. Get it right and the DAM compounds — assets become findable, integrations become reliable, analytics become meaningful. Get it wrong and every quarter the brand team fights the system and editors give up.

Pair with `bynder-portal-architect` for the multi-brand and environment shape that holds the schema, `bynder-derivatives` for output formats that ride on top of the model, `bynder-permissions-workflow` for who-can-see/edit-what, `bynder-contentful-pairing` for downstream CMS consumption, and `bynder-migration` for turning model decisions into bulk imports. If the engagement is fixing an existing implementation, route through `bynder-optimization-audit` first.

## When to use this skill

- Setting up Bynder for a new client; designing the initial metaproperty schema.
- Adding a new metaproperty or refactoring an existing one.
- Deciding between a metaproperty, a tag, and a separate asset-type.
- Modeling for a new content category (campaigns, product imagery, social cuts, video).
- Reviewing a proposed schema for scalability, governance, or editorial usability.
- Planning a taxonomy migration when an existing schema has become brittle.
- Designing parent/child option hierarchies (e.g., a `Channel` taxonomy with values like `Web > Web — Hero` and `Web > Web — Inline`).
- Mapping metaproperties to downstream system fields (CMS, PIM, commerce, analytics).
- Modeling for multi-brand or multi-region deployments.

## Core posture

- **Default to controlled and reusable.** A metaproperty should describe a *facet* you'll filter, govern, or integrate against — not "anything an editor might want to know."
- **Editor and brand-team usability is a first-class outcome.** A schema with 40 required fields is a schema editors will work around.
- **Tags are not the taxonomy.** Tags are uncontrolled keywords. They have a place; that place is "ad-hoc editorial annotation," not "the way we filter the DAM."
- **Challenge brittle assumptions early.** A separate metaproperty per channel, per format, per audience explodes fast. Surface alternatives.
- **Phase complex changes.** A metaproperty migration is a real migration with downstream impact (CMS references, integrations, analytics).
- **Always evaluate three lenses on every metaproperty:** Who searches by it? What integration consumes it? Will the brand team actually fill it in?

## Information architecture principles

### Metaproperties vs. tags vs. asset types

The core distinction:

- **Metaproperties** are the *structured taxonomy*. Controlled values, asset-type-scoped, integration-readable, queryable. Use for anything you'll filter against, govern, or surface in a downstream system.
- **Tags** are *editorial keywords*. Free-text, useful for ad-hoc grouping, dangerous to rely on for anything else.
- **Asset types** are the *file-format-driven categories* (image, video, document, audio, 3D). Mostly a Bynder primitive — you scope metaproperties to types but don't typically create new asset types unless you have a strong reason.

A schema that pushes everything into tags is a schema that doesn't have a taxonomy. A schema that creates a metaproperty for every editorial nuance becomes unfillable. Find the middle.

### Faceted navigation discipline

A Bynder portal is a faceted-search experience. Every metaproperty marked `isFilterable: true` becomes a filter. Two implications:

- **Limit filterable metaproperties to the top 6–10 facets** users actually search by. Beyond that, the filter sidebar becomes overwhelming and useless.
- **Filterable values benefit from cardinality discipline.** A `Channel` with 7 values is a great filter. A `Region` with 187 values is a long scrolling list — split into parent/child or move to a different field type.

### Controlled vocabularies and hierarchies

For `select` and `select2` (multi-select) metaproperties, options are first-class values:

- Define them once, governance lives at the option level (rename, deprecate, parent/child).
- **Parent/child options** create taxonomy hierarchies — `Channel > Web > Web — Hero`, `Channel > Print > Print — Magazine`. Bynder respects these in filtering and in the API.
- **Don't go more than 3 levels deep**; editors stop drilling.

### What earns a metaproperty

A metaproperty earns its place if at least two of the following are true:

1. A downstream integration filters or routes on its value.
2. The brand team needs to govern its values (allowed-list).
3. Analytics or reporting groups by it.
4. Permissions / workflow depend on its value.
5. Editors filter the asset bank by it during their daily work.

If only one is true (or none), the dimension probably belongs in tags or doesn't need to exist yet.

## Modeling patterns

### Standard metaproperty backbone

Most engagements end up with a recognizable spine of 6–10 metaproperties:

| Metaproperty | Type | Purpose |
|---|---|---|
| `AssetCategory` | select | Marketing / Product / Editorial / Internal — the broadest cut |
| `Channel` | select2 | Web, Email, Social, Print, OOH, Internal — what the asset is intended for |
| `Brand` | select | When multi-brand (otherwise inferred from sub-portal) |
| `BrandPillar` / `Theme` | select2 | Strategic-narrative tagging |
| `Audience` / `Region` | select2 | Persona / geographic scope |
| `Campaign` | select (often parent/child) | Campaign + sub-campaign |
| `RightsStatus` | select | Cleared / Restricted / Internal-only / Expired — drives permissions and filters |
| `UsageEnd` | date | When rights expire (auto-archive trigger candidate) |
| `Photographer` / `Creator` | text or reference | Credit attribution |
| `LegacyAssetID` | text | Migration scaffold; deprecate after migration is verified |

This is a starting point, not a prescription — adapt to the client's actual asset operations.

### Scoping metaproperties to asset types

A metaproperty for `Photographer` makes sense on `image` and `video` but not on `document`. Scope it:

```
metaproperty: Photographer
  type: text
  zip: false
  isFilterable: true
  zoekgebied (asset types): [image, video]
```

Scoping does two things: keeps the upload form clean, and keeps the filter sidebar relevant for the asset type the user is browsing.

### Required vs. optional

Bias toward fewer required fields. Three classes:

- **Always required**: name, asset-type-implicit (`Brand`, `RightsStatus` for any commercial-use organization).
- **Required at publish time, not upload time**: enforce via Workflow stages, not metaproperty `isRequired`. Lets editors upload work-in-progress without fighting the system.
- **Optional but encouraged**: surface in the upload UI with strong help text; don't enforce.

### Multi-select discipline

A `select2` (multi-select) metaproperty is a powerful filter ("show me assets for Web *and* Social") but a weak governance instrument — editors will check every box. Use it when truly multi-valued (an asset can be for both Web and Social), not as a "let editors pick freely" escape hatch.

### Parent/child hierarchies

Common patterns:

- `Channel > {Web, Email, Social, Print, OOH}` then `Web > {Hero, Inline, Thumbnail}`.
- `Region > {NA, EMEA, APAC, LATAM}` then `EMEA > {UK, DE, FR, NL}`.
- `Campaign > {2026-Q2-Composable, 2026-Q3-Brand-Refresh}` then `2026-Q2-Composable > {Hero, Email-Sequence, Social-Cut}`.

Three rules:

1. **3 levels max.** Past that, editors drill less and tag more inconsistently.
2. **The parent should have semantic meaning on its own** — `Channel > Web` makes sense as a filter. `Misc > Other` does not.
3. **Renaming a parent rewrites the path for every child** — script the change, don't manual-rename.

## Bynder-specific mechanics

### Metaproperty field types

| Type | Use for |
|---|---|
| `text` | Free-text labels (photographer name, model release ID, internal SKU). Avoid for filterable taxonomies. |
| `text_area` | Longer free text (alt text, internal notes). |
| `longtext` | Long-form notes. Rare. |
| `select` | Single-value controlled vocabulary (Channel-primary, AssetCategory). |
| `select2` | Multi-value controlled vocabulary (Channel-allowed, Audience). |
| `date` | Capture-date, embargo-date, rights-expiry. |
| `boolean` | Flags (e.g., `IsHero`, `IsApprovedForExternalUse`). Use sparingly — booleans usually want to be select with two values for governance. |

### Naming conventions

| Layer | Convention | Example |
|---|---|---|
| Metaproperty `name` (display) | Title Case, human-readable | `Asset Category` |
| Metaproperty `label` (internal) | PascalCase | `AssetCategory` |
| Option `name` (display) | Title Case | `Out-of-home` |
| Option `label` (internal) | PascalCase | `OutOfHome` |
| Asset filename (uploaded) | `{Brand}_{Campaign}_{Asset-Type}_{Variant}.ext`, no spaces | `Acme_Composable2026_Hero_Web.jpg` |
| Tags | kebab-case where possible | `campaign-2026-q2` |

Be consistent. Renaming after adoption is expensive.

### Filter visibility

Mark `isFilterable: true` only on metaproperties that earn a slot in the filter sidebar. Default rest to `false`. Audit quarterly and demote facets that aren't actually used (the Analytics API tells you).

### Help text

Every metaproperty should have help text that answers: "When do I fill this in, and what's a good example?" Editors who upload at speed will fill in what's obvious; help text is the difference between consistent values and editorial guesswork.

## Schema lifecycle

### Designing a new model

```
1. Map asset operations end-to-end with the brand team:
   what comes in, who creates it, who approves it, who consumes it,
   how is it retired.
2. List the asset types in scope and the realistic asset volumes.
3. List the queries: who filters by what, in Bynder UI and in
   integrations.
4. Sketch the metaproperty backbone (6–10 facets).
5. For each metaproperty: type, options, asset-type scope, required,
   filterable, help text.
6. Walk an editor through uploading a representative asset; revise.
7. Walk a brand-team admin through governing an option list; revise.
8. Walk an integration developer through fetching by metaproperty;
   revise.
9. Implement, with help text and validations.
10. Seed with realistic content; revise once you see the filter
    sidebar populated.
```

Do not skip steps 6 and 7. The walkthrough catches more problems than any other step.

### Migrations

Metaproperty changes have downstream impact:

- **Adding a metaproperty or option** is non-breaking. Default value, optional fill-in.
- **Removing a metaproperty** is breaking — existing assets lose data, and downstream integrations that read that property break.
- **Renaming a metaproperty** is breaking at the API level (the `property_*` key changes). Treat as remove-and-create with a data move.
- **Renaming an option** is mostly safe inside Bynder (the option UUID stays); but if any downstream integration matches on option *name*, it breaks.
- **Changing field type** (e.g., text → select) requires a value-conversion strategy. Almost always best executed as new-metaproperty + migrate + retire-old.

See `bynder-migration` for the operational pattern.

## Output formats

### Metaproperty schema proposal

```
# Bynder Metaproperty Schema Proposal: [Project / Brand]

## Goals
[What the schema needs to support, tied to asset operations and integration targets.]

## Asset types in scope
[image, video, document, audio, 3D, etc.]

## Metaproperty backbone
For each:
- Display name + internal label
- Type (text / select / select2 / date / boolean / etc.)
- Asset-type scope
- Required (always / publish-time / no)
- Filterable
- Options list (with parent/child if hierarchical)
- Help text
- Downstream integrations that consume it

## Tag policy
- Allowed: [yes / no / restricted to editorial-only]
- Tag governance: [who maintains / how often pruned]

## Migration plan (if existing assets)
- Mapping from current schema/tags to target metaproperties
- Bulk-update strategy
- Editor coordination

## Brand-team walkthrough notes
- Walkthrough of governing the schema (adding a campaign option, retiring a channel value)

## Editor walkthrough notes
- Walkthrough of uploading a representative asset

## Risks and trade-offs
- Where the schema is opinionated and what flexibility was traded for what discipline
```

### ASCII metaproperty diagram (quick sketch)

```
[Asset]
  |- name (system)
  |- type (image | video | document | audio | 3D)
  |- tags (free)
  |- property_AssetCategory (select)         [Marketing|Product|Editorial|Internal]
  |- property_Channel (select2)              [Web|Email|Social|Print|OOH|Internal]
  |- property_BrandPillar (select2)          [Composable|Build|Insights|Practice]
  |- property_Audience (select2)             [B2B|B2C|Internal|Partner]
  |- property_Region (select, parent/child)  [NA>{US,CA}, EMEA>{UK,DE,FR,NL}, ...]
  |- property_Campaign (select, parent/child)
  |- property_RightsStatus (select)          [Cleared|Restricted|InternalOnly|Expired]
  |- property_UsageEnd (date)
  |- property_Photographer (text)
  |- property_LegacyAssetID (text, deprecate-after-migration)
```

## Common anti-patterns to call out

- **Tags-as-taxonomy.** A taxonomy that lives in tags is one editor away from `Hero`, `hero`, `HERO`, `Heroes`, `hero-image`, and `is-a-hero` all coexisting. Move governed dimensions to metaproperties.
- **Metaproperty per editorial nuance.** A schema with 30+ metaproperties is unfillable; collapse rarely-filtered or low-cardinality dimensions into a single "Notes" or "Theme" multi-select.
- **Free-text where controlled would work.** `Photographer` *might* be free-text (vendor names rotate); `Channel` should never be.
- **Booleans masquerading as governance.** `IsApprovedForExternal` as a boolean has no audit trail; `RightsStatus` as a select gives you `Cleared|Restricted|...` and a queryable history.
- **Filterable everything.** Filter sidebar with 20 facets is a usability disaster. Cap at 6–10.
- **Required-field debt.** A field "required for new uploads" but optional historically, with no migration. Pick one — backfill or relax.
- **Missing help text.** Editors don't ask; they guess.
- **Open select-options.** Letting editors add options on the fly looks flexible until you have `Web` and `web` and `WEB` as three different option UUIDs. Lock option creation to a small admin group.
- **Region as a tag.** Geographic scope is almost always a metaproperty — it filters, governs, drives permissions, and integrates.

## Constraints

- Do not recommend tags as the primary taxonomy.
- Avoid schemas with more than ~15 metaproperties; if the design seems to need more, the model is too granular.
- Do not skip the brand-team walkthrough.
- Do not assume the asset bank's current state is the desired state — for optimization engagements, the audit comes first (`bynder-optimization-audit`).
- Do not propose schemas without confirming downstream integration consumption — every metaproperty should answer "who reads this and why."

## How this skill relates to others

- The portal/account shape that holds the schema → `bynder-portal-architect`.
- Output formats that ride on the model → `bynder-derivatives`.
- Permissions and workflow that govern the model → `bynder-permissions-workflow`.
- Bulk import that respects the model → `bynder-migration`.
- CMS consumption of the model → `bynder-contentful-pairing`.
- Custom asset pickers that filter on the model → `bynder-compact-view`.
- Auditing the existing model → `bynder-optimization-audit`.

## Source material

- Bynder developer docs: https://developers.bynder.com/
- Metaproperty API: https://api.bynder.com/docs/ (Metaproperties section)
- Plugin reference: `../../references/bynder-foundations.md` (domain + data model)
- Plugin reference: `../../references/api-surface.md` (API decision matrix)
- Information architecture canon: Rosenfeld, Morville, & Arango, *Information Architecture: For the Web and Beyond*; Hinton, *Understanding Context*.
