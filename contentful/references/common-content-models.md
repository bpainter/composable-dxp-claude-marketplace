---
type: reference
project: skills-library
scope: plugin
plugin: contentful
tags: [type/reference, plugin/contentful, scope/foundational, topic/contentful, topic/content-modeling]
status: active
---
# Common content models — Slalom Demo 2.0 reference

A real-world reference model pulled directly from the **Slalom — Demo 2.0 (Contentful Template)** space (`o00gvn4y1axt`, environment `main`) on 2026-05-09. This is the model Slalom is shipping as a starting point on Contentful pursuits — it's worth treating as the canonical "what does a well-modeled enterprise composable site actually look like" example.

44 content types organized by purpose. Skills cite this file when they need to ground recommendations in real patterns rather than invented examples. Pair with `contentful-content-model` (the principles), `contentful-personalization` (the `nt_*` types come from there), and `contentful-rich-text` (most body fields use sophisticated rich text validations).

## What this space teaches

A handful of patterns are worth internalizing because they keep showing up in good Contentful work:

- **Topics and assemblies are clearly separated.** `landingPage`, `studioLandingPage`, and `dynamicListingPage` are assemblies. `blogPost`, `event`, `supportArticle`, `policy`, `product`, `person`, `storeLocation` are topics. Components like `callToAction`, `banner`, `collection`, `faqItem` slot into the assemblies.
- **One reusable hero/CTA pattern.** A single `callToAction` and a single `banner` cover hero usage everywhere; assemblies pick which one via a polymorphic `hero` field accepting either. No `HomepageHero` / `ProductPageHero` proliferation.
- **`collection` is the universal "section-of-anything" assembly.** A polymorphic `items` field accepting 20 different content types, plus `layout` and `theme` enums for presentation. This single type does the job that two dozen one-off section types would have done in a worse model.
- **`internalName` is the recurring "editor handle" field.** Almost every type has it. Editors search and disambiguate by it; the public-facing title field is separate. Worth copying.
- **Rich text validations are aggressive.** Allowed marks, allowed nodes, allowed embedded content types, max sizes — all set per field. The `body` field on `blogPost`, `policy`, `supportArticle` each tunes its own allowlist. Pair with `contentful-rich-text`.
- **Topic & Assemblies app annotation on Studio types.** `studioLandingPage` carries `metadata.annotations.ContentType: { id: "Contentful:ExperienceType" }` — that's how the visual builder marks an experience type.
- **Personalization is first-class via `nt_audience`, `nt_experience`, `nt_mergetag`.** Every assembly and most topics expose an `nt_experiences` field that wires variants in. The merge tag is an inline-rich-text widget used inside `body` fields. This IS the "former Ninetailed, now Contentful Personalization" wiring described in `contentful-personalization`.
- **External integrations have wrapper types.** `externalImage` (Bynder), `wistiaVideo`, `youTubeVideo`, `coinProduct` (external product), `sapProductDataProvider`, `salesforceForm`. Each wraps a third-party integration in a small typed record so the front end and editor see something predictable.
- **Locale strategy is partial.** Editorial copy (`title`, `body`, `slug`, `description`, `media position`) is `localized: true`. Reference fields and structural enums are not. Matches the guidance in `contentful-localization`.
- **`appSettings` is the singleton-config type.** Site-level config — logo, default SEO, primary/footer navigation, default CTA, color tokens, store currencies — lives in a single content type with one entry. The pattern shows up on every Slalom site.

## Type catalog

44 types grouped by purpose. Per-type fields and validations are in the JSON export at `common-content-models.json` adjacent to this file (when generated).

### Assemblies (page-level routable types)

| ID | Display | Fields | What it is |
|---|---|---|---|
| `landingPage` | 📄 Landing Page | 9 | The default page assembly. Hero (callToAction \| banner) + sections (collection \| callToAction \| banner). `seoMetadata`, `nt_experiences`. |
| `studioLandingPage` | 📄 Studio: Landing Page | 8 | Visual-builder-driven page. Stores `componentTree`, `dataSource`, `unboundValues`, `componentSettings`. Carries the `Contentful:ExperienceType` annotation. |
| `dynamicListingPage` | 📄 Dynamic Listing Page | 10 | Generates listings of a content type (`blogPost`, `policy`, `event`, `person`, `supportArticleCategory`, `storeLocation`) with sort order and additional content. |

### Topics (core content with their own lifecycle)

| ID | Display | Fields | What it is |
|---|---|---|---|
| `blogPost` | 📄 Blog Post | 11 | Long-form editorial. Rich text body with embedded CTAs, customer reviews, quotes, media mentions. Up to 3 authors, 3 related posts. |
| `event` | 🗓️ Event | 18 | Marketing/community events (in person, online, webinar, partner). Place ref, time/duration/timezone, status, registration form, form fallback. |
| `supportArticle` | 📄 Support Article | 11 | Help center article. Categorized, can list related articles, has CTA. |
| `supportArticleCategory` | 🔠 Support Article Category | 5 | Group support articles. Title, slug, description, icon. |
| `policy` | 📄 Policy | 9 | Legal and policy content. `effectiveDate`, `updatedDate`, `expirationDate`, body, SEO. |
| `product` | 🛍️ Product | 12 | E-commerce product. Brand (Company), images, prices array, categories, additional sections. |
| `productCategory` | 🛍️ Product Category | 6 | Hierarchical taxonomy with self-referencing `subcategories`. |
| `coinProduct` | 🛍️ External Product | 7 | External product line wrapper. Productname, SKU, slug, thumbnail, teaser. |
| `person` | 👤 Person | 7 | First name, last name, role, short bio, avatar, company. Used as author, leadership, attribution. |
| `company` | 🏢 Company | 3 | Brand or media outlet. Name, logo, alt logo. |
| `storeLocation` | 📍 Store Location | 14 | Branded retail location. Place, hours, services, reviews, staff (Person refs). |

### Compositional component blocks (slot into assemblies)

| ID | Display | Fields | What it is |
|---|---|---|---|
| `callToAction` | 💬 Call to Action | 9 | Heading, body (rich text), media, label, internal/external target, data provider, `nt_experiences`. The default CTA / hero. |
| `banner` | 💬 Banner | 10 | Larger hero alternative. Heading, body, links, media, content position (9 cells), media position, theme, size. |
| `alert` | 💬 Alert | 3 | Compact alert (Info / Warning / Danger). Rich text message with merge-tag inline support. |
| `collection` | 🗂️ Collection | 7 | The polymorphic "section of anything" type. Heading, intro, `items` accepting 20 content types, theme, layout, click-through. |
| `quote` | 💬 Quote | 5 | Pull quote. Theme + rich-text quote + author (Person). |
| `mediaMention` | 💬 Media Mention | 3 | Quote + source Company. Press / outlet review pattern. |
| `customerReview` | 💬 Customer Review | 4 | Rating + rich text + attribution (Person). |
| `benefit` | ✅ Benefit | 5 | Feature/benefit. Headline, description, icon, editorial image. |
| `stats` | 📈 Stats | 4 | Value, label, editorial thumbnail. |
| `faqItem` | 💬 FAQ Item | 4 | Theme + question + rich-text answer. Collection layout `faq` aggregates these. |
| `assetWrapper` | 📁 Asset Wrapper | 3 | Wraps `imageWithFocalPoint`, `youTubeVideo`, or `wistiaVideo` with caption. |

### Media types

| ID | Display | Fields | What it is |
|---|---|---|---|
| `imageWithFocalPoint` | 📷 Image with Focal Point | 3 | Asset link + focal point JSON. Used everywhere as the image carrier. |
| `externalImage` | 📷 External Image | 2 | Bynder DAM image — the integration JSON sits in `image: Object`. |
| `youTubeVideo` | 🎥 YouTube Video | 2 | URL with strict regex validation (`youtube.com/watch?v=` and `youtu.be/`). |
| `wistiaVideo` | 🎥 Wistia Video | 2 | Wistia integration JSON. |

### Forms and interactivity

| ID | Display | Fields | What it is |
|---|---|---|---|
| `salesforceForm` | 💎 Salesforce form | 6 | Wrapper for Salesforce web-to-lead forms. Form fields JSON, button label, post-submission status. |
| `formStatus` | 💎 Form Status | 5 | Success / fallback message replacement for a form. |
| `embeddedModule` | 🔌 Embedded Module | 2 | Currently scoped to Typeform; placeholder for other third-party widgets. |

### Navigation, links, and global config

| ID | Display | Fields | What it is |
|---|---|---|---|
| `navigation` | 🧭 Navigation | 6 | Hierarchical nav (Editorial / Category / Link / Icon Group). `children` is recursive — nav links to nav. |
| `link` | 🔗 Link | 6 | Reusable link with internal target (page-like types) or validated external URL regex. |
| `redirect` | 🔗 Redirect | 4 | Vanity URL, internal search redirect, 301 / 302. |
| `appSettings` | ⚙️ Global Settings | 20 | The site singleton. Logo, domain, default SEO, primary nav, footer nav, default CTA, color tokens, store currencies, store root categories, announcement (alert ref). |
| `microcopy` | 🎛️ Microcopy | 2 | Tokenized strings for UI (key-value JSON, localized). |

### SEO

| ID | Display | Fields | What it is |
|---|---|---|---|
| `seo` | 🔍 SEO | 9 | OG meta, type enum (Website/Article/Blog/Author/Company/Product), keywords array, image, robots flags, sitemap toggle. Description has `size [155, 260]` validation — the SEO-best-practice length. |

### External data integrations

| ID | Display | Fields | What it is |
|---|---|---|---|
| `sapProductDataProvider` | SAP Product Data Provider | 2 | JSON object holding SAP-derived data attached to other content types. Pattern for "external system data on a typed record." |
| `price` | 🛍️ Price | 3 | Amount + currency (USD / EUR). Nested under `product.price` as an array. |
| `place` | 📍 Place | 4 | Coordinates (Location field) + address. Used by `event` and `storeLocation`. |

### Personalization (Ninetailed → Contentful Personalization)

| ID | Display | Fields | What it is |
|---|---|---|---|
| `nt_audience` | Ninetailed Audience | 5 | Name, description, rules JSON, audience id, metadata. |
| `nt_experience` | Ninetailed Experience | 8 | Name, description, type (`nt_experiment` \| `nt_personalization`), config JSON, audience ref, variants array, experience id, metadata. |
| `nt_mergetag` | Ninetailed Merge Tag | 3 | Name, fallback, merge tag id. Used inline in `body` rich text fields across the system. |

## Patterns worth copying

### The hero pattern (assembly accepts multiple hero types)

```
[landingPage].hero — Reference (callToAction | banner)
[landingPage].sections — Array<Reference (collection | callToAction | banner)>
```

Front-end renders by `__typename`. New hero type? Add to the validation, add to the section router. See `contentful-react-wrapper`.

### The collection pattern (universal section)

`collection` accepts items of 20 content types, plus a `layout` enum (`grid`, `carousel`, `faq`, `grid-col-2/3/4`, `wrap`, `vertical-list`). One model decision; many visual outputs. This is what well-modeled assemblies look like.

### The merge-tag pattern (personalization inside rich text)

`nt_mergetag` is allowed as `embedded-entry-inline` in `body`, `quote`, `answer`, `message` fields throughout. Editors insert "{{first_name}}" semantics inline. Front-end resolves at render time using the visitor's profile.

### The internalName-as-handle pattern

Every type has `internalName: Symbol`, often the `displayField`. Editors get a stable handle even when the public title is empty or localized. Worth adopting on every Slalom Contentful build.

### The size-validated SEO description

```
seo.description: Text, size: { min: 155, max: 260 }
```

Forces editors to write descriptions Google actually displays. Trivial validation; large UX win.

### The recursive navigation pattern

```
navigation.children: Array<Reference (navigation | link | <page-types>)>
```

Self-referential navigation lets the model express depth without inventing per-level types. Lint for cycles in the front-end build.

## What's missing / opinions

A model this complete still has gaps relative to what `contentful-content-model` recommends:

- **No `Topic` / taxonomy content type.** Tags on `blogPost`, `policy`, `supportArticle` would be additive.
- **`navigation.target` is an open `Link`** with no `linkContentType` validation. Recommend tightening.
- **`embeddedModule` is Typeform-specific.** As more widget types appear, a `provider` enum + per-provider config object would be cleaner than adding flat fields.
- **Body rich-text validations vary across types.** Reasonable today; will drift. Worth establishing a shared rich-text profile (e.g., "longform body," "compact body," "snippet") and applying consistently.
- **No `Author` distinct from `Person`.** `Person` is doing triple duty (author of blog, leadership profile, customer-review attribution). Often fine; sometimes wrong if leadership and authors have different lifecycles.

## Source

Pulled live from `o00gvn4y1axt` / `main` on 2026-05-09 via Contentful MCP. Re-pull anytime with:

```
list_content_types(spaceId="o00gvn4y1axt", environmentId="main", skip=N, limit=10) for N in [0,10,20,30,40]
```

The full per-field JSON export sits adjacent to this file as `common-content-models.json` (generated separately if needed).

## How skills should use this

- **`contentful-content-model`** — when modeling a new client space, hand them this set as a baseline; cherry-pick the topics/assemblies that map to their domain.
- **`contentful-react-wrapper`** — the polymorphic-section pattern (`landingPage.sections` + `collection.items`) is the canonical example for the section-router exhaustive switch.
- **`contentful-graphql`** — query examples should mirror this model rather than inventing one. The polymorphic `sectionsCollection` against `Hero | FaqSection | RichTextBlock` here becomes `Collection | CallToAction | Banner` in real queries.
- **`contentful-personalization`** — the `nt_audience` / `nt_experience` / `nt_mergetag` triad is exactly what skills describe; ground examples in these IDs.
- **`contentful-rich-text`** — the `body`, `quote`, `answer`, `message`, `editorialDescription` fields show the validation profiles in action; cite specific examples.
