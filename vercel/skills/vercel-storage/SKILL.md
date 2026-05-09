---
name: vercel-storage
description: Pick the right Vercel-managed storage primitive for the job — Blob (object storage), KV (Upstash Redis), Postgres (Neon), Edge Config — and know when to bypass to an external service (S3, Cloudflare R2, MongoDB, PlanetScale, Supabase). Use this skill when a "where do we put this data" question lands, when integrating a storage SDK, when storage costs are getting attention, when migration off a Vercel-managed store is being discussed, or when an Edge Config use case comes up. Trigger on any "where do we store X" question on Vercel.

# Project context
type: skill
project: skills-library
plugin: vercel
aliases: [vercel-storage]
tags: [type/skill, plugin/vercel, topic/vercel, topic/storage]
status: active
---

# Vercel Storage

Owns the storage decision and integration layer. Pair with `vercel-deploy-pipeline` (env vars get wired automatically when you provision via dashboard), `vercel-fluid-compute` (connection pooling under Fluid), `vercel-observability` (storage operation metrics), and the marketplace integrations reference for non-native alternatives.

## When to use this skill

- New project deciding where data lives.
- Adding a new data type to an existing project.
- Cost spike on a storage line item.
- Connection-pool issues under Fluid Compute.
- Migration off a Vercel-managed store (or onto one).
- Edge Config feasibility check.

## Core posture

- **Vercel-native first** when there's a clean fit and no scale / cost concern.
- **Match data shape to storage model.** Don't shoehorn relational data into KV.
- **Edge Config for read-on-every-request, write-occasionally** config — feature flags, redirect tables. Not for application data.
- **Bypass to external services** when scale, cost, or capability gap is named explicitly.
- **Single source of truth.** Don't duplicate the same data across two stores unless the duplication has a named reason.

## The four primitives

| Vercel | Underlying | What it is | Read pattern | Write pattern |
|---|---|---|---|---|
| **Blob** | Vercel-managed (S3-compatible) | Object storage for files | Direct URL or SDK | SDK `put` / `del` |
| **KV** | Upstash Redis | Edge-replicated KV with Redis semantics | HTTP per-call (low ms) | HTTP per-call |
| **Postgres** | Neon | Serverless Postgres with branching | SQL via pooled or direct connection | SQL |
| **Edge Config** | Vercel | Read-only-fast config | Sub-millisecond at edge, sync read | Slow write via API |

## Vercel Blob — when and how

Use for:

- User uploads (avatars, documents, media).
- CMS-uploaded assets.
- Generated artifacts (server-rendered PDFs, OG images).
- Anything you'd otherwise put on S3.

```ts
// app/api/upload/route.ts
import { put, del, list } from "@vercel/blob";

export async function POST(req: Request) {
  const file = await req.blob();
  const blob = await put("avatars/user-123.png", file, {
    access: "public", // or "private" with signed URLs
    contentType: "image/png",
    addRandomSuffix: true, // avoid collisions
  });
  return Response.json({ url: blob.url });
}
```

Notes:

- Public blobs are CDN-cached and served from `*.public.blob.vercel-storage.com`. Pair with `next/image` for optimization.
- Private blobs return signed URLs via SDK; better for downloads with access control.
- Pricing: bandwidth + storage GB-months + operations. Roughly comparable to S3.
- File size limit and request rate limits apply (check current docs).

When to skip Blob:

- Petabyte-scale archival → S3 / R2 directly.
- Files you need to mutate in-place (not a Blob model — write a new key, delete the old).
- Files needing fine-grained ACLs at scale.

## Vercel KV (Upstash Redis) — when and how

Use for:

- Session storage.
- Rate limit counters.
- Feature flags (lightweight).
- Hot-key caches (e.g., the result of an expensive computation).
- Pub/sub light usage.

```ts
import { kv } from "@vercel/kv";

await kv.set("user:123:session", session, { ex: 3600 });
const sess = await kv.get<Session>("user:123:session");

// Counter for rate limits
const count = await kv.incr(`rl:${ip}:${minute}`);
await kv.expire(`rl:${ip}:${minute}`, 60);
```

Notes:

- Edge-replicated; read latency low globally.
- Standard Redis commands available; Upstash has a docs page for compat.
- Stateless HTTP-based; no connection pooling concern under Fluid.
- Pricing: per-command (with a free tier).

When to skip KV:

- Transactional, multi-key consistency → Postgres.
- Complex queries → Postgres.
- Large blobs → Blob.

## Vercel Postgres (Neon) — when and how

Use for:

- Standard application data.
- Anything with relational integrity / transactional writes.
- Anything you'd put in RDS / Aurora at smaller scale.

```ts
import { sql } from "@vercel/postgres";

const { rows } = await sql<Article>`
  SELECT id, title, slug
  FROM articles
  WHERE slug = ${slug}
  LIMIT 1
`;
```

Notes:

- Branchable database — preview environments can spin up DB branches matching git branches. Powerful for safer schema migrations.
- Use the **pooled connection string** (`POSTGRES_URL`) for serverless functions; the **direct connection string** (`POSTGRES_URL_NON_POOLING`) for migrations or long-lived connections.
- Drizzle ORM and Prisma both work; both have Vercel-specific docs.
- Pricing: compute hours + storage. Free tier covers small workloads.

Connection patterns under Fluid Compute:

- The `@vercel/postgres` SDK manages pooling for you.
- For a custom client (`pg`), initialize at module level so the connection persists across Fluid invocations:

```ts
import { Pool } from "pg";
const pool = new Pool({
  connectionString: process.env.POSTGRES_URL,
  max: 20,
});
export { pool };
```

When to skip Vercel Postgres:

- Strict data residency outside Neon's regions.
- MySQL requirement → PlanetScale or RDS.
- Truly massive scale → AWS Aurora or self-managed.
- Need for MongoDB-style flexibility → MongoDB Atlas.

## Edge Config — when and how

Use for **read on every request, write rarely**:

- Feature flags.
- A/B test variant tables.
- Redirect tables.
- Maintenance-mode flag.
- Configuration the edge needs to make routing decisions.

```ts
import { get, getAll } from "@vercel/edge-config";

const featureFlag = await get<boolean>("dark-mode-enabled");
const allFlags = await getAll();
```

Reads are sub-millisecond at the edge — they're cached in the Edge runtime and don't make a network round-trip on each call. Writes happen via the dashboard or REST API and propagate within seconds.

Don't use Edge Config for:

- Per-user data (it's global, not per-user).
- Frequently-written data (writes are slow).
- Large datasets (size limits apply).

Pair with `vercel-security` for WAF-driven feature flag patterns.

## Choosing — decision matrix

| Question | Answer |
|---|---|
| User-uploaded file? | **Blob** |
| Server-generated artifact (PDF, OG image)? | **Blob** |
| Session, rate limit, or hot cache? | **KV** |
| Relational app data with transactions? | **Postgres** |
| Feature flag or redirect table read on every request? | **Edge Config** |
| Vector embeddings for RAG? | External (Pinecone, Weaviate, pgvector on Postgres) |
| Time-series metrics? | External (TimescaleDB on Postgres, Influx, Datadog) |
| Document-store / flexible schema? | External (MongoDB Atlas, Firebase) |

## When to bypass to external services

Vercel-native isn't always the answer. Reach for external when:

- **Data residency.** Vercel-managed locations may not satisfy regulatory requirements.
- **Existing client infrastructure.** Client has Snowflake, MongoDB, Postgres-on-RDS — match it.
- **Cost at scale.** S3 + CloudFront is cheaper than Blob at 100s of TB.
- **Specific capabilities.** PlanetScale's branching, Supabase's auth+realtime, Cloudflare R2's egress-free model.
- **Cross-cloud.** Workloads that need to sit alongside other AWS / GCP services.

The marketplace integrations reference (`../../references/marketplace-integrations.md`) lists the canonical alternatives.

## Connection pooling under Fluid Compute

Fluid Compute reuses Function instances across requests. That makes module-level connection pools genuinely useful (unlike classic serverless where they're often per-instance and short-lived).

Patterns:

- **Vercel-managed (Postgres / KV / Blob)**: SDKs handle pooling internally.
- **External Postgres**: module-level `Pool` initialization. Tune `max` against your DB connection limit and expected concurrency.
- **External MongoDB**: module-level `MongoClient` reuse.
- **HTTP-based stores (Upstash, Supabase REST)**: stateless, no pool needed.

Watch for connection-limit exhaustion. If you have many Fluid instances each with a 20-connection pool against a 100-connection-limit DB, you're 5 instances away from exhaustion. Use a server-side pooler (PgBouncer, Supabase pooler) or reduce per-instance pool size.

## Cost shape

Costs vary by primitive:

- **Blob**: storage GB-months + bandwidth + operations.
- **KV**: per-command (commands per month).
- **Postgres**: compute hours (idle vs. active) + storage + data transfer.
- **Edge Config**: included generously on most plans; pricing changes if you exceed limits.

Cost optimization:

- **Cache reads.** A Postgres query that's hammered should land in KV or Edge Config.
- **Right-size Postgres compute.** Set autoscaling limits.
- **Audit Blob retention.** Delete what you don't need; storage compounds.
- **Don't write to Edge Config from app code.** Writes propagate slowly and the API is rate-limited.

## Migration patterns

Adding a Vercel-native store:

1. Provision in dashboard.
2. Vercel auto-creates env vars on the project.
3. Pull to local: `vercel env pull`.
4. Wire the SDK.

Migrating off:

1. Stand up the new store in parallel (dual-write or shadow-read).
2. Backfill historical data.
3. Switch reads.
4. Switch writes.
5. Decommission the old store after a quiet period.

Don't do hard cutovers in production. The dual-write window is what gives you confidence.

## Common failure modes

- **Pool exhaustion.** Too many Fluid instances × per-instance pool size. Use a pooler.
- **Blob URLs without optimization.** Public blob served raw, eating bandwidth. Wrap in `next/image`.
- **KV TTL forgotten.** Sessions never expire; data accumulates. Always set `ex`.
- **Postgres connection from Edge runtime.** Edge can't make TCP connections in many cases. Use HTTP-based clients (Neon serverless driver) or stay in Node.
- **Edge Config used for per-user data.** Shared global; writes are slow. Wrong primitive.
- **Vercel-managed store + the underlying provider's dashboard out of sync.** Provision through Vercel; manage primary state through Vercel; don't dual-control via Upstash / Neon dashboards.

## Output formats

### Storage architecture doc

```
# Storage Architecture: [Project]

## Data inventory
| Concern | Store | Reason |
|---|---|---|
| User avatars | Vercel Blob (public) | CDN-cached, simple |
| User documents | Vercel Blob (private + signed URLs) | Access controlled |
| Sessions | Vercel KV | Hot reads, TTL-friendly |
| Rate limits | Vercel KV | Atomic INCR, TTL |
| Application data | Vercel Postgres | Relational, transactional |
| Feature flags | Vercel Edge Config | Read on every request |
| Vector embeddings | Pinecone | External; specialty store |

## Schema
{Postgres schema overview, KV key conventions, Edge Config keys}

## Connection patterns
{Pool configuration, connection-limit budgeting}

## Backups
{Postgres branching for previews, Blob retention policy, KV — none, Edge Config — version history}

## Costs
{Per-store budget, alerts}

## Migration plan (if any)
{Source → target, dual-write window, cutover}
```

## How this skill relates to others

- Storage bindings configured at deploy time → `vercel-deploy-pipeline`.
- Connection pooling under Fluid concurrency → `vercel-fluid-compute`.
- Storage operation metrics → `vercel-observability`.
- External alternatives → `../../references/marketplace-integrations.md`.
- Storage-driven cache strategy → `vercel-cdn-edge`.
- AI / RAG vector stores → `vercel-ai-sdk` (paired with external vector DB).
- Application-side data architecture → `software-engineering-data-engineer`.

## Source material

- Storage overview: https://vercel.com/docs/storage
- Blob: https://vercel.com/docs/storage/vercel-blob
- KV: https://vercel.com/docs/storage/vercel-kv
- Postgres: https://vercel.com/docs/storage/vercel-postgres
- Edge Config: https://vercel.com/docs/edge-config
- Plugin reference: `../../references/vercel-foundations.md`
- Plugin reference: `../../references/marketplace-integrations.md`
