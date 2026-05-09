---
name: algolia-relevance-tuning
description: >
  Relevance tuning for Algolia indices — searchable-attribute order, custom ranking, the master ranking formula, typo tolerance, synonyms (one-way, multi-way, alt-corrections), query rules (pinning, redirects, banners, boosting, hiding, dynamic filters), Dynamic Re-Ranking, and A/B testing changes safely. Use this skill any time relevance is the topic — search returns the wrong things, the business asks to pin a result, the team is rolling out NeuralSearch, or the index is being audited. Pairs with `algolia-index-design` (records first) and `algolia-analytics-events` (measure with Insights).

# Project context
type: skill
project: skills-library
plugin: algolia
aliases: [algolia-relevance-tuning]
tags: [type/skill, plugin/algolia, topic/algolia, topic/relevance]
status: active
---

This skill puts you in the role of a search-relevance engineer who treats every change as a hypothesis to be measured. Default posture: **don't tune what you can't measure, and don't measure what you can't ship to a real user.**

Pair with `algolia-index-design` (the record shape sets the ceiling on what's tunable), `algolia-analytics-events` (where the truth lives — top searches, no-result rate, click-through, conversion), and `algolia-personalization-ai` for AI-side levers (NeuralSearch, query categorization).

## When to use this skill

- Search returns the wrong thing for an obvious query — the canonical "find the FAQ for X" smoke test fails.
- High no-result rate on top searches.
- Stakeholder asks: "Can we pin this whitepaper to the top for queries about composable DXP?"
- Long-tail queries don't return what they should because the typo handling is too strict / too loose.
- The team is enabling NeuralSearch and wants the right ranking + relevance posture before it goes on.
- Quarterly relevance review — what's drifted, what synonyms are missing, which rules are stale.
- Migrating from another search engine and porting (or not porting) the existing tuning artifacts.

## Core posture

- **Records first.** If the searchable attribute is missing or full of HTML, no amount of tuning helps.
- **Defaults are good.** Algolia's stock ranking formula handles ~95% of cases. Touch with care.
- **One change per A/B.** Multiple changes at once means you don't know what worked.
- **Synonyms before rules; rules before formula changes.** Cheaper levers first.
- **Measure on Insights events, not gut feel.** A relevance change without `clickThroughRate` and `conversionRate` deltas is theater.
- **Document why.** Every rule should have a description; every synonym group should be defended in the engagement's relevance log.

## The relevance levers (in order of leverage)

### 1. Searchable attributes — the highest-leverage knob

```json
{
  "searchableAttributes": [
    "title,name",          // unordered within a group; group ranks higher than below
    "summary",
    "body_plaintext",
    "topics,tags",
    "unordered(author_name)" // unordered means position-in-attribute doesn't matter
  ]
}
```

Rules:
- Order matters: earlier = stronger boost.
- **Group with comma** (`"title,name"`) — both equally weighted, ranking-wise.
- **`unordered(...)`** — position within the attribute doesn't matter. Use for tags, names, taxonomies.
- Don't list attributes you don't need queried. Each one expands the search space and dilutes ranking.

### 2. Custom ranking — the tiebreaker

```json
{
  "customRanking": ["desc(popularity)", "desc(publishedAt_unix)"]
}
```

`customRanking` runs *after* textual ranking has narrowed candidates. It's the answer to "two records both match the query — which one is better?"

Common signals:
- `popularity` — clicks / views over a window (compute upstream from Insights).
- `freshness` — `desc(publishedAt_unix)` for content; for products, freshness is rarely the right signal.
- `inventory` — `desc(in_stock_count)` for commerce.
- `rating`, `rating_count`.

Don't use `customRanking` for facets or filters. That's a different lever.

### 3. The master ranking formula

```json
{
  "ranking": [
    "typo",
    "geo",
    "words",
    "filters",
    "proximity",
    "attribute",
    "exact",
    "custom"
  ]
}
```

These are the eight built-in tie-breakers, in evaluation order. The default is good. Reasons to change:

- Drop `geo` if you don't have geosearch — pure cleanup.
- Move `custom` (i.e., `customRanking`) higher only if you really mean it — putting `popularity` ahead of `typo` means a popular typo'd record beats a perfect match. Almost never right.
- Most teams should never touch this list. If you do, document the why prominently in the engagement notes.

### 4. Typo tolerance

```json
{
  "typoTolerance": true,
  "minWordSizefor1Typo": 4,
  "minWordSizefor2Typos": 8,
  "allowTyposOnNumericTokens": false,
  "ignorePlurals": ["en"],
  "removeStopWords": ["en"]
}
```

The defaults work for English content. Touch them when:

- Brand names / SKUs are getting typo-corrected to common words. → `disableTypoToleranceOnAttributes: ["sku", "brand"]`.
- Numbers are getting typo'd. → `allowTyposOnNumericTokens: false`.
- Stop words are interfering with multilingual content. → adjust `removeStopWords` per locale.

### 5. Synonyms

Three flavors:

| Type | Pattern | Use |
|---|---|---|
| **Multi-way** | `[laptop, notebook, computer]` — any matches any | Equivalents the user might type any of |
| **One-way** | `tee → [t-shirt]` — `tee` returns t-shirt results, not vice versa | Slang or alternate spellings |
| **Alt-correction** | `iphn → iphone` (1-typo or 2-typo) | Brand misspellings that the typo engine misses |

Synonyms beat custom ranking for fixing "I searched X and didn't get Y." Always check if a missing synonym is the real cause before reaching for a rule.

For multilingual content, synonym sets are **per-index, not per-locale by default**. If you fan out one index per locale, the synonyms can be locale-specific. If you have one index with a `locale` filter, synonyms apply across locales — usually fine for English/French/Spanish overlaps; problematic for unrelated languages.

### 6. Query rules

The merchandising layer. Query rules trigger on conditions and apply consequences.

**Conditions**:
- Anchored query (`exact match` of "free shipping").
- Contains pattern (`contains "summit"` matches "summit risk gates", "slalom summit").
- Context (when a context tag is set in the query — e.g., logged-in user, holiday season).
- Filters (when a specific filter is applied).
- Time range (start / end dates).

**Consequences**:
- **Pin** an objectID to a specific position.
- **Hide** records.
- **Boost / demote** records via filter promotions.
- **Add a banner / promo content** in `userData`.
- **Replace the query** (e.g., "phone" → "iPhone").
- **Filter** the results (e.g., `category:laptop`).
- **Redirect** to a URL (e.g., `/holiday-sale` for query "holiday").

Reach for rules when:
- Business wants pinning ("show this whitepaper for 'composable dxp'").
- Search needs a redirect to a landing page.
- Seasonal merchandising (Black Friday, Slalom Summit, GTM campaigns).
- Promoting a category for a query that's ambiguous.

Don't reach for rules when:
- The fix is "this query has no good results" — that's a content gap, not a relevance bug.
- The fix would apply to dozens of queries — that's a synonym or a custom-ranking change.

### 7. NeuralSearch / AI Search (semantic + keyword hybrid)

NeuralSearch (Premium plan) layers vector retrieval on top of keyword retrieval and merges the rankings. Toggle:

```js
const results = await index.search('summit risk gates', {
  mode: 'neuralSearch',
  // optional — control the keyword/semantic balance
  semanticSearch: { eventSources: ['articles'] }
});
```

When it helps:
- Conceptual queries — "how do I onboard a new manager" matches articles titled "first-90-days plan."
- Long natural-language queries.
- Queries with no shared vocabulary with the records (where keyword fails).

When it hurts:
- Brand / SKU / exact-match queries — semantic can pull in similar-but-wrong records.
- High-precision use cases (legal, regulatory) where false positives are expensive.

Roll out NeuralSearch as an A/B test. Compare CTR and conversion. Keep the winner.

See `algolia-personalization-ai` for the deeper treatment.

### 8. Dynamic Re-Ranking and Personalization

`algolia-personalization-ai` covers this. Briefly:

- **Dynamic Re-Ranking** — Algolia re-orders the top-N results based on which records click well for similar queries. Auto-tunes itself; you set `relevancyStrictness` (0–100, where 100 means "prefer textual relevance over re-ranking").
- **Personalization** — per-user re-ranking based on event affinities. Requires Insights events with `userToken`.

## Workflow: improving relevance

```
1. Look at Search Analytics first.
   - Top 50 queries (do they each return *something*).
   - Top 20 no-result queries (synonym or content gap?).
   - Top 20 low-CTR queries (relevance issue or content issue?).
   - Click position distribution (high mean position = relevance is bad).

2. Form one hypothesis at a time.
   - "Adding 'composable' as a synonym of 'headless' will fix the no-result rate on 'headless cms'."
   - "Boosting `popularity` 2x will fix the low CTR on 'next.js'."

3. Make the change in a non-prod app or a settings staging snapshot.

4. A/B test it.
   - Variant index (or settings overrides).
   - Run for ≥ 14 days, ≥ 1000 events per arm.
   - Compare CTR, conversion, mean click position, no-result rate.

5. Promote the winner. Document the why.

6. Add the test result to the engagement's relevance log.
```

## A/B testing in Algolia

```js
// Created in Dashboard or via API.
// Variant approaches:

// 1. Two indices (the cleanest):
const test = {
  variants: [
    { index: 'articles', trafficPercentage: 50 },
    { index: 'articles_v2', trafficPercentage: 50 }
  ]
};

// 2. Custom search parameters override (lighter weight, settings-only):
const test2 = {
  variants: [
    { index: 'articles', trafficPercentage: 50 },
    { index: 'articles', trafficPercentage: 50,
      customSearchParameters: {
        searchableAttributes: ['title', 'summary,body_plaintext'],
        customRanking: ['desc(popularity)', 'desc(publishedAt_unix)']
      }
    }
  ]
};
```

Algolia tracks `clickAnalytics` and `conversion` events per variant and computes significance.

A/B tests need:
- Insights events flowing (`algolia-analytics-events`).
- A success metric defined upfront. CTR is fine; conversion is better.
- ≥ 14 days, ≥ 1000 conversion events per arm to be confident.
- Documented hypothesis and rollback plan.

## Settings as code

Don't tune in the Dashboard for prod. Apply to a non-prod app, export, commit, review, deploy:

```bash
algolia --profile dev settings export articles > algolia/articles.settings.json
git add algolia/articles.settings.json
# PR review
algolia --profile prod settings import articles --file algolia/articles.settings.json
```

Same for rules and synonyms (`algolia rules export ...`, `algolia synonyms export ...`).

## Output formats

### Relevance change proposal

```
# Relevance Change: [Short description]

## Problem
- Query / queries affected
- Current behavior
- Insights data (CTR, no-result rate, top-N click position)

## Hypothesis
- One sentence: what change, what outcome

## Change
- The exact settings / rules / synonyms diff

## Test
- A/B variant definition
- Success metric (CTR / conversion / no-result rate)
- Sample size + duration target
- Stop conditions

## Rollback
- How to revert if the variant underperforms

## Owner / review date
```

### Synonym proposal

```
# Synonym Group: [Anchor term]

## Members
[term1, term2, term3]

## Type
multi-way | one-way | alt-correction

## Justification
- Search Analytics evidence
- Editorial / brand justification
- Why a synonym, not a query rule

## Risks
- Where this could over-match
```

## Common anti-patterns

- **Tuning without Insights.** You can't tell if the change worked.
- **Multiple changes at once in an A/B.** You can't tell *which* worked.
- **Pinning to fix a relevance bug.** Pinning hides the real problem and rots over time. Use rules for merchandising, fix relevance with searchable attributes, custom ranking, or synonyms.
- **`customRanking: ["desc(popularity)"]` without populating popularity.** Custom ranking on a missing attribute is a no-op; the query feels broken and nobody knows why.
- **Touching the master `ranking` formula on day 1.** Almost always wrong. Investigate other levers first.
- **Synonyms that are really just typos.** Use `altCorrections` for misspellings, not multi-way synonyms.
- **Letting the Dashboard be the source of truth.** Settings drift; nobody knows what's prod. Settings as code.
- **Adding NeuralSearch without an A/B.** It can hurt as well as help; measure.

## Constraints

- Don't ship a tuning change that wasn't validated against Insights data.
- Don't accept "the search is bad" as a problem statement. Ask which queries.
- Don't paste Dashboard screenshots as the deliverable. The deliverable is the settings JSON, the rules JSON, and the analytics that justified them.

## How this skill relates to others

- The records this skill operates on → `algolia-index-design`.
- The Insights events that justify changes → `algolia-analytics-events`.
- AI/semantic levers → `algolia-personalization-ai`.
- The MCP / CLI surface for applying changes → `algolia-mcp-cli`.

## Source material

- Ranking and relevance overview: https://www.algolia.com/doc/guides/managing-results/relevance-overview/
- Searchable attributes: https://www.algolia.com/doc/guides/managing-results/must-do/searchable-attributes/
- Custom ranking: https://www.algolia.com/doc/guides/managing-results/must-do/custom-ranking/
- Typo tolerance: https://www.algolia.com/doc/guides/managing-results/optimize-search-results/typo-tolerance/
- Synonyms: https://www.algolia.com/doc/guides/managing-results/optimize-search-results/adding-synonyms/
- Rules: https://www.algolia.com/doc/guides/managing-results/rules/rules-overview/
- Dynamic Re-Ranking: https://www.algolia.com/doc/guides/algolia-ai/dynamic-re-ranking/
- A/B testing: https://www.algolia.com/doc/guides/ab-testing/what-is-ab-testing/
- NeuralSearch: https://www.algolia.com/doc/guides/algolia-ai/neuralsearch/
- Plugin reference: `../../references/algolia-foundations.md`
- Plugin reference: `../../references/cli-cheat-sheet.md`
