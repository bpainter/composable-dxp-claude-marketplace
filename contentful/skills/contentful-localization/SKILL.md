---
name: contentful-localization
description: Design and operate Contentful localization — choose the right locales and fallback chains, mark fields as localizable correctly, integrate translation workflow apps, query locale-aware via REST and GraphQL, route locales in Next.js, and avoid the cache-fragmentation cliffs that come with multi-locale sites. Use this skill any time multi-locale enters a project; when an existing locale strategy is brittle; when adding a new locale; when "the French site shows English text"; or when the team is debating field-level localization vs. duplicate spaces. Trigger on any localization decision.

# Project context
type: skill
project: skills-library
plugin: contentful
aliases: [contentful-localization]
tags: [type/skill, plugin/contentful, topic/contentful, topic/localization, topic/i18n]
status: active
---

# Contentful Localization

Localization decisions made in week one constrain every model and every page for the life of the platform. Contentful makes the right thing easy if you set up locales and fallback chains correctly; brittle if you don't.

This skill owns locale strategy and operations. Pair with `contentful-content-model` (which fields should be localizable), `contentful-graphql` (locale-aware queries), `contentful-react-wrapper` (locale routing in Next.js), `contentful-migrations` (adding/removing locales is a migration), and `contentful-delivery-optimization` (locale × cache).

## When to use this skill

- Standing up multi-locale on a new project.
- Adding a new locale to an existing space.
- Diagnosing missing-translation behavior.
- Integrating a translation workflow app (Smartling, Phrase, Lokalise).
- Debating field-level localization vs. duplicate-space-per-region.
- Locale × personalization × cache interactions.

## Core posture

- **Locale strategy belongs at the space level.** Adding/removing locales is a space-wide change with editorial implications. Plan upfront.
- **Localization is per field.** Decide which fields localize and which inherit. References almost always inherit; copy localizes.
- **Fallback chains are deliberate.** Don't accidentally have French fall back to German falling back to English.
- **One source-of-truth locale.** Authors create in `en-US` (or whatever is canonical); translation flows from there.
- **Slug rules differ.** A slug field might localize per language but might also stay shared (an article URL like `/2026/04/composable-architecture-2026` is locale-prefixed, not locale-translated). Decide deliberately.
- **Cache by locale.** Vary fetch tags / URLs / headers by locale; otherwise English content shows on French pages.

## Locale design

### Choosing locales

Start with the actual content commitments, not aspirations. Common patterns:

- **Single locale (`en-US`).** Default. Most US-only or English-language-only Slalom builds.
- **Two locales (`en-US`, `es-MX`).** Common in retail and CPG with US Hispanic markets.
- **Multi-region English (`en-US`, `en-GB`, `en-CA`, `en-AU`).** When tone, currency, and legal differ but vocabulary is mostly shared. Consider variant-style fields rather than full locales.
- **EMEA expansion (`en-GB`, `fr-FR`, `de-DE`, `es-ES`, `it-IT`).** A real localization program with translation vendors.
- **Global (`en-US`, `fr-FR`, `de-DE`, `es-ES`, `pt-BR`, `ja-JP`, `zh-CN`, `ko-KR`).** Enterprise scale; usually a Translation Management System (TMS) drives the workflow.

Don't add a locale you can't keep current. A perpetually-out-of-date locale is worse than no locale.

### Locale codes

Use ICU-style locale codes: `en-US`, `fr-FR`, `de-DE`, `pt-BR`, `zh-CN`. Two parts (language + region). Contentful accepts both BCP 47 forms; pick one and stick with it.

### Default locale

Every space has one. Once set, changing it is painful (every entry has values keyed by the default). Pick deliberately:

- US-first business: `en-US`.
- UK-first / European business: `en-GB`.
- LatAm: pick the dominant audience (`pt-BR` for Brazil-led, `es-MX` for North-of-Mexico LatAm, etc.).

### Fallback chains

Each locale has a fallback. The chain resolves missing field values:

```
fr-FR → fr (generic) → en-US (default)
es-MX → es-ES → en-US
ja-JP → en-US
```

Configure deliberately:

- Should a missing French value fall back to English (probably yes for most copy)?
- Should a slug fall back? **Almost never.** A French page slugged in English signals broken translation.
- Mark fields where fallback is wrong as **required** with no fallback in the field config.

## Field-level localization

Every field has a "localizable" boolean.

| Localize | Don't localize |
|---|---|
| Body copy, headlines, descriptions | References (the linked entry has its own locale dimension) |
| SEO title, meta description | Slugs in some patterns (see "URL strategy" below) |
| CTA labels | Boolean flags, dates, IDs |
| Asset alt text | Asset references themselves (the asset has its own locale-resolved URL via the Images API) |
| Localized media (a hero image with translated text baked in) | Most images |

Set localizable when a field has a per-locale meaning. Otherwise leave it global; the default-locale value is shared.

### Localized assets

Mark `Media` (asset reference) fields as localizable when:

- Different regions need different imagery (a US hero with text vs. a French hero with text, both as images).
- Logos differ by region.

Otherwise leave global; the asset is shared.

### Localized references

Cross-entry references can be marked localizable, but think hard. A localized `relatedArticles` field means each locale specifies its own related articles. Sometimes correct (regional editorial curation); often unnecessary (the related articles inherit, each comes back in its own locale on read).

Slalom default: **leave references global**. Localize only when there's a specific editorial reason.

## Translation workflow

Three integration patterns, in order of cost:

### 1. In-app translation (no integration)

Editors translate by hand in the Contentful UI. The locale-switcher in the entry editor lets them flip and edit per locale.

Pros: zero setup. Cons: manual; no progress tracking; quality varies.

Use for: small projects with a single bilingual editor.

### 2. Translation management app from the marketplace

Apps like **Smartling**, **Phrase**, **Lokalise**, **GlobalLink Connect** integrate as App Framework apps. They:

- Pull source content from Contentful (default locale).
- Push it to the TMS for translator workflow.
- Pull translated content back and write it to the target-locale fields.
- Surface progress in the editor sidebar (entry-level) and dashboard (page-level).

Pros: real translation workflow with vendor-quality translators. Cons: per-character cost; setup overhead; vendor lock-in.

Use for: most multi-locale Slalom builds. Pick the TMS based on the client's existing relationships.

### 3. Custom workflow

Build via the App Framework if no marketplace app fits. Rare. See `contentful-app-framework`.

## Adding a locale (operationally)

```js
// migrations/2026-05-09_add_fr_fr_locale.cjs
module.exports = function (migration) {
  migration.createLocale("fr-FR")
    .name("French (France)")
    .fallbackCode("en-US")
    .optional(true);
};
```

Then:

1. Run the migration on a non-prod env first (see `contentful-migrations`).
2. **Backfill or trigger translation** for existing entries. The locale exists but has no values; field-required fallback handles reads, but new authoring needs values.
3. **Update the front end** to handle the new locale (routing, language switcher).
4. Promote through environments per `contentful-space-architect`.

Removing a locale is much harder — Contentful supports it, but you lose all the values keyed to that locale. Soft-deprecate first (keep the locale, mark it not-publicly-routed) before hard-removing.

## Querying locale-aware

### REST (CDA / CMA)

```
GET /spaces/{id}/environments/{env}/entries?content_type=article&locale=fr-FR
```

Pass `locale=*` to get all locales in one response (CMA always returns all; CDA needs the wildcard).

### GraphQL

```graphql
query Article($slug: String!, $locale: String!, $preview: Boolean) {
  articleCollection(
    where: { slug: $slug }
    locale: $locale
    preview: $preview
    limit: 1
  ) {
    items { title body { json } }
  }
}
```

Locale is per query, applied at the field level inside Contentful's resolver.

## Locale routing in Next.js (App Router)

Two main patterns:

### Path-prefixed routing

URLs are `/{locale}/...`:

```
/en-US/products/composable
/fr-FR/products/composable
```

```
app/
  [locale]/
    layout.tsx
    page.tsx
    products/
      [slug]/
        page.tsx
```

Pros: clear, SEO-friendly, easy to share. Cons: middleware redirects on root visit.

```ts
// middleware.ts
export function middleware(req: NextRequest) {
  const { pathname } = req.nextUrl;
  if (pathname === "/" || !pathname.match(/^\/[a-z]{2}(-[A-Z]{2})?\//)) {
    const locale = pickLocaleFromHeaders(req.headers);
    return NextResponse.redirect(new URL(`/${locale}${pathname}`, req.url));
  }
}
```

### Domain / subdomain routing

`fr.example.com`, `de.example.com`. Less common; harder ops; better for distinct brand identities per region.

For most Slalom builds, **path-prefixed**.

## URL slug strategy

Three options:

1. **Locale-translated slugs.** `/en-US/composable-architecture` and `/fr-FR/architecture-composable`. Best UX in-locale. Hardest to operate — every slug is a content decision per locale.
2. **Shared slugs.** `/en-US/composable-architecture` and `/fr-FR/composable-architecture`. Easier ops. SEO discounts the localized search potential.
3. **Hybrid.** Slug shared per content type by default, locale-translated for high-value pages (top product pages, top campaigns).

Hybrid is the Slalom default. Make the model decision per content type:

- `Product` → shared slug. Easier inventory matching.
- `Article` → translated slug for top-content pages, shared otherwise.
- `LandingPage` → translated slug always (campaign-specific).

## Cache and locale

Locale fragments cache space. For a 5-locale site, naive caching is 5× the cache footprint and 1/5 the hit ratio. Mitigations:

- **Tag with locale.** `revalidateTag("article:abc:fr-FR")` instead of `article:abc`. Pair with the wrapper / webhook layers to set/invalidate locale-specific tags.
- **Listing pages keyed per locale.** Each locale's listing has its own cache.
- **Static export per locale** when the surface is small enough.

Locale × variant (personalization) × preview can multiply. Be deliberate.

## Locale-aware webhooks

Webhook payloads identify the modified entry but don't carry locale context — a publish event affects all locales of that entry. The handler decides whether to invalidate a specific locale or all.

Slalom default: invalidate aggregate (`article:abc`) and trust per-locale fetch tags to scope. If you find aggregate invalidation is too coarse, build a per-locale invalidation by reading which locales have field-level changes (CMA exposes this on the entry version diff).

## Common pitfalls

- **Over-localizing references.** Every related article needing per-locale curation is a lot of editorial work. Default global; localize when there's a specific reason.
- **Slugs falling back to English on French.** SEO penalty; user-facing URL surprise. Mark slug as required with no fallback per locale.
- **Adding a locale without a translation plan.** The locale exists, fields fall back, the site looks "kind of bilingual" but isn't.
- **`locale=*` on the front end fetch path.** Pulls every locale's value for every read. Use specific locale in the query.
- **Default locale changed mid-project.** Painful. If you must, do it as a migration with full backfill; never via the UI.
- **Translation app installed but not enforced.** Editors edit in-line in target locales, the TMS gets out of sync. Pick one workflow.
- **Cache invalidation only on the default locale.** Editor publishes a French translation; cache invalidates English; French stays stale.
- **Locale × personalization without thought.** A French + enterprise variant × French + founder variant × French baseline × English × ... combinatorial explosion. Prune deliberately.

## Output formats

### Locale plan

```
# Locale Plan: [Project]

## Locales
| Code | Name | Default | Fallback chain | Optional? |
|---|---|---|---|---|

## Per-content-type localization
| Content type | Localizable fields | Non-localizable fields | Notes |
|---|---|---|---|

## URL strategy
- Routing: path-prefixed | domain | subdomain
- Slug strategy: shared | translated | hybrid (per content type)

## Translation workflow
- App: {Smartling | Phrase | Lokalise | manual}
- Workflow: source → target → review → publish
- Vendor: {if external}

## Cache strategy
- Tagging: per-locale | aggregate
- Invalidation: how locale changes propagate

## Migration plan
- Initial seed: how existing content gets translated
- Add-locale process: per-environment promotion

## Risks
- Out-of-date translations, slug ambiguity, cache fragmentation
```

## How this skill relates to others

- The model decisions on which fields to localize → `contentful-content-model`.
- Locale-aware queries → `contentful-graphql`.
- Locale-keyed cache and fetch tags → `contentful-delivery-optimization`.
- Adding/removing locales as migrations → `contentful-migrations`.
- Routing locales in Next.js → `contentful-react-wrapper` and `software-engineering-nextjs-scaffold`.
- Translation apps → `contentful-app-framework` and `references/marketplace-apps.md`.
- Locale × variant interaction → `contentful-personalization`.
- Webhook handlers that invalidate per-locale → `contentful-webhooks`.

## Source material

- Locales: https://www.contentful.com/help/working-with-locales/
- Localization basics: https://www.contentful.com/help/localization-basics/
- GraphQL locale arg: https://www.contentful.com/developers/docs/references/graphql/
- Plugin reference: `../../references/contentful-foundations.md`
