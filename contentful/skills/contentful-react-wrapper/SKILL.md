---
name: contentful-react-wrapper
description: Wire React components to consume Contentful content correctly across preview and production — typed clients, GraphQL fragments, on-demand revalidation via webhook, draft/preview routing, and the wrapper pattern that keeps presentational components clean. Use this skill any time a component needs to render content from Contentful, when content is shipping but not appearing on the preview environment, when on-publish revalidation is being wired, when a content type is being mapped to a React component, or when "preview vs. production" comes up — even when "wrapper" isn't said. Trigger whenever the contract between Contentful entries and React components is being implemented or fixed.

# Project context
type: skill
project: skills-library
plugin: contentful
aliases: [contentful-react-wrapper]
tags: [type/skill, plugin/contentful, topic/contentful, topic/react, topic/nextjs]
status: active
---

# Contentful Component Wrapper

This skill is the implementation layer between the Contentful content model and the React components that render it. The model is governed by `contentful-content-model`. Query design is governed by `contentful-graphql`. Caching/perf strategy is governed by `contentful-delivery-optimization`. The component primitives are governed by `software-engineering-shadcn-component`. This skill is how those layers meet.

The wrapper pattern keeps presentational components free of CMS-specific knowledge. A `Hero` doesn't know it came from Contentful; a `HeroFromContentful` wrapper fetches and shapes the data, then renders the `Hero`. The split makes both halves testable, swappable, and easier to reason about.

Pair with `contentful-content-model` (the schema this skill consumes), `contentful-graphql` (the queries this skill issues), `contentful-webhooks` (the trigger for revalidation), `contentful-delivery-optimization` (cache and perf shape), `contentful-rich-text` (rich text rendering), `software-engineering-nextjs-scaffold` (App Router patterns), and `software-engineering-quality-engineer` (contract testing).

## When to use this skill

- Wiring a new component to Contentful content.
- Setting up the typed Contentful client for the project.
- Configuring on-publish revalidation via webhook.
- Building or fixing the preview / draft mode routing.
- Diagnosing "I published in Contentful and the site didn't update."
- Mapping a Contentful component type to a React block (Topics & Assemblies in code).
- Migrating Phase 1 content (markdown / static data) into Phase 2 Contentful-driven content.

## Core posture

- **Two clients, two purposes.** The Delivery API is read-only and serves published content. The Preview API serves drafts. Use the right one.
- **Types come from the schema.** Generate TypeScript types from the Contentful content model; don't hand-write them. They drift.
- **GraphQL by default.** The GraphQL endpoint lets you request only the fields you need, which matters for performance and clarity. Use the REST API only for cases the GraphQL endpoint doesn't cover (large-scale exports, certain Management API operations).
- **Wrapper components are thin.** A wrapper fetches, shapes, and forwards. Business logic and presentation stay in the underlying block.
- **Revalidation is event-driven first.** Webhooks from Contentful trigger `revalidateTag` calls on the affected pages. Time-based revalidation is the safety net, not the primary mechanism.
- **Preview never leaks to production.** Preview API reads draft + published; Delivery API reads published only. Never use the Preview API in production.

## Setup: clients and tokens

Contentful exposes three APIs you'll touch from the front end:

- **Delivery API** — published content, read-only. Token: `CONTENTFUL_DELIVERY_TOKEN`.
- **Preview API** — published + drafts, read-only. Token: `CONTENTFUL_PREVIEW_TOKEN`.
- **Management API** — read/write, used for migrations and admin. Token: `CONTENTFUL_MANAGEMENT_TOKEN`. **Server-only**, never exposed to the browser. The wrapper layer doesn't normally use it.

Environment setup:

```env
CONTENTFUL_SPACE_ID=...
CONTENTFUL_ENVIRONMENT=master  # or development, staging
CONTENTFUL_DELIVERY_TOKEN=...
CONTENTFUL_PREVIEW_TOKEN=...
CONTENTFUL_MANAGEMENT_TOKEN=...   # server-side only, omit from preview environments
CONTENTFUL_PREVIEW_SECRET=...      # for protecting the preview-mode route
CONTENTFUL_REVALIDATE_SECRET=...   # for protecting the webhook route
```

Server-side client setup:

```ts
// src/lib/contentful/client.ts
import { createClient } from "contentful";

const space = process.env.CONTENTFUL_SPACE_ID!;
const environment = process.env.CONTENTFUL_ENVIRONMENT ?? "master";

export const deliveryClient = createClient({
  space,
  environment,
  accessToken: process.env.CONTENTFUL_DELIVERY_TOKEN!,
});

export const previewClient = createClient({
  space,
  environment,
  accessToken: process.env.CONTENTFUL_PREVIEW_TOKEN!,
  host: "preview.contentful.com",
});

export function getClient(preview: boolean) {
  return preview ? previewClient : deliveryClient;
}
```

For GraphQL specifically, use `fetch` directly against the GraphQL endpoint (lighter than the SDK and gives you native typing via codegen):

```ts
// src/lib/contentful/graphql.ts
const endpoint = (preview: boolean) =>
  `https://graphql.contentful.com/content/v1/spaces/${process.env.CONTENTFUL_SPACE_ID}/environments/${process.env.CONTENTFUL_ENVIRONMENT ?? "master"}`;

export async function contentfulGraphQL<T>(
  query: string,
  variables: Record<string, unknown> = {},
  options: { preview?: boolean; tags?: string[] } = {},
): Promise<T> {
  const { preview = false, tags = [] } = options;
  const token = preview
    ? process.env.CONTENTFUL_PREVIEW_TOKEN!
    : process.env.CONTENTFUL_DELIVERY_TOKEN!;

  const res = await fetch(endpoint(preview), {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      Authorization: `Bearer ${token}`,
    },
    body: JSON.stringify({ query, variables }),
    next: {
      // Time-based safety-net revalidation; on-demand via tags is preferred.
      revalidate: 3600,
      tags,
    },
  });

  if (!res.ok) {
    throw new Error(`Contentful GraphQL error: ${res.status} ${await res.text()}`);
  }

  const json = await res.json();
  if (json.errors) {
    throw new Error(`Contentful GraphQL errors: ${JSON.stringify(json.errors)}`);
  }
  return json.data as T;
}
```

## Type generation

Generate TypeScript types from the Contentful schema, don't hand-write them:

- **GraphQL Code Generator** — `@graphql-codegen/cli` with `@graphql-codegen/typescript` and `@graphql-codegen/typescript-operations`. Pulls the schema from the GraphQL endpoint, types per query.
- **`contentful-typescript-codegen`** — schema-level types from the Management API. Less granular but good for shared types.

Both work; `graphql-codegen` is the better default when you're using GraphQL for fetches. See `contentful-graphql` for query-design depth.

`codegen.yml`:

```yaml
schema:
  - https://graphql.contentful.com/content/v1/spaces/${CONTENTFUL_SPACE_ID}/environments/master:
      headers:
        Authorization: Bearer ${CONTENTFUL_DELIVERY_TOKEN}
documents: src/**/*.graphql
generates:
  src/lib/contentful/__generated__/types.ts:
    plugins:
      - typescript
      - typescript-operations
```

Run on schema change: `pnpm codegen`. Commit the generated file (don't gitignore it; reviewers should see schema changes flow through to types).

## The wrapper pattern

Two components per content-driven block:

1. **The presentational block** (`components/blocks/hero.tsx`) — pure, takes typed props, renders. No fetching, no Contentful, no async.
2. **The Contentful wrapper** (`components/blocks/hero-from-contentful.tsx`) — async server component, fetches the content, shapes it, hands props to the presentational block.

```tsx
// components/blocks/hero.tsx
import { Button } from "@/components/ui/button";

export type HeroProps = {
  eyebrow?: string;
  headline: string;
  subhead?: string;
  primaryCta?: { label: string; href: string };
  secondaryCta?: { label: string; href: string };
};

export function Hero({ eyebrow, headline, subhead, primaryCta, secondaryCta }: HeroProps) {
  return (
    <section className="mx-auto max-w-3xl px-6 py-24 text-center">
      {eyebrow && <p className="text-sm uppercase tracking-wide text-muted-foreground">{eyebrow}</p>}
      <h1 className="mt-2 text-5xl font-semibold tracking-tight">{headline}</h1>
      {subhead && <p className="mt-4 text-lg text-muted-foreground">{subhead}</p>}
      {(primaryCta || secondaryCta) && (
        <div className="mt-8 flex justify-center gap-4">
          {primaryCta && <Button asChild><a href={primaryCta.href}>{primaryCta.label}</a></Button>}
          {secondaryCta && <Button asChild variant="outline"><a href={secondaryCta.href}>{secondaryCta.label}</a></Button>}
        </div>
      )}
    </section>
  );
}
```

```tsx
// components/blocks/hero-from-contentful.tsx
import { contentfulGraphQL } from "@/lib/contentful/graphql";
import { draftMode } from "next/headers";
import { Hero, type HeroProps } from "./hero";

const HERO_QUERY = /* GraphQL */ `
  query HeroById($id: String!, $preview: Boolean) {
    hero(id: $id, preview: $preview) {
      eyebrow
      headline
      subhead
      primaryCta {
        label
        href
      }
      secondaryCta {
        label
        href
      }
    }
  }
`;

type HeroQueryResult = {
  hero: HeroProps | null;
};

export async function HeroFromContentful({ id }: { id: string }) {
  const { isEnabled: preview } = draftMode();
  const data = await contentfulGraphQL<HeroQueryResult>(
    HERO_QUERY,
    { id, preview },
    { preview, tags: [`hero:${id}`] },
  );

  if (!data.hero) return null;
  return <Hero {...data.hero} />;
}
```

The presentational block is testable with mock props (Storybook, unit tests). The wrapper is exercised in integration tests against a known fixture space. Contract tests (codegen + type checks) catch schema-vs-query drift before runtime.

## Section router for assemblies

For the Topics & Assemblies pattern, polymorphic section lists are rendered with a section-router component:

```tsx
// components/blocks/section-router.tsx
import { Hero } from "./hero";
import { FaqSection } from "./faq-section";
import { RichTextBlock } from "./rich-text-block";

type Section =
  | { __typename: "Hero" } & React.ComponentProps<typeof Hero>
  | { __typename: "FaqSection" } & React.ComponentProps<typeof FaqSection>
  | { __typename: "RichTextBlock" } & React.ComponentProps<typeof RichTextBlock>;

export function SectionRouter({ section }: { section: Section }) {
  switch (section.__typename) {
    case "Hero": return <Hero {...section} />;
    case "FaqSection": return <FaqSection {...section} />;
    case "RichTextBlock": return <RichTextBlock {...section} />;
    default: {
      // Exhaustiveness check; new section types fail at compile time.
      const _exhaustive: never = section;
      return null;
    }
  }
}
```

When a new content type is added in `contentful-content-model`, the `Section` union, the GraphQL fragment (see `contentful-graphql`), and the switch all need updates. The exhaustiveness check makes the compiler enforce it.

## Preview / draft mode

Next.js App Router uses `draftMode()` for preview. Set up two routes:

### Enable preview

```ts
// app/api/preview/enable/route.ts
import { draftMode } from "next/headers";
import { redirect } from "next/navigation";
import { NextRequest } from "next/server";

export async function GET(request: NextRequest) {
  const secret = request.nextUrl.searchParams.get("secret");
  const slug = request.nextUrl.searchParams.get("slug") ?? "/";

  if (secret !== process.env.CONTENTFUL_PREVIEW_SECRET) {
    return new Response("Invalid token", { status: 401 });
  }

  draftMode().enable();
  redirect(slug);
}
```

In Contentful, add a "Preview URL" pointing at:
`https://your-domain.com/api/preview/enable?secret=YOUR_SECRET&slug={entry.fields.slug}`

### Disable preview

```ts
// app/api/preview/disable/route.ts
import { draftMode } from "next/headers";
import { redirect } from "next/navigation";

export async function GET() {
  draftMode().disable();
  redirect("/");
}
```

Add an obvious "exit preview" button in the layout when `draftMode().isEnabled` is true so editors don't get stuck in preview.

## On-demand revalidation via webhook

Configure a Contentful webhook to fire on entry publish/unpublish. See `contentful-webhooks` for the configuration, signing, and retry patterns. The handler:

```ts
// app/api/revalidate/route.ts
import { revalidateTag } from "next/cache";
import { NextRequest } from "next/server";
import crypto from "node:crypto";

export async function POST(request: NextRequest) {
  const signature = request.headers.get("x-contentful-webhook-signature");
  const body = await request.text();

  // Verify signature (Contentful supports HMAC)
  const expected = crypto
    .createHmac("sha256", process.env.CONTENTFUL_REVALIDATE_SECRET!)
    .update(body)
    .digest("hex");
  if (signature !== expected) {
    return new Response("Invalid signature", { status: 401 });
  }

  const payload = JSON.parse(body);
  const contentType = payload.sys.contentType?.sys?.id;
  const id = payload.sys.id;

  if (contentType && id) {
    revalidateTag(`${contentType}:${id}`);
    // Also revalidate any aggregate tags (e.g., the whole homepage that references this)
    revalidateTag(contentType);
  }

  return Response.json({ revalidated: true });
}
```

For aggregate pages that reference many entries (homepage, listing pages), tag the page with multiple specific tags and a single aggregate tag. Webhook hits the aggregate; the page revalidates.

## Asset handling

Contentful assets come back as URLs to its CDN (`images.ctfassets.net`). Use `next/image` to consume them with proper optimization:

```tsx
import Image from "next/image";

<Image
  src={asset.url}
  alt={asset.description ?? ""}
  width={asset.width}
  height={asset.height}
  className="..."
/>
```

`next.config.mjs` allowlist:

```js
const config = {
  images: {
    remotePatterns: [
      { protocol: "https", hostname: "images.ctfassets.net" },
    ],
  },
};
export default config;
```

For heavy images, use Contentful's Images API parameters in the URL (`?w=1200&fm=webp&q=80`) and let `next/image` further optimize. See `contentful-delivery-optimization`.

## Rich text

Contentful's rich text is a structured AST, not HTML. See `contentful-rich-text` for renderer setup, embedded entries/assets, and design-system mapping.

## Migration from Phase 1 (markdown/static) to Phase 2 (Contentful)

The transition pattern:

1. **Mirror the data shape.** Build the Contentful content type to match the props the existing Phase 1 component takes. Migration becomes a swap, not a rewrite.
2. **Author the same content in Contentful.** Don't migrate accidentally; copy and verify.
3. **Add the `*FromContentful` wrapper.** The existing presentational component is unchanged.
4. **Switch the page.** Replace `<Hero {...staticProps} />` with `<HeroFromContentful id="..." />`.
5. **Verify and remove the static data.** Confirm preview and prod render identical output, then delete the static source.

Do this content type by content type, not all at once. Start with the lowest-risk surface (footer copy, then a single article, then a landing page).

## Contract testing

Per `software-engineering-quality-engineer`:

- The codegen run is itself a contract test — if the schema and the queries diverge, codegen fails.
- Add a CI job that runs `pnpm codegen` and fails the build if the generated file changes (drift from a forgotten regen).
- For wrapper components, test against a fixture space (or recorded responses) so the test doesn't hit the live CMS on every run.

## Common failure modes

- **Hand-written types drift.** Always codegen.
- **Preview token leaked to production.** Production uses Delivery only. Audit env vars; never expose `CONTENTFUL_PREVIEW_TOKEN` to client code.
- **Webhook not signed.** Anyone can hit the revalidate endpoint and trigger work. Always verify signature. See `contentful-webhooks`.
- **Time-based revalidation only.** Editors publish, then wait an hour. Use webhooks; time-based is the fallback.
- **Wrapper does too much.** Wrappers fetch and shape. If you find business logic in the wrapper, push it down to the block or up to a service.
- **Polymorphic section list with no exhaustiveness check.** New content type added to Contentful, never rendered in code. Add the `_exhaustive: never` switch.
- **Rich text rendered raw / unstyled.** Wire it through the design system's typography components. See `contentful-rich-text`.
- **Cache fragmentation from preview.** Don't cache preview responses; use `cache: "no-store"` or skip the `next.revalidate` config for the preview path.
- **Forgotten "exit preview" affordance.** Editors get stuck in preview mode and don't realize they're seeing drafts.
- **Image alt missing.** Contentful asset descriptions are commonly empty. Set authoring expectations: every asset gets a description, or alt is intentionally empty for decorative images.

## Output format

When this skill is asked to wire a new component to Contentful, the deliverable typically includes:

```
1. Content type confirmed in Contentful (links to contentful-content-model)
2. GraphQL query / fragment in src/lib/contentful/queries/ (links to contentful-graphql)
3. Codegen run; types updated
4. Presentational block in components/blocks/[name].tsx
5. Wrapper in components/blocks/[name]-from-contentful.tsx
6. Page or assembly updated to use the wrapper
7. Storybook story for the presentational block (mock props)
8. Webhook tag (`[contentType]:[id]`) added to fetch options (links to contentful-webhooks)
9. Tests (component-level for the block, integration-level for the wrapper)
10. Documentation: spec doc per software-engineering-component-spec
```

## How this skill relates to others

- The schema this skill consumes → `contentful-content-model`.
- The query layer this skill issues → `contentful-graphql`.
- The trigger for revalidation → `contentful-webhooks`.
- Cache and image strategy → `contentful-delivery-optimization`.
- Rich text rendering → `contentful-rich-text`.
- Locale-aware wrapper variants → `contentful-localization`.
- Variant rendering for personalization → `contentful-personalization`.
- App Router patterns and metadata → `software-engineering-nextjs-scaffold`.
- The primitives the rendered output uses → `software-engineering-shadcn-component`.
- Architecture-level rendering and revalidation strategy → `software-engineering-composable-architect`.
- Contract tests and webhook validation → `software-engineering-quality-engineer`.

## Source material

- Contentful Delivery API: https://www.contentful.com/developers/docs/references/content-delivery-api/
- Contentful GraphQL Content API: https://www.contentful.com/developers/docs/references/graphql/
- Next.js draft mode: https://nextjs.org/docs/app/building-your-application/configuring/draft-mode
- Next.js on-demand revalidation: https://nextjs.org/docs/app/building-your-application/data-fetching/incremental-static-regeneration
- GraphQL Code Generator: https://the-guild.dev/graphql/codegen
- `@contentful/rich-text-react-renderer`: https://www.npmjs.com/package/@contentful/rich-text-react-renderer
- Plugin reference: `../../references/api-surface.md`
