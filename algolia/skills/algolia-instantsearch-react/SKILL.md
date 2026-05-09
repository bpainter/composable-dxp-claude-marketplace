---
name: algolia-instantsearch-react
description: >
  Building search UIs with React InstantSearch and Next.js InstantSearch Hooks — server-rendered search pages, refinement widgets, infinite scroll, federation across multiple indices, routing, theming, accessibility, and the patterns that survive App Router / Server Components / Vercel deployments. Use this skill when wiring search to a Next.js or React front end, when migrating from `react-instantsearch-dom` to the current `react-instantsearch` Hooks, or when the search page needs to be SEO-friendly, fast, and refinement-rich.

# Project context
type: skill
project: skills-library
plugin: algolia
aliases: [algolia-instantsearch-react]
tags: [type/skill, plugin/algolia, topic/algolia, topic/react, topic/nextjs]
status: active
---

This skill puts you in the role of a senior front-end engineer building a high-fidelity search experience on React. Default posture: **start from `react-instantsearch` (Hooks), server-render the initial state for SEO and TTFB, hand off to client-side refinement after hydration.**

`react-instantsearch-dom` is deprecated. New work uses `react-instantsearch` (the Hooks-based package). If you find a project still on `-dom`, plan the migration; don't extend `-dom` further.

Pair with `algolia-index-design` (the record shape determines what the UI can show), `algolia-relevance-tuning` (settings drive the result quality), `algolia-search-client` (when InstantSearch is the wrong shape), `algolia-autocomplete` (header search; *different* library), and `algolia-analytics-events` (Insights wiring on click).

## When to use this skill

- Building or refactoring a site search page (`/search`, `/find`, `/articles`).
- Listing pages backed by Algolia (article browse, product browse, glossary index).
- Federated search across multiple indices.
- Adding facets / refinement to a content area that's currently navigation-only.
- Migrating from `react-instantsearch-dom`.
- Optimizing search-page Core Web Vitals.
- Wiring InstantSearch into Next.js App Router (Server Components + Client Components mix).

## Core posture

- **Hooks > legacy widgets.** Use `react-instantsearch` (Hooks API) for any new work.
- **SSR matters.** Server-render the initial query state; hydrate with the same. The user sees results before JS, the bot sees results, and TTFB stays low.
- **Composable widgets.** Use `useHits`, `useRefinementList`, `useSearchBox`, `useStats`, etc., to build branded UI on top of shadcn or your design system. Don't ship the stock widgets to a real product unless you're prototyping.
- **One `<InstantSearch>` per page.** Multiple roots are a smell.
- **Federation via `<Index>`.** Multi-index search is built in.
- **Insights from day one.** Wire `useInsights` (or the Insights middleware); don't add it "later."

## The minimal Next.js App Router setup

Install:

```bash
pnpm add algoliasearch react-instantsearch react-instantsearch-nextjs
```

Server Component (`app/search/page.tsx`):

```tsx
import { InstantSearchNext } from 'react-instantsearch-nextjs';
import { algoliasearch } from 'algoliasearch';
import { SearchExperience } from './search-experience';

const client = algoliasearch(
  process.env.NEXT_PUBLIC_ALGOLIA_APP_ID!,
  process.env.NEXT_PUBLIC_ALGOLIA_SEARCH_KEY!
);

export default async function SearchPage({
  searchParams,
}: {
  searchParams: Promise<Record<string, string | string[] | undefined>>;
}) {
  const params = await searchParams;

  return (
    <InstantSearchNext
      indexName="articles"
      searchClient={client}
      routing={true}                  // sync state to URL
      future={{ preserveSharedStateOnUnmount: true }}
    >
      <SearchExperience />
    </InstantSearchNext>
  );
}
```

Client Component (`app/search/search-experience.tsx`):

```tsx
'use client';

import {
  useSearchBox,
  useHits,
  useRefinementList,
  useStats,
  Configure,
} from 'react-instantsearch';

export function SearchExperience() {
  return (
    <div className="grid grid-cols-[280px_1fr] gap-8">
      <Configure hitsPerPage={20} clickAnalytics />
      <aside className="space-y-6">
        <Refinements />
      </aside>
      <main className="space-y-6">
        <SearchBox />
        <Stats />
        <Hits />
      </main>
    </div>
  );
}

function SearchBox() {
  const { query, refine } = useSearchBox();
  return (
    <input
      type="search"
      value={query}
      onChange={(e) => refine(e.currentTarget.value)}
      placeholder="Search articles…"
      className="w-full rounded-md border px-3 py-2"
    />
  );
}

function Refinements() {
  return (
    <>
      <RefinementList attribute="topics" title="Topics" />
      <RefinementList attribute="audience" title="Audience" />
    </>
  );
}

function RefinementList({ attribute, title }: { attribute: string; title: string }) {
  const { items, refine } = useRefinementList({ attribute, limit: 10 });
  return (
    <section>
      <h3 className="mb-2 text-sm font-semibold">{title}</h3>
      <ul>
        {items.map((item) => (
          <li key={item.value}>
            <label>
              <input type="checkbox" checked={item.isRefined} onChange={() => refine(item.value)} />
              <span className="ml-2">{item.label}</span>
              <span className="ml-2 text-muted-foreground">({item.count})</span>
            </label>
          </li>
        ))}
      </ul>
    </section>
  );
}

function Stats() {
  const { nbHits, processingTimeMS } = useStats();
  return (
    <p className="text-sm text-muted-foreground">
      {nbHits.toLocaleString()} results in {processingTimeMS}ms
    </p>
  );
}

function Hits() {
  const { hits } = useHits<ArticleHit>();
  return (
    <ol className="space-y-4">
      {hits.map((hit) => (
        <li key={hit.objectID}>
          <a href={hit.url} className="block">
            <h2 className="font-semibold">{hit.title}</h2>
            <p className="text-sm text-muted-foreground">{hit.summary}</p>
          </a>
        </li>
      ))}
    </ol>
  );
}

type ArticleHit = {
  objectID: string;
  title: string;
  summary: string;
  url: string;
};
```

That's the skeleton. Real work happens in:

- **Highlighting** — `<Highlight />` and `<Snippet />` from `react-instantsearch` for `<em>`-wrapped query matches.
- **Pagination** — `usePagination` (numbered) or `useInfiniteHits` (infinite scroll).
- **URL routing** — `routing={true}` syncs state to URL by default; customize via `stateMapping` if your URLs need to differ.
- **Empty state and error state** — `useStats().nbHits === 0` for the empty state; wrap the root in an error boundary.

## Server-side rendering — what `InstantSearchNext` actually does

`react-instantsearch-nextjs` is the App Router-aware wrapper. It:

- Reads `searchParams` from the URL on the server, runs the initial Algolia query, renders the HTML with results.
- Passes the same state to the client; hydration is seamless.
- Handles the `routing` integration without manual `next/navigation` plumbing.

Without it, `<InstantSearch>` renders an empty UI on the server and only fills in after JS hydration — bad for SEO, bad for TTFB, bad for the user.

For Pages Router projects, use `getServerState` + `<InstantSearchSSRProvider>` (the older pattern). Same idea, more boilerplate.

## Federation — multiple indices on one page

```tsx
import { Index } from 'react-instantsearch';

<InstantSearchNext indexName="articles" searchClient={client} routing>
  <SearchBox />
  <Hits />        {/* hits from "articles" */}
  <Index indexName="people">
    <Hits />      {/* hits from "people" */}
  </Index>
  <Index indexName="glossary_terms">
    <Hits />      {/* hits from "glossary_terms" */}
  </Index>
</InstantSearchNext>
```

The query is shared across all three, but each `<Index>` has its own state (refinements, pagination). For UI, render them as tabbed sections, side-by-side cards, or a collapsing accordion.

For *truly* unified ranking across types (one ranked list of mixed types), use **multi-index search via the search client** (`useMultipleQueries` or direct calls) rather than `<Index>`. Federation in InstantSearch keeps types separate.

## Refinements done right

Common widgets and the Hooks behind them:

| UI | Hook |
|---|---|
| Checkbox list with counts | `useRefinementList` |
| Single-select dropdown | `useMenu` |
| Hierarchical (categories tree) | `useHierarchicalMenu` (paired with `categories_lvl0`, `lvl1`, `lvl2` attributes) |
| Range filter (price, date) | `useRange` |
| Toggle (in stock only) | `useToggleRefinement` |
| Numeric range selector | `useNumericMenu` |
| Current refinements summary | `useCurrentRefinements` |
| Clear all | `useClearRefinements` |

For each, build the UI on shadcn primitives — `Checkbox`, `Slider`, `RadioGroup`, `Select` — to match the design system. Don't ship `<RefinementList>` (the legacy widget) into a designed product.

## Highlighting

```tsx
import { Highlight, Snippet } from 'react-instantsearch';

function Hit({ hit }: { hit: ArticleHit }) {
  return (
    <article>
      <h2>
        <Highlight attribute="title" hit={hit} />
      </h2>
      <p>
        <Snippet attribute="body_plaintext" hit={hit} />
      </p>
    </article>
  );
}
```

`<Highlight>` wraps matched query terms in `<mark>` (or a tag of your choice). `<Snippet>` does the same but with truncation around the match. Configure `attributesToHighlight` and `attributesToSnippet` in the index settings.

## Routing — URL as source of truth

`routing={true}` enables URL sync out of the box. State serializes to `?query=...&topics=composable,dxp&page=2`.

Customize:

```tsx
import { history } from 'instantsearch.js/es/lib/routers';
import { simple } from 'instantsearch.js/es/lib/stateMappings';

<InstantSearchNext
  indexName="articles"
  searchClient={client}
  routing={{
    router: history(),
    stateMapping: simple()
  }}
>
```

For pretty URLs (`/search/composable-dxp`) instead of `/search?query=composable+dxp`, write a custom `stateMapping`. Worth it on flagship search pages; not worth it on internal tools.

## Insights / click events

```tsx
import { Configure } from 'react-instantsearch';
import 'instantsearch.js/es/lib/plugins/insights';

<InstantSearchNext
  indexName="articles"
  searchClient={client}
  insights={true}        // enables the middleware
>
  <Configure clickAnalytics={true} />
  ...
</InstantSearchNext>
```

When a user clicks a hit, the middleware sends `click` events with `queryID` and `objectIDs` to Insights automatically. For conversions (filling a form, downloading), call the Insights client manually — see `algolia-analytics-events`.

## Performance

- **Server-render the initial state** with `InstantSearchNext`. SSR is the biggest single win.
- **Use the lite client** (`@algolia/client-search`) on the front end for smaller bundles. (`algoliasearch` is fine if you're already using it server-side.)
- **Debounce the search box.** The default is real-time; for keystrokes-heavy UX, debounce by 150–300ms via a custom `useSearchBox` wrapper.
- **`hitsPerPage`** — set it. Default is too high for the typical SERP. 12–20 is normal.
- **Lazy-load images** in hits. Use `loading="lazy"` and Next's `<Image>`.
- **Infinite scroll only when justified.** Numbered pagination is fine and friendlier to bookmarks.
- **Cache** — Algolia caches at the CDN; the client doesn't add much beyond that. Don't try to second-guess it.

## Accessibility

- The search box must have a label (visible or `aria-label`).
- Refinement checkboxes must have labels.
- Result count should be announced via `aria-live="polite"`.
- Focus management on pagination — focus the first new hit, not the page top.
- Keyboard nav through hits — arrow keys + Enter is the standard pattern.

A search SERP that doesn't pass `axe` is a search SERP that's quietly failing power users and screen-reader users alike.

## Theming

Build on shadcn primitives. The InstantSearch Hooks are headless — they give you state, not DOM. Compose your own components to match the design system. The legacy widgets (`<SearchBox>`, `<Hits>`, `<Pagination>`) ship a default DOM that's hard to theme; avoid them.

## Common patterns

### Listing page (no search box, just refinements)

```tsx
<InstantSearchNext indexName="articles" searchClient={client} routing>
  <Configure hitsPerPage={20} />
  {/* No SearchBox — the URL filters drive everything */}
  <Refinements />
  <Hits />
  <Pagination />
</InstantSearchNext>
```

### Search-as-you-type with debouncing

```tsx
function DebouncedSearchBox({ delay = 200 }) {
  const { query, refine } = useSearchBox();
  const [value, setValue] = useState(query);

  useEffect(() => {
    const t = setTimeout(() => refine(value), delay);
    return () => clearTimeout(t);
  }, [value, delay, refine]);

  return <input value={value} onChange={(e) => setValue(e.currentTarget.value)} />;
}
```

### Conditional refinement (only show "Author" facet for articles index)

```tsx
const { items: typeItems } = useRefinementList({ attribute: 'type' });
const isArticleSelected = typeItems.some((i) => i.value === 'article' && i.isRefined);

{isArticleSelected && <RefinementList attribute="author_name" title="Author" />}
```

## Output formats

### Page architecture sketch

```
/app/search/
├── page.tsx                  // Server Component, InstantSearchNext root
├── search-experience.tsx     // Client Component, composition of widgets
├── components/
│   ├── search-box.tsx
│   ├── refinements.tsx
│   ├── hits.tsx
│   ├── pagination.tsx
│   └── empty-state.tsx
└── lib/
    └── algolia-client.ts     // shared client factory
```

## Common anti-patterns

- **Using `react-instantsearch-dom` in 2026.** Migrate.
- **Client-only rendering.** Slow first paint, no SEO; use `InstantSearchNext`.
- **Multiple `<InstantSearch>` roots on a page.** Use `<Index>` for federation.
- **Putting the search-only key in a server-only env var.** It's *meant* to ship to the client; that's why it's called search-only.
- **Putting an admin key anywhere near the front end.** Different category of problem; see `algolia-api-keys-security`.
- **Skipping Insights.** No CTR data, no Personalization training data, no A/B test data.
- **Stock widgets in production.** Compose on shadcn. Stock widgets are for prototypes.
- **Search-as-you-type with no debounce.** Fires a query per keystroke; bad latency, bad rate-limit hygiene.

## Constraints

- Don't ship a search page without SSR for the initial state.
- Don't ship without Insights wired.
- Don't introduce `react-instantsearch-dom` to a new project.
- Don't render hits without highlighting.
- Don't build refinement UI from scratch when a Hook + a shadcn primitive does it in 20 lines.

## How this skill relates to others

- The records and settings the UI consumes → `algolia-index-design`, `algolia-relevance-tuning`.
- The header / global autocomplete (different library) → `algolia-autocomplete`.
- Custom UIs that don't fit InstantSearch's shape → `algolia-search-client`.
- Insights wiring details → `algolia-analytics-events`.
- API-key strategy for the client → `algolia-api-keys-security`.
- Page composition / Tailwind / shadcn primitives → `software-engineering-frontend-developer`, `software-engineering-shadcn-component`.

## Source material

- React InstantSearch (Hooks): https://www.algolia.com/doc/api-reference/widgets/react/
- React InstantSearch SSR with Next.js: https://www.algolia.com/doc/guides/building-search-ui/going-further/server-side-rendering/react/
- `react-instantsearch-nextjs`: https://www.npmjs.com/package/react-instantsearch-nextjs
- Routing: https://www.algolia.com/doc/guides/building-search-ui/going-further/routing-urls/react/
- Federation: https://www.algolia.com/doc/guides/building-search-ui/widgets/showing-results/react/#federated-search
- InstantSearch.js (vanilla): https://www.algolia.com/doc/api-reference/widgets/js/
- Plugin reference: `../../references/algolia-foundations.md`
