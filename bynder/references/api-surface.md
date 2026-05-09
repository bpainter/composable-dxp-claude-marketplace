---
type: reference
project: skills-library
scope: plugin
plugin: bynder
tags: [type/reference, plugin/bynder, scope/foundational, topic/bynder, topic/api]
status: active
---
# Bynder API surface — when to reach for which

Five primary APIs plus webhooks and the Compact View embed surface. Each skill in this plugin tells you which one to use; this reference tells you why.

## Decision matrix

| If you need to... | Use | Skill |
|---|---|---|
| List, search, or fetch assets | Asset Bank API (REST) | `bynder-js-sdk`, `bynder-contentful-pairing` |
| Upload, update, or delete assets | Asset Bank API + chunked upload | `bynder-js-sdk`, `bynder-migration` |
| Manage metaproperties and options programmatically | Asset Bank API (Metaproperties endpoints) | `bynder-asset-model`, `bynder-migration` |
| Drive a creative-ops review/approval cycle | Workflow API | `bynder-permissions-workflow` |
| Pull asset usage and download analytics | Analytics API | `bynder-optimization-audit` |
| Manage Brand Guidelines content programmatically | Brand Guidelines API | `bynder-brand-guidelines` |
| Embed an asset picker in a host app | Universal Compact View + JS SDK | `bynder-compact-view`, `bynder-contentful-pairing` |
| React to asset events downstream | Webhooks | `bynder-webhooks-events` |
| Provision users / groups / profiles | Asset Bank API (User Management endpoints) | (admin; usually portal UI) |

## Asset Bank API

The core read/write surface for assets, metaproperties, tags, collections, derivatives.

- Base URL: `https://{your-portal}.bynder.com/api/v4/` (and `v5/` and `v6/` endpoints depending on the resource — the API has evolved in versions; check the [reference](https://api.bynder.com/docs/) for each endpoint's current version).
- Auth: OAuth2 (Authorization Code or Client Credentials) or Permanent Token.
- Returns JSON; assets carry `property_*` fields for metaproperty values, plus thumbnails block keyed by derivative name.
- Pagination: `limit` (max 1000) + `page` for list endpoints. Some search endpoints use cursor-style `lastDate` instead.
- Rate limit: ~5–10 req/sec/token typical; bulk uploads stricter.
- Filtering: `?propertyOptionId={uuid}` for metaproperty value filters, `?keyword=...` for free-text, `?type=image|video|document|audio|3d` for asset type.

Key endpoints:

```
GET    /api/v4/media/                    # list/search assets
GET    /api/v4/media/{id}/               # get a single asset
POST   /api/v4/media/                    # create an asset (kicks off upload)
POST   /api/v4/media/{id}/save/          # finalize upload, set metadata
PUT    /api/v4/media/{id}/               # update metadata
DELETE /api/v4/media/{id}/               # delete

GET    /api/v4/metaproperties/           # list metaproperties
POST   /api/v4/metaproperties/           # create metaproperty
PUT    /api/v4/metaproperties/{id}/      # update
POST   /api/v4/metaproperties/{id}/options/  # add an option to a select metaproperty

GET    /api/v4/tags/                     # list tags
GET    /api/v4/collections/              # list collections
GET    /api/v4/users/                    # list users
GET    /api/v4/profiles/                 # list permission profiles
```

Reach for it when: any programmatic interaction with the asset bank — from `bynder-js-sdk` to a custom marketplace app to a migration script.

Docs: https://api.bynder.com/docs/

## Workflow API

Drives the creative-ops side of Bynder — campaigns (top-level container), jobs (units of work), tasks (steps within a job), reviews and approvals.

Key endpoints:

```
GET    /workflow/campaigns/              # list campaigns
GET    /workflow/jobs/                   # list jobs (often filtered by campaign)
POST   /workflow/jobs/                   # create a job
GET    /workflow/jobs/{id}/tasks/        # list tasks in a job
POST   /workflow/jobs/{id}/tasks/        # add a task
POST   /workflow/tasks/{id}/review/      # submit a review decision
```

Reach for it when: integrating Bynder Workflow with an external project tool (Asana, Workfront, Jira), automating asset-creation pipelines, or pulling cycle-time metrics for adoption reporting.

Docs: https://api.bynder.com/docs/ (Workflow section).

## Analytics API

Daily-grain exports of asset behavior — downloads, views, searches, link-share opens, integration pulls. Lower-frequency than the other APIs (intended for reporting pipelines, not real-time queries).

Key endpoints:

```
GET    /api/v4/analytics/assets/         # asset-level usage
GET    /api/v4/analytics/searches/       # search behavior
GET    /api/v4/analytics/integrations/   # which connectors pulled what
GET    /api/v4/analytics/users/          # user activity
```

Reach for it when: building an adoption dashboard, doing an optimization audit (this is one of the most powerful inputs to `bynder-optimization-audit`), or proving ROI in the QBR with the client.

Docs: https://api.bynder.com/docs/ (Analytics section).

## Brand Guidelines API

Sections, pages, content blocks, downloadables. Lets you programmatically populate or sync guidelines content (e.g., from a design-system source of truth).

Key endpoints:

```
GET    /brandguidelines/sections/        # list top-level sections
GET    /brandguidelines/sections/{id}/   # get a section's pages
GET    /brandguidelines/pages/{id}/      # get a page's blocks
POST   /brandguidelines/pages/{id}/blocks/  # add a content block
```

Reach for it when: keeping Brand Guidelines in sync with a design system in code, or pre-populating guidelines during a brand launch / migration.

Docs: https://api.bynder.com/docs/ (Brand Guidelines section).

## Universal Compact View (UCV)

Not strictly an API in the REST sense — UCV is an embeddable widget that runs inside an iframe in a host application. It exposes a JS configuration surface (theme, allowed asset types, default metaproperty filters, return-derivative selection) and emits selection events back to the host via postMessage.

Wire it up via the `@bynder/bynder-compactview` package or via the host app's connector (Contentful's Bynder app uses UCV under the hood).

Two auth modes:

- **OAuth flow** — host renders the picker; user authenticates inline; picker enforces their permissions. Best for "editor uses CMS" scenarios.
- **Token flow** — host backend authenticates against Bynder, mints a short-lived session token, passes it to the embedded picker. Best for headless/server-side control.

Reach for it when: building any "pick an asset" UX outside Bynder itself.

Docs: https://developers.bynder.com/universal-compact-view

## Webhooks

Pushed JSON envelopes for asset and metaproperty events. Configured in the portal admin UI or via the Webhooks subsystem of the API.

Events to know:

- `asset.created`, `asset.updated`, `asset.deleted`, `asset.archived`
- `metaproperty.created`, `metaproperty.updated`, `metaproperty.deleted`
- `metaproperty_option.created`, `metaproperty_option.updated`, `metaproperty_option.deleted`

Payload is small — ID + event type + a few system fields. Receivers re-fetch the asset for current state.

Sign with HMAC-SHA256. Verify with the shared secret from the portal config.

Reach for it when: keeping a downstream cache (CDN, search index, CMS asset reference) in sync with Bynder. Pair with `contentful-webhooks` if Contentful is the consumer.

Docs: https://developers.bynder.com/webhooks/ (and the Webhooks section of the API reference).

## OAuth2 — the auth substrate

Three flows, all over OAuth2.

### Authorization Code (3-legged)

User-in-the-loop. Used when you're building an app a marketer will log into.

```
GET https://{portal}.bynder.com/v6/authentication/oauth2/auth
    ?client_id={CLIENT_ID}
    &redirect_uri={REDIRECT_URI}
    &response_type=code
    &scope=offline+asset:read+meta.assetbank:read
    &state={CSRF_TOKEN}

# user authenticates, redirected back with ?code=

POST https://{portal}.bynder.com/v6/authentication/oauth2/token
  grant_type=authorization_code
  code={CODE}
  client_id={CLIENT_ID}
  client_secret={CLIENT_SECRET}
  redirect_uri={REDIRECT_URI}

→ { access_token, refresh_token, expires_in, scope, token_type }
```

Refresh:

```
POST .../v6/authentication/oauth2/token
  grant_type=refresh_token
  refresh_token={REFRESH_TOKEN}
  client_id={CLIENT_ID}
  client_secret={CLIENT_SECRET}
```

### Client Credentials (2-legged, server-to-server)

No user. Used for backend integrations, batch jobs, webhook receivers that re-fetch.

```
POST .../v6/authentication/oauth2/token
  grant_type=client_credentials
  client_id={CLIENT_ID}
  client_secret={CLIENT_SECRET}
  scope=asset:read+meta.assetbank:read

→ { access_token, expires_in, scope, token_type }
```

No refresh token; re-issue at expiry.

### Permanent Token

Generated in the portal admin UI. Bound to a user. Use header:

```
Authorization: Bearer {PERMANENT_TOKEN}
```

Slalom default: don't ship production integrations on a permanent token tied to a person — it dies when the person leaves. Use Client Credentials.

### Scopes

| Scope | Grants |
|---|---|
| `offline` | Refresh tokens (only meaningful for Authorization Code flow) |
| `current.user:read` | Read the authenticated user |
| `asset:read` | List, search, get assets |
| `asset:write` | Create, update, delete assets |
| `meta.assetbank:read` | List metaproperties |
| `meta.assetbank:write` | Create, update, delete metaproperties |
| `collection:read` / `collection:write` | Collections |

Pass scopes space-separated (`+` URL-encoded). Always request the minimum needed.

Reference: https://api.bynder.com/reference/get_v6-authentication-oauth2-auth and https://api.bynder.com/docs/getting-started#authorization

## Cross-cutting concerns

- **Tokens belong in env vars and a secret manager, never in code or repos.** Rotate on staff changes (Permanent Tokens especially).
- **Rate limits are per token.** If you hit them, partition tokens by purpose (build-time codegen vs. runtime fetch vs. migration scripts).
- **Asset URLs come in two varieties** — public CDN URLs that work without auth, and private signed URLs that expire. Don't store signed URLs long-term; store the asset ID and re-resolve.
- **Errors return JSON with `status`, `code`, and `message`.** Log them; the HTTP status alone usually isn't enough.
- **Pagination defaults are small.** Asset Bank list endpoints default to 50; raise to 1000 for migrations or you'll do 200x the work.

## Source

- API reference: https://api.bynder.com/docs/
- Getting started: https://api.bynder.com/docs/getting-started
- OAuth2 specifics: https://api.bynder.com/reference/get_v6-authentication-oauth2-auth
- Webhook events: https://developers.bynder.com/webhooks/
- Universal Compact View: https://developers.bynder.com/universal-compact-view
