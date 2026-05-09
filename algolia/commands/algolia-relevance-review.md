---
description: Quarterly relevance review using Search Analytics — top searches, no-result queries, low-CTR queries, click position. Proposes synonyms, rules, settings tweaks; defines the A/B plan. Read-only; doesn't apply changes.

# Project context
type: command
project: skills-library
plugin: algolia
tags: [type/command, plugin/algolia]
status: active
---

Use the `algolia-agent` to run a relevance review on an Algolia index. The output is a set of proposed changes (synonyms, rules, settings) with hypotheses and an A/B plan, ready for review.

Cadence: quarterly per index, or on-demand when stakeholders report "search is bad."

Pre-flight: confirm the index has been live with Insights events flowing for ≥ 30 days; scoped read-only key with `analytics`, `search`, `browse`, `listIndexes`.

Run the six stages. Each is anchored to the matching skill.

1. **Search Analytics intake** (`algolia-analytics-events`) — pull from the last 30–90 days:
   - Top 50 searches with click-through rate and conversion rate
   - Top 20 searches with **no results** — the cleanest fix candidates
   - Top 20 searches with **clicks but no conversions** — UI or content issue, not relevance
   - Top 20 searches with **high mean click position** (>3) — relevance is reordering away from position 1
   - Click position distribution across the top 200 queries

2. **Triage** — for each candidate query, classify:
   - **Synonym gap** (the index has the right record; the query just doesn't match it)
   - **Content gap** (no record exists; escalate to the content team via the contentful agent)
   - **Ranking issue** (records exist and match, but the wrong one ranks first)
   - **UI/UX issue** (relevance is fine; the SERP is failing the user)
   - **Discovery anti-pattern** (the query reveals the user expected a different mental model — informs IA, not search)

3. **Synonym proposals** (`algolia-relevance-tuning`) — for synonym gaps, draft each as multi-way / one-way / alt-correction with the justification. Group related synonyms; avoid one-off entries. Format per `algolia-relevance-tuning` output spec.

4. **Rule proposals** — for merchandising or seasonal asks (pinning, redirects, banners), draft the rules with conditions, consequences, time bounds, and ownership. Stale rules from prior reviews get retirement candidates surfaced too.

5. **Settings tweaks** — only when the synonym/rule lever doesn't fit:
   - `searchableAttributes` reorder if a strong-signal attribute is too far down
   - `customRanking` add if a popularity / freshness signal isn't yet weighted
   - `typoTolerance` adjustment for SKU / brand contamination
   - `relevancyStrictness` if Dynamic Re-Ranking is misbehaving

6. **A/B plan** — for each proposed change, define:
   - Variant (settings change, new rule, NeuralSearch on/off)
   - Success metric (CTR, conversion, mean click position, no-result rate)
   - Sample-size target (≥ 1000 conversion events per arm; longer if low-traffic)
   - Duration target (≥ 14 days)
   - Stop conditions (significance reached, regression detected)
   - Rollback procedure

Produce the deliverable as a single document the team can review:

```
# Relevance Review — {index} — {date}

## Headline metrics (delta vs. prior review)

## No-result queries (synonym fixes proposed)

## Low-CTR queries (root-cause classified)

## High mean-click-position queries (ranking fixes proposed)

## Synonym proposals (full list with justification)

## Rule proposals (additions, retirements)

## Settings tweaks (with hypothesis)

## A/B plan (per change, ready to wire)

## Stale-rule retirement candidates

## Open items (escalations to contentful agent, IA team, content team)
```

Save to `Runbooks/algolia-relevance-review-{index}-{date}.md` or the equivalent engagement notes location.

Don't apply changes during the review. The output is a proposal package for human approval and CI deploy.

If a change requires user data not in the analytics surface (e.g., qualitative feedback), call it out as an open item; don't fabricate justification.

If the relevance issues turn out to be content gaps (>30% of triage), pause the review and escalate to the contentful agent for a content-side investigation. Search can't compensate for missing content.
