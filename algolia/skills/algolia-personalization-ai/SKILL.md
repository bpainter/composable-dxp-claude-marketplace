---
name: algolia-personalization-ai
description: >
  AI and personalization on Algolia — Personalization (affinity-based per-user re-ranking),
  Dynamic Re-Ranking (auto-tuned by similar-query click signals), NeuralSearch (vector +
  keyword hybrid retrieval, Premium plan), Query Categorization (intent classification on the
  query), AI Browse (conversational discovery), and how the pieces compose. Covers when each
  earns its cost, the data prerequisites, the rollout posture (always A/B), and the specific
  points where AI-on-search makes things worse, not better. Use this skill when planning the AI
  surface for an engagement, debugging weird results after enabling NeuralSearch, or evaluating
  whether a feature is worth its plan tier.

# Project context
type: skill
project: skills-library
plugin: algolia
aliases: [algolia-personalization-ai]
tags: [type/skill, plugin/algolia, topic/algolia, topic/ai, topic/personalization]
status: active
---

This skill puts you in the role of an engineer evaluating which AI features actually earn their cost on a given engagement. Default posture: **AI-on-search is plan-gated, training-gated, and easy to ship without measuring. Treat every enable as an A/B test.**

Pair with `algolia-analytics-events` (Insights is the substrate everything trains on), `algolia-relevance-tuning` (the non-AI levers that should be exhausted first), `algolia-recommend` (Recommend is its own AI surface), and `algolia-search-client` (where the per-query parameters live).

## When to use this skill

- Planning the AI surface for a new engagement — what to enable, when, and at what plan tier.
- Stakeholder asks: "Should we turn on NeuralSearch?"
- Personalization is enabled but search results don't feel personalized.
- NeuralSearch was enabled and now common queries return weird matches.
- Evaluating Premium plan upgrade for the AI features.
- Debugging Personalization profiles or Query Categorization output.

## Core posture

- **AI is not a replacement for relevance tuning.** It compounds with it. Records, settings, and synonyms still matter.
- **Every AI enable is a hypothesis.** A/B against the previous configuration; ship the winner; document the loser.
- **Insights events are the substrate.** No events → no Personalization, no NeuralSearch quality signal, no A/B significance.
- **Plan gating is real.** Personalization needs Grow+. NeuralSearch and AI Browse need Premium. Confirm plan before scoping.
- **AI features can hurt high-precision queries.** Brand searches, SKU lookups, exact-match needs — semantic re-ranking can pull these off course. Use settings to scope where AI applies.

## The features, by plan and substrate

| Feature | Plan | Substrate | What it does |
|---|---|---|---|
| **Dynamic Re-Ranking** | All paid | Click events on the same query patterns | Re-orders top-N results based on similar-query click data. Auto-tuned. |
| **Personalization** | Grow+ | Insights events with `userToken` | Re-ranks per user based on event affinities (categories, brands, facets they engage with). |
| **NeuralSearch** | Premium | None to enable; Insights to evaluate | Hybrid vector + keyword retrieval. Broadens recall on conceptual queries. |
| **Query Categorization** | All paid | The query text + index facets | Classifies the query into a category, returned as `userData` in the response. |
| **AI Browse** | Premium | The index | Conversational browsing — natural-language refinement on a category page. |

### Dynamic Re-Ranking — the cheapest win

Already on by default for most paid plans (verify in Dashboard → AI → Dynamic Re-Ranking). The model learns from click data on similar queries — if users repeatedly click position 3 over position 1 for a query, position 3 floats up.

Tuning via `relevancyStrictness` (0–100):

- 0 = re-ranking disagrees with textual relevance freely.
- 100 = textual relevance wins; re-ranking is suppressed.
- Default: ~70 in most cases.

When to touch it:
- High-precision queries are getting reordered into worse positions → bump strictness up.
- Long-tail queries lack signal → leave strictness lower.

This usually doesn't need an A/B; it's auto-tuned. Just observe the analytics.

### Personalization — affinity-based re-ranking

Personalization builds a profile per `userToken` from Insights events. The profile is "this user has engaged with `topic:composable` 12 times, `audience:executive` 4 times." On search, Personalization re-ranks records that match those facets higher for that user.

Setup:

1. Algolia Dashboard → AI → Personalization → Strategy.
2. Pick which facets contribute to the profile (e.g., `topic`, `audience`, `category`).
3. Set per-event weights (e.g., `conversion: 1.0`, `click: 0.5`, `view: 0.1`).
4. Save.

Per-query usage:

```ts
client.search({
  requests: [{
    indexName: 'articles',
    query,
    enablePersonalization: true,
    userToken: `user-${userId}`, // stable per-user identifier
    personalizationImpact: 50,    // 0–100; how strongly to re-rank
  }],
});
```

The `userToken` must:
- Be stable across sessions (cookie or auth-derived).
- NOT be PII. Use `user-${hash(userId)}` not `user-${email}`.
- Match the `userToken` used when sending Insights events. (The same token writes the profile and reads it.)

Anonymous users can have Personalization too — assign a stable cookie ID as the `userToken`. The profile is anonymous but coherent across sessions on the same device.

Personalization works best when:
- Users have ≥ 5–10 events of history.
- Facets are meaningful (5–20 values, not 200).
- The catalog is large enough that re-ranking changes meaningful order.

Personalization works poorly when:
- Sparse user history.
- Catalog is small enough that re-ranking has nothing to do.
- Queries are highly specific (the textual relevance is already correct).

### NeuralSearch — vector + keyword hybrid

NeuralSearch (Premium) layers a vector retrieval pass over the keyword retrieval and merges. Two effects:

- **Recall improves on conceptual queries** — "how do I onboard a new manager" matches articles titled "first-90-days plan" without an explicit synonym.
- **Tail queries improve** — natural-language phrasings that don't share vocabulary with the records.

Per-query usage:

```ts
client.search({
  requests: [{
    indexName: 'articles',
    query,
    mode: 'neuralSearch',
  }],
});
```

Index settings:

```json
{
  "mode": "neuralSearch"  // index-level default; per-query mode overrides
}
```

When NeuralSearch hurts:

- **Brand and SKU queries.** "Slalom Summit" should return the literal pages; semantic can pull in similar-brand-name pages.
- **High-precision domains** (legal, medical). False positives are expensive.
- **Short queries with strong intent** (one or two words pointing at a known thing).

Mitigations:

- **Per-attribute control** — `searchableAttributes` order still applies. Strong-signal attributes (title, brand, SKU) lead.
- **Per-query mode flip** — for known precision queries, force `mode: 'keyword'`.
- **A/B before going on-by-default.** Run for ≥ 2 weeks. Compare CTR + conversion + zero-result rate.

Roll out NeuralSearch as **opt-in per query** first, measure, then default.

### Query Categorization

Classifies the query into a top-level category (or facet value) using a model trained on your index facets. Returns the category in the response's `userData`.

Use cases:
- **Routing** — search query for "running shoes" classified as `category:shoes` triggers a category-page redirect.
- **Refinement seeding** — pre-filter the SERP based on the predicted category.
- **Analytics** — segment search queries by intent.

Setup in Dashboard. No per-query parameter needed; categorization is automatic when enabled.

For multi-category catalogs (commerce, large content libraries), this is high-leverage. For single-domain content (e.g., a documentation site), the lift is smaller.

### AI Browse

Premium-only. Conversational browsing — instead of clicking through facets, the user asks "show me articles from 2025 about composable architecture for retail clients." AI Browse parses the question and applies the matching filters.

Setup is per-index in the Dashboard. Front-end integration uses InstantSearch widgets that expose AI Browse state.

Where it earns its place: large catalogs where faceting is too granular for casual users. Where it doesn't: small catalogs where the navigation is already simple.

Treat as experimental on most engagements until the team has Insights data showing browse-over-search behavior dominates.

## How they compose

```
Query
  │
  ▼
Keyword retrieval ──┐
                    ├─→ Hybrid merge (NeuralSearch)
Vector retrieval  ──┘
  │
  ▼
Settings ranking (typo, words, custom ranking, ...)
  │
  ▼
Dynamic Re-Ranking (per-query history)
  │
  ▼
Personalization (per-user history, if userToken)
  │
  ▼
Final hits
```

Each stage can be tuned independently. Most engagements should:

1. Get keyword retrieval + settings ranking right (`algolia-relevance-tuning`).
2. Wire Insights (`algolia-analytics-events`).
3. Let Dynamic Re-Ranking auto-tune.
4. Add Personalization once user history exists.
5. Evaluate NeuralSearch as an A/B once #1–4 are stable.

Don't enable everything on Day 1.

## GDPR / right to be forgotten

Personalization profiles are tied to `userToken`. To delete:

```bash
curl -X DELETE \
  https://personalization.algolia.com/1/profiles/{userToken} \
  -H "X-Algolia-API-Key: {admin-key}" \
  -H "X-Algolia-Application-Id: {app-id}"
```

This deletes the profile but doesn't delete the underlying Insights events. To also delete the events, contact Algolia support — there's no public DELETE for events.

For engagements with strict privacy posture: avoid persistent `userToken` for unauthenticated users; use session-only IDs, accept the lower-quality Personalization. Document the trade-off.

## Output formats

### AI rollout plan

```
# Algolia AI Rollout: [Engagement]

## Plan tier confirmed
- Plan: [Build / Grow / Premium]
- Features available
- Features needing upgrade

## Rollout sequence
1. Dynamic Re-Ranking — verify enabled, observe via analytics
2. Personalization — wire userToken; configure facets + weights
3. Query Categorization — enable; route as needed
4. NeuralSearch — A/B per index
5. AI Browse — evaluate on largest catalog

## Per-feature plan
For each:
- What it does on this engagement
- Substrate (events, time-to-train, plan)
- Setup steps
- A/B test design
- Success metric + threshold
- Rollback plan

## Privacy posture
- userToken policy
- Right-to-be-forgotten path
- Documentation in privacy notice

## Cost
- Plan upgrade required (if any)
- Per-feature billable units
```

### Personalization strategy spec

```
# Personalization Strategy: [Index]

## Facets (per-event affinity contribution)
| Facet     | Type     | Min count to consider | Notes                           |
|-----------|----------|------------------------|---------------------------------|
| topic     | string[] | 3                      | Primary affinity                |
| audience  | string[] | 2                      |                                 |
| author_id | string   | 5                      | Strong signal for "fan" pattern |

## Event weights
| Event       | Weight |
|-------------|--------|
| conversion  | 1.0    |
| click       | 0.4    |
| view        | 0.1    |

## Per-query parameters
- enablePersonalization: true
- personalizationImpact: 50 (start; tune later)
- userToken: stable cookie or hashed user ID

## Anonymous handling
- Cookie-derived userToken
- Reset on logout

## Validation
- Profile inspection endpoint for support
- A/B vs. non-personalized for 30 days
```

## Common anti-patterns

- **Enabling NeuralSearch globally on Day 1.** Causes regression on precision queries, hard to debug. Roll out per-query, measure, then default.
- **Personalization without Insights.** Profile stays empty.
- **Using PII as `userToken`.** Email, full name, account ID. Hash it.
- **Different `userToken` between Insights and search.** Profile writes to one, reads from another. Personalization silently does nothing.
- **No A/B on AI features.** Can't quantify the lift; can't tell when it's hurting.
- **Treating AI as a replacement for tuning.** Bad records and bad settings make AI worse, not better.
- **Forgetting privacy posture.** Persistent `userToken` for anonymous users is a tracking decision; document and disclose.
- **No fallback when AI Browse misfires.** A wild filter combination from a bad parse → user sees zero results, no idea why.

## Constraints

- Don't promise AI features that the active plan doesn't include.
- Don't ship Personalization without a stable `userToken` and matching Insights events.
- Don't ship NeuralSearch without an A/B and a per-query escape hatch.
- Don't bake editorial logic into Personalization weights. Use rules for that.
- Don't expose admin-level Personalization API endpoints to the front end.

## How this skill relates to others

- The events that everything trains on → `algolia-analytics-events`.
- The non-AI levers to exhaust first → `algolia-relevance-tuning`.
- The Recommend surface (its own AI features) → `algolia-recommend`.
- Per-query parameters at the client → `algolia-search-client`.
- The InstantSearch widgets that expose Personalization state → `algolia-instantsearch-react`.

## Source material

- Personalization overview: https://www.algolia.com/doc/guides/algolia-ai/personalization/
- Dynamic Re-Ranking: https://www.algolia.com/doc/guides/algolia-ai/dynamic-re-ranking/
- NeuralSearch: https://www.algolia.com/doc/guides/algolia-ai/neuralsearch/
- Query Categorization: https://www.algolia.com/doc/guides/algolia-ai/query-categorization/
- AI Browse: https://www.algolia.com/doc/guides/algolia-ai/ai-browse/
- Personalization API: https://www.algolia.com/doc/rest-api/personalization/
- Plugin reference: `../../references/algolia-foundations.md`
- Plugin reference: `../../references/api-surface.md`