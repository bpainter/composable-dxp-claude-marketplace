---
name: vercel-fluid-compute
description: Make the right runtime and function-config decisions on Vercel — Fluid Compute (the Node-runtime concurrency model that's the default for new projects), the Edge runtime (V8 isolates for low-latency paths), per-function settings (`maxDuration`, `memory`, `regions`), cold-start mitigation, and the trade-offs that show up only on Vercel. Use this skill when a function is slow, when "Edge or Node?" comes up, when functions are getting killed at the duration limit, when cold-start latency is the complaint, or when costs are spiking on function executions. Trigger on any "how do we configure this function" question.

# Project context
type: skill
project: skills-library
plugin: vercel
aliases: [vercel-fluid-compute]
tags: [type/skill, plugin/vercel, topic/vercel, topic/runtime, topic/fluid-compute]
status: active
---

# Vercel Fluid Compute & Runtimes

Owns the runtime layer. The framework (Next.js) decides what code runs; this skill decides how Vercel runs it.

Pair with `vercel-deploy-pipeline` (function config lives in `vercel.json`), `vercel-cdn-edge` (cache decisions affect runtime cost), `vercel-observability` (function logs and durations), and `software-engineering-nextjs-routing` / `software-engineering-nextjs-cache` for the framework-side decisions that produce these functions.

## When to use this skill

- A function is slow and you suspect cold start.
- Costs are spiking on function executions.
- "Should this be Edge or Node?"
- A function hits the duration limit and gets killed.
- Memory pressure errors.
- Region selection (multi-region deploy).
- The team is asking what Fluid Compute means for their mental model.

## Core posture

- **Default to Node + Fluid Compute.** It's the new baseline; lower cold-start cost than classic serverless, full Node ecosystem.
- **Edge for tiny latency-critical paths only.** Auth check, A/B routing, redirect logic. Watch for package compat and bundle-size budgets.
- **`maxDuration` and `memory` are deliberate, not default.** Set them per route based on actual workload.
- **Regions are about user latency.** Multi-region cost-effective only when latency matters; default to a single region.
- **Cold starts are a Fluid Compute non-issue most of the time.** Don't over-engineer for them.

## Fluid Compute — the mental model

In classic serverless, every request to a function gets its own isolated instance (or reuses a warm one if available). Cold starts happen because there are no warm instances. Per-instance cost = full base cost.

Fluid Compute changes the per-instance model: a single Node Function instance can serve **many concurrent requests**. The instance stays warm longer (because it's serving more traffic), and per-invocation cost drops because the base cost amortizes across concurrency.

Implications for how you write code:

- **State doesn't isolate per request.** If your function uses module-level state, multiple in-flight requests share it. This is mostly fine (same as a long-running Node server), but can break naive caches and per-request global mutations.
- **Database connections can be reused.** Pooling becomes more useful, not less. Don't open + close on every request.
- **Cold starts hit much less often.** Don't waste effort on warm-up tricks for low-traffic projects.
- **Concurrency limits matter.** Too many concurrent requests on one instance = backpressure. Vercel auto-scales by spinning up more instances when needed.

Fluid Compute is enabled by default for new projects on supported plans. Verify in Project Settings → Functions.

## Runtime options at a glance

| Runtime | What it is | Cold start | Concurrency | Compat |
|---|---|---|---|---|
| **Node (default + Fluid)** | Full Node.js, multi-tenant Fluid instances | Low | High per-instance | Full Node ecosystem |
| **Edge** | V8 isolates near user | ~0 | Per-isolate | Subset of Node API; bundle-size budget |
| **Static** (build-time prerender) | No runtime | N/A | N/A | N/A |

For a typical Next.js 16 app on Vercel:

- **Server components and server actions**: Node.
- **`proxy.ts`** (replacement for `middleware.ts`): Node only. The Edge runtime for proxy was deprecated.
- **Route handlers**: Node by default; Edge via `export const runtime = "edge"`.
- **`opengraph-image.tsx`**: Node by default; Edge available where supported.

## Choosing Edge vs. Node

Decision tree:

```
Is this a tiny, latency-critical path? (auth check, redirect, A/B route)
├─ Yes → consider Edge
│   ├─ Does it use any non-Edge-compat package? → Node
│   ├─ Bundle size > ~1MB? → Node
│   └─ Need < 50ms TTFB at edge? → Edge
└─ No → Node (with Fluid Compute)
```

Edge is great for:

- Auth checks (verify a JWT, allow / redirect).
- A/B routing.
- Geolocation-aware redirects.
- Header rewrites.
- Lightweight personalization decisions (see `contentful-personalization`).

Edge is bad for:

- Heavy compute (LLM streaming, image processing).
- Anything that needs a Node-only library (most database SDKs, `sharp`, `puppeteer`).
- Long-running operations (Edge has its own timeout limits).

## Function configuration

`vercel.json` is the canonical place for per-function settings:

```json
{
  "functions": {
    "app/api/chat/route.ts": {
      "maxDuration": 300,
      "memory": 1024
    },
    "app/api/upload/route.ts": {
      "maxDuration": 60,
      "memory": 3008
    },
    "app/api/health/route.ts": {
      "maxDuration": 5,
      "memory": 256
    }
  }
}
```

`maxDuration`:

- Plan-dependent ceiling. Hobby ~10s, Pro ~60s default (configurable up to 300s with Fluid), Enterprise higher.
- LLM streaming routes need ~300s for long-form generations.
- Set the lowest ceiling that still completes happy-path; tighter limits surface latent bugs faster.

`memory`:

- Default 1024MB. Range typically 128MB–3008MB.
- Higher memory = proportionally more CPU.
- Image / video processing often needs 2048+; most routes are fine at 1024.

`regions`:

- Default "iad1" (Washington, DC) on most plans.
- Multi-region: configure in Project Settings or per-function. Costs more.
- Use multi-region only when sub-100ms latency to multiple geographies actually matters. Most Slalom builds stay single-region.

## Cold-start mitigation

If you're seeing consistent cold starts despite Fluid Compute:

1. **Check traffic pattern.** Long quiet periods → cold scales. Add a synthetic monitor (Checkly) to keep at least one instance warm if needed.
2. **Reduce bundle size.** Tree-shake aggressively. Big bundles take longer to start.
3. **Lazy-load heavy deps.** Don't `import OpenAI from "openai"` at module top if only one route uses it.
4. **Database connection pool at module level.** Reuse across invocations.
5. **Avoid sync I/O at startup.** Reading large files synchronously delays cold start.

Don't over-engineer this. For typical Slalom builds, Fluid Compute handles cold-start concerns natively; the above is for the 5% of routes that actually have a problem.

## Region selection

Vercel functions execute in regions you choose. Two patterns:

- **Single region (default).** All functions run in `iad1`. Lowest cost, simplest mental model.
- **Multi-region.** Functions run in multiple regions; users hit the closest. Cost multiplied; observability complexity multiplied.

Reach for multi-region only when:

- Users span continents and latency matters (real-time interaction).
- A specific region has data-residency requirements (EU customer data must execute in EU).

For most Slalom builds, single region + edge cache (CDN) handles geographic latency cheaper than multi-region functions.

## Database connections in Fluid Compute

Connection pooling is the question that shows up most. Patterns:

- **Vercel Postgres (Neon)** — uses pgbouncer. Pool is managed for you. Use the connection pool URL.
- **External Postgres** — bring your own pooler (PgBouncer, RDS Proxy). Module-level pool initialization works in Fluid since the instance reuses across requests.
- **Vercel KV** — Upstash Redis. Stateless HTTP-based; no pool needed.
- **Vercel Blob** — HTTP-based, no pool.
- **MongoDB** — driver maintains a pool; module-level client initialization works.

Pseudocode for a module-level pool:

```ts
// lib/db.ts
import { Pool } from "pg";

const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
  max: 20, // tune per concurrency expectations
});

export async function query(sql: string, params?: unknown[]) {
  return pool.query(sql, params);
}
```

Each Fluid instance reuses this pool across its concurrent requests. New instances spin up their own pool. Plan max concurrency × pool size against your DB connection limit.

## Common failure modes

- **`Function execution timeout`** — `maxDuration` exceeded. Either raise the limit (up to plan ceiling) or refactor (stream, queue, split).
- **`Function payload too large`** — function output > 4.5MB (Vercel limit). Stream instead.
- **`Module not found: sharp`** — native dep needs to be in dependencies, not devDependencies; or the binary isn't compatible with the build environment.
- **Connection pool exhaustion** — too many concurrent requests across instances overwhelming the database. Use a server-side pooler (PgBouncer) or reduce `max` on each pool.
- **Memory OOM** — function holding too much per-request state. Check heap profile; usually a leak.
- **Cold start on critical path** — instrument first; don't speculate. Most Fluid projects don't have a cold-start problem.
- **Edge runtime build error: "Module not supported"** — package uses Node APIs not in Edge. Switch to Node runtime or find an Edge-compat alternative.

## Cost shape

Function pricing has three drivers:

1. **Invocations.** Per request. Fluid amortizes the base cost; classic serverless billed every invocation.
2. **GB-seconds.** Memory × duration. Higher memory = higher per-second cost; longer duration = more cost.
3. **Bandwidth (egress).** Response size to clients.

Optimization order:

1. **Cache aggressively** (reduces invocations). See `vercel-cdn-edge`.
2. **Right-size memory** (don't run a 32MB route on 3008MB memory).
3. **Tighten `maxDuration`** (catches infinite loops; reduces tail-latency cost).
4. **Stream long responses** (releases the function back to handle the next request faster).

For LLM streaming routes specifically, `maxDuration: 300` + stream-back-immediately dramatically reduces effective cost vs. buffer-then-respond.

## Output formats

### Function configuration audit

```
# Function Audit: [Project]

## Per-route inventory
| Route | Runtime | maxDuration | memory | Notes |
|---|---|---|---|---|
| app/api/chat/route.ts | node | 300 | 1024 | LLM stream |
| app/api/health/route.ts | node | 5 | 256 | quick health check |
| proxy.ts | node (only option) | (n/a) | (n/a) | auth + A/B |

## Hot routes (top 10 by invocation)
{from observability dashboard}

## Concerns
- {long-running routes without justification}
- {oversized memory allocations}
- {Edge candidates that should move there}

## Recommendations
- {specific actions, ranked}
```

### Edge-vs-Node decision record

```
# Runtime decision: [Route]

Path: {path}
Decision: {Edge | Node}
Rationale: {one paragraph}

Constraints:
- Latency requirement: {N ms p95}
- Bundle size: {N KB}
- Package compat: {list of non-Edge deps if any}
- maxDuration needed: {N s}

Trade-offs:
- {what we gain, what we give up}
```

## How this skill relates to others

- Function config lives in `vercel.json` deployed via → `vercel-deploy-pipeline`.
- Cache decisions that affect invocation count → `vercel-cdn-edge`.
- Storage choices that affect connection patterns → `vercel-storage`.
- Function logs and duration metrics → `vercel-observability`.
- Long-running AI streaming → `vercel-ai-sdk` and `vercel-ai-gateway` (gateway request needs `maxDuration`).
- Durable / multi-step flows that don't fit a single function → `vercel-agent-runtime` (Workflows).
- Framework-side decisions producing these functions → `software-engineering-nextjs-routing`, `-nextjs-server-client`, `-nextjs-actions`.

## Source material

- Fluid Compute: https://vercel.com/fluid
- Functions overview: https://vercel.com/docs/functions
- Edge functions: https://vercel.com/docs/functions/edge-functions
- Plugin reference: `../../references/vercel-foundations.md`
