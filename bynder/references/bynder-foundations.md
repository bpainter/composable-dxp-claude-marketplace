---
type: reference
project: skills-library
scope: plugin
plugin: bynder
tags: [type/reference, plugin/bynder, scope/foundational, topic/bynder]
status: active
---
# Bynder foundations — the concepts every skill assumes

This is the shared mental model. Read once, cite from every skill. If you find yourself re-explaining a concept inside a skill, put it here instead.

Canonical references: Bynder developer portal at https://developers.bynder.com/, the API reference at https://api.bynder.com/docs/, and the getting-started guide at https://api.bynder.com/docs/getting-started.

## The domain model

Bynder organizes things in a layered hierarchy:

```
Account (the tenant — your bynder.com instance)
  └── Sub-portal(s) (optional brand/region/division splits)
        ├── Asset Bank
        │     ├── Assets (the files — image, video, document, audio, 3D)
        │     ├── Metaproperties (the schema — taxonomy, attributes)
        │     ├── Tags (uncontrolled keywords; mostly an anti-pattern at scale)
        │     └── Collections (curated groupings, often for download bundles)
        ├── Brand Guidelines (sections, pages, downloadables)
        ├── Workflow (campaigns, jobs, tasks, review cycles — Bynder Workflow module)
        ├── Studio (creative automation templates — Bynder Studio module)
        ├── Analytics (asset usage, downloads, search behavior)
        └── Users / Groups / Profiles (identity + permissions)
```

Key facts that trip people up:

- **One account can host many sub-portals.** Sub-portals are the natural multi-brand boundary. Most clients start with one and add sub-portals when a brand or region needs its own visual surface.
- **Metaproperties are the schema.** They are *not* tags. A metaproperty has a defined type (text, dropdown, multi-select, date, boolean), allowed values for controlled vocabularies, and is linked to specific asset types. This is the structural layer that makes assets findable.
- **Tags are intentionally uncontrolled.** Useful for editorial keywords, dangerous for taxonomy. Slalom default: tags allowed but not relied upon for filtering or integration.
- **Asset types** are the categories — image, video, document, audio, 3D, and any custom types the account has enabled. They determine which metaproperties apply.
- **Derivatives are pre-rendered output formats** (e.g., `web_thumbnail`, `social_1200x630`, `print_300dpi`). Created when an asset is uploaded; addressable via stable URLs. The on-the-fly transformation engine (when enabled) lets you generate derivatives by URL parameter, similar to Contentful's Images API.
- **Collections are curated groupings**, not folders. Bynder is intentionally not folder-structured — taxonomy comes from metaproperties.

## The data model

Every asset in Bynder has a system block (IDs, timestamps, type) and a metadata block (the metaproperty values). The Asset Bank API returns shapes like:

```jsonc
{
  "id": "5KsDBWseXY6QegucYAoacS",
  "name": "Hero_Image_Composable_2026.jpg",
  "type": "image",
  "extension": ["jpg"],
  "fileSize": 4823910,
  "width": 4000,
  "height": 2250,
  "datePublished": "2026-04-08T12:00:00Z",
  "dateModified": "2026-05-02T17:14:01Z",
  "isPublic": 0,
  "tags": ["hero", "campaign-2026"],
  "property_AssetCategory": ["Marketing"],
  "property_Channel": ["Web", "Social"],
  "property_BrandPillar": ["Composable"],
  "thumbnails": {
    "mini": "https://...",
    "thul": "https://...",
    "webimage": "https://...",
    "social_1200x630": "https://..."
  },
  "original": "https://..."
}
```

Things to internalize:

- **`property_*` keys are metaproperty values.** The naming convention is `property_{MetapropertyName}` and the value is always an array (even for single-select). Custom metaproperties show up as additional `property_*` keys.
- **Thumbnail/derivative URLs are stable but expire.** The CDN-fronted URLs work indefinitely once minted, but signed/private-asset URLs have expiry. Plan downstream caching accordingly.
- **`isPublic` flips access.** Public assets are CDN-cached and shareable by URL alone; private assets require an authenticated request. Most enterprise deployments keep assets private.
- **Multi-asset types beyond images.** Videos carry `videoPreviewURLs`, documents carry `pdfPreviewURL`, etc. Don't assume an asset is an image.
- **Custom fields scale nonlinearly.** Each metaproperty is a column on the asset; lots of metaproperties + lots of allowed values → search-index pressure. See `bynder-asset-model` for the discipline.

## Metaproperties — the taxonomy primitive

Metaproperties are where Bynder's modeling power lives. They have:

- A **name** (what shows in the UI) and **label** (an internal ID).
- A **type** — `text`, `select`, `select2` (multi-select), `date`, `text_area`, `boolean`, `longtext`.
- **Options** for select types — controlled-vocabulary values. Options can themselves carry metadata and be parent/child to support hierarchies (taxonomy trees).
- **Asset-type scoping** — which asset types the metaproperty applies to.
- **Required flag** — whether asset uploads can complete without this property set.
- **Filter visibility** — whether the property appears in the search-filter sidebar.

Two big anti-patterns to call out:

- **Treating tags as the taxonomy.** Tags do not have controlled values, are not required, and don't filter cleanly. Use metaproperties for anything you'll filter, govern, or integrate against.
- **Modeling every editorial dimension as a metaproperty.** A 40-metaproperty schema becomes unfillable for editors. The discipline: model what you'll *query against*, not what's *interesting to know*.

## Derivatives and on-the-fly transformations

Every asset uploaded to Bynder generates a set of derivatives — pre-rendered output formats configured on the account. There is also a **dynamic / on-the-fly transformation** engine (Bynder Dynamic Asset Transformation, sometimes called BDAT or the URL-based transformation API) that can resize, crop, and convert images via URL parameters at request time.

```
https://d3p7zgxmpk5ezh.cloudfront.net/m/{transform-id}/{asset-id}/{filename}
```

Examples (where `?w=`, `?h=`, etc. apply on accounts that have the transformation engine enabled):

- `?w=1200&h=630&q=80&fm=webp` — resize for social card
- `?fit=crop&crop=focalpoint&fp-x=0.5&fp-y=0.4` — focal-point crop
- `?fm=avif&q=70` — modern format with quality control

Two practical rules:

- **Pre-rendered derivatives** are fast and free at request time but require account-level configuration and a re-render when the rule changes.
- **On-the-fly transformations** are flexible but can pile up unique URL variants in your CDN cache. Lock the dimensions used by the front end behind a small set of presets.

See `bynder-derivatives` for the full strategy.

## Brand Guidelines

A first-class module, not "just another set of pages". Brand Guidelines lives in the same account but has its own permission model, its own URL surface, and its own publishing flow. It's the front door for non-technical users — designers, agencies, freelance contributors — who need to know "what does this brand look like?"

Two characteristics that matter:

- **Sections, pages, blocks** — guidelines have a hierarchy. Each block can embed assets directly from the asset bank, so guidelines stay in sync with source-of-truth media.
- **Versioning and publish/draft** — independent from the asset bank. Brand teams can iterate guidelines without touching the assets they reference.

See `bynder-brand-guidelines`.

## Workflow

The Bynder Workflow module governs *creation* of assets, not just storage. Campaigns, jobs, tasks, review/approval, scheduled publishing. Optional but high-value when the client has a creative-ops shop. If the client asks "where do my designers hand off final assets to marketing?" — this is the answer.

See `bynder-permissions-workflow`.

## Authentication

Bynder supports three auth flows; pick based on context:

- **OAuth2 Authorization Code** — when a user is in the loop (a custom app a marketer logs into). Three-legged. Token refresh.
- **OAuth2 Client Credentials** — server-to-server, no user. Used for backend integrations, webhooks, batch imports. The pattern Slalom defaults to for CMS pairing.
- **Permanent Tokens (Personal Access Tokens)** — generated in the portal admin UI; bound to a user. Useful for one-off scripts, dangerous for production because they tie the integration's lifetime to the user account. Slalom default: avoid for production.

OAuth scopes to know: `offline`, `current.user:read`, `asset:read`, `asset:write`, `meta.assetbank:read`, `meta.assetbank:write`, `collection:read`, `collection:write`. See [api.bynder.com/docs/getting-started#authorization](https://api.bynder.com/docs/getting-started#authorization) for the full surface.

For OAuth2 details: https://api.bynder.com/docs/getting-started#authorization

## API surface — the five you'll touch

| API | What for | Auth | Mutation? |
|---|---|---|---|
| **Asset Bank API** | List, search, get, upload, update, delete assets and metaproperties | OAuth2 | Yes |
| **Workflow API** | Campaigns, jobs, tasks, review/approval cycles | OAuth2 | Yes |
| **Analytics API** | Asset usage, downloads, search behavior, integrations | OAuth2 | No (read) |
| **Brand Guidelines API** | Sections, pages, blocks, downloadables | OAuth2 | Yes |
| **Compact View API** | Embeddable asset picker — config, tokens, callbacks | OAuth2 + JS SDK | N/A (UI surface) |

Plus the **Webhooks** subsystem (asset events: `asset.created`, `asset.updated`, `asset.deleted`, `metaproperty.updated`) and the **GraphQL Asset API** in environments where it's enabled.

See `api-surface.md` for a deeper map of when to reach for which.

## Rate limits — the ones that bite

- **Asset Bank API**: ~5–10 req/sec/token typical (varies by plan tier and operation). Bulk uploads are rate-limited more strictly than reads.
- **Search-by-metaproperty queries** can be expensive — a `propertyOptionId` filter against a high-cardinality metaproperty is fine, a free-text search across millions of assets is not. Cache aggressively.
- **Asset uploads** are multi-step (init upload → upload chunks → finalize). Each step counts toward your rate budget. The `bynder-js-sdk` handles this for you; if you're rolling your own, expect more friction than CDA-style reads.
- **Analytics API** is heavy — daily exports rather than real-time queries.

## Webhooks at a glance

- Triggered by asset and metaproperty events: `asset.created`, `asset.updated`, `asset.deleted`, `asset.archived`, `metaproperty_option.created`, etc.
- Payload is a JSON envelope with the asset/property ID and the event type — *not* the full asset record. Receivers re-fetch via API to get current state.
- Sign with HMAC; verify on receipt. Bynder rotates the signing secret in the portal.
- Retries: exponential backoff, configurable retention. Configure idempotent receivers.

See `bynder-webhooks-events` for full handling patterns.

## Marketplace

The Bynder Marketplace at https://marketplace.bynder.com/en-US/home is a catalog of OOTB connectors:

- **CMS**: Contentful, Sitecore, Drupal, WordPress, Adobe Experience Manager, Optimizely, Magnolia
- **Commerce**: Salesforce Commerce Cloud, commercetools, Shopify
- **Creative**: Adobe Creative Cloud (Photoshop, Illustrator, InDesign), Figma
- **Marketing automation**: Marketo, HubSpot, Salesforce Marketing Cloud
- **Productivity**: Microsoft Teams, Slack, Google Workspace
- **PIM**: Akeneo, Salsify, inRiver
- **Workflow**: Workfront, Hootsuite, Sprinklr

These are real connectors, not just listings — most are JSON-config or click-config with no engineering required. The decision is usually **OOTB connector vs. custom Compact View embed** — see `bynder-marketplace-connectors`.

## Universal Compact View

The Universal Compact View (UCV) is Bynder's embeddable asset picker. It runs in an iframe inside any host application — Contentful, Salesforce, a custom React app — and returns selected assets with their metadata and URLs to the host via postMessage. Documented at https://developers.bynder.com/universal-compact-view.

Two modes:

- **OAuth-flow** — the user authenticates against Bynder when the picker opens; the picker enforces their permissions
- **Token-flow** — the host app authenticates server-side, mints a session token, passes it to the picker

UCV is the integration primitive. If you're building a custom CMS plugin, a Salesforce Lightning component, or any "let editors pick assets" surface and you don't have an OOTB connector, UCV is the answer. See `bynder-compact-view`.

## Source documents

- Developer portal: https://developers.bynder.com/
- API reference: https://api.bynder.com/docs/
- Getting started: https://api.bynder.com/docs/getting-started
- Authorization (OAuth2): https://api.bynder.com/docs/getting-started#authorization
- Universal Compact View: https://developers.bynder.com/universal-compact-view
- JS SDK: https://github.com/Bynder/bynder-js-sdk
- Marketplace: https://marketplace.bynder.com/en-US/home
- Bynder Academy (training): https://academy.bynder.com/
