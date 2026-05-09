---
name: contentful-personalization
description: Implement Contentful Personalization (the former Ninetailed product, now first-party) — design audiences, structure experiences with baseline + variant entries, run A/B tests, integrate the Experience API at the edge or in server components, and instrument analytics for measurable results. Use this skill any time audience-based personalization, A/B testing, or experiment-driven content variants come up; when "Ninetailed" is referenced; when a stakeholder asks "can we show different content to logged-in users / different segments / by geography / by referrer"; or when an existing personalization rollout is being extended. Trigger whenever the question is "how do we vary content per audience without forking the model."

# Project context
type: skill
project: skills-library
plugin: contentful
aliases: [contentful-personalization]
tags: [type/skill, plugin/contentful, topic/contentful, topic/personalization, topic/experimentation]
status: active
---

# Contentful Personalization

Contentful acquired Ninetailed in 2024; the personalization product is now first-party. This skill covers the working model — audiences, experiences, baseline/variant entries, the Experience API runtime, and the front-end integration patterns. The product is moving fast; verify any specific feature against current docs (https://www.contentful.com/developers/docs/personalization/).

Pair with `contentful-content-model` (variant patterns shape the model), `contentful-react-wrapper` (variant fetching at the edge or in server components), `contentful-graphql` (variant-aware queries), `contentful-delivery-optimization` (variant-keyed caching), and `contentful-app-framework` (the Personalization UI is itself an app).

## When to use this skill

- Standing up personalization on a new or existing Contentful site.
- A stakeholder asks for "show different hero by audience."
- Running an A/B or multivariate test.
- Migrating from Ninetailed-as-a-vendor to Contentful-native Personalization.
- Adding new audiences or experiences to a live program.
- Diagnosing a personalization decision that "isn't firing."

## Core posture

- **Personalization belongs in the model, not in the page.** Define a `Variant` pattern; the page references the experience, the experience references variants. Don't hand-roll IF/ELSE in the front end.
- **Edge or server, never client-only.** Client-side variant flicker is a worst-case experience. Decide at the edge function or in a server component, render once.
- **Default to baseline, not blanks.** Every experience has a baseline (what un-targeted visitors see). Variants are deliberate divergences.
- **Audience criteria should be inspectable.** A complex composed audience that nobody can describe in a sentence is a failed audience.
- **Measure or remove.** Every active experience produces a primary metric. If the metric isn't being read, retire the experience.

## Domain vocabulary

| Term | What it is |
|---|---|
| **Audience** | A rule that evaluates to true/false for a given visitor profile (geography, device, UTM, custom traits, segment from CDP) |
| **Experience** | A targeting unit — pairs a baseline content reference with one or more variant content references and a set of audiences |
| **Variant** | The content shown to a visitor matched to a specific audience |
| **Baseline** | The content shown when no variant matches |
| **Profile** | The visitor's resolved set of attributes used for evaluation |
| **Decision** | The runtime resolution of which variant to render, returned by the Experience API |

## Audience design

Audiences fall into a few categories:

- **Behavioral** — pages visited, events fired, recency.
- **Demographic / firmographic** — country, language, company size if you've passed it in.
- **Source** — UTM parameters, referrer, campaign id.
- **Identity-based** — logged-in vs. anonymous, customer tier, segment from a CDP.
- **Device / context** — mobile vs. desktop, time of day.

Naming conventions:

- Lowercase-kebab-case for audience IDs (`founders-pre-seed`, `enterprise-buyer`).
- Descriptive, segment-only names. Don't bake metric expectations into the name.
- One sentence describing the audience in the help-text field.

## Experience patterns

Three model patterns, in order of preference:

### 1. Reference-style variants (recommended)

The page references an `Experience` entry. The `Experience` references a baseline entry and a set of variant entries. The front end evaluates the experience and renders the chosen entry.

```
[Page]
  └── hero (Reference: Experience)
        ├── baseline (Reference: Hero)
        ├── variants (Array<{audience, content: Reference: Hero}>)
        └── primaryMetric (Symbol — name of the conversion event)
```

Pros: model stays clean; the page doesn't know which variant rendered; analytics can attribute by reading the decision metadata.

### 2. Inline variants on the consumer

Experiences live as a field on the consuming entry rather than as a standalone reference type.

```
[Hero]
  └── personalization (single field — JSON or reference list)
```

Use when there are very few experiences and they're tightly coupled to the consumer. Discouraged at scale.

### 3. Multi-variant pages

Two parallel `Page` entries — `home-baseline`, `home-variant-enterprise` — selected by a routing rule.

Use only when the variants are so divergent that a single page can't reasonably reference both. Comes with maintenance overhead — every change has to land in both pages.

## The Experience API runtime

The Experience API takes a visitor profile + a list of experiences and returns decisions. Pseudocode:

```
POST https://experience-api.contentful.com/v1/decisions
{
  "profileId": "anon-abc-123",
  "experienceIds": ["exp-home-hero", "exp-pricing-cta"],
  "traits": { "country": "US", "logged_in": false, "utm_campaign": "spring" }
}

→ {
  "decisions": {
    "exp-home-hero": {
      "variantId": "variant-enterprise",
      "audienceId": "enterprise-buyer",
      "isControl": false
    },
    "exp-pricing-cta": {
      "variantId": "baseline",
      "audienceId": null,
      "isControl": true
    }
  }
}
```

(Verify the exact shape and endpoint against current Personalization docs; the product is post-acquisition and consolidating.)

## Integration patterns

### Edge function (best for marketing pages)

```ts
// middleware.ts (Next.js App Router)
import { NextResponse, NextRequest } from "next/server";

export async function middleware(req: NextRequest) {
  const profileId = req.cookies.get("ctp_pid")?.value ?? generateId();
  const decisions = await resolveExperiences(profileId, req);
  const res = NextResponse.next();
  res.cookies.set("ctp_pid", profileId, { httpOnly: false, secure: true });
  res.headers.set("x-ctp-decisions", JSON.stringify(decisions));
  return res;
}
```

Server component reads the header:

```tsx
import { headers } from "next/headers";

export default async function Page() {
  const decisions = JSON.parse(headers().get("x-ctp-decisions") ?? "{}");
  const heroEntryId = decisions["exp-home-hero"]?.variantId ?? BASELINE_HERO_ID;
  return <HeroFromContentful id={heroEntryId} />;
}
```

### Server component direct call

For pages where edge middleware is overkill:

```tsx
export default async function Page() {
  const profileId = await getOrCreateProfileId();
  const decisions = await resolveExperiences(profileId, ["exp-home-hero"]);
  return <HeroFromContentful id={decisions["exp-home-hero"].variantId} />;
}
```

### Client SDK (when interactivity needs the profile)

The Personalization SDK provides a React hook for client-side decisions when:

- A modal or click-driven element needs to pick a variant on the fly.
- You're capturing client-side events (UTM enrichment, scroll triggers).

Avoid client-only resolution for above-the-fold content; you'll see the baseline-then-variant flicker.

## Caching with personalization

Personalization fragments cache space. Two approaches:

### Variant-keyed cache

Each variant's URL is unique (or each variant carries a tag in its fetch). Vercel caches per variant, the edge function selects which cached page to serve.

```
/                      → baseline (cached)
/?variant=enterprise   → enterprise variant (cached)
/?variant=founder      → founder variant (cached)
```

Pros: cache hit ratio stays high. Cons: URLs branch; SEO has to canonicalize.

### Decision-only at the edge

The page is server-rendered per request, but the Contentful fetches inside it cache by entry id. The first variant render warms the cache; subsequent same-variant visitors are fast.

```
Edge: resolve decision → 200ms
Server render: fetch entry by id (cached) → 50ms
Total: 250ms TTFB, with subsequent variant-matched visitors at 50ms
```

Pros: no URL branching. Cons: edge function on every request.

For Slalom default — the second pattern (decision-only at the edge) for pages with 2–4 variants; variant-keyed for high-traffic pages with stable variant sets.

## A/B testing inside Contentful Personalization

A/B testing is a special case of personalization where:

- Audiences are randomly assigned cohorts (50/50, 33/33/33, etc.).
- Decisions are sticky per profile (a visitor stays in the same cohort across visits).
- A primary metric is tracked.

Modeling pattern is identical to audience-based personalization; the audiences are the cohorts. Set sample size and significance threshold deliberately; the SDK or your analytics layer enforces statistical sanity.

## Analytics integration

Every variant render emits an analytics event:

```ts
// after deciding
analytics.track("contentful_personalization_decision", {
  experienceId: decision.experienceId,
  variantId: decision.variantId,
  audienceId: decision.audienceId,
  isControl: decision.isControl,
  profileId,
});
```

Downstream:

- Plug into Amplitude / Mixpanel / Heap / Segment for funnel analysis.
- Pull experience metadata into BigQuery / Snowflake for long-running analysis.
- Pair with the primary metric event (signup, conversion) to attribute lift.

If your CDP (Segment, mParticle) is feeding traits into Contentful, the same CDP receives the decision events. This is the canonical Slalom CDP-Contentful loop.

## Migration from Ninetailed-the-vendor to first-party

If a project is using legacy Ninetailed SDK:

- The SDK packages are still functional and being consolidated under Contentful.
- New installs should use the Contentful-branded packages; existing code can migrate incrementally.
- Audience definitions may need to be re-imported into the new UI; verify against current migration tooling.

Check current migration guidance before assuming version compatibility.

## Common pitfalls

- **Personalization in the page template, not the model.** A `<Hero variant={isEnterprise ? 'a' : 'b'} />` style ladder in the front end is a maintenance disaster. Always model the experience.
- **Client-side decisions on hero content.** Visible flicker. Move to edge or server.
- **Cache coupling.** A naive cache invalidation on the page-level entry blows away every variant's cache. Tag per variant.
- **Audience proliferation.** Twelve audiences active, three actually move the metric. Quarterly retire-or-promote review.
- **Experiments without primary metrics.** "We turned it on, content was personalized" is not a measurement. Define and track the metric before launch.
- **Profile id generation in places that don't agree.** Edge says `pid-abc`, server says `pid-def`. Decide once, set the cookie at the edge, read it everywhere.
- **PII in profile traits.** Don't pass email, name, or anything that maps to PII through Personalization unless you've vetted it with the privacy review.
- **Personalization on logged-out users without consent banners.** GDPR / CPRA compliance; pair with the consent layer.

## Output formats

### Personalization plan

```
# Personalization Plan: [Surface]

## Goals
{which metrics, which audiences, why}

## Audiences
| ID | One-sentence description | Source of traits |
|---|---|---|

## Experiences
| ID | Surface | Baseline | Variants | Primary metric |
|---|---|---|---|---|

## Model changes (if any)
{new content types or fields needed}

## Integration
- Where decisions resolve: edge / server / client
- Cache strategy: variant-keyed / decision-only
- Analytics: events emitted, downstream consumers

## Rollout
1. {staged rollout}

## Measurement plan
- Sample size, significance, decision criteria
- Review cadence

## Risks
- {flicker, cache, privacy}
```

## How this skill relates to others

- The model patterns variants live in → `contentful-content-model`.
- Variant-aware queries → `contentful-graphql`.
- Where decisions get applied to a render → `contentful-react-wrapper`.
- Caching with variants → `contentful-delivery-optimization`.
- The Personalization UI as an app → `contentful-app-framework`.
- Locale × variant interaction → `contentful-localization`.

## Source material

- Personalization overview: https://www.contentful.com/developers/docs/personalization/overview/
- Core concepts: https://www.contentful.com/developers/docs/personalization/core-concepts/
- Quick start: https://www.contentful.com/developers/docs/personalization/quick-start-guide/
- Experience API: https://www.contentful.com/developers/docs/personalization/experience-api/
- Plugin reference: `../../references/api-surface.md`
