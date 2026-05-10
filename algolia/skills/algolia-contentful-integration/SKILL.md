---
name: algolia-contentful-integration
description: >
  The Contentful ↔ Algolia integration we ship by default on Composable DXP engagements —
  choosing between the Marketplace app, the Ingestion API source connector, and a custom
  Vercel-Function indexer; mapping Topics & Assemblies to Algolia records; per-locale fan-out;
  preview vs. delivery indices; webhook signature verification; on-publish revalidation;
  backfills; the moments where the Marketplace app is enough and the moments where it's not.
  Use this skill any time a Composable DXP engagement needs Contentful as the source of truth
  for an Algolia index — first integration, new content type, locale rollout, or migration from
  a hand-rolled indexer to a managed connector (or vice versa).

# Project context
type: skill
project: skills-library
plugin: algolia
aliases: [algolia-contentful-integration]
tags: [type/skill, plugin/algolia, plugin/contentful, topic/algolia, topic/contentful, topic/integration]
status: active
---

This skill puts you in the role of an engineer who has shipped this integration enough times to know the forks. Default posture: **start with the Contentful Marketplace Algolia app for simple cases; graduate to a custom Vercel-Function indexer for anything that needs to denormalize Topics & Assemblies, fan out locales non-trivially, or compute fields from outside Contentful.**

Pair with `contentful-content-model` (the source structure), `contentful-webhooks` (the trigger surface), `contentful-graphql` (the query layer for non-trivial mappings), `algolia-index-design` (the destination structure), `algolia-indexing-pipeline` (the operational scaffolding), and `algolia-api-keys-security` (the keys the integration uses).

## When to use this skill

- New Composable DXP engagement standing up search for the first time.
- Adding a new content type that needs to be searchable.
- Adding a new locale or restructuring the locale strategy.
- Editorial team complains: "I published an article and it's not in search yet."
- Migrating from the Marketplace app to a custom indexer (or back).
- Designing the preview vs. delivery split.
- Backfilling a new index from the Contentful space.

## Core posture

- **One Contentful Topic type → one Algolia index** (per environment). Articles → `articles`, Glossary Terms → `glossary_terms`. Assemblies (Pages, PageSections) usually don't get indexed directly — they're navigation, not search results.
- **Locale fan-out: one record per (entry, locale).** `objectID = ${sys.id}-${locale}`.
- **Preview content lives in a separate `*_preview` index** in a non-prod app.
- **Marketplace app for simple, custom indexer for everything else.** The fork is real; pick deliberately.
- **Verify webhook signatures.** Don't trust unsigned posts.
- **Always have a backfill path.** Webhooks miss; reconciliation matters.

## The decision: Marketplace app vs. custom indexer

```
Does the mapping fit "1 entry → 1 record per locale, with simple field mapping"?
├── Yes
│   ├── Locales: ≤ 5 with consistent field-level localization → Marketplace app
│   ├── Locales: many or with fallback chains → Custom indexer
│   └── Computed fields needed (popularity, denormalized author) → Custom indexer
└── No (denormalizes references, flattens assemblies, multi-source records, etc.)
    └── Custom indexer
```

### When the Marketplace app fits

- Small to mid-size content models (≤ 10 indexed types).
- Each Algolia record is a 1-to-1 projection of a Contentful entry.
- Locale set is fixed and consistent.
- No need to compute fields from outside Contentful.
- The team is small and operational simplicity wins.

The Marketplace app:
- Configured in the Contentful UI under Apps.
- Lets you map Contentful fields → Algolia attributes.
- Fires on Entry Published / Unpublished / Deleted.
- Handles batch backfill via "Index All."
- No code; no deploy; no Vercel function to maintain.

Limits:
- Mapping logic is field-to-field; no transforms, no computed fields, no cross-entry joins.
- Reference resolution: limited (it'll inline the linked entry's `sys.id` and a few top-level fields, but not deep traversal).
- Locale handling assumes one record per (entry, locale) with the Contentful field-level localization.
- One app instance per Contentful environment.

### When the custom indexer wins

- Records denormalize linked entries (e.g., the Algolia article record needs the author's `name` and `role` flattened in, not just the `author_id`).
- Assembly flattening (e.g., Page → flatten section components into a single searchable `body` field).
- Computed fields from outside Contentful (popularity from analytics, search-popularity score, etc.).
- Locale fallback chains (German falls back to English with a marker).
- Multi-source records (the search hit blends Contentful content with product data from another system).
- Strict observability requirements that the Marketplace app's logs don't satisfy.

The custom indexer:
- Code lives in the engagement's repo (typically `app/api/algolia/sync/route.ts`).
- Fires on Contentful webhook (Entry Published / Unpublished / Deleted, optionally Entry Saved for drafts).
- Optional scheduled full reindex (Vercel Cron, Inngest) to repair drift.
- Uses the GraphQL Content API or the CDA to fetch the full entry with linked references resolved.
- Writes to Algolia via `partialUpdateObjects` or `saveObjects`.
- Source-controlled, code-reviewed, observable.

## The custom-indexer pattern (Slalom default)

### Architecture

```
Contentful (source of truth)
    │
    │ Webhook on Entry Published / Unpublished / Deleted
    ▼
Vercel Function (POST /api/algolia/sync)
    │
    ├─ Verify webhook signature (HMAC)
    ├─ Fetch full entry via GraphQL (linked refs resolved)
    ├─ Map to Algolia record(s) — one per locale
    └─ saveObjects / deleteObjects on the right index
            │
            ▼
        Algolia index (e.g., articles)

[Scheduled cron: nightly reindex from Contentful for drift repair]
```

### Webhook configuration in Contentful

In Contentful → Settings → Webhooks:

- URL: `https://{site}/api/algolia/sync`
- Topics: `Entry.publish`, `Entry.unpublish`, `Entry.delete`
- Optional: `Entry.archive`, `Entry.unarchive`
- Filters: by content type (only fire for indexed types).
- Headers: `X-Contentful-Webhook-Source: cms` (custom header to disambiguate from other webhooks).
- Secret: `CONTENTFUL_WEBHOOK_SECRET` (used for HMAC verification).

### Mapping module

```ts
// lib/algolia/mappers/article.ts
import type { Entry } from 'contentful';

export function mapArticleToAlgolia(entry: Entry, locale: string) {
  const fields = entry.fields as ArticleFields;
  const author = fields.author?.[locale]?.fields ?? {};

  return {
    objectID: `${entry.sys.id}-${locale}`,
    type: 'article',

    title: fields.title?.[locale] ?? '',
    summary: fields.summary?.[locale] ?? '',
    body_plaintext: stripHtml(fields.body?.[locale] ?? ''),

    topics: fields.topics?.[locale] ?? [],
    audience: fields.audience?.[locale] ?? [],
    author_id: entry.fields.author?.[locale]?.sys?.id ?? null,
    author_name: author.name ?? null,
    publishedAt_unix: fields.publishedAt?.[locale]
      ? Math.floor(new Date(fields.publishedAt[locale]).getTime() / 1000)
      : null,
    locale,

    slug: fields.slug?.[locale] ?? '',
    url: `/articles/${fields.slug?.[locale]}`,
    image: fields.heroImage?.[locale]?.fields?.file?.url ?? null,

    popularity: 0, // backfilled by analytics job
  };
}

function stripHtml(input: string) {
  return input.replace(/<[^>]+>/g, ' ').replace(/\s+/g, ' ').trim();
}
```

For Rich Text fields, use `@contentful/rich-text-html-renderer` to render to HTML, then strip. Or use `@contentful/rich-text-plain-text-renderer` directly.

### Webhook handler

```ts
// app/api/algolia/sync/route.ts
import { NextResponse } from 'next/server';
import crypto from 'node:crypto';
import { algoliasearch } from 'algoliasearch';
import { getEntryWithRefs } from '@/lib/contentful/server';
import { mapArticleToAlgolia } from '@/lib/algolia/mappers/article';
import { mapGlossaryTermToAlgolia } from '@/lib/algolia/mappers/glossary-term';

const algolia = algoliasearch(process.env.ALGOLIA_APP_ID!, process.env.ALGOLIA_INDEXER_KEY!);

const indexedTypes = {
  article: { index: 'articles', map: mapArticleToAlgolia },
  glossaryTerm: { index: 'glossary_terms', map: mapGlossaryTermToAlgolia },
} as const;

export async function POST(req: Request) {
  const raw = await req.text();
  if (!verifySignature(req, raw)) {
    return NextResponse.json({ ok: false, error: 'invalid signature' }, { status: 401 });
  }

  const event = JSON.parse(raw);
  const topic = req.headers.get('x-contentful-topic') ?? '';
  const contentTypeId = event.sys?.contentType?.sys?.id;

  const config = indexedTypes[contentTypeId as keyof typeof indexedTypes];
  if (!config) {
    return NextResponse.json({ ok: true, skipped: true, reason: 'not indexed' });
  }

  const isDelete = topic.endsWith('Entry.unpublish') || topic.endsWith('Entry.delete');
  const locales = await getActiveLocales(); // ['en-US', 'fr-FR', ...]

  if (isDelete) {
    const objectIDs = locales.map((l) => `${event.sys.id}-${l}`);
    await algolia.deleteObjects({ indexName: config.index, objectIDs });
    return NextResponse.json({ ok: true, deleted: objectIDs.length });
  }

  // Fetch full entry with linked references resolved
  const entry = await getEntryWithRefs(event.sys.id, { include: 4 });
  const records = locales.map((l) => config.map(entry, l)).filter(Boolean);
  await algolia.saveObjects({ indexName: config.index, objects: records });

  return NextResponse.json({ ok: true, count: records.length });
}

function verifySignature(req: Request, body: string): boolean {
  const secret = process.env.CONTENTFUL_WEBHOOK_SECRET;
  if (!secret) return false;
  const provided = req.headers.get('x-contentful-signature');
  if (!provided) return false;
  const expected = crypto.createHmac('sha256', secret).update(body).digest('hex');
  return crypto.timingSafeEqual(
    Buffer.from(provided),
    Buffer.from(expected.length === provided.length ? expected : '')
  );
}

async function getActiveLocales() {
  // Cache in a module-level map for the function's warm life
  return ['en-US', 'fr-FR'];
}
```

### Backfill / drift repair

```ts
// scripts/backfill.ts — run via `pnpm tsx scripts/backfill.ts`
import { algoliasearch } from 'algoliasearch';
import { getCdaClient } from '@/lib/contentful/server';
import { mapArticleToAlgolia } from '@/lib/algolia/mappers/article';

const algolia = algoliasearch(APP_ID, ADMIN_KEY);
const cda = getCdaClient();
const locales = ['en-US', 'fr-FR'];

async function backfillArticles() {
  let skip = 0;
  const limit = 100;
  let total = Infinity;
  const allRecords: any[] = [];

  while (skip < total) {
    const page = await cda.getEntries({
      content_type: 'article',
      skip,
      limit,
      include: 4,
      locale: '*', // all locales in one fetch
    });
    total = page.total;

    for (const entry of page.items) {
      for (const locale of locales) {
        allRecords.push(mapArticleToAlgolia(entry, locale));
      }
    }
    skip += limit;
  }

  await algolia.replaceAllObjects({
    indexName: 'articles',
    objects: allRecords,
    batchSize: 1000,
  });
}

backfillArticles().catch(console.error);
```

Run via:
- Manual one-off (developer machine).
- Vercel Cron (nightly).
- A protected admin endpoint (`POST /api/algolia/backfill?type=article`).

## Preview vs. delivery indices

Editorial workflows need draft content visible in preview environments. Pattern:

- **Production indices** (`articles`, `glossary_terms`) — populated from CDA (published only).
- **Preview indices** (`articles_preview`, `glossary_terms_preview`) — populated from Preview API (drafts + published), in a separate Algolia application (e.g., `slalom-{client}-staging`).
- **Preview environment** routes the front-end to the preview indices via env-aware client config.
- **Webhooks** fire to the preview indexer on `Entry.save` (drafts), to the production indexer on `Entry.publish`.

Config:

```ts
// lib/algolia/index-name.ts
export function getIndexName(type: string) {
  return process.env.NEXT_PUBLIC_CONTENTFUL_PREVIEW_MODE === 'true'
    ? `${type}s_preview`
    : `${type}s`;
}
```

Don't mix preview and production records in one index. Preview content leaks into search, and editors panic.

## Locale strategy

For Contentful spaces with field-level localization (the typical case):

- One Algolia record per (entry, locale).
- `objectID = ${sys.id}-${locale}`.
- Each record carries a `locale` attribute.
- Front-end queries always filter `locale:{currentLocale}`.

For locales with fallback chains (German falls back to English):

- The mapper checks each field for the requested locale, falls back to the default locale if absent.
- Add a `_locale_actual` attribute showing where the content actually came from (for debugging).
- Optionally add a `_localized` boolean (true if all fields are in `locale`; false if some fell back).

For very large locale sets (>10), consider one index per locale (`articles_en_us`, `articles_fr_fr`). Operational overhead is higher; per-index settings let you tune relevance per language.

## Assembly handling — most assemblies don't get indexed

Topics (`Article`, `GlossaryTerm`, `Person`, `Product`) → Algolia records.
Assemblies (`Page`, `PageSection`, `LandingPage`) → usually not indexed; they're navigation, not search hits.

Exceptions:

- **Landing pages** with curated copy that should be findable → index the `LandingPage` as its own type with title, summary, URL.
- **Pages that compose unique content** not present in any topic → index a flattened representation (title + concatenated section copy).

When indexing assemblies:

- Flatten components to plain text in a `body_plaintext` field.
- Don't try to index sections separately — they aren't pages on their own.
- Mark with `type: 'page'` to differentiate from topics.

## Editorial concerns

- **Publish lag** — webhook → indexer → Algolia is typically <2s. Set expectation.
- **Manual reindex** — provide an admin button or endpoint editors can hit when something feels stuck.
- **"Why isn't my article showing up?"** debugging:
  1. Did the webhook fire? (Contentful → Webhooks → Activity Log)
  2. Did the indexer receive it? (Vercel logs)
  3. Did the indexer succeed? (Vercel logs + Algolia response)
  4. Is the record in the index? (Algolia Dashboard → Index)
  5. Does the front end query include this record? (filters, locale)

Document the runbook in the engagement's `Runbooks/` folder.

## Output formats

### Integration design proposal

```
# Contentful → Algolia Integration: [Engagement]

## Choice: Marketplace app | Custom indexer | Hybrid
- Justification

## Indices
| Contentful type | Algolia index | Locales | Volume estimate |
|-----------------|---------------|---------|-----------------|
| article         | articles      | en-US, fr-FR | ~200 entries |
| glossaryTerm    | glossary_terms| en-US        | ~50 entries  |

## Preview vs. delivery
- Production: app `slalom-{client}-prod`, indices: articles, glossary_terms
- Preview: app `slalom-{client}-staging`, indices: articles_preview, glossary_terms_preview
- Routing: env-aware client

## Webhook config
- Topics
- Filters
- Signature secret

## Mapping
- Per-type mapping module
- Linked-reference resolution depth
- Computed fields source

## Backfill
- Manual endpoint
- Scheduled cron

## Observability
- Logging
- Alert on indexer failure
- Drift metric (records-in-source vs records-in-index)

## Roles
- Who maintains the indexer code
- Who updates Algolia settings
- Who handles editor-facing issues

## Open questions
```

## Common anti-patterns

- **Indexing the entire entry tree, deeply.** Records explode in size; relevance suffers.
- **One Algolia record carrying all locales.** Confuses ranking; can't filter cleanly.
- **Webhook handler that does heavy work synchronously.** Contentful retries; you get duplicates.
- **No backfill path.** Drift compounds.
- **Skipping signature verification.** Public mutation endpoint.
- **Mixing preview and production records.** Editors see leaked drafts; users see leaked drafts.
- **Indexing assemblies without flattening.** Search returns "Page" records that don't make sense as hits.
- **Hand-rolling the indexer when the Marketplace app would do.** Wasted effort.
- **Using the Marketplace app when a custom indexer is needed.** Forced into bad mappings.
- **Not handling the unpublish/delete case.** Stale records linger forever.

## Constraints

- Don't ship a Contentful → Algolia path without a backfill mechanism.
- Don't put preview content in a production index.
- Don't denormalize linked entries deeper than 4 levels — Contentful's `include` cap and your record-size budget both bite.
- Don't trust the Marketplace app's mapping for Topics & Assemblies that need real flattening — use a custom indexer.
- Don't deploy without observability on the indexer.

## How this skill relates to others

- The Contentful side: model, webhooks, GraphQL → `contentful-content-model`, `contentful-webhooks`, `contentful-graphql` in the contentful plugin.
- The Algolia side: record shape, settings, ops → `algolia-index-design`, `algolia-relevance-tuning`, `algolia-indexing-pipeline`.
- The keys the indexer uses → `algolia-api-keys-security`.
- The agent-led ops surface → `algolia-mcp-cli`.

## Source material

- Contentful Marketplace Algolia app: https://www.contentful.com/marketplace/algolia/
- Contentful webhooks: https://www.contentful.com/developers/docs/concepts/webhooks/
- Contentful GraphQL: https://www.contentful.com/developers/docs/references/graphql/
- Algolia Ingestion API: https://www.algolia.com/doc/guides/sending-and-managing-data/send-and-update-your-data/connectors/
- Plugin reference: `../../references/integrations-map.md`
- Plugin reference: `../../references/algolia-foundations.md`
- Adjacent plugin: `contentful` (full Contentful surface)