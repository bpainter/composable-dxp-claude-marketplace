---
name: contentful-delivery-optimization
description: Make Contentful-driven sites fast and stay fast — pick between Delivery and Preview correctly, use the Sync API for incremental hydration, exploit the Images API for responsive imagery, design CDN cache headers and Next.js revalidation tags together, and avoid the cold-cache cliffs that show up on launch day. Use this skill any time Core Web Vitals are at issue, when Contentful queries are showing up in performance traces, when image weight is killing LCP, when a search index needs incremental sync, when Vercel cache is fighting Contentful's CDN, or when the team is debating "static export vs. ISR vs. on-demand revalidation." Trigger on any "this is slow" question that touches Contentful.

# Project context
type: skill
project: skills-library
plugin: contentful
aliases: [contentful-delivery-optimization]
tags: [type/skill, plugin/contentful, topic/contentful, topic/performance]
status: active
---

# Contentful Delivery Optimization

Performance is a contract between Contentful's CDN, your Next.js cache, the browser, and the editor's mental model of "publish should be fast." This skill owns that contract.

Pair with `contentful-graphql` (query shape is the upstream variable), `contentful-react-wrapper` (where revalidation tags live), `contentful-webhooks` (the event source), `contentful-rich-text` (image-heavy bodies), and `contentful-localization` (locale-keyed cache fragmentation).

## When to use this skill

- Core Web Vitals work on a Contentful-driven site.
- Cold-cache pages on launch day are slow.
- Image weight is dominating LCP.
- A search index or downstream consumer needs to stay in sync without ramming the API.
- The team is debating static export, ISR, or on-demand revalidation.
- A dashboard or editor tool needs sub-second freshness.

## Core posture

- **Cache the steady state, invalidate on event.** Time-based revalidation as floor, on-demand as the actual mechanism.
- **Right API for the job.** Delivery for production, Preview for editorial, GraphQL for front-end queries, Sync for downstream hydration.
- **Don't fight Contentful's CDN.** Its cache is generally an asset; structure your queries so the CDN can serve them.
- **Image weight is a content problem before it's a code problem.** Author intent + Images API + `next/image` together.
- **Locale and preview shouldn't share cache space.** Tag and key correctly.

## Delivery vs. Preview

Two read APIs, two purposes:

| | Delivery | Preview |
|---|---|---|
| Reads | Published content only | Drafts + published |
| Token | `CONTENTFUL_DELIVERY_TOKEN` | `CONTENTFUL_PREVIEW_TOKEN` |
| Host | `cdn.contentful.com` (REST) / GraphQL via standard endpoint | `preview.contentful.com` / GraphQL with `preview: true` |
| CDN cache | Aggressive | Minimal |
| Rate limit | ~78 rps | Lower |
| Use for | Production | Editor preview routes only |

Production is Delivery only. Preview never leaks. The wrapper layer chooses per-request based on `draftMode()` (Next.js App Router); see `contentful-react-wrapper`.

## Caching layers

The stack from cold to hot:

```
Browser
  ↓
Edge cache (Vercel / Cloudflare)
  ↓
App-level cache (Next.js fetch cache, "force-cache" / tags / revalidate)
  ↓
Contentful CDN cache (cdn.contentful.com)
  ↓
Contentful origin
```

Each layer has its own invalidation. The trick is to structure them so an event invalidates *only* the layers that need it.

### Next.js fetch cache (App Router)

Three primary controls:

```ts
fetch(url, {
  next: {
    revalidate: 3600,         // safety-net TTL
    tags: ["page:home", "hero:abc123"],  // on-demand invalidation
  },
  cache: "force-cache" | "no-store",
});
```

Practical pattern:

- **`revalidate: 3600`** as the floor. If a webhook is dropped, content goes stale for at most an hour.
- **`tags`** are the actual mechanism. Each fetch tags itself with both an aggregate (`hero`) and a specific (`hero:${id}`) tag. The webhook handler revalidates the matching tags (see `contentful-webhooks`).
- **`cache: "no-store"`** in preview mode. Editors expect every page to reflect latest draft state.

### Contentful CDN

The CDA / GraphQL response is CDN-cached based on URL (and authorization). Same query → same cache key. Implications:

- Don't add cache-busting query parameters; you're paying full origin cost for every request.
- Time-stable variables (slug, id) are cheap; volatile variables (timestamps) are expensive.
- **Preview is largely uncached at the CDN layer**, which is why Preview traffic should be small.

### Browser

Standard `Cache-Control` semantics apply to your front-end responses. `s-maxage` for the edge, `max-age` for the browser, `stale-while-revalidate` for graceful serving.

## On-demand revalidation pattern (the canonical loop)

```
Editor publishes in Contentful
        │
        ▼
Webhook fires (signed, environment-filtered) — see contentful-webhooks
        │
        ▼
Handler in Next.js: revalidateTag("article:abc"); revalidateTag("article")
        │
        ▼
Next request to the affected page misses the Next.js cache, refetches from
Contentful, repopulates the cache, returns fresh content.
```

Tag design:

| Tag pattern | What it covers |
|---|---|
| `${contentType}:${id}` | a single specific entry — revalidates pages that read that entry |
| `${contentType}` | all entries of a type — revalidates listing pages |
| `page:${slug}` | a specific assembled page — revalidates that page even if the trigger isn't an entry on it |
| `nav` | global nav fetched on every page — revalidates all of `<layout>` |

Tag every fetch with the most specific tag that's accurate. Aggregate tags are cheap on the read; expensive on the invalidate (everything matching is recomputed).

### What if a webhook drops?

`revalidate: 3600` catches it. The page is stale for up to an hour. For business-critical content (homepage, top product), reduce to `revalidate: 600` or 300. Don't go below that without a reason; you're paying real origin cost.

## Sync API

The Sync API gives you a `nextSyncToken` after an initial pull and returns deltas on subsequent calls. Reach for it when:

- **Hydrating a search index.** Algolia, Typesense, Elasticsearch — sync once on initial setup, then poll deltas every few minutes (or react to webhooks for low-latency cases).
- **Building offline-capable apps.** Mobile, kiosks, or PWAs that need to operate disconnected.
- **Feeding analytics.** Push every published entry into a warehouse; you need the full set.

Pattern:

```ts
import { createClient } from "contentful";

const client = createClient({
  space: process.env.CONTENTFUL_SPACE_ID!,
  environment: process.env.CONTENTFUL_ENVIRONMENT ?? "master",
  accessToken: process.env.CONTENTFUL_DELIVERY_TOKEN!,
});

// Initial sync (one time per consumer, expensive)
let { entries, deletedEntries, nextSyncToken } = await client.sync({ initial: true });
saveTokenSomewhere(nextSyncToken);
indexAll(entries);

// Subsequent (cheap, run on a cadence or on webhook)
({ entries, deletedEntries, nextSyncToken } = await client.sync({ nextSyncToken: loadTokenSomewhere() }));
saveTokenSomewhere(nextSyncToken);
indexUpdates(entries);
removeDeletions(deletedEntries);
```

Implications:

- Initial sync of a large space is heavy. Run it offline; don't make production wait.
- The token *must* be persisted; lose it and you re-sync everything.
- Sync returns what's *changed*, not what's *current*. If you need the current state of a single entry, use CDA, not sync.
- Sync API does not return resolved references — it gives you the entries as-is. Joins happen in your indexer.

## Images API

Contentful asset URLs accept transformation parameters. The CDN caches per parameter set, so a sane responsive-image strategy is essentially free at scale.

Key parameters:

- **Size**: `?w=1200`, `?h=600` (proportions preserved by default)
- **Fit**: `?fit=fill | pad | scale | crop | thumb`
- **Focus**: `?f=face | center | top | left | ...` (focal point on `crop`)
- **Format**: `?fm=webp | avif | jpg | png` — usually let `auto` choose via `?fm=avif` fallback or rely on `next/image` to set the right one
- **Quality**: `?q=80` (1–100; default 75)
- **Background**: `?bg=rgb:f0f0f0` (for `fit=pad`)
- **Border radius**: `?r=12`

Pattern with `next/image`:

```tsx
<Image
  src={asset.url}            // Contentful asset URL
  alt={asset.description ?? ""}
  width={asset.width}
  height={asset.height}
  sizes="(min-width: 1024px) 50vw, 100vw"
  // Next will append its own width param via the loader; you can also pre-bake quality / format
/>
```

For art-directed crops on hero images, set the focal point via the Images API parameters or via the Contentful focal-point UI on the asset. Pass through to `next/image`.

For above-the-fold imagery, set `priority` on the `Image` and let `next/image`'s preload handle LCP.

## Query shape and CDN warmth

Some patterns hurt CDN warmth:

- **Polymorphic queries with locale + preview as variables.** Each combination is its own cache key. For a 5-locale site with preview, you're 10x your effective cache depth.
- **Large include depths (`?include=10`).** Single response is bigger; CDN payload is heavier; first-byte slower.
- **Time-window filters.** `where: { publishedAt_gte: <now-7d> }` keeps shifting cache keys. Pin to discrete buckets if possible (`gte: <last week's monday>`).

Patterns that help:

- **Stable variables.** Slug, id, locale are good. Timestamps and "now" are not.
- **Listing pages with deliberate page sizes.** 12, 24, 48 — not "give me everything I might need."
- **Per-component queries on server components.** Each block fetches its own slice in parallel; cache hits compound.

## Cold-cache launch day

The first 24 hours after launch are pathological because:

- Vercel has zero cached pages.
- Contentful CDN may also be cold for unique query shapes.
- Search indexes often hydrate in this window.

Mitigations:

1. **Pre-warm before flipping DNS.** Crawl the production URL list while traffic is small.
2. **Pre-build the static surface.** Set `dynamicParams = true` and `generateStaticParams` for known slugs so the first request is a build-cache hit.
3. **Warm the search index ahead of time** via a sync job, not a webhook flood.
4. **Lower `revalidate` for the first day**, then raise it.

## Choosing static, ISR, or on-demand

| Pattern | When |
|---|---|
| **Pure static** (`generateStaticParams` + no revalidate) | Marketing pages with rare updates, when build-time hydration is fine |
| **ISR (time-based)** | Listings that need to feel fresh but don't need second-by-second |
| **On-demand revalidation (tag-based)** | The default for Composable DXP work — webhook-driven cache invalidation |
| **Server-rendered (`no-store` or `revalidate: 0`)** | Personalized pages, dashboards, editor preview |

Slalom default is on-demand revalidation with a 1h floor. ISR is the fallback when the webhook surface is messy; server-rendered is for personalization and preview. Pure static is rare in modern composable work.

## Observability

What to instrument:

- **Cache hit/miss** per Contentful query (Vercel Analytics + custom headers).
- **Contentful response time** (p50/p95/p99) per query.
- **Webhook latency** — time from publish to invalidation to next-page-fetch returning fresh.
- **Image LCP** by route.
- **Sync job duration** and delta size.

Surface in a single dashboard so the team can spot regressions (cache fragmentation from a new locale, image weight from a new art-directed asset, query complexity from a new fragment).

## Common failure modes

- **Cache hit on preview.** `cache: "force-cache"` left in for the preview path. Editors lose minds. Always opt out in preview.
- **Aggregate tag invalidates everything.** A `revalidateTag("article")` on every article publish triggers the rebuild of every page that reads any article. Use specific tags for hot pages.
- **Locale-cross-contamination.** Tag `article:abc` invalidates fetches across all locales. Sometimes correct; sometimes you want `article:abc:fr-FR`. Be deliberate.
- **Image weight from non-optimized formats.** PNG hero at 2400×1200, no transform parameters. Eats LCP. Always parameterize.
- **Sync on the request path.** Sync API is for background hydration; do not put it on a hot request.
- **Per-request CDA fallback when GraphQL fails.** Belt-and-suspenders that doubles latency. Pick one and handle errors there.
- **Background revalidation never running.** `revalidate: 3600` only fires on next request after expiry. Low-traffic pages stay stale for days. Pair with explicit invalidation.

## How this skill relates to others

- Query shape is the upstream variable → `contentful-graphql`.
- Cache tags live in the wrapper → `contentful-react-wrapper`.
- The event source for invalidation → `contentful-webhooks`.
- Image-heavy bodies → `contentful-rich-text`.
- Locale-keyed cache fragmentation → `contentful-localization`.
- Variant-aware caching → `contentful-personalization`.
- App Router patterns → `software-engineering-nextjs-scaffold`.

## Source material

- CDA reference: https://www.contentful.com/developers/docs/references/content-delivery-api/
- GraphQL Content API: https://www.contentful.com/developers/docs/references/graphql/
- Sync API: https://www.contentful.com/developers/docs/concepts/sync/
- Images API: https://www.contentful.com/developers/docs/references/images-api/ and https://www.contentful.com/developers/docs/concepts/images/
- Advanced caching: https://www.contentful.com/developers/docs/infrastructure/advanced-caching/
- Next.js caching: https://nextjs.org/docs/app/building-your-application/caching
- Plugin reference: `../../references/api-surface.md`
