---
name: algolia-indexing-pipeline
description: >
  Getting data into Algolia — full reindex vs. partial updates, atomic replacement via
  `replaceAllObjects`, batch sizing, idempotency, streaming pipelines (webhook-driven
  incremental sync), backfills, the Ingestion API for managed source connectors, and the Slalom
  default pattern of Vercel Functions + Contentful webhooks for the Composable DXP stack. Use
  this skill any time records need to flow from a source system into Algolia — initial seeding,
  scheduled refresh, on-publish webhook sync, scripted backfill, or migration from another
  search engine.

# Project context
type: skill
project: skills-library
plugin: algolia
aliases: [algolia-indexing-pipeline]
tags: [type/skill, plugin/algolia, topic/algolia, topic/indexing]
status: active
---

This skill puts you in the role of an engineer designing the path from source-of-truth to Algolia index. Default posture: **the indexing pipeline is a real system — idempotent, observable, recoverable, with scheduled and event-driven paths.** Treat it accordingly.

Pair with `algolia-index-design` (the records this pipeline produces), `algolia-contentful-integration` (when Contentful is the source — by far our most common path), `algolia-api-keys-security` (the keys the pipeline uses), and the Ingestion API surface in `../../references/api-surface.md` (when a managed connector beats a hand-rolled pipeline).

## When to use this skill

- Initial seed of a new Algolia index from a content source.
- Scheduled (nightly / hourly) refresh of an index from a source that doesn't push events.
- Webhook-driven incremental sync — entry published in source → record updated in Algolia.
- Bulk backfill after a schema change.
- Migrating from another search engine.
- Re-indexing after the records change shape (`algolia-index-design` → new mapping → reindex).
- Multi-tenant indexing where each tenant's records need independent ops.

## Core posture

- **Decide build vs. install first.** Ingestion API source connectors and Marketplace apps cover most cases. Build only when they don't fit.
- **Idempotent objectIDs.** A reindex must be a no-op when nothing changed.
- **Atomic full reindex.** Use `replaceAllObjects` — never `clearIndex` followed by `saveObjects` (window of empty index = bad).
- **Batch.** 1000 records per call, ≤10 MB per call. The SDK helpers handle chunking; trust them.
- **Decoupled pipeline.** The webhook handler should enqueue work, not do work synchronously. Algolia is fast, but a pipeline that retries on its own retries is dangerous.
- **Observable.** Log job IDs, record counts, durations. Surface to the engagement's metrics. A silent pipeline is a broken pipeline.

## Build vs. install

Decision tree:

```
Source has an Algolia connector / app?
├── Yes — does the mapping fit out of the box?
│   ├── Yes → Use it. (Contentful Marketplace app, Shopify connector, etc.)
│   └── No, but the gap is small → Use it + a small post-transform layer
│   └── No, the gap is large → Build custom
└── No → Build custom (or use the Ingestion API with custom source)
```

In Slalom Composable DXP work, the **Contentful Marketplace Algolia app** is the default first stop. It indexes one Contentful entry → one Algolia record per locale. Custom pipelines come in when:

- Records denormalize linked references heavily (assemblies → flattened topics).
- Locale fan-out is non-trivial (fallback chains, partial localization).
- Records derive computed fields (popularity from Insights, author affiliation lookups).
- Multi-source records (a "search hit" combines content from Contentful + product data + an internal API).

For non-Contentful sources, check `../../references/integrations-map.md` first.

## Pipeline shapes — three tiers

### Tier 1 — Ingestion API + managed source connector

Best for: Contentful, Shopify, BigQuery, Snowflake, Postgres, S3, etc. Configured in the Algolia Dashboard.

- **Source** — Algolia pulls from the source on schedule + webhook.
- **Transformation** — JS function inside Algolia maps source records to Algolia records.
- **Destination** — Algolia index.
- **Task** — schedule (cron) + webhook trigger.

Operationally cheap. Limits:

- The transformation runs in Algolia's sandbox; no external API calls inside it.
- Cross-record lookups (e.g., resolving a reference to fetch its title) need to happen *before* the source delivers the data, or in a custom pipeline.
- The mapping logic is harder to version-control (lives in the Dashboard or via the Ingestion API).

When this fits, ship it. Don't write Node.

### Tier 2 — Vercel Function indexer + source webhook (the Slalom default)

Best for: Contentful with non-trivial denormalization. Our default pattern for Composable DXP engagements.

```
Contentful entry published
  │
  └─→ Webhook → POST /api/algolia/sync
                  │
                  ├─ Fetch full entry + linked references (CDA / GraphQL)
                  ├─ Map to Algolia record(s) — one per locale
                  ├─ partialUpdateObject / saveObjects on the index
                  └─ Log to Vercel logs / observability
```

Code shape:

```ts
// app/api/algolia/sync/route.ts
import { NextResponse } from 'next/server';
import { algoliasearch } from 'algoliasearch';
import { getEntry } from '@/lib/contentful/server';
import { mapToAlgolia } from '@/lib/algolia/mappers';
import { verifyContentfulSignature } from '@/lib/contentful/webhook';

const algolia = algoliasearch(process.env.ALGOLIA_APP_ID!, process.env.ALGOLIA_ADMIN_KEY!);

export async function POST(req: Request) {
  const raw = await req.text();
  if (!verifyContentfulSignature(req, raw)) {
    return NextResponse.json({ ok: false }, { status: 401 });
  }

  const event = JSON.parse(raw);
  const { sys, fields } = event;

  // Refetch with linked references resolved (the webhook payload is shallow)
  const entry = await getEntry(sys.id, sys.locale ?? 'en-US', { include: 4 });

  const records = mapToAlgolia(entry); // one record per locale, possibly multiple per entry
  const indexName = `${entry.sys.contentType.sys.id}s`; // articles, glossaryTerms, etc.

  if (event.type === 'ContentManagement.Entry.unpublish' || event.type === 'ContentManagement.Entry.delete') {
    await algolia.deleteObjects({
      indexName,
      objectIDs: records.map((r) => r.objectID),
    });
  } else {
    await algolia.saveObjects({ indexName, objects: records });
  }

  return NextResponse.json({ ok: true, count: records.length });
}
```

Pair with:

- A **scheduled full reindex** (cron via Vercel Cron or Inngest) that runs nightly to repair drift.
- A **manual reindex endpoint** (admin-only, behind auth) for one-off resets.
- **Idempotent objectIDs** — `${entry.sys.id}-${locale}`.
- **Webhook signature verification** (HMAC) — don't trust unsigned posts.
- **Async work for large entries** — push to a queue (Inngest, Trigger.dev, SQS) when fan-out is heavy.

### Tier 3 — full custom pipeline (queues, workers, ETL)

Best for: very high volume (>1M records, >100 changes/sec), complex multi-source records, or strict SLAs.

Pattern: source → event bus → workers → Algolia. Use this when Tier 2 starts queuing on every spike. Most Slalom engagements never need it.

## Atomic full reindex

`replaceAllObjects` writes to a temporary index, swaps. No empty-index window:

```ts
import { algoliasearch } from 'algoliasearch';

const algolia = algoliasearch(APP_ID, ADMIN_KEY);

async function fullReindex() {
  const allRecords = await fetchAllFromSource(); // generator preferred for memory
  await algolia.replaceAllObjects({
    indexName: 'articles',
    objects: allRecords,
    batchSize: 1000,
  });
}
```

Use for:
- Initial seed.
- Schema migrations (record shape changed, every record needs reissue).
- Recovery from drift.

Don't use for incremental updates (it overwrites everything).

## Partial updates

For changing one or two attributes without sending the whole record:

```ts
await algolia.partialUpdateObjects({
  indexName: 'articles',
  objects: [
    { objectID: 'a1', popularity: 142 },
    { objectID: 'a2', popularity: 89 },
  ],
  createIfNotExists: false,
});
```

Use for:
- Updating computed fields (popularity, freshness scores) from analytics jobs.
- Inventory updates in commerce.
- Anything where the source still owns most of the record but one attribute moves frequently.

## Batching and rate limits

- **Batch size**: 1000 records or 10 MB per call. SDK helpers (`saveObjects`, `partialUpdateObjects`) auto-chunk.
- **Indexing rate limit**: per-app, per-key, plan-dependent. Typical: tens of batches per second.
- **Spike handling**: queue large fan-outs (Inngest / SQS); don't fire 10k requests in parallel. Algolia will rate-limit and you'll get partial state.

## Idempotency

Always supply `objectID`. Source it from a stable identifier:

| Source | objectID pattern |
|---|---|
| Contentful entry | `${sys.id}-${locale}` |
| Shopify product variant | `${product.id}-${variant.id}` |
| BigQuery row | the source primary key, prefixed by table |
| Custom system | UUID + locale |

Never let Algolia generate the ID (it will, if you don't supply one — but then reindexes create duplicates).

## Backfill from a source

```ts
// scripts/backfill.ts
import { algoliasearch } from 'algoliasearch';
import { getCdaClient } from '@/lib/contentful/server';

const algolia = algoliasearch(APP_ID, ADMIN_KEY);
const cda = getCdaClient();

async function backfill() {
  const tempIndex = `articles_backfill_${Date.now()}`;
  let skip = 0;
  const limit = 100;
  let total = Infinity;

  while (skip < total) {
    const page = await cda.getEntries({
      content_type: 'article',
      skip,
      limit,
      include: 4,
    });
    total = page.total;

    const records = page.items.flatMap(mapToAlgolia);
    await algolia.saveObjects({ indexName: tempIndex, objects: records });

    skip += limit;
    console.log(`Indexed ${skip}/${total}`);
  }

  // Atomic swap
  await algolia.operationIndex({
    indexName: tempIndex,
    operationIndexParams: { operation: 'move', destination: 'articles' },
  });

  console.log('Done.');
}

backfill().catch(console.error);
```

Run from a developer machine or a scheduled job. Use a dedicated key with the minimum ACLs (`addObject`, `editSettings` for settings deploy, `listIndexes`, `settings`).

## Webhook signature verification

For Contentful webhooks, verify the HMAC signature:

```ts
import crypto from 'node:crypto';

export function verifyContentfulSignature(req: Request, body: string): boolean {
  const secret = process.env.CONTENTFUL_WEBHOOK_SECRET!;
  const provided = req.headers.get('x-contentful-signature');
  if (!provided) return false;
  const expected = crypto.createHmac('sha256', secret).update(body).digest('hex');
  return crypto.timingSafeEqual(Buffer.from(provided), Buffer.from(expected));
}
```

Reject unsigned or mismatched. The endpoint is public; treat anything unsigned as suspect.

## Observability

What to log on every run:

- Source identifier (entry ID, batch ID).
- Index name + operation (save / delete / partial).
- Record count.
- Duration.
- Algolia task ID (returned from save/delete) — pair with `wait()` only when downstream needs strong consistency.

Push to the engagement's observability stack — Datadog, New Relic, Logtail, or just Vercel logs aggregated via a log drain.

For scheduled jobs, fail loudly: a missed nightly reindex means drift from the source, which compounds. PagerDuty / Slack alerts on cron failure.

## Multi-tenant patterns

Tenant filtering happens at query time via secured API keys (see `algolia-api-keys-security`). Indexing strategy:

- **Single index, `tenantId` attribute on every record**. Cheapest. Default unless tenant counts are huge (>1000) or per-tenant settings differ.
- **Per-tenant index**. Use when settings, synonyms, or ranking differ per tenant. Comes with operational overhead (per-tenant settings deploys).

For tenant offboarding: `deleteObjects` filtered by `tenantId`. Periodic audit to ensure nothing leaks.

## Migrating from another search engine

Sequence:

1. **Design the records** (`algolia-index-design`).
2. **Backfill into a non-prod Algolia app** from the source-of-truth, not from the legacy search index. The legacy index is already a projection; double-projecting compounds quality issues.
3. **Tune relevance** (`algolia-relevance-tuning`) against representative queries.
4. **A/B against the legacy search** behind a feature flag.
5. **Cut over** when CTR / conversion meet the threshold.
6. **Decommission** legacy search.

Don't try to port the legacy search's tuning artifacts wholesale. Port the synonyms and the merchandising rules; let everything else come from a fresh tune.

## Settings, rules, synonyms — co-deploy with records

Treat them as code:

```
algolia/
├── articles.settings.json
├── articles.rules.json
├── articles.synonyms.json
├── glossary_terms.settings.json
└── ...
```

CI deploys settings on merge to main. Records flow continuously. The two are decoupled but co-versioned.

## Output formats

### Pipeline design proposal

```
# Algolia Indexing Pipeline: [Project]

## Source(s)
- System(s) of record
- Volume (records, change frequency)
- How changes propagate (webhook, polling, manual)

## Tier
- Ingestion API connector | Vercel Function + webhook | Custom queue+worker

## Mapping
- Reference to `algolia-index-design` doc
- Per-content-type mapping module path

## Triggers
- Webhook events handled
- Scheduled reindex cadence
- Manual reindex endpoint (auth)

## Idempotency
- objectID derivation
- Atomic operations used (replaceAllObjects vs partialUpdate)

## Observability
- Logging + alerting
- Health metric (records-per-day, error rate, lag from source)

## Failure modes
- What happens if Algolia is down
- What happens if the source webhook is missed
- Recovery procedure (manual reindex)

## Security
- Webhook signature verification
- Algolia key ACLs and rotation cadence

## Open questions
```

### Pipeline run log entry (for ops debugging)

```
[2026-05-09T14:02:11Z] sync start
  source: contentful entry sys.id=2x4abc, locale=en-US
  trigger: webhook ContentManagement.Entry.publish
  index: articles
  records: 1
  taskId: 17234
  duration: 312ms
  status: ok
```

## Common anti-patterns

- **Building an indexer when the Marketplace app or Ingestion connector fits.** Wasted effort.
- **`clearIndex` + `saveObjects` for a full reindex.** Empty-index window. Use `replaceAllObjects`.
- **No `objectID`.** Reindexes create duplicates.
- **Webhook handler that does the indexing synchronously and slowly.** Source webhooks have retry budgets; long handlers cause repeats.
- **No webhook signature verification.** Public endpoint == public mutation.
- **One huge `saveObjects` call.** SDK auto-chunks, but if you bypass the helper, you'll hit the 10 MB limit.
- **Using the admin key in a client-bundled module.** Server-only.
- **No scheduled reindex.** Drift compounds; you find out months later that 8% of the index is stale.
- **Indexing draft / preview content into prod.** Use a separate `*_preview` index.

## Constraints

- Don't ship a pipeline without observability.
- Don't put the admin key in a Marketplace app or ingest connector if a scoped key would do.
- Don't run untested mappings against prod. Always non-prod first.
- Don't couple the pipeline's deploy to settings deploy. Records and settings change at different cadences.

## How this skill relates to others

- The records the pipeline produces → `algolia-index-design`.
- The Contentful-specific mapping → `algolia-contentful-integration`.
- The keys the pipeline uses → `algolia-api-keys-security`.
- The dashboard / API surface for operations → `algolia-mcp-cli`, `../../references/cli-cheat-sheet.md`.
- The settings / rules / synonyms deploy that pairs with records → `algolia-relevance-tuning`.

## Source material

- Indexing overview: https://www.algolia.com/doc/guides/sending-and-managing-data/send-and-update-your-data/
- Atomic reindex: https://www.algolia.com/doc/guides/sending-and-managing-data/send-and-update-your-data/how-to/safely-update-the-index/
- Partial updates: https://www.algolia.com/doc/api-reference/api-methods/partial-update-objects/
- Ingestion API: https://www.algolia.com/doc/guides/sending-and-managing-data/send-and-update-your-data/connectors/
- Plugin reference: `../../references/api-surface.md`
- Plugin reference: `../../references/integrations-map.md`