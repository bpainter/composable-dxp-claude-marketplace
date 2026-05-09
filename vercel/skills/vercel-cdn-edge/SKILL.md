---
name: vercel-cdn-edge
description: Make the Vercel Edge Network and CDN do what you want — cache layers and headers, ISR / PPR behavior on Vercel's runtime, on-demand revalidation, image optimization metering, bandwidth cost guardrails, and when to bypass to an external CDN (Cloudflare, Bunny). Use this skill when caching is misbehaving, when bandwidth or transform costs are spiking, when "why is this stale" or "why is this slow" questions land, or when a launch is imminent and the cache strategy needs review. Trigger on any "edge" or "CDN" question on Vercel.

# Project context
type: skill
project: skills-library
plugin: vercel
aliases: [vercel-cdn-edge]
tags: [type/skill, plugin/vercel, topic/vercel, topic/cdn, topic/edge]
status: active
---

# Vercel CDN & Edge Network

Owns the cache and edge-delivery layer. Pair with `vercel-fluid-compute` (cache reduces function invocations), `vercel-deploy-pipeline` (cache invalidation on deploy), `software-engineering-nextjs-cache` (the framework-level `"use cache"` decisions), `software-engineering-nextjs-performance` (image optimization patterns), and `contentful-delivery-optimization` (when CMS-driven cache shapes are involved).

## When to use this skill

- Cache is stale longer than expected.
- Cache is recomputing too often (cost spike).
- "Why is this image so slow on first load?"
- Image transform costs are high.
- Bandwidth bill is surprising.
- Launch day cold-cache cliff.
- Considering a CDN bypass to Cloudflare / Bunny.
- ISR / PPR behaviors that don't match local development.

## Core posture

- **Cache the steady state, invalidate on event.** Time-based revalidation as floor, on-demand as the actual mechanism.
- **Don't fight Vercel's CDN.** It's smart; structure responses so it can do its job.
- **Image optimization is metered.** `sizes` discipline saves money.
- **Bandwidth costs scale.** At very high scale, an external CDN can be cheaper. Most projects are fine native.
- **Launch-day cold cache is real and predictable.** Pre-warm.

## The Vercel Edge Network

Every deployment is fronted by Vercel's global edge network. Responses are cached at the edge based on:

- The route's caching directives (`Cache-Control`, Next.js fetch options).
- The runtime (static vs. ISR vs. dynamic vs. streamed).
- The function's response headers.

Caching layers (top to bottom):

```
Browser cache
  ↓ (max-age, must-revalidate)
Vercel Edge cache (CDN)
  ↓ (s-maxage, stale-while-revalidate, surrogate keys for tag-based invalidation)
Vercel Function execution
  ↓
Origin (your code, your data sources)
```

Each layer has its own invalidation. Tag-based invalidation (`revalidateTag`) talks to the Edge cache layer; deploy invalidation flushes the Function-output cache.

## Static, ISR, PPR, Dynamic — the four modes

Next.js 16 on Vercel has four routing-level cache shapes:

| Mode | When | Vercel behavior |
|---|---|---|
| **Static (SSG)** | `generateStaticParams` + no revalidate | Built at deploy, cached forever at edge until next deploy |
| **ISR (Incremental Static Regeneration)** | `revalidate: N` or `revalidate: false` with on-demand tags | Cached at edge; refreshed in background per tag or on time-based interval |
| **PPR (Partial Prerendering)** | `experimental.ppr: true` + Suspense for dynamic parts | Static shell + dynamic Suspense islands; Vercel streams the dynamic parts |
| **Dynamic** | `cache: "no-store"`, `dynamic: "force-dynamic"`, or runtime-only signals (`headers()`, `cookies()`) | Function executes per request; CDN doesn't cache |

For Slalom Composable DXP defaults: **PPR or ISR with on-demand tag revalidation**. Pure static for marketing pages with rare updates; dynamic only for personalized routes.

## Cache directives and tags

```ts
// In a server component fetch
const data = await fetch(url, {
  next: {
    revalidate: 3600,           // safety-net TTL in seconds
    tags: ["page:home", "hero:abc123"],
  },
});
```

Then in a webhook handler or server action:

```ts
import { revalidateTag, revalidatePath } from "next/cache";

revalidateTag("hero:abc123"); // invalidates everything tagged with this
revalidatePath("/products/[slug]"); // invalidates this path's segments
```

Patterns:

- **Specific tags** (`hero:abc123`) for surgical invalidation.
- **Aggregate tags** (`articles`, `nav`) for bulk invalidation.
- **Path-based** (`revalidatePath`) when you don't know which entries changed but you know the affected page.

For Contentful-driven sites, see `contentful-webhooks` and `contentful-delivery-optimization` for the canonical webhook → tag pattern.

## ISR vs. on-demand revalidation

```
ISR (time-based):
  fetch(... { next: { revalidate: 3600 } })
  Page is cached for up to 1 hour. After expiry, the next request triggers a
  background regen; subsequent requests get the new version.

On-demand (tag-based):
  fetch(... { next: { tags: ["page:home"] } })
  + webhook calls revalidateTag("page:home") on event
  Page is invalidated the moment the event fires; next request rebuilds.

Combined (the right pattern):
  fetch(... { next: { revalidate: 3600, tags: ["page:home"] } })
  On-demand is primary; time-based is the safety net for missed webhooks.
```

For Slalom defaults: combined, with `revalidate` set high enough that the time-based branch rarely fires (3600–86400s depending on content cadence).

## PPR (Partial Prerendering)

PPR splits a route into static + dynamic parts:

```tsx
// app/page.tsx
export const experimental_ppr = true;

export default function Page() {
  return (
    <>
      <StaticHero />                       {/* prerendered */}
      <Suspense fallback={<Skeleton />}>
        <PersonalizedRecommendations />    {/* streamed in */}
      </Suspense>
      <StaticFooter />                     {/* prerendered */}
    </>
  );
}
```

Vercel serves the static shell from the CDN immediately; the dynamic Suspense parts stream from a Function. Best of both worlds for marketing pages with lightly personalized elements.

When to use:

- Page is mostly static but has small personalized / dynamic islands.
- LCP wins from instant shell delivery.
- The dynamic islands tolerate streaming.

When to skip:

- Fully dynamic (everything is per-request).
- Fully static (no dynamic islands; ISR is simpler).

## Image optimization

`next/image` on Vercel uses Vercel's image optimizer. Each unique combination of `(source, width, format, quality)` is a **transform** — billed per-transform.

Optimization rules:

1. **Allowlist domains.** `images.remotePatterns` in `next.config.mjs`. Vercel will refuse to proxy unknown hosts.
2. **`sizes` discipline.** Tells the browser which width to request; Vercel only generates those.
   ```tsx
   <Image src={url} sizes="(min-width: 1024px) 50vw, 100vw" ... />
   ```
3. **Format defaulting.** `next/image` picks AVIF / WebP automatically. Don't override unless you have a reason.
4. **Quality.** Default 75 is usually fine; lower for non-hero images.
5. **`priority`** on the LCP image only.
6. **Vercel Blob** integrates natively — same domain, automatic optimization, fewer cross-origin headaches.

For very heavy use (millions of unique images, complex art direction), consider:

- **Imgix / Cloudinary** — bypass Vercel's optimizer entirely; their pricing model favors that scale.
- **Pre-optimized assets** in the CMS (DAM-handled transforms) — Vercel just serves them.

## Bandwidth and cost

Bandwidth (egress) is the cost line that surprises clients most. Drivers:

- **Cached pages**: cheap. Edge serves from cache; bandwidth counts but no function execution.
- **Dynamic pages**: more expensive. Function execution + bandwidth.
- **Image optimization output**: counts as bandwidth in addition to transform cost.
- **Asset downloads**: linear in size.

Cost control:

- Set spend caps on the Team account.
- Audit large response sizes (should that JSON really be 2MB?).
- Use `next/image` with `sizes` properly.
- Consider Cloudflare or Bunny in front of Vercel for hundreds of TB/mo workloads.

## Bypassing to an external CDN

For very high bandwidth, the cost math sometimes favors an external CDN sitting in front:

```
User → Cloudflare → Vercel
                     ↑
                   (Functions still run on Vercel)
```

Pros: Cloudflare's bandwidth pricing scales differently; gets cheaper at TB volumes.
Cons: Two cache layers to manage; observability split; some Vercel features (Speed Insights) need careful configuration.

Decision threshold: if Vercel bandwidth costs exceed ~$2k/month, do the math on Cloudflare in front. Below that, native Vercel is operationally simpler.

## Headers

Custom headers via `vercel.json` or `next.config.mjs`:

```js
// next.config.mjs
async function headers() {
  return [
    {
      source: "/(.*)",
      headers: [
        { key: "Cache-Control", value: "public, max-age=0, s-maxage=3600, stale-while-revalidate=86400" },
        { key: "X-Frame-Options", value: "DENY" },
        { key: "X-Content-Type-Options", value: "nosniff" },
        { key: "Referrer-Policy", value: "strict-origin-when-cross-origin" },
        { key: "Permissions-Policy", value: "camera=(), microphone=(), geolocation=()" },
      ],
    },
  ];
}
```

Cache headers behavior on Vercel:

- `s-maxage=N` controls Vercel Edge cache TTL.
- `max-age=N` controls browser cache.
- `stale-while-revalidate=N` lets Vercel serve stale while revalidating in background.
- `Cache-Control: no-store` skips Vercel cache entirely.

For Next.js app-router routes, prefer `revalidate` and tags over manual `Cache-Control` headers — Next.js manages the cache layer for you.

## Launch-day cold cache

The first 24 hours after launch have predictable cliffs:

- Edge cache is empty for unique URL/variable combinations.
- Image transforms haven't been generated.
- ISR hasn't built any pages yet.

Mitigations:

1. **Pre-warm.** Crawl the production URL list before flipping DNS. Vercel's edge fills as you crawl.
2. **Pre-build.** `generateStaticParams` for known slugs so first request is a build-cache hit.
3. **Tighten `revalidate` for the first day**, then loosen.
4. **Watch the Speed Insights dashboard** for tail latency that drops off as cache warms.

## Common failure modes

- **`cache: "force-cache"` left on a personalized route.** Different users see the same cached page. Audit before launch.
- **No on-demand revalidation hooked up.** Editor publishes; site stays stale until time-based revalidate fires.
- **Tag aggregation invalidates everything.** A `revalidateTag("articles")` on every article publish recomputes every page that lists articles. Use specific tags for high-traffic pages.
- **Image transform explosion.** Same image at 50 different widths generates 50 transforms. Use `sizes` to constrain.
- **Bandwidth spike from uncached API responses.** A `cache: "no-store"` path hammered by a polling client. Either cache or move to a streaming pattern.
- **Cache fragmenting on locale × variant × preview.** Each combination is a separate cache key. Be deliberate.
- **`vercel.json` rewrite shadowing Next.js routing.** Two sources of truth disagreeing. Pick one.
- **Stale CDN after env var change.** Env vars don't invalidate the CDN. Redeploy.

## Output formats

### Cache strategy doc

```
# Cache Strategy: [Project]

## Per-route mode
| Route | Mode | revalidate (s) | Tags | Notes |
|---|---|---|---|---|
| /(marketing) | PPR | 3600 | page:*, nav | static shell + personalized hero |
| /products/[slug] | ISR | 86400 | product:[id] | webhook-driven |
| /api/auth | dynamic | n/a | n/a | per-request |

## Invalidation triggers
- Contentful Entry.publish → revalidateTag("[contentType]:[id]")
- Algolia index sync → revalidateTag("search")
- Manual flush → POST /api/admin/flush

## Image strategy
- Source: Contentful / Vercel Blob
- Sizes default: "(min-width: 1024px) 50vw, 100vw"
- Quality: 75
- Format: auto

## Cost guardrails
- Bandwidth: spend cap $X
- Transforms: alert at Y% of monthly budget
- Function GB-s: alert at Y%

## Risks
- {launch-day cold cache plan}
- {high-traffic events}
```

## How this skill relates to others

- Function execution count is the upstream variable for cost → `vercel-fluid-compute`.
- Deploy-time invalidation → `vercel-deploy-pipeline`.
- The framework-side cache decisions → `software-engineering-nextjs-cache`.
- Image optimization patterns → `software-engineering-nextjs-performance`.
- Cache observability → `vercel-observability`.
- CMS-driven cache shapes → `contentful-delivery-optimization`, `contentful-webhooks`.

## Source material

- Vercel CDN: https://vercel.com/cdn
- Edge Network: https://vercel.com/docs/edge-network
- Caching: https://vercel.com/docs/edge-network/caching
- Image Optimization: https://vercel.com/docs/image-optimization
- ISR: https://nextjs.org/docs/app/building-your-application/data-fetching/incremental-static-regeneration
- PPR: https://nextjs.org/docs/app/api-reference/next-config-js/ppr
- Plugin reference: `../../references/vercel-foundations.md`
