---
name: algolia-recommend
description: >
  Algolia Recommend — pre-trained recommendation models served as a separate API. Covers the
  four model families (Related Items, Frequently Bought Together, Trending Items / Trending
  Facets, Looking Similar), training data requirements (Insights events), nightly retraining
  cadence, fallback strategies, the React widgets and Hooks (`useRelatedProducts`,
  `useFrequentlyBoughtTogether`, etc.), and where Recommend earns its place vs. building it
  yourself. Use this skill when designing detail-page widgets ("you might also like",
  "frequently bought together", "trending now"), when launching Recommend on an engagement, or
  when a Recommend model isn't returning results and we need to debug the training data.

# Project context
type: skill
project: skills-library
plugin: algolia
aliases: [algolia-recommend]
tags: [type/skill, plugin/algolia, topic/algolia, topic/recommend]
status: active
---

This skill puts you in the role of an engineer rolling out Algolia Recommend on a content or commerce site. Default posture: **Recommend earns its place when you have enough Insights events to train it; otherwise it's a placeholder that won't return results.**

Pair with `algolia-analytics-events` (the events that train the models), `algolia-instantsearch-react` (the rendering layer for Recommend widgets), `algolia-search-client` (when you need direct Recommend API calls), and `algolia-personalization-ai` (the broader AI surface).

## When to use this skill

- Detail page needs a "you might also like" / "related products" / "people also read" widget.
- Cart page needs "frequently bought together."
- Homepage / category page needs "trending now."
- Image-driven discovery needs "looking similar."
- Recommend is enabled but isn't returning results (debugging the training pipeline).
- Building the rollout plan for an engagement.

## When NOT to use this skill

- The site has no Insights events flowing yet → wire Insights first (`algolia-analytics-events`).
- Volume is too low for the model to train (<a few thousand events / 30 days per model).
- The recommendation logic is deterministic and editorial-driven (e.g., "show the next part in the series") → just use a normal search query with filters.

## Core posture

- **Recommend rides on Insights.** No events, no model. Wire Insights as soon as the index goes live; Recommend can come months later, but the training data needs to start flowing immediately.
- **One model per surface, not one per intuition.** "Related Items" is one widget; "Frequently Bought Together" is another. Don't stack four models on one page expecting magic.
- **Always have a fallback.** If the model returns 0 results (sparse training, new objectID), render a sensible default — recent items, popular items, editor's pick.
- **Skeleton + lazy-load.** Recommend adds a network call. Render a skeleton, fetch, hydrate.
- **Measure the lift.** A/B test Recommend vs. the fallback. CTR and conversion-attributable-to-Recommend are the metrics.

## The four model families

### Related Items

"People who interact with X also interact with Y."

- Trains on `view` and `click` events.
- Per-objectID model — given an `objectID`, returns related `objectIDs`.
- Best for: detail page "you might also like" / "people also read" / "related articles."
- Minimum events: 1k+ events on the source `objectID` over the past 30 days for a useful model; 10k+ for the model to feel sharp.

### Frequently Bought Together

"X is often co-purchased with Y."

- Trains on `conversion` events with a subtype of `addToCart` or `purchase`.
- Specifically built for commerce; for content, **Related Items** usually serves the "consumed together" intent better.
- Per-objectID — given `objectID`, returns FBT items.
- Minimum events: needs strong purchase signal. Low-volume catalogs won't get useful models.

### Trending Items

"What's gaining steam right now."

- Trains on `view`, `click`, and `conversion` events; weighted by recency.
- Returns a list of objectIDs trending across the index (or filtered by a facet).
- Best for: homepage hero, category pages, content homepages.
- Minimum events: works at lower volumes than Related Items. A site getting hundreds of events / day across a few hundred items can produce useful trending.

### Trending Facets

Same as Trending Items, but returns trending **facet values** (e.g., trending categories, trending topics).

- Useful for navigation: "trending topics this week."

### Looking Similar

"Items visually similar to X."

- Image-similarity model. Requires `image` URLs in records.
- Opt-in; runs an image-embedding job over your records.
- Best for: visual catalogs (fashion, design assets, photography).
- Not useful for content sites where the image isn't the discriminator.

## Training data requirements

Recommend models train **nightly** on Insights events from the **past 30 days**. Implications:

- Day 1 after enabling: model is empty. Returns 0 results.
- Days 2–7: model is sparse. Some popular `objectIDs` have results; many don't.
- Day 30+: model is steady-state. Lift becomes measurable.

Plan the rollout accordingly:

- Wire Insights events first.
- Wait ≥30 days before judging Recommend quality.
- Always have a fallback for objectIDs the model doesn't have results for.

## Setup in the Dashboard

1. Algolia Dashboard → AI → Recommend → New model.
2. Pick the model family.
3. Pick the source index.
4. Configure the events the model uses (e.g., for Related Items: `view`, `click`).
5. Optional: configure facets to consider for diversity (e.g., return at most 1 item per `category`).
6. Save. Wait for first training (overnight).

For multi-region apps, models are per-app. Configure per environment as needed.

## React Hooks for Recommend

The `react-instantsearch` package ships Hooks for Recommend:

```bash
pnpm add react-instantsearch @algolia/recommend
```

```tsx
'use client';
import { useRelatedProducts } from 'react-instantsearch';

export function RelatedArticles({ objectID }: { objectID: string }) {
  const { items, status } = useRelatedProducts({
    objectIDs: [objectID],
    limit: 4,
    // Optional fallback parameters
    fallbackParameters: {
      facetFilters: [['type:article']],
    },
    queryParameters: {
      facetFilters: [['locale:en-US']],
    },
  });

  if (status === 'loading') return <RelatedSkeleton />;
  if (items.length === 0) return <RelatedFallback />; // editorial pick

  return (
    <section>
      <h3>Related articles</h3>
      <ul className="grid grid-cols-2 gap-4">
        {items.map((item) => (
          <li key={item.objectID}>
            <a href={item.url}>
              <h4>{item.title}</h4>
              <p>{item.summary}</p>
            </a>
          </li>
        ))}
      </ul>
    </section>
  );
}
```

Available Hooks:

- `useRelatedProducts({ objectIDs, ... })`
- `useFrequentlyBoughtTogether({ objectIDs, ... })`
- `useTrendingItems({ facetName?, facetValue?, ... })`
- `useTrendingFacets({ facetName, ... })`
- `useLookingSimilar({ objectIDs, ... })`

The Hook returns `{ items, status, recommendations, ... }`. `items` is the array of records (with full record fields, like a search hit). `recommendations` is the same thing in object-form.

## Direct Recommend client (without InstantSearch)

```ts
import { recommend } from '@algolia/recommend';

const recommendClient = recommend(APP_ID, SEARCH_KEY);

const { results } = await recommendClient.getRecommendations({
  requests: [
    {
      model: 'related-products',
      indexName: 'articles',
      objectID: 'article-2026-composable-dxp',
      maxRecommendations: 4,
      threshold: 70, // 0–100; higher = stricter relevance
      fallbackParameters: {
        facetFilters: [['type:article'], ['locale:en-US']],
      },
    },
  ],
});

const items = results[0].hits;
```

Use direct calls when:
- Server-rendering recommendations into HTML (no client-side fetch).
- Recommendations feed an email or notification.
- The UI doesn't otherwise use InstantSearch.

## Fallback strategy

When the model returns no results (sparse training, brand-new object), the Hook supports a `fallbackParameters` that runs a normal search-shaped query. Use this to ensure the widget never renders empty.

Common fallbacks:

| Widget | Fallback |
|---|---|
| Related articles | Recent articles in same `topic` or `audience` facet |
| Related products | Other products in same `category_lvl1` |
| Frequently bought together | Most popular accessories / curated bundle |
| Trending | Editor's picks |

Keep the fallback editorial-quality. Bad fallback is worse than no widget.

## Threshold tuning

The `threshold` parameter (0–100) controls relevance strictness. Default: 0 (no threshold — return whatever the model has).

- Set higher (60–80) when the model is mature and you want only confident recommendations.
- Set lower (0–30) when the model is still warming up and you'd rather get *something* than nothing.

Watch CTR by threshold during the first month. Adjust to the sweet spot.

## A/B testing Recommend

Always A/B before declaring victory. Variants:

- **Variant A**: existing UI (no recommendations or hand-rolled "recent in category").
- **Variant B**: Recommend widget.

Metric: CTR on the widget area, downstream conversion attributable to clicks on the widget.

Run for ≥ 2 weeks; longer for low-traffic pages. Measure on Insights events with explicit `eventName: 'recommended-item-clicked'` and `objectIDs` for attribution.

If Recommend doesn't beat the fallback, the model isn't ready (training data) or the placement is wrong (audience doesn't engage with that surface).

## Cost considerations

Recommend is **a separate billable feature** on most plans. Per-request pricing on some models (notably Looking Similar). Check the active plan and set budget alerts.

Mitigations:

- **Cache the response** — if the same `objectID` is viewed thousands of times per day, cache the recommendation list for 5–60 minutes server-side.
- **Lazy-load the widget** — render it on intersection, not on initial page render. Avoids requests for users who never scroll to it.
- **Pre-compute for high-traffic pages** — for the homepage trending widget, pre-compute hourly and serve from your CDN.

## Editorial overrides

Recommend doesn't replace editorial judgment. Common overrides:

- **Pin a specific item** for a given source `objectID` (e.g., "always recommend X when viewing Y").
- **Block specific items** (e.g., never recommend a withdrawn product).

Implement at the application layer:

```ts
const items = await recommendClient.getRecommendations({ /* ... */ });
const filtered = items.filter((i) => !blocklist.includes(i.objectID));
const final = pinned.length ? [...pinned, ...filtered].slice(0, limit) : filtered;
```

Don't try to encode editorial logic into the model itself. Keep it transparent at the read layer.

## Output formats

### Recommend rollout plan

```
# Algolia Recommend Rollout: [Engagement]

## Surfaces
- Article detail → Related Articles
- Product detail → Related Products + Frequently Bought Together
- Category page → Trending Items (filtered to category)
- Homepage → Trending Items (global)

## Models to enable
For each:
- Family
- Source index
- Events used
- Threshold
- Diversity facet (if any)

## Insights prerequisites
- Events flowing for ≥ 30 days before judging quality
- Event names + counts to verify

## Fallbacks
For each surface:
- Fallback query / strategy
- Skeleton component

## A/B plan
- Variant definitions
- Metrics
- Duration

## Cost
- Estimated requests per month
- Plan budget impact
- Caching strategy

## Editorial overrides
- Pin / block infrastructure (if needed)
```

## Common anti-patterns

- **Enabling Recommend before Insights is wired.** Model never trains.
- **No fallback.** Empty widget on every new article. Looks broken.
- **Wrong model for the job.** FBT on a content site (no purchase events). Use Related Items.
- **No A/B test.** Can't tell if Recommend is helping.
- **Stacking four widgets on one page.** "Related," "Trending," "Looking Similar," "Recently Viewed" — too much. Pick one or two.
- **Trusting the model on Day 1.** Training takes ≥ 30 days.
- **No cost monitoring.** Recommend is billable. Track requests per month.
- **No skeleton.** Page jumps when the widget hydrates.
- **Forgetting `queryParameters`** (locale filter, visibility filter). The model returns recommendations from the whole index by default; without filters you'll show French articles to English readers.

## Constraints

- Don't promise Recommend results on Day 1.
- Don't ship Recommend without a fallback.
- Don't run Recommend without Insights events.
- Don't render the widget without a skeleton or graceful empty state.
- Don't paste editorial overrides into the model; do them in code.

## How this skill relates to others

- The Insights events that train the models → `algolia-analytics-events`.
- The React widgets that render results → `algolia-instantsearch-react`.
- Direct Recommend API → `algolia-search-client` patterns + this skill.
- Personalization (per-user re-ranking on top of Recommend) → `algolia-personalization-ai`.
- The keys that the Recommend client uses → `algolia-api-keys-security`.

## Source material

- Recommend overview: https://www.algolia.com/doc/guides/algolia-recommend/overview/
- React Recommend: https://www.algolia.com/doc/guides/algolia-recommend/how-to/integrating-recommend/react/
- Recommend API: https://www.algolia.com/doc/rest-api/recommend/
- Looking Similar: https://www.algolia.com/doc/guides/algolia-recommend/how-to/looking-similar/
- Plugin reference: `../../references/algolia-foundations.md`
- Plugin reference: `../../references/api-surface.md`