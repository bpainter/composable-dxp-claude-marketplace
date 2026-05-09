---
name: algolia-api-keys-security
description: >
  API-key strategy for Algolia — admin vs. search vs. monitoring vs. analytics keys, scoped keys with explicit ACLs, the secured API key pattern (HMAC-derived per-request keys with embedded filters and expiry) for multi-tenant filtering, IP allowlists, rate-limit partitioning, key rotation, and the audit trail. Use this skill any time the user is touching keys — first integration, multi-tenant rollout, security audit, key rotation after a staff change, or "we got a 403, what changed." Keys are the surface where production search experiences fail; treat them with discipline.

# Project context
type: skill
project: skills-library
plugin: algolia
aliases: [algolia-api-keys-security]
tags: [type/skill, plugin/algolia, topic/algolia, topic/security]
status: active
---

This skill puts you in the role of a platform engineer thinking about who gets to read what, and how to make that auditable. Default posture: **least-privilege everywhere, separate keys per workload, secured keys for any per-user filtering, rotate on staff changes.**

Pair with `algolia-indexing-pipeline` (the keys the indexer uses), `algolia-search-client` (the keys the runtime uses), and `algolia-instantsearch-react` / `algolia-autocomplete` (the keys that ship to the browser).

## When to use this skill

- First Algolia integration on a project — what keys to create, what ACLs, what rotation cadence.
- Multi-tenant SaaS where each user must only see their own records.
- Per-content-type access control (logged-in users see private articles; anonymous users see public).
- Security audit / penetration test prep.
- Key rotation after a staff change or suspected leak.
- Investigating a 403 in production.
- Adding a new workload (a build-time codegen, a new microservice).

## Core posture

- **Admin keys never leave the server.** Period. Not in env vars labeled `NEXT_PUBLIC_*`. Not in CI logs. Not in screenshots.
- **One key per workload.** Indexer key, settings-deploy key, browser search-only key, secured-key parent for per-user filtering. Don't share.
- **Secured keys are signed envelopes.** They embed filters + expiry + IP that the user can't tamper with. Use them for multi-tenant.
- **IP allowlist where you can.** Keys used only from a known set of CI/runtime IPs should be locked down.
- **Rotate on staff changes.** When someone with key access leaves, rotate. When a key leaks, rotate immediately.
- **Audit logs exist.** The Dashboard shows recent key activity. Use it.

## The key types

Algolia keys come in two layers:

### 1. Built-in keys (created with the application, can't be deleted)

| Key | What it does | Who sees it |
|---|---|---|
| **Admin API Key** | Full read/write/configure on everything | Server runtime, account owner only |
| **Search-Only API Key** | Read-only search across all indices | Sometimes the browser; usually a parent for secured keys |
| **Monitoring API Key** | Read monitoring endpoints | Observability tools |
| **Usage API Key** | Read billing and usage | Finance / ops dashboards |
| **Analytics API Key** | Read analytics endpoints | BI / dashboards |

The Admin key is the nuclear option. It can `clearIndex`, `deleteIndex`, change settings, rotate keys (yes, including itself), invite users. **Never** put it where it's not strictly needed.

### 2. Custom (scoped) keys

Created via the Search API (`POST /1/keys`) or Dashboard. Each scoped key has:

- An array of **ACLs** — `search`, `browse`, `addObject`, `deleteObject`, `deleteIndex`, `settings`, `editSettings`, `analytics`, `usage`, `recommendation`, `logs`, `seeUnretrievableAttributes`.
- Optional **index restriction** — limits the key to specific indices (or a glob).
- Optional **rate limits** — `maxQueriesPerIPPerHour`, `maxHitsPerQuery`.
- Optional **referer restriction** — only requests from a given domain are accepted.
- Optional **validity** — `validUntil` (Unix seconds) for time-bound keys.
- Optional **description** — for the audit trail. Always set it.

Use scoped keys for every workload that isn't the runtime browser search. Examples:

| Workload | Recommended ACLs | Index restriction |
|---|---|---|
| Vercel webhook indexer | `addObject`, `deleteObject`, `editSettings` | The indices it manages |
| CI settings/rules deploy | `editSettings`, `settings`, `analytics` | The indices it deploys |
| MCP server (read-only ops) | `search`, `browse`, `listIndexes`, `analytics` | All |
| MCP server (write session) | `search`, `browse`, `listIndexes`, `addObject`, `editSettings` | Specific indices, time-bound |
| Algolia CLI in CI | Same as the CI use case | Specific |
| BI dashboard puller | `analytics`, `usage` | All |
| Dataset extractor | `browse`, `search` | Specific |

### 3. Secured API keys — the multi-tenant pattern

A secured API key is **derived from a parent search key**, plus extra parameters embedded and HMAC-signed. The user can't strip the filters.

```ts
import { algoliasearch } from 'algoliasearch';

const SEARCH_KEY = process.env.ALGOLIA_SEARCH_KEY!;

const securedKey = algoliasearch(APP_ID, SEARCH_KEY).generateSecuredApiKey({
  parentApiKey: SEARCH_KEY,
  restrictions: {
    filters: 'tenantId:42 AND visibility:public',
    validUntil: Math.floor(Date.now() / 1000) + 3600, // 1 hour
    userToken: `user-${userId}`,                      // also pins userToken for Personalization
    restrictIndices: ['articles', 'glossary_terms'],
    restrictSources: '203.0.113.0/24',                // optional IP cidr
  },
});

// Pass `securedKey` to the front end. Use it as the API key in the lite client.
```

The browser then makes search calls with this key. Algolia validates the signature and merges the embedded filters with any front-end query — the user's filters can't loosen the embedded ones.

### When to use secured keys

- Multi-tenant SaaS — every search is filtered to the current tenant.
- Logged-in vs. anonymous content — different keys for different access levels.
- Per-user Personalization — bake `userToken` into the key.
- Time-bound trial access — `validUntil` for a temporary session.

### When NOT to use secured keys

- Public-only content — a plain search-only key is simpler.
- Anything that would make the embedded filter querystring-visible. (Secured keys are opaque tokens; they don't leak the filters.)
- High-throughput unauthenticated traffic — generating per-request secured keys adds CPU; cache by tenant + role for short windows.

## Putting keys in env vars

```bash
# .env (server only — never committed, never NEXT_PUBLIC)
ALGOLIA_APP_ID=ABC123
ALGOLIA_ADMIN_KEY=                 # Admin — only the indexer / CI / scripts
ALGOLIA_INDEXER_KEY=               # Scoped: addObject, editSettings on specific indices
ALGOLIA_SETTINGS_DEPLOY_KEY=       # Scoped: editSettings, settings, analytics
ALGOLIA_SEARCH_KEY=                # Search-only — parent for secured keys

# .env (client — NEXT_PUBLIC required for browser bundling)
NEXT_PUBLIC_ALGOLIA_APP_ID=ABC123
NEXT_PUBLIC_ALGOLIA_SEARCH_KEY=    # Search-only, low-privilege; OR omitted in favor of secured keys
```

Never `NEXT_PUBLIC_ALGOLIA_ADMIN_KEY`. The IDE's autocomplete shouldn't even see that name.

## Secured-key pattern in a Next.js Server Component

```tsx
// app/search/page.tsx
import { generateSecuredApiKey } from 'algoliasearch';
import { getServerSession } from '@/lib/auth';
import { SearchClient } from './search-client';

export default async function SearchPage() {
  const session = await getServerSession();
  const tenantId = session?.user?.tenantId ?? null;

  // Build the secured key on the server, fresh each render (or cached for 5 min)
  const securedKey = tenantId
    ? generateSecuredApiKey(process.env.ALGOLIA_SEARCH_KEY!, {
        filters: `tenantId:${tenantId} AND visibility:tenant`,
        validUntil: Math.floor(Date.now() / 1000) + 3600,
        userToken: `user-${session!.user!.id}`,
      })
    : process.env.NEXT_PUBLIC_ALGOLIA_SEARCH_KEY!; // fallback for anonymous → public-only

  return <SearchClient apiKey={securedKey} />;
}
```

The Client Component instantiates the lite client with the supplied key. The user can't see the filters; they can only see the opaque token.

For Server Actions or Route Handlers issuing the search themselves, just use the search key directly with the filters as normal request params — secured keys are about *trusting the client*, not the server.

## Rotation

```
Cadence:
- Quarterly for non-critical scoped keys.
- Immediately on staff offboarding for any key the leaver had access to.
- Immediately on suspected leak.
- Immediately if the key was logged anywhere it shouldn't be (CI logs, error tracking, screenshots).

Process:
1. Create the new key (Dashboard or API).
2. Deploy the new key to the runtime that uses it.
3. Verify the new key works.
4. Delete (or `validUntil`-set-to-now) the old key.
5. Watch for 403s in logs over the following hour.
```

Don't rotate by overwriting in place. Always create-deploy-verify-retire to avoid downtime.

## Rate limit and DDoS protection

Algolia's plans include burst tolerance, but a key in the wild that gets abused can spike usage and bill. Mitigations:

- **`maxQueriesPerIPPerHour`** on browser-facing keys. Stops scrapers from running through your traffic at high QPS.
- **`maxHitsPerQuery`** caps the largest possible response. Stops `hitsPerPage: 1000` abuse.
- **Referer restriction** (`referers: ['*.example.com/*']`) on browser keys. Blocks usage from other origins. Not a security boundary (referer is spoofable) but a reasonable abuse deterrent.
- **IP allowlist** (`restrictSources`) on server-only keys. Strong boundary for known runtimes (Vercel egress, CI runners).

## Auditing

The Dashboard's Logs view shows recent operations per key. For deeper audit:

- **Logs API** — `GET /1/logs` returns the recent N operations per app. Pipe to your log warehouse.
- **Custom audit** — every key creation/deletion via API is itself logged. Watch the Admin key's activity.

For high-compliance engagements, set up a daily cron that pulls Logs and pushes to the engagement's SIEM.

## Multi-environment posture

Per-environment **applications** (not just per-environment indices) is the recommended pattern. Why:

- Keys are per-app. A leak of a staging key doesn't compromise prod.
- Rate limits are per-app. Staging traffic doesn't starve prod.
- Audit logs are per-app. Easier to read.
- A `clearIndex` mistake in staging stays in staging.

Per-engagement, expect:

- `slalom-{client}-prod`
- `slalom-{client}-staging`
- `slalom-{client}-dev`

Each with its own admin key, its own search key, its own scoped keys for indexing and CI.

## Output formats

### Key plan for an engagement

```
# Algolia Key Plan: [Engagement]

## Applications
- slalom-{client}-prod    appId=...
- slalom-{client}-staging appId=...

## Keys (per application)
| Key | ACLs | Index restriction | Stored where | Rotation |
|---|---|---|---|---|
| Admin | * | * | 1Password (ops vault) | On staff change |
| Indexer | addObject, deleteObject, editSettings | articles*, glossary_terms* | Vercel env (server) | Quarterly |
| Settings deploy | editSettings, settings, analytics | * | GitHub Actions secret | Quarterly |
| Search-only | search | * | Vercel env (NEXT_PUBLIC) + parent for secured keys | Annually |
| MCP read | search, browse, listIndexes, analytics | * | Local (per-engineer) | On engagement end |

## Secured-key pattern
- When: on every search request from authenticated users
- Filter: `tenantId:{user.tenantId} AND ...`
- Expiry: 1 hour
- userToken: `user-{user.id}`

## Rotation runbook
- Trigger conditions
- Process (create new → deploy → verify → retire old)
- Verification (synthetic search call from new key)

## Audit
- Logs API → SIEM (daily)
- Quarterly key inventory review
```

## Common anti-patterns

- **Admin key in `NEXT_PUBLIC_*`.** Catastrophic. Rotate immediately if discovered.
- **One key for everything.** No least-privilege; every workload has the blast radius of the most-privileged.
- **Browser key with no rate limit.** Inviting abuse.
- **Secured-key generation in the browser.** Defeats the purpose; the parent key would have to be in the browser. Server-only.
- **`validUntil` set far in the future on secured keys.** A leaked secured key is valid until expiry; short windows minimize blast radius.
- **No description on scoped keys.** Six months later, nobody knows what a key is for.
- **Hardcoded keys in code.** Even temporarily, even in tests.
- **Sharing keys across environments.** A staging mistake takes down prod.
- **No rotation log.** Rotate, but you can't prove you rotated.

## Constraints

- Don't deploy without per-workload scoped keys.
- Don't ship a multi-tenant search without secured keys.
- Don't put admin keys in CI logs (mask them in your CI tool).
- Don't generate secured keys with `validUntil` longer than the user's session.
- Don't treat the audit log as optional. It's the only forensic trail when something goes wrong.

## How this skill relates to others

- The runtime that uses these keys → `algolia-search-client`, `algolia-instantsearch-react`, `algolia-autocomplete`.
- The pipeline that uses the indexer key → `algolia-indexing-pipeline`.
- The MCP/CLI that needs scoped keys for ops → `algolia-mcp-cli`.
- Per-user identity for Personalization (`userToken`) → `algolia-personalization-ai`, `algolia-analytics-events`.

## Source material

- API keys overview: https://www.algolia.com/doc/guides/security/api-keys/
- Secured API keys: https://www.algolia.com/doc/guides/security/api-keys/how-to/user-restricted-access-to-data/
- ACL list: https://www.algolia.com/doc/guides/security/api-keys/how-to/restrict-access-with-virtual-api-keys/
- Logs API: https://www.algolia.com/doc/api-reference/api-methods/get-logs/
- Plugin reference: `../../references/algolia-foundations.md`
