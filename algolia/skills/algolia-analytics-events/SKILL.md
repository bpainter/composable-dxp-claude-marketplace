---
name: algolia-analytics-events
description: >
  Insights events and search analytics — designing the event taxonomy (eventType, eventName,
  objectIDs, queryID, userToken), wiring `view`, `click`, and `conversion` events from React /
  Next.js, the Insights middleware in InstantSearch, attributing search-driven revenue, A/B
  test setup, reading Search Analytics (top searches, no-result queries, click position, CTR by
  query), and the patterns that make every other AI feature work. Use this skill any time
  events are the topic — first integration, fixing missing attribution, debugging
  Personalization, evaluating relevance changes, or building the analytics dashboard for the
  engagement.

# Project context
type: skill
project: skills-library
plugin: algolia
aliases: [algolia-analytics-events]
tags: [type/skill, plugin/algolia, topic/algolia, topic/analytics]
status: active
---

This skill puts you in the role of an engineer who treats search analytics as production infrastructure, not an afterthought. Default posture: **Insights events are the substrate that everything else stands on. Wire them on day one or accept that Recommend, Personalization, A/B, and Search Analytics will be flying blind.**

Pair with `algolia-instantsearch-react` (the click/view middleware), `algolia-autocomplete` (the Insights plugin), `algolia-relevance-tuning` (analytics drives every relevance change), and `algolia-personalization-ai` (Personalization is downstream of these events).

## When to use this skill

- First Algolia integration — what events to send, what to name them, what to attribute.
- Insights is wired but Personalization isn't working — usually a `userToken` mismatch.
- Recommend models aren't returning results — usually missing or low-volume events.
- A/B test setup — defining the success metric, ensuring events tag the variant.
- Building the Search Analytics dashboard for an engagement — what to monitor weekly.
- Debugging an "all our search is broken" report — Search Analytics is the first place to look.

## Core posture

- **Three event types only.** `view`, `click`, `conversion`. Don't invent more.
- **Event names are the analytics taxonomy.** Pick a small set; document them; don't proliferate.
- **`queryID` is the attribution key.** Without it, click and conversion can't be tied to the search.
- **`userToken` must be stable and consistent.** The same token across search, click, view, conversion, Personalization. Otherwise nothing connects.
- **Server events for important conversions.** Front-end events get blocked by ad blockers; conversions worth real money should fire server-side too.
- **Privacy first.** Never use PII as `userToken`. Document the user-tracking posture.

## The three event types

### `view`

Fired when a record is rendered to a user.

```ts
insightsClient('viewedObjectIDs', {
  index: 'articles',
  eventName: 'Articles Viewed',
  userToken: 'user-abc',
  objectIDs: hits.map((h) => h.objectID),
});
```

Use for:
- Hits rendered in a SERP (typically only the visible ones, not all 1000 nbHits).
- Detail-page views.
- Recommend widget impressions.

Don't use for:
- Every render of every component (waste).
- Off-screen content.

### `click`

Fired when the user clicks a record.

```ts
insightsClient('clickedObjectIDsAfterSearch', {
  index: 'articles',
  eventName: 'Article Clicked',
  userToken: 'user-abc',
  objectIDs: [hit.objectID],
  positions: [hit.__position],     // 1-indexed position in the SERP
  queryID: hit.__queryID,          // attribution to the search
});
```

For non-search clicks (e.g., clicking a Recommend item):

```ts
insightsClient('clickedObjectIDs', {  // no queryID — not a search
  index: 'articles',
  eventName: 'Recommended Article Clicked',
  userToken: 'user-abc',
  objectIDs: [hit.objectID],
});
```

The two API names matter — `*AfterSearch` carries `queryID`; the other doesn't. Pick the right one.

### `conversion`

Fired when the user does the meaningful thing.

```ts
// Search-attributed conversion
insightsClient('convertedObjectIDsAfterSearch', {
  index: 'articles',
  eventName: 'Whitepaper Downloaded',
  userToken: 'user-abc',
  objectIDs: [hit.objectID],
  queryID: hit.__queryID,
});

// Commerce conversion with revenue
insightsClient('purchasedObjectIDsAfterSearch', {
  index: 'products',
  eventName: 'Order Placed',
  userToken: 'user-abc',
  objectIDs: orderItems.map((i) => i.objectID),
  queryID: lastQueryID,
  objectData: orderItems.map((i) => ({
    queryID: lastQueryID,
    price: i.price,
    quantity: i.quantity,
    discount: i.discount,
  })),
  currency: 'USD',
});
```

Conversion is whatever the engagement defines as success: download, signup, purchase, "request a meeting." Pick 1–3 conversions per surface and instrument them.

## Event taxonomy — pick a small set

Don't let `eventName` proliferate. A taxonomy that grows past ~25 distinct names becomes unanalyzable. Sample:

| eventName | Type | When |
|---|---|---|
| Articles Viewed | view | SERP render of articles |
| Article Clicked | click | User clicks an article hit |
| Article Read | conversion | Scrolled past 50% on the article |
| Whitepaper Downloaded | conversion | User downloaded a whitepaper |
| Search Submitted | conversion (no objectIDs, but tracked) | User pressed enter on the search box |
| Recommended Item Clicked | click | Click on a Recommend widget item |
| Cart Item Added | conversion (commerce) | addToCart |
| Order Placed | conversion (commerce) | Purchase |

Document the taxonomy in the engagement's analytics doc. New events go through review.

## `userToken` discipline

The `userToken` ties everything together. Rules:

- **Stable per identity** across sessions (cookie or auth-derived).
- **Same value** on the search request, the click event, the conversion event, and any Personalization read.
- **Not PII.** Hash any identifying info; use `user-{hash}`.
- **Consistent for anonymous users** — assign a stable cookie ID; persist across sessions.
- **Reset on logout** — or carry the auth-derived token only while logged in; switch back to the cookie ID after.

For multi-tenant: include the tenant in or derive from the token (`user-${tenantHash}-${userHash}`) to avoid cross-tenant Personalization bleed.

## InstantSearch + Insights wiring

```tsx
import { InstantSearchNext } from 'react-instantsearch-nextjs';
import { Configure } from 'react-instantsearch';

<InstantSearchNext
  indexName="articles"
  searchClient={client}
  insights={true}
  routing
>
  <Configure clickAnalytics />
  ...
</InstantSearchNext>
```

`insights={true}` enables the middleware. `clickAnalytics` on `<Configure>` ensures every search response includes `queryID`. The middleware automatically:

- Sends `view` events for hits as they render.
- Sends `click` events when the user clicks a hit.
- Carries `queryID` and `userToken` (from `setUserToken`).

For setting `userToken`:

```tsx
const insights = useInsights();

useEffect(() => {
  insights('setUserToken', userToken);
}, [userToken, insights]);
```

Or via the InstantSearch root:

```tsx
<InstantSearchNext
  insights={{ insightsInitParams: { useCookie: true } }}
  ...
>
```

`useCookie: true` lets Algolia generate a cookie-based anonymous token. Reasonable default for sites that haven't built their own.

## Conversions are usually code you write

InstantSearch instruments view + click. Conversion is the engagement's job:

```tsx
// On a download link
import { useInsights } from 'react-instantsearch';

function DownloadButton({ hit }: { hit: ArticleHit }) {
  const insights = useInsights();

  function onClick() {
    insights('convertedObjectIDsAfterSearch', {
      eventName: 'Whitepaper Downloaded',
      index: 'articles',
      objectIDs: [hit.objectID],
      queryID: hit.__queryID,
    });
    // continue with download
  }

  return <button onClick={onClick}>Download</button>;
}
```

For server-side conversions (form submit, purchase webhook), call the Insights REST API directly:

```ts
await fetch('https://insights.algolia.io/1/events', {
  method: 'POST',
  headers: {
    'X-Algolia-Application-Id': APP_ID,
    'X-Algolia-API-Key': SEARCH_KEY,
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    events: [
      {
        eventType: 'conversion',
        eventName: 'Order Placed',
        index: 'products',
        userToken: order.userToken,
        objectIDs: order.items.map((i) => i.objectID),
        queryID: order.lastQueryID,  // captured during checkout
        timestamp: Date.now(),
      },
    ],
  }),
});
```

Server events always fire (no ad-blocker risk). Most engagements should mirror critical conversions server-side.

## Capturing `queryID` through a multi-page funnel

The user searches on `/search`, clicks a hit, lands on `/articles/{slug}`, then clicks "request a demo," which submits a form. The conversion needs to know the original `queryID`.

Pattern:

1. On click, the InstantSearch middleware fires the `click` event (good). It also stores the `queryID` somewhere — a session cookie or query param.
2. The detail page reads the `queryID` (from `?queryID=...` or cookie).
3. The form submission carries the `queryID` to the server.
4. The server fires `conversion` with that `queryID`.

```tsx
// On clicking a hit — add queryID to the URL
<a href={`${hit.url}?queryID=${hit.__queryID}`}>{hit.title}</a>

// On the detail page
const params = useSearchParams();
const queryID = params.get('queryID');
sessionStorage.setItem('lastSearchQueryID', queryID || '');

// On form submit
const queryID = sessionStorage.getItem('lastSearchQueryID');
await fetch('/api/lead', { method: 'POST', body: JSON.stringify({ ..., queryID }) });
```

If the funnel is long (>30 minutes between search and conversion), the attribution gets fuzzier. Algolia stops attributing conversions after 24 hours by default.

## Search Analytics — what to read weekly

The Dashboard's Analytics surface (or the Analytics API for piping elsewhere):

| Metric | What it tells you |
|---|---|
| **Top searches** | What people are looking for. Compare week-over-week. |
| **Top searches with no results** | Synonym gaps or content gaps. Both fixable. |
| **Click-through rate (CTR)** | Per-query CTR — low CTR on a frequent query means the SERP isn't compelling. |
| **No-click rate** | Percentage of searches with zero clicks — a different lens than no-results. |
| **Mean click position** | Higher = relevance is bad. Top hit isn't the best hit. |
| **Top filters** | What facets people use. Informs UI emphasis. |
| **Conversion rate** | Per-query conversion. Where the SERP earns its place. |
| **Revenue per search** (commerce) | The money number. |

Set up a weekly review:

- Top 50 searches: do they all return reasonable results?
- Top 20 no-result searches: any synonym we should add?
- Top 20 low-CTR searches: relevance issue, content issue, or UI issue?
- Trend lines on the headline metrics: are they moving the right way?

## A/B testing + analytics

A/B tests in Algolia tag every search and event with a variant ID. Analytics shows per-variant metrics. To set up:

1. Define the variant — different index or `customSearchParameters`.
2. Set traffic split.
3. Pick the success metric (CTR, conversion, revenue per search).
4. Run for ≥ 14 days, ≥ 1000 conversions per arm.
5. Read the significance in the Dashboard.

The tests need conversion events to be meaningful. CTR alone is a weaker signal — hits can be clickbait.

## Privacy posture

Insights events are **first-party tracking**. The engagement should:

- Disclose in the privacy notice.
- Honor "do not track" or equivalent (skip events for opted-out users).
- Use anonymous tokens for unauthenticated users; document persistence.
- Provide a deletion path (`DELETE /1/profiles/{userToken}` for Personalization profiles).
- For EU users, ensure GDPR posture is reviewed.

For high-privacy engagements (legal, healthcare), consider:
- Session-only tokens (no persistence) — Personalization quality drops; accept the trade-off.
- Server-only event sending — front-end events go through your server, which scrubs IP and other identifiers before forwarding.

## Output formats

### Event taxonomy spec

```
# Algolia Insights Taxonomy: [Engagement]

## userToken policy
- Source (auth ID hash, cookie, session)
- Persistence
- Reset triggers
- Privacy disclosure

## Events
| eventName | eventType | Surface | Trigger | objectIDs | queryID | userData |
|-----------|-----------|---------|---------|-----------|---------|----------|
| Articles Viewed | view | SERP | Hit rendered | yes | — | — |
| Article Clicked | click | SERP | Hit clicked | yes | yes | — |
| Article Read | conversion | Detail page | Scroll > 50% | yes | yes (from URL) | — |
| Whitepaper Downloaded | conversion | Detail page | Download click | yes | yes | — |
| ...

## Server-side mirrors
- Which conversions also fire server-side
- Implementation reference

## Privacy compliance
- Disclosure path
- Opt-out behavior
- Deletion path
```

### Weekly analytics review template

```
# Search Analytics Weekly Review — Week of YYYY-MM-DD

## Headline metrics (delta vs. prior week)
- Searches: 12,400 (+8%)
- CTR: 41% (-2pp)        ⚠
- Conversion rate: 3.1% (=)
- No-result rate: 4.2% (-0.5pp)

## Top searches
[paste top 20]

## Top no-result searches (action items)
- "summit risk gates" — 42 searches, 0 results
  → Action: add synonym `summit, slalom-summit`
- "delivery quality review" — 28 searches, 0 results
  → Action: confirm content exists; if not, content gap

## A/B tests in flight
- [name] — [status]

## Recommendations to ship
- [synonym, rule, settings change]
```

## Common anti-patterns

- **Skipping Insights at MVP.** Then everything else is broken.
- **`userToken` mismatch** between search and event.
- **PII in `userToken`** (email, account ID).
- **Front-end-only conversion events.** Ad blockers eat them; revenue attribution is unreliable.
- **Too many `eventName` values.** Taxonomy entropy.
- **No `queryID` on click events.** No attribution to search.
- **Sending events for off-screen hits.** View counts inflate, model training noise increases.
- **Not reviewing analytics weekly.** Drift goes unnoticed.
- **Using Insights events as the only analytics.** Pair with GA4 / Segment for full-funnel.

## Constraints

- Don't ship an Algolia integration without Insights.
- Don't propose Personalization, Recommend, or A/B testing without the events to support them.
- Don't define a `userToken` policy without privacy review.
- Don't paraphrase an event taxonomy in chat — write it into the engagement doc.
- Don't depend on front-end events alone for revenue-attributable conversions.

## How this skill relates to others

- The widgets that the Insights middleware instruments → `algolia-instantsearch-react`, `algolia-autocomplete`.
- Models that train on these events → `algolia-recommend`, `algolia-personalization-ai`.
- Tuning informed by these analytics → `algolia-relevance-tuning`.
- API surface for sending and reading events → `../../references/api-surface.md`.
- Privacy controls and key permissions → `algolia-api-keys-security`.

## Source material

- Insights overview: https://www.algolia.com/doc/guides/sending-events/getting-started/
- Insights API: https://www.algolia.com/doc/rest-api/insights/
- search-insights library: https://www.npmjs.com/package/search-insights
- Click and conversion analytics: https://www.algolia.com/doc/guides/search-analytics/concepts/click-and-conversion-analytics/
- A/B testing: https://www.algolia.com/doc/guides/ab-testing/what-is-ab-testing/
- Plugin reference: `../../references/algolia-foundations.md`
- Plugin reference: `../../references/api-surface.md`