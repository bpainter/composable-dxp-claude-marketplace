---
type: reference
project: skills-library
scope: plugin
plugin: contentful
tags: [type/reference, plugin/contentful, scope/foundational, topic/contentful]
status: active
---
# Contentful foundations — the concepts every skill assumes

This is the shared mental model. Read once, cite from every skill. If you find yourself re-explaining a concept inside a skill, put it here instead.

Sources are linked at the end. The canonical references are the [domain model](https://www.contentful.com/developers/docs/concepts/domain-model/), [data model](https://www.contentful.com/developers/docs/concepts/data-model/), and [links](https://www.contentful.com/developers/docs/concepts/links/) docs.

## The domain model

Contentful organizes things in a strict hierarchy:

```
Organization
  └── Space
        └── Environment (and Environment Aliases pointing at environments)
              ├── Content Types (the schema)
              ├── Entries (the content, conforming to a content type)
              ├── Assets (files — images, video, docs)
              ├── Locales (the languages content is written in)
              └── Tags / Taxonomy concepts (cross-cutting metadata)
```

Key facts that trip people up:

- **One organization can own many spaces.** Spaces are the natural billing/permissions boundary. Most clients have one space; multi-brand clients often have one per brand.
- **Every space has at least one environment.** `master` is the default. You add `staging`, `dev`, `feature-x` as needed.
- **Environments have their own schema and content.** Each environment is a snapshot you can branch from another. They share assets via a copy-on-write model.
- **Aliases (Environment Aliases) point at environments.** `master` (alias) → `release-2026-04-15` (environment). The alias is what your delivery API token reads from. Swap aliases to ship.
- **Locales live at the space level.** Adding/removing a locale is a space-wide, schema-impacting change. Localization happens at the field level inside content types.
- **Tags (the modern taxonomy) live inside an environment.** They're not the same as the legacy "metadata tags" surfaced in some SDKs. Use the Taxonomy API for governed vocabularies.

## The data model

Every published thing in Contentful — entry, asset, content type, even a webhook — has a `sys` block (system metadata) and a `fields` block (the data).

```jsonc
{
  "sys": {
    "id": "5KsDBWseXY6QegucYAoacS",
    "type": "Entry",
    "createdAt": "2026-04-08T12:00:00Z",
    "updatedAt": "2026-05-02T17:14:01Z",
    "publishedAt": "2026-05-02T17:14:02Z",
    "version": 7,
    "publishedVersion": 6,
    "contentType": { "sys": { "type": "Link", "linkType": "ContentType", "id": "article" } },
    "space": { "sys": { "type": "Link", "linkType": "Space", "id": "..." } },
    "environment": { "sys": { "type": "Link", "linkType": "Environment", "id": "master" } },
    "locale": "en-US"
  },
  "fields": {
    "title": "Composable architecture in 2026",
    "slug": "composable-architecture-2026",
    "body": { /* Rich Text AST or other field type */ }
  }
}
```

Things to internalize:

- **Versions.** Every entry tracks a draft `version` and a `publishedVersion`. They diverge whenever the entry is edited and re-saved without re-publishing. The Delivery API only sees `publishedVersion`; the Preview API sees both.
- **Publish ≠ save.** Saving updates the draft. Publishing snapshots the draft as the published version. CMA writes save by default; you publish with a separate call.
- **Locale on the envelope.** Each entry response is keyed to a single locale. To get all locales in one round trip, query with `locale=*` (CMA / sync) or pass a locale-aware query (GraphQL).
- **Field types are typed and validated** at the content-type level. `Symbol` (short text), `Text` (long), `RichText`, `Number`, `Integer`, `Date`, `Boolean`, `Location`, `Object` (JSON), `Link` (single ref), `Array` of any of those (multi or list).

## Links — Contentful's relationship primitive

Two ideas live under "link":

- **Reference fields** in a content type: a field whose value is a Link to another Entry or Asset. Always validate `linkContentType` and (for assets) `linkMimetypeGroup` to keep editors honest.
- **Sys links** inside response payloads: any time one resource references another (an entry's content type, a content type's space, a webhook's environment), you see `{ sys: { type: "Link", linkType: "...", id: "..." } }`.

Two practical consequences:

- **Resolution is API-specific.** The Delivery REST API auto-resolves links up to a `?include=` depth (default 1, max 10). GraphQL resolves through your selection set. The CMA does not auto-resolve — you handle joins client-side.
- **Circular references are real.** If `Article.relatedArticles → Article`, your include depth has to terminate. Build the model with a soft 2-level ceiling and lint for cycles.

## Environments and aliases

The deployment-pipeline pattern that ships with Contentful:

```
master (alias) ─── points at ──► release-2026-04-15 (environment)
staging (alias) ── points at ──► release-2026-04-22 (environment)
                                       ▲
                                    (clone of master, add changes here)
```

How a release works:

1. Branch a new environment from the current `master`-aliased one (`contentful space environment create --source master --environment-id release-2026-04-22`).
2. Apply schema migrations and any seed/fixture data changes against the new environment.
3. Point the `staging` alias at it; QA against `staging`.
4. When ready, swap `master` to point at the new environment. Old environment becomes the rollback target.

This works because tokens read from aliases. The front end sees a clean cut-over.

Constraints to remember:

- **Cross-space references only resolve in the Delivery / Preview / GraphQL APIs**, and only for spaces in the same organization with the feature enabled. Plan around this when you split content across spaces.
- **Environment aliases are an enterprise-tier feature** historically, though now broadly available — confirm the customer's plan early.
- **Webhooks fire per environment.** A webhook configured on the environment behind `master` does not automatically follow when you swap aliases. Use environment-targeted filters carefully.

## Locales

- One **default locale** per space. Often `en-US`.
- Each additional locale has a **fallback** chain. Missing fields can fall back to the default; turn that off for fields that must be authored per locale (e.g., `slug`).
- Localization is **per field** in the content type. A localizable field has a value object keyed by locale code in CMA payloads (`{ "en-US": "...", "fr-FR": "..." }`). Delivery / Preview / GraphQL flatten this for the requested locale.

## Tags vs. fields vs. references for metadata

- **Reference to a dedicated content type** (`Topic`, `Audience`) — when the value is itself rich (has its own description, slug, related content). Most taxonomies should be modeled this way.
- **`Symbol` field with allowed-values validation** — controlled vocabularies that are text-only and shared across types.
- **Tags / Taxonomy** — cross-cutting metadata that needs to apply to entries *and* assets and that doesn't deserve a content type. Use the Taxonomy API.

The mistake is to use array-of-Symbol for everything; it fragments to dozens of tag variants in a quarter.

## API surface — the eight you'll touch

| API | What for | Auth | Mutation? |
|---|---|---|---|
| **Content Delivery API (CDA)** | Read published content for production | Delivery token | No |
| **Content Preview API** | Read drafts + published for editorial / preview environments | Preview token | No |
| **Content Management API (CMA)** | Create / update / delete entries, assets, content types, environments, webhooks, etc. | Management token (PAT) or OAuth | Yes |
| **GraphQL Content API** | Schema-aware query layer over CDA / CDP | Delivery or Preview token | No |
| **Images API** | URL-driven image resizing, format conversion, focal-point cropping | None (CDN URL) | No |
| **User Management API** | Manage org users, roles, teams | Management token | Yes |
| **SCIM API** | IdP-driven user/group provisioning (enterprise SSO) | OAuth + SCIM token | Yes |
| **Sync API** | Incremental, offline-friendly content sync | Delivery token | No |

Plus the **Personalization Experience API** (former Ninetailed) for variant/audience targeting, and **GraphQL Subscriptions** for live updates.

See `api-surface.md` for a deeper map of when to reach for which.

## Rate limits — the ones that bite

- **CDA**: 78 reqs/sec per token, 7,800/min, generous burst. The CDN absorbs most of it; cold-cache pathological queries hurt.
- **CMA**: 7 reqs/sec per token. This is what catches people running scripts. Use the CLI's built-in throttle, or batch with backoff.
- **GraphQL**: complexity-budget based, not request-rate. A single deeply-nested query can exceed your complexity budget. Slim down with fragments and pagination.
- **Sync API**: throttled by default; first sync of a large space is slow. Plan an offline initial sync rather than a hot path.

## Webhooks at a glance

- Triggered by content events on a specific environment: `Entry.publish`, `Entry.unpublish`, `Entry.archive`, `Entry.delete`, `Entry.create`, `Entry.save`, `Entry.auto_save`, `Asset.*`, `ContentType.*`.
- Filterable by content type, environment, tags, custom expression.
- Payload is the standard Contentful entry envelope (sys + fields). Custom payload templates are supported.
- Sign with HMAC; verify on receipt. Never trust the source IP alone.
- Retries: exponential backoff for ~24 hours. Configure idempotent receivers.

See `contentful-webhooks` for full handling patterns.

## Source documents

- Domain model: https://www.contentful.com/developers/docs/concepts/domain-model/
- Data model: https://www.contentful.com/developers/docs/concepts/data-model/
- Links: https://www.contentful.com/developers/docs/concepts/links/
- Multiple environments: https://www.contentful.com/developers/docs/concepts/multiple-environments/
- Environment aliases: https://www.contentful.com/developers/docs/concepts/environment-aliases/
- Cross-space references: https://www.contentful.com/developers/docs/concepts/environment-support-for-cross-space-references/
- Best practices for environments and aliases: https://www.contentful.com/developers/docs/concepts/environments-and-environment-aliases-best-practices/
- Deployment pipeline: https://www.contentful.com/developers/docs/concepts/deployment-pipeline/
- Sync: https://www.contentful.com/developers/docs/concepts/sync/
- Rich Text: https://www.contentful.com/developers/docs/concepts/rich-text/
- Relational queries: https://www.contentful.com/developers/docs/concepts/relational-queries/
- Images: https://www.contentful.com/developers/docs/concepts/images/
- External references: https://www.contentful.com/developers/docs/concepts/external-references/
- Advanced caching: https://www.contentful.com/developers/docs/infrastructure/advanced-caching/
- Enterprise observability: https://www.contentful.com/developers/docs/concepts/enterprise-observability/
