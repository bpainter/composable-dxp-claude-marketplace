---
name: algolia-autocomplete
description: >
  Building autocomplete and search-suggestion experiences with the Algolia Autocomplete UI
  library — distinct from InstantSearch. Covers query suggestions, multi-source autocomplete
  (suggestions + recent searches + records + redirects + content), the plugin architecture
  (`@algolia/autocomplete-plugin-*`), keyboard nav, accessibility, theming on shadcn,
  integration into a Next.js header, and the Query Suggestions index. Use this skill when
  building the global header search box, a command-palette experience, or any "type ahead and
  pick" UI. Don't reach for InstantSearch for these — Autocomplete is a different library with
  a different mental model.

# Project context
type: skill
project: skills-library
plugin: algolia
aliases: [algolia-autocomplete]
tags: [type/skill, plugin/algolia, topic/algolia, topic/autocomplete]
status: active
---

This skill puts you in the role of a front-end engineer building a typeahead-and-pick experience. Default posture: **Autocomplete is not a smaller InstantSearch — it's a different library with its own architecture, optimized for the header-search and command-palette pattern.**

The two libraries solve different jobs. InstantSearch builds full SERPs with refinement. Autocomplete builds dropdowns where the user picks one thing and goes. Mixing them on one page is fine; using one for the other's job creates pain.

Pair with `algolia-relevance-tuning` (the Query Suggestions index needs separate tuning), `algolia-search-client` (Autocomplete uses the search client under the hood), and `algolia-instantsearch-react` (the SERP page Autocomplete usually points to).

## When to use this skill

- Global header search box that suggests as the user types.
- Command palette (`⌘K`) for navigating an app or site.
- Multi-source typeahead: query suggestions + recent searches + actual records + content + redirects.
- Replacing a custom dropdown built on a search client because the UX got complicated (pagination, keyboard nav, screen-reader support, recent searches).
- Building Autocomplete inside a Next.js App Router header.

## When NOT to use this skill

- Full search page with refinements → `algolia-instantsearch-react`.
- Pre-filled facet UI on a listing page → `algolia-instantsearch-react`.
- A simple "show last 5 searches" with no Algolia → just LocalStorage; don't reach for the library.

## Core posture

- **Sources are the abstraction.** An Autocomplete dropdown is composed of one or more sources, each with its own data fetch and item template.
- **Plugins compose.** Recent searches, query suggestions, and redirects are official plugins. Use them; don't rebuild.
- **Headless rendering.** The library gives you state and accessibility; you bring the DOM. Defaults exist but are placeholder, not production.
- **Keyboard and screen-reader behavior is non-trivial.** Use the library's a11y primitives; don't roll your own.
- **Suggest queries, not records, for "find" jobs.** When the user is searching to land on the SERP, suggest queries. When the user is searching to land directly on a thing (one record, one resource), suggest records.

## Install

```bash
pnpm add @algolia/autocomplete-js \
         @algolia/autocomplete-plugin-recent-searches \
         @algolia/autocomplete-plugin-query-suggestions \
         @algolia/autocomplete-plugin-redirect-url \
         algoliasearch
```

For React-rendered items (recommended on a Next.js site):

```bash
pnpm add @algolia/autocomplete-js  # the renderer is framework-agnostic; you can use h() or createRoot
```

(Autocomplete doesn't ship a "React InstantSearch"-style component package. You get the primitive `autocomplete()` factory and bring your own framework rendering. There's a thin React example pattern below.)

## The minimal Autocomplete

```ts
import { autocomplete } from '@algolia/autocomplete-js';
import { algoliasearch } from 'algoliasearch';

const client = algoliasearch(APP_ID, SEARCH_KEY);

autocomplete({
  container: '#autocomplete',
  placeholder: 'Search the second brain…',
  openOnFocus: true,
  getSources({ query }) {
    return [
      {
        sourceId: 'articles',
        getItems() {
          return client
            .search({
              requests: [{ indexName: 'articles', query, hitsPerPage: 5 }],
            })
            .then(({ results }) => results[0].hits);
        },
        templates: {
          item({ item, html }) {
            return html`<a href="${item.url}">${item.title}</a>`;
          },
        },
      },
    ];
  },
});
```

That's the bones. Real header search has 4–6 sources stacked.

## Multi-source pattern

```ts
import { autocomplete, getAlgoliaResults } from '@algolia/autocomplete-js';
import { createLocalStorageRecentSearchesPlugin } from '@algolia/autocomplete-plugin-recent-searches';
import { createQuerySuggestionsPlugin } from '@algolia/autocomplete-plugin-query-suggestions';
import { createRedirectUrlPlugin } from '@algolia/autocomplete-plugin-redirect-url';

const recentSearchesPlugin = createLocalStorageRecentSearchesPlugin({
  key: 'composable-dxp-recent-searches',
  limit: 5,
});

const querySuggestionsPlugin = createQuerySuggestionsPlugin({
  searchClient: client,
  indexName: 'articles_query_suggestions',
  getSearchParams() {
    return recentSearchesPlugin.data!.getAlgoliaSearchParams({ hitsPerPage: 5 });
  },
});

const redirectUrlPlugin = createRedirectUrlPlugin();

autocomplete({
  container: '#autocomplete',
  openOnFocus: true,
  plugins: [recentSearchesPlugin, querySuggestionsPlugin, redirectUrlPlugin],
  getSources({ query }) {
    if (!query) return []; // recent + suggestions handle the empty state via plugins
    return [
      {
        sourceId: 'articles',
        getItems() {
          return getAlgoliaResults({
            searchClient: client,
            queries: [
              { indexName: 'articles', params: { query, hitsPerPage: 4, clickAnalytics: true } },
            ],
          });
        },
        templates: {
          header: () => 'Articles',
          item: ArticleItem,
          noResults: () => 'No articles match.',
        },
      },
      {
        sourceId: 'people',
        getItems() {
          return getAlgoliaResults({
            searchClient: client,
            queries: [{ indexName: 'people', params: { query, hitsPerPage: 3 } }],
          });
        },
        templates: {
          header: () => 'People',
          item: PersonItem,
        },
      },
      {
        sourceId: 'glossary',
        getItems() {
          return getAlgoliaResults({
            searchClient: client,
            queries: [{ indexName: 'glossary_terms', params: { query, hitsPerPage: 3 } }],
          });
        },
        templates: {
          header: () => 'Glossary',
          item: GlossaryItem,
        },
      },
    ];
  },
});
```

Order matters. Most teams do: **recent searches → query suggestions → records (most-relevant index first) → redirects (handled by plugin)**.

## Query Suggestions — the index behind "as-you-type" suggestions

The Query Suggestions index is generated from your traffic. You don't write to it; you configure a source index in the Dashboard and Algolia builds the suggestions index nightly.

Setup:

1. Algolia Dashboard → Data → Query Suggestions → Create.
2. Source index: e.g., `articles`. Algolia derives suggestions from queries that produced results.
3. Output index: e.g., `articles_query_suggestions`. This is what `createQuerySuggestionsPlugin` queries.
4. Optional: filter, dedupe, generate from custom signals, attach popularity to a facet.

Until enough Insights events flow, the suggestions index is sparse. Wire Insights early.

## Recent searches

`createLocalStorageRecentSearchesPlugin` stores the last N queries the user submitted (locally). Shows them on focus when query is empty. Tracks a query as "submitted" when the user picks an item or hits enter.

For cross-device persistence, swap to a server-backed store. The plugin's API supports a custom `data` source. Most engagements are fine with LocalStorage.

## Redirects via query rules

Server-side: define a query rule in Algolia (`algolia-relevance-tuning`) that triggers on a query and sets `userData: { redirectUrl: '/holiday-sale' }`. Client-side: `createRedirectUrlPlugin()` watches for that and renders a "Press enter to go" hint.

Use case: the user types "holiday" and gets pulled directly to the holiday landing page instead of the SERP.

## Item rendering — bring your own DOM

The `templates.item` function gets `{ item, html, components }` and returns DOM. The `html` tagged template is from `htm` — works with `preact`. For React, render via `createRoot`:

```tsx
import { createRoot } from 'react-dom/client';

autocomplete({
  // ...
  renderer: { createElement: React.createElement, Fragment: React.Fragment, render: () => {} },
  render({ children }, root) {
    if (!root._autocompleteRoot) {
      root._autocompleteRoot = createRoot(root);
    }
    root._autocompleteRoot.render(<>{children}</>);
  },
});
```

For new projects, use Preact via `htm` (the default) — smaller bundle, less ceremony. Save React-render integration for when the team needs it.

## Header integration in Next.js

Pattern: a Client Component header that mounts Autocomplete on first render, in a portal-friendly container.

```tsx
// components/header-search.tsx
'use client';
import { useEffect, useRef } from 'react';

export function HeaderSearch() {
  const containerRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    if (!containerRef.current) return;
    let cleanup: (() => void) | undefined;

    import('./autocomplete-instance').then(({ mount }) => {
      cleanup = mount(containerRef.current!);
    });

    return () => cleanup?.();
  }, []);

  return <div ref={containerRef} className="w-full max-w-md" />;
}
```

```ts
// components/autocomplete-instance.ts
import { autocomplete } from '@algolia/autocomplete-js';
// ... configure plugins, sources

export function mount(container: HTMLElement) {
  const instance = autocomplete({
    container,
    placeholder: 'Search…',
    plugins: [/* ... */],
    getSources: /* ... */,
  });
  return () => instance.destroy();
}
```

Dynamic import keeps Autocomplete out of the initial bundle. The header renders instantly; the dropdown wires up on hydration.

## Submit behavior — go to the SERP

When the user presses Enter or clicks a query suggestion, route to the SERP with the query in the URL:

```ts
autocomplete({
  // ...
  navigator: {
    navigate({ itemUrl }) {
      window.location.assign(itemUrl);
    },
    navigateNewTab({ itemUrl }) {
      window.open(itemUrl, '_blank');
    },
  },
  onSubmit({ state }) {
    window.location.assign(`/search?query=${encodeURIComponent(state.query)}`);
  },
});
```

For client-side routing in Next.js, use `useRouter` instead of `window.location`. Wrap the navigator + `onSubmit` in a closure that has access to the router.

## Insights wiring

```ts
import { createAlgoliaInsightsPlugin } from '@algolia/autocomplete-plugin-algolia-insights';
import insightsClient from 'search-insights';

insightsClient('init', { appId: APP_ID, apiKey: SEARCH_KEY });

const insightsPlugin = createAlgoliaInsightsPlugin({ insightsClient });

autocomplete({
  // ...
  plugins: [insightsPlugin, recentSearchesPlugin, querySuggestionsPlugin],
});
```

The plugin sends `view` and `click` events with `queryID` automatically when items have `__autocomplete_queryID` (they do, when fetched via `getAlgoliaResults`).

## Accessibility

The library handles:
- `aria-expanded`, `aria-controls`, `aria-activedescendant` on the input.
- `role="listbox"`, `role="option"` on the panel.
- Arrow key navigation (up/down through items, left/right between sources optionally).
- Escape to close.
- Enter to select.

You're responsible for:
- A visible label or `aria-label` on the input.
- Sufficient color contrast on the panel.
- Touch target size on mobile (44px minimum).
- Don't auto-focus on mobile (annoying virtual keyboard).

## Theming on shadcn

The library ships base CSS (`@algolia/autocomplete-theme-classic`); skip it. Style the rendered DOM with Tailwind classes via the templates. Match shadcn's `bg-popover`, `text-popover-foreground`, ring tokens for focus, and `rounded-md` for the panel. The `wrapper`, `panel`, `source`, `header`, `list`, `item` slots all accept `classNames` / class attributes.

## Performance

- **Dynamic import the Autocomplete bundle.** It's not needed on first render.
- **Use the lite client** in the browser — `@algolia/client-search`.
- **Debounce upstream** if your sources are not Algolia. The library debounces internal state updates, but `getItems` runs on every keystroke; expensive sources need debouncing.
- **`hitsPerPage` per source: 3–5.** Anything more clutters the dropdown.
- **`openOnFocus: true`** for the header pattern (shows recent + suggestions on focus); `false` for command palettes (require typing).

## Common patterns

### Command palette (⌘K)

```tsx
useEffect(() => {
  const handler = (e: KeyboardEvent) => {
    if ((e.metaKey || e.ctrlKey) && e.key === 'k') {
      e.preventDefault();
      // open the autocomplete
    }
  };
  window.addEventListener('keydown', handler);
  return () => window.removeEventListener('keydown', handler);
}, []);
```

Render Autocomplete inside a Dialog (shadcn `<Dialog>`); wire ⌘K to open / close.

### Federated header (recent + suggestions + 2–3 indices)

The pattern at the top of this skill. Default for editorial sites with multiple content types.

### Geo-aware suggestions

```ts
getItems() {
  return getAlgoliaResults({
    searchClient: client,
    queries: [{
      indexName: 'locations',
      params: {
        query,
        aroundLatLng: `${lat},${lng}`,
        aroundRadius: 50000,
        hitsPerPage: 5,
      },
    }],
  });
}
```

## Common anti-patterns

- **Building Autocomplete from scratch on top of `algoliasearch`.** Reinvents keyboard nav, recent searches, source composition, accessibility. The library exists for a reason.
- **Stuffing 10 sources into the panel.** 3–5 is the limit; past that the user can't scan.
- **Using InstantSearch widgets inside an Autocomplete panel.** Different library; will not compose cleanly.
- **Skipping query suggestions.** Then the empty/typing state is dead weight, and the user gets no help articulating their query.
- **No Insights wiring.** Means no signal for the suggestions index quality, no Personalization, no A/B.
- **Eager-loading the bundle.** Autocomplete should be a dynamic import for header use.
- **Auto-focusing on mobile.** Pops the keyboard, jumps the layout, makes the user angry.
- **Skipping the `noResults` template.** Empty-on-typo is a worse UX than "no matches; try X."

## Constraints

- Don't use Autocomplete to build a SERP. Use InstantSearch for that.
- Don't ship without recent-searches and query-suggestions plugins on the global header pattern.
- Don't mix Autocomplete and InstantSearch state directly. Submit Autocomplete → URL → InstantSearch reads from URL.
- Don't put admin keys in the Autocomplete client.

## How this skill relates to others

- The SERP that Autocomplete submits to → `algolia-instantsearch-react`.
- Direct client API beneath Autocomplete → `algolia-search-client`.
- Query Suggestions index tuning → `algolia-relevance-tuning`.
- Insights events from the dropdown → `algolia-analytics-events`.
- API-key strategy for the browser-side client → `algolia-api-keys-security`.

## Source material

- Autocomplete intro: https://www.algolia.com/doc/ui-libraries/autocomplete/introduction/what-is-autocomplete/
- Autocomplete getting started: https://www.algolia.com/doc/ui-libraries/autocomplete/getting-started/
- Plugins: https://www.algolia.com/doc/ui-libraries/autocomplete/core-concepts/plugins/
- Query Suggestions plugin: https://www.algolia.com/doc/ui-libraries/autocomplete/api-reference/autocomplete-plugin-query-suggestions/
- Recent Searches plugin: https://www.algolia.com/doc/ui-libraries/autocomplete/api-reference/autocomplete-plugin-recent-searches/
- Redirect URL plugin: https://www.algolia.com/doc/ui-libraries/autocomplete/api-reference/autocomplete-plugin-redirect-url/
- Algolia Insights plugin: https://www.algolia.com/doc/ui-libraries/autocomplete/api-reference/autocomplete-plugin-algolia-insights/
- Plugin reference: `../../references/algolia-foundations.md`