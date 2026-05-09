---
type: reference
project: skills-library
scope: plugin
plugin: contentful
tags: [type/reference, plugin/contentful, scope/foundational, topic/contentful, topic/api]
status: active
---
# Contentful API surface — when to reach for which

Eight APIs, plus the Personalization Experience API. Each skill in this plugin tells you which one to use; this reference tells you why.

## Decision matrix

| If you need to... | Use | Skill |
|---|---|---|
| Read published content for production | CDA (REST) or **GraphQL** | `contentful-react-wrapper`, `contentful-graphql` |
| Read drafts + published in preview/staging | Preview API or GraphQL with `preview: true` | `contentful-react-wrapper` |
| Create / edit / delete entries, assets, content types | CMA | `contentful-migrations`, `contentful-mcp-cli` |
| Migrate schema as code | CMA via `contentful-migration` library | `contentful-migrations` |
| Manage spaces, environments, aliases, webhooks | CMA | `contentful-space-architect`, `contentful-webhooks` |
| Sync large content sets incrementally | Sync API | `contentful-delivery-optimization` |
| Resize / convert images on the fly | Images API (URL params) | `contentful-delivery-optimization`, `contentful-rich-text` |
| Provision users/groups from an IdP | SCIM | (enterprise admin; out of plugin scope but referenced) |
| Manage roles, memberships programmatically | User Management API | (enterprise admin) |
| Personalize content by audience / experiment | Personalization Experience API | `contentful-personalization` |

## CDA — Content Delivery API

Read-only, CDN-fronted, the production read path. REST or GraphQL. Returns published content only. Token: `CONTENTFUL_DELIVERY_TOKEN`.

- Auto-resolves linked entries and assets up to `?include=N` (default 1, max 10).
- Filterable via field-level query params (`fields.slug=foo`, `fields.publishedAt[gte]=...`).
- Locale-aware via `?locale=en-US` or `?locale=*` for all.
- Rate limit: ~78 req/sec/token, with generous burst behind the CDN.
- Caching: aggressive CDN cache headers. Use the `If-Modified-Since` / ETag pattern for revalidation.

Reach for it when: REST is simpler than building a GraphQL query, when you need an `include` graph, or when you're building incremental sync.

Docs: https://www.contentful.com/developers/docs/references/content-delivery-api/

## Preview API

Same shape as CDA but reads draft + published. Lower rate limits, no CDN caching by default. Token: `CONTENTFUL_PREVIEW_TOKEN`. Host is `preview.contentful.com`.

Reach for it when: building a preview environment, embedding draft content in the editor experience, or validating an unpublished change.

Never use Preview in production. The token can read drafts; if it leaks, unpublished business content is exposed.

Docs: https://www.contentful.com/developers/docs/references/content-preview-api/

## CMA — Content Management API

Read/write. The full administrative surface — entries, assets, content types, locales, environments, roles, webhooks, app installations, scheduled actions, tasks, comments. Token: `CONTENTFUL_MANAGEMENT_TOKEN` (or OAuth). Host is `api.contentful.com`.

- Rate limit: 7 req/sec/token. Strict. Use built-in throttling in the CLI / SDKs.
- Server-only. Never expose a CMA token to the browser.
- Pagination is mandatory for large lists.
- Idempotency: most updates require a `version` header to prevent lost updates.

Reach for it when: writing migrations, doing bulk edits, building an integration that needs to author content, or extracting data for analysis. Pair with the `contentful-management` SDK or `contentful-migration`.

Docs: https://www.contentful.com/developers/docs/references/content-management-api/

## GraphQL Content API

Schema-aware query layer over Delivery and Preview. One endpoint per environment: `https://graphql.contentful.com/content/v1/spaces/{spaceId}/environments/{env}`.

- Auth header: Bearer Delivery or Preview token.
- Selection-set bandwidth: ask only for the fields you render.
- Polymorphic unions for Topics & Assemblies (`... on Hero { ... }`).
- Built-in pagination via `skip`, `limit`, and `_collection` types.
- Complexity-budget rate limit: deep nested queries cost more than wide flat ones.

Reach for it when: building any non-trivial front end. The selection-set discipline pays off in performance and clarity. Pair with GraphQL Code Generator for typing.

Docs: https://www.contentful.com/developers/docs/references/graphql/

## Images API

URL-parameter-driven image transformation. No SDK needed; you append params to the asset URL.

- `?w=1200` width, `?h=600` height, `?fit=fill|pad|scale|crop|thumb`, `?f=face|center|top|...` focal area, `?fm=webp|avif|jpg|png` format, `?q=80` quality.
- Cached by the CDN — same URL parameters → same cached image.
- Use with `next/image` to layer Next's optimization on top.

Reach for it always when rendering Contentful assets. Hand-tuning in the wrapper is faster than waiting for editors to upload pre-optimized versions.

Docs: https://www.contentful.com/developers/docs/references/images-api/

## User Management API

Org-level admin: spaces, organizations, memberships, roles, teams, space-membership invitations.

Reach for it when: standing up a new client space programmatically, automating offboarding, or building a custom admin tool. Day-to-day delivery rarely touches this — admins handle it in the UI.

Docs: https://www.contentful.com/developers/docs/references/user-management-api/

## SCIM API

System for Cross-domain Identity Management. The standard IdP-to-app provisioning protocol. Enterprise-only.

Reach for it when: configuring SSO/IdP-driven user lifecycle. Hand off to the client's identity team; this isn't a delivery-team concern in most engagements.

Docs: https://www.contentful.com/developers/docs/references/scim-api/

## Sync API

Incremental sync. Initial sync returns all content; subsequent calls return deltas via a `nextSyncToken`.

Reach for it when: building offline-capable apps, hydrating a search index (Algolia), warming a downstream cache, or feeding an analytics pipeline. Don't use it for primary read paths — CDA + GraphQL are faster.

Docs: https://www.contentful.com/developers/docs/concepts/sync/

## Personalization Experience API

The targeting/audience/variant evaluation runtime. The Contentful UI defines audiences and experiences; this API tells your front end which variant to show for a given visitor profile.

- Edge-friendly — runs server-side or at the edge function layer
- Returns the resolved variant + the targeting decision metadata
- Pairs with the front-end Personalization SDK for analytics/event tracking

Reach for it when: implementing audience-based personalization, A/B tests, or feature flags driven by Contentful.

Docs: https://www.contentful.com/developers/docs/personalization/experience-api/

## Cross-cutting concerns

- **Tokens belong in env vars, never in code or repos.** Rotate on staff changes.
- **Rate limits are per token.** If you hit them, partition tokens by purpose (build-time codegen vs. runtime fetch) so one bad migration doesn't take down the front end.
- **Versioning headers** (`X-Contentful-Version`) are required on most CMA writes. Lost-update protection.
- **Errors return JSON with `sys.id` and `message`.** Log them; they're more useful than the HTTP code alone.
