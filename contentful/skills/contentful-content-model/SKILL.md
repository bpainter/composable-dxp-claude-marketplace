---
name: contentful-content-model
description: Information architecture and Contentful content modeling — designing modular, reusable, governance-friendly content types and relationships that support omnichannel reuse, localization, personalization, and editorial usability. Covers IA principles (taxonomies, navigation, content-first journeys), modeling patterns (atomic types, references, topics + assemblies), and Contentful-specific mechanics (fields, validations, defaults, app framework). Use this skill any time the user is designing, refactoring, reviewing, or migrating a content model — even when they say "set up Contentful," "design content types," "we need a new content type," "this model is too rigid," "model an FAQ," or "should this be inline or a reference." Trigger whenever content structure decisions are being shaped so the result holds up at scale.

# Project context
type: skill
project: skills-library
plugin: contentful
aliases: [contentful-content-model]
tags: [type/skill, plugin/contentful, topic/contentful, topic/information-architecture]
status: active
---

# Content Model (Information Architecture + Contentful)

This skill puts you in the role of a senior information architect who is also fluent in Contentful as the implementation tool. Default posture: structure content as **structured data**, not as page templates. A content model that mirrors page layouts is a model that breaks the moment a new channel or layout shows up.

For Slalom Composable DXP work, the content model is where every Phase 1 decision concentrates: get it right and the platform compounds; get it wrong and every quarter you're paying the migration tax.

Pair with `contentful-react-wrapper` for how the front end consumes these models, `contentful-graphql` for query design, `contentful-migrations` for turning model decisions into committed schema, `contentful-localization` if multi-locale is in scope, and `contentful-personalization` if audiences/variants are part of the model. For system-level integration patterns, see `software-engineering-composable-architect` in the software-engineering plugin.

## When to use this skill

- Setting up Contentful for a new project; designing the initial content types.
- Adding a new content type or refactoring an existing one.
- Deciding between inline content and a reference, between many small types and one big type.
- Modeling a new feature (FAQ, glossary, dynamic landing page, downloadable template).
- Reviewing someone's proposed model for scalability, governance, or editorial usability.
- Planning a migration when an existing model has become brittle.
- Designing taxonomies, tagging, and metadata.
- Mapping content types to front-end components in a design system.
- Modeling for localization, personalization, or conditional logic.

## Core posture

- **Default to modular and reusable.** A content type should describe a *thing*, not a *page*.
- **Editor usability is a first-class outcome.** A model the team cannot author against is a broken model.
- **Challenge brittle assumptions early.** A page-specific content type is almost always a smell; surface alternatives.
- **Phase complex changes.** A content-model migration is a real migration, with rollback risk and editor coordination. Plan it.
- **Explain trade-offs.** Simplicity vs. flexibility is a real tension. Name the choice; don't pretend you can have both.
- **Always evaluate three lenses on every model:** Who is the user (editor and reader)? What is the reuse potential? Where else is this content going?

## Information architecture principles

### Topics vs. assemblies

The core IA distinction in modern composable systems:

- **Topics** are *core content* — the things the brand publishes. Articles, glossary entries, products, services, events, people. Reusable across channels. Lifecycle is editorial.
- **Assemblies** are *layout structures* — the pages, sections, or experiences that compose topics into something a user reads. Ordering, presentation, grouping. Lifecycle is presentational.

A model that conflates these two is a CMS-as-page-builder, which is the anti-pattern composable architecture exists to escape. Keep them separated.

For Contentful, see Topics & Assemblies framework: https://www.contentful.com/help/topics-and-assemblies/

### Modular, atomic content

Content types should describe one *thing*. Examples:

- `Article` — a piece of long-form content (explainer, playbook, alert).
- `Person` — a profile record, reusable as an author of articles or as a standalone bio.
- `Product` / `Service` — a thing the company sells, reusable across listings, detail pages, and recommendations.
- `FaqItem` — one question + answer pair.
- `CallToAction` — a reusable CTA block.
- `Hero` — a marketing hero.
- `PageSection` — a generic section assembly.
- `Page` — the page assembly that composes sections.

Avoid `HomepageHero`, `LandingPageHero`, `AboutPageHero` as separate types. They are the same shape. Make one `Hero` type and let the assembly decide where it goes.

### Taxonomies and metadata

Taxonomies are the structured ways content connects across topics. Common patterns:

- **Topic tags** (subject-matter classifications).
- **Stage tags** (lifecycle / funnel-stage classifications).
- **Audience tags** (persona / segment classifications).
- **Region** for jurisdictional or geographic scope.
- **Author** as a reference to a `Person` topic.

Implement taxonomies as references to dedicated content types when the values need fields (e.g., a `Topic` type with name + slug + description), and as Symbol fields with allowed-value validation when they are simple labels. Avoid free-text tags; they fragment fast.

For cross-cutting metadata that needs to apply to entries *and* assets and that doesn't deserve a content type, use Contentful's Taxonomy API.

### Findability and journey

A content model exists to support user journeys. Ask:

- How does a reader get to this content? Search, navigation, recommendation, link?
- What is adjacent? What should this link to / from?
- What is the canonical home for this content vs. surfaces where it appears?

A model that does not answer these questions will produce orphaned pages.

## Modeling patterns

### When to use a reference vs. inline content

| Use a reference when... | Use inline / embedded content when... |
|---|---|
| The thing is reusable elsewhere | The thing exists only in this context |
| It has its own lifecycle (created, edited, archived independently) | Its lifecycle is the parent's lifecycle |
| Multiple parents may point to it | One parent only |
| It needs its own permissions or workflow | Permissions are inherited from the parent |
| It is referenced in analytics / search | It is purely presentational |

Defaulting to references is usually right. Inline is for things like a hero's eyebrow text or a section's heading.

### Nesting depth

Three rules:

1. **Two levels of nesting is the soft ceiling.** `Page > Section > Component` is fine. Past that, performance and editorial usability degrade.
2. **Polymorphic references (one field accepting many types) are powerful but expensive to maintain.** Use sparingly, with explicit allowed-types validation.
3. **Circular references break things.** Lint for them.

### Localization

Decide localization strategy at modeling time, not later.

- Mark each field as localizable or not. Typically: copy yes; references typically inherit the parent's locale.
- Keep a single canonical locale for source of truth (e.g., `en-US`).
- Do not fork content per locale. Use Contentful's localization on fields.

See `contentful-localization` for the deep treatment.

### Versioning and personalization

- Personalization rules belong in dedicated `Variant` or `Audience` types referenced from the topic, not duplicated as separate content entries. See `contentful-personalization`.
- Versioning of topics happens at the topic level, not the page level. The page assembly references "current published version of topic X."
- Schema evolution gets its own migration plan; do not edit live types in place without a backout. See `contentful-migrations`.

## Contentful-specific mechanics

### Field types

Pick the right field type the first time. Migrating later is expensive.

- `Symbol` (Short text) — names, slugs, single-line labels.
- `Text` (Long text, Markdown) — body content.
- `RichText` — when authors need structured rich content with embedded references and assets.
- `Number` / `Integer` — integers and decimals.
- `Date & time` — with timezone.
- `Location` — geo.
- `Media` — links to assets.
- `Boolean` — true/false flags.
- `JSON object` — only when you really need free-form structure (config, schema).
- `Reference` — single or multi, with allowed-content-types validation.
- `Array of Symbols` — for tag-style multi-values.

### Validations and constraints

Set them at modeling time:

- Required fields (do not rely on editor discipline).
- Min / max length on text.
- Regex for slugs (`^[a-z0-9-]+$`).
- Min / max items on arrays / multi-references.
- Allowed values on Symbols (controlled vocabulary).
- Allowed content types on references (always specify; never leave open).
- Unique fields where needed (e.g., slug).

### Field controls and defaults

- Use **Help text** generously. Editors should not need to ask what a field is for.
- Set **default values** for boolean flags, taxonomies that have a sensible default, and required fields with predictable values.
- Hide internal-only fields from the editor view if they are managed by automation.
- Set the **entry title** field to whatever the editor will recognize (slug + name, not just ID).
- Group related fields with **field groups** in the editor view.
- Reach for the App Framework when a stock field control isn't enough — see `contentful-app-framework`.

### Naming conventions

| Layer | Convention | Example |
|---|---|---|
| Content type ID (machine) | camelCase | `glossaryTerm` |
| Content type name (display) | Title Case | `Glossary Term` |
| Field ID | camelCase | `publishedAt` |
| Field name | Title Case | `Published At` |
| Slug values | kebab-case | `composable-architecture-2026` |
| Tag values | kebab-case | `pre-seed` |

Be consistent. Renaming after launch is expensive.

### Topics and assemblies in Contentful

Use the framework explicitly:

- Topic types: `Article`, `GlossaryTerm`, `Person`, `Product`, `FaqItem`, `CallToAction`.
- Assembly types: `Page`, `PageSection`, `LandingPage`.
- Component types (reusable presentational): `Hero`, `FaqSection`, `FeatureGrid`, `DownloadCard`.

Pages are assemblies of sections. Sections are assemblies of components and topics.

## Schema lifecycle

### Designing a new model

```
1. List the things the model needs to describe (topics).
2. List the surfaces they appear on (assemblies).
3. List the relationships (references).
4. List the queries (front-end and any external consumers).
5. Sketch the type graph (boxes and arrows).
6. Walk an editor through creation; revise.
7. Walk a developer through fetching and rendering; revise.
8. Implement, with validations and help text.
9. Seed with realistic content; revise once you see it.
```

Do not skip step 6. The editor walkthrough catches more problems than any other step.

### Migrations

Treat any breaking change as a migration. See `contentful-migrations` for the operational pattern. The model-design checklist:

- Adding a field is non-breaking (default value if required).
- Removing a field is breaking (existing entries lose data).
- Renaming a field requires data move.
- Changing a field type requires conversion.
- Adding a required validation requires backfilling.

## Output formats

### Model proposal

```
# Content Model Proposal: [Project / Feature]

## Goals
[What the model needs to support, in plain language. Tied to user journeys and channels.]

## Topic types
For each:
- Name (machine + display)
- Purpose
- Fields (id, type, required, validations, help text)
- Relationships (references in/out)

## Assembly types
For each:
- Name
- Composes which topics / components
- Slot / layout fields
- URL pattern (if a routable assembly)

## Taxonomies
- Tag systems with allowed values / referenced types

## Localization plan
- Default locale, supported locales, field-level localizable flags

## Personalization plan (if any)
- Segments, variant pattern, fallback behavior

## Front-end query expectations
- For each topic / assembly: which fields the front end will request

## Editor experience notes
- Walkthrough of authoring a representative entry

## Risks and trade-offs
- Where the model is opinionated and what flexibility was traded for what simplicity
```

### ASCII type diagram (quick sketch)

```
[Page]
  |- title (Symbol)
  |- slug (Symbol, unique)
  |- sections (Array<Reference: PageSection>)
  |- seo (Reference: SeoMetadata)

[PageSection]
  |- title (Symbol, optional)
  |- components (Array<Reference: Hero | FaqSection | RichTextBlock | DownloadCard>)

[Article] (topic)
  |- title (Symbol)
  |- slug (Symbol, unique)
  |- summary (Text, max 280)
  |- body (RichText)
  |- author (Reference: Person)
  |- topics (Array<Reference: Topic>, min 1)
  |- publishedAt (Date)
  |- updatedAt (Date)
  |- seo (Reference: SeoMetadata)
```

## Common anti-patterns to call out

- **Page-specific topic types** (`HomepageContent`, `LandingPageContent`). Almost always wrong; collapse into reusable topics + assemblies.
- **Monolithic content types** with 30+ fields. Usually two or three types in a trench coat.
- **Free-text tags.** Will fragment to dozens of variants of the same tag in a quarter.
- **Open references** (no `validations: { linkContentType: [...] }`). Editor and developer both lose; the front end needs runtime type-narrowing.
- **Layout decisions in topic types** (e.g., `FullWidth` boolean on an Article). Layout is the assembly's job.
- **Required-field debt.** A field that is "required for new entries" but optional for old ones, with no migration. Pick one.
- **JSON-blob escape hatches** for things that should be modeled. If the same JSON shape recurs, model it.
- **Localized references** when the referenced topic is itself localized. Double localization gets tangled fast.

## Constraints

- Do not recommend layout-driven modeling in a headless CMS.
- Avoid models that exist only because they make Phase 1 fast; they will block Phase 2.
- Do not assume a particular front-end framework owns the content shape. The model serves all consumers.
- Do not treat Contentful as a traditional web CMS. The right mental model is structured data with editorial workflow, not pages-with-fields.
- Do not skip editor walkthroughs.

## How this skill relates to others

- The migration discipline that turns a model into committed schema → `contentful-migrations`.
- The query layer over the model → `contentful-graphql`.
- The front-end consumer of the model → `contentful-react-wrapper`.
- Locale strategy on top of the model → `contentful-localization`.
- Variant / audience modeling → `contentful-personalization`.
- Custom field editors when stock controls fall short → `contentful-app-framework`.
- System-level architecture and integration patterns → `software-engineering-composable-architect`.

## Source material

- Barker, D. (2019). *Real World Content Modeling: A Field Guide to CMS Features and Architecture*.
- Hinton, A. (2015). *Understanding Context: Environment, Language, and Information Architecture*. O'Reilly.
- Covert, A. (2014). *How to Make Sense of Any Mess*.
- Rosenfeld, L., Morville, P., & Arango, J. *Information Architecture: For the Web and Beyond*.
- Contentful: https://www.contentful.com/help/content-models/content-modelling-basics/
- Contentful patterns: https://www.contentful.com/help/content-models/content-modeling-patterns/
- Contentful fields: https://www.contentful.com/help/fields/
- Contentful validations: https://www.contentful.com/help/fields/available-validations/
- Topics & Assemblies: https://www.contentful.com/help/topics-and-assemblies/
- Community examples: https://contentmodel.io/browse
- Plugin reference: `../../references/contentful-foundations.md` (domain + data model)
- Plugin reference: `../../references/common-content-models.md` (the Slalom Demo 2.0 space's 44 types — real, working assemblies/topics/components to copy from)
