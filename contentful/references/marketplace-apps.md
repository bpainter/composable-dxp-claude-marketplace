---
type: reference
project: skills-library
scope: plugin
plugin: contentful
tags: [type/reference, plugin/contentful, scope/foundational, topic/marketplace, topic/integrations]
status: active
---
# Contentful Marketplace — apps and webhook templates

A curated map of the [Contentful Marketplace](https://www.contentful.com/marketplace/apps/) for Slalom Composable DXP work. The marketplace changes constantly; this reference catches the apps that actually show up on enterprise pursuits and tells you which ones to install first vs. when to build via the App Framework instead. Re-pull anytime via `web_fetch` against the marketplace URL.

The default posture in `contentful-app-framework`: **check the marketplace before you build**. This file is what you check.

## Two artifact types in the marketplace

- **Apps** — full App Framework UIs that install into a space and render in editor locations (sidebar, field, page, dialog, config, home). See `contentful-app-framework`.
- **Webhook templates** — pre-shaped webhook configurations for common downstream targets (Algolia index updates, AWS Lambda invocations, CI build triggers, S3 archival, SQS messaging, Bitbucket / GitHub / CircleCI deploy hooks). See `contentful-webhooks`.

When skills mention "install from the marketplace," they mean either, depending on the integration shape.

## Categories (Contentful's taxonomy)

A/B Testing, Analytics, Artificial intelligence, Collaboration, Deployment & delivery, Developer productivity, Digital asset management, Ecommerce, Editor productivity, Marketing automation, Orchestration, Personalization, Search, Studio, Translation & localization, Video.

## Slalom-relevant apps by category

Annotated for Composable DXP work — the ones to default-install vs. evaluate vs. skip on a typical engagement.

### Artificial intelligence (editor-side AI)

| App | Provider | What it does | Slalom default |
|---|---|---|---|
| **AI Content Generator powered by OpenAI** | Contentful | Generate content, SEO keywords, descriptions, translations | Default install on most editorial-heavy builds |
| **AI Content Generator powered by Amazon Bedrock** | AWS | Same as above, on Bedrock models | When the client has an AWS-first AI policy |
| **AI Image Generator powered by OpenAI** | Contentful | Generate / transform images inside the editor | Evaluate per client; image generation governance often kills it |
| **AI Image Tagging** | Contentful | Auto-tag images on upload | Default install if the client has any volume of imagery |
| **AltText AI** | AltText.ai | Generates alt text for images | Default install for accessibility commitments |
| **Brand Guardian** | VML | AI brand-quality review of content | Evaluate; useful for multi-brand portfolios |

### Translation & localization

| App | Provider | Notes |
|---|---|---|
| **Smartling**, **Phrase**, **Lokalise**, **GlobalLink Connect** | Various | The four enterprise TMS integrations. Pick based on client's existing TMS contract. See `contentful-localization`. |
| **Acclaro Translations** | Acclaro | Lighter-weight translation workflow; good for mid-market |
| **AI Content Generator** | Contentful | Has translation as a feature; often enough for low-stakes locales |

For multi-locale builds, install one TMS app + the AI translator as a secondary lane. Don't run two TMS apps in parallel — workflow conflicts.

### Digital asset management (DAM)

| App | Provider | When to use |
|---|---|---|
| **Bynder App** | Contentful | Slalom default DAM when the client doesn't have one. Tier-1 partner relationship. |
| **Brandfolder App** | Brandfolder | Common in retail / lifestyle brands |
| **Aprimo** | Aprimo | Channel-optimized content; common in CPG |
| **Cloudinary** *(via marketplace)* | Cloudinary | When the client has Cloudinary already; image transformations integrate with Contentful Images API |
| **Bynder Content Workflow** | Bynder | Workflow on top of Bynder DAM; install when content production is high-volume |

DAM choice tends to be set before Slalom arrives. Install the app for whichever DAM is in use; resist re-platforming the DAM as part of a Contentful engagement unless the client asks.

### Search

| App | Provider | Notes |
|---|---|---|
| **Algolia** *(app + webhook template)* | Algolia / Contentful | Slalom default search. Webhook auto-syncs published entries. Deep guidance lives in `contentful-webhooks`. |
| **Coveo** | Coveo | Less common but excellent for B2B / enterprise relevance work |

Algolia is the right default. Coveo when the client already has it or has specific relevance / personalization needs.

### Personalization & A/B testing

| App | Provider | Notes |
|---|---|---|
| **Contentful Personalization** *(former Ninetailed)* | Contentful (first-party) | Default for new builds since the 2024 acquisition. See `contentful-personalization`. |
| **AB Tasty** | AB Tasty | When the client has it, or for visual editor A/B without changing the model |
| **Amplitude Experiment** | Amplitude | When Amplitude is the analytics vendor of record |
| **Optimizely** *(via webhook / template)* | Optimizely | Long-standing enterprise AB; usually pre-existing |

For a greenfield build, default to **Contentful Personalization**. Adopt third-party only if the client's analytics / experimentation stack is fixed.

### Marketing automation

| App | Provider | Notes |
|---|---|---|
| **Braze** | Contentful | Slalom partner stack; default if the client uses Braze for messaging |
| **Adobe Marketo Form Selector** | Contentful | Marketo forms inside Contentful entries |
| **HubSpot** *(via marketplace / webhooks)* | Various | Common in B2B mid-market |
| **Salesforce form** | Custom (typical) | Salesforce web-to-lead forms — see the Slalom Demo space's `salesforceForm` content type pattern |

### Ecommerce

| App | Provider | Notes |
|---|---|---|
| **commercetools** | Contentful / commercetools | Default for new composable commerce builds |
| **BigCommerce** | Candyspace | Mid-market ecommerce |
| **Shopify** *(via marketplace)* | Various | Lifestyle / DTC |
| **SAP Commerce Cloud** | Various | Enterprise B2B; the Slalom Demo's `sapProductDataProvider` is the wrapper pattern |
| **Salesforce B2C Commerce** *(via custom)* | Slalom Build | Slalom's Composable Accelerator pattern |

For a new client, install the ecommerce app for whatever their commerce backend is. The Topics & Assemblies model in `contentful-content-model` keeps the content side independent.

### Editor productivity

| App | Provider | Notes |
|---|---|---|
| **Bulk Edit** | Contentful | First-party multi-entry editing. Install on every space with > 100 entries. |
| **Adapt Essentials: Bulk Asset Fields** | Adapt | Bulk edit asset titles, alt text, descriptions |
| **Auto-prefix** | Contentful | Effortless updates across entries — pairs with bulk edit |
| **Closest Preview** | Contentful | Jump from a referenced entry to the page that uses it. Underrated; install everywhere. |
| **Arboretum Sitemap App** | Bright IT | Visual sitemap / page-tree editor inside Contentful |
| **Compose & Launch** | Contentful (first-party) | Page-builder UI on top of the model. Evaluate per client; sometimes overkill. |
| **AI Image Tagging** | Contentful | Auto-tag images |

Default-install set for any Slalom build: **Bulk Edit, Closest Preview, AI Image Tagging, AltText AI, Bulk Asset Fields**. They pay for themselves in editor satisfaction.

### Studio (visual builder)

The Studio category is for apps that work alongside or extend Contentful Studio (the visual page builder, formerly the SmartTypes / experience-type assembler). When the client uses Studio, install the studio-category apps that match their needs. The Slalom Demo space's `studioLandingPage` content type with the `Contentful:ExperienceType` annotation is the model side of Studio.

### Deployment & delivery (build / hosting integrations)

| App / Template | Provider | Notes |
|---|---|---|
| **Vercel** *(integration)* | Vercel | Slalom default deploy target — pair the integration with Contentful webhooks for revalidation |
| **AWS Amplify** | Contentful | When the client is AWS-first |
| **Netlify** *(via marketplace)* | Netlify | Alternative to Vercel |
| **AWS Lambda** *(webhook template)* | Contentful | Invoke serverless on content change |
| **AWS S3** *(webhook template)* | Contentful | Archive content to S3 |
| **AWS SQS** *(webhook template)* | Contentful | Queue downstream work |
| **CircleCI / GitHub / Bitbucket** *(webhook templates)* | Contentful | Trigger CI on publish |

For Slalom Composable DXP defaults: **Vercel integration + Algolia webhook + a CI webhook for build automation** is the baseline three.

### Analytics & observability

| App | Provider | Notes |
|---|---|---|
| **Amplitude Experiment** | Amplitude | Pairs with Personalization for experiment analysis |
| **Various analytics integrations** | Various | Most analytics stacks consume from the front end, not from Contentful directly |

### Video

| App | Provider | Notes |
|---|---|---|
| **api.video** | api.video | Cloud video upload + streaming |
| **Wistia** *(custom + Slalom Demo wrapper)* | Custom | Slalom Demo space ships `wistiaVideo` content type as the wrapper pattern |
| **YouTube** *(custom + Slalom Demo wrapper)* | Custom | `youTubeVideo` with regex URL validation, same pattern |

### Developer productivity

| App / Template | Provider | Notes |
|---|---|---|
| **GraphQL Playground** | Contentful (first-party) | Default install — every dev needs it |
| **Webhook templates (Lambda, S3, SQS, CircleCI, Bitbucket, etc.)** | Contentful | See "Deployment & delivery" |
| **Codegen / type generation** *(via npm, not marketplace)* | Various | `contentful-typescript-codegen`, `graphql-codegen` — see `contentful-graphql` |

## Default-install sets

Two opinionated defaults for Slalom engagements:

### Standard engagement (single brand, standard complexity)

```
Editor productivity:
  - Bulk Edit
  - Closest Preview
  - AltText AI
  - AI Image Tagging
  - Bulk Asset Fields

AI authoring:
  - AI Content Generator (OpenAI or Bedrock per client preference)

Search:
  - Algolia App + Algolia Webhook template

Personalization:
  - Contentful Personalization

DAM:
  - Whichever the client has (Bynder default if greenfield)

Translation:
  - Whichever TMS they have (Smartling / Phrase / Lokalise / GlobalLink)
  - AI Content Generator as the secondary lane

Deployment:
  - Vercel integration + GitHub or Bitbucket webhook for CI
```

### Complex engagement (multi-brand, regulated, or enterprise)

Same as above plus:

```
- Brand Guardian (multi-brand brand-quality review)
- Compose & Launch (when the client wants page-builder UX)
- AWS Lambda + SQS webhook templates (for downstream sync)
- Translation: pair the TMS with a workflow app (Smartling Workflow or equivalent)
- Custom apps via App Framework for proprietary integrations
```

## When to build instead of install

From `contentful-app-framework`:

- Marketplace app exists but the UX or data flow doesn't match the engagement.
- The integration is proprietary to the client (internal taxonomy service, internal AI model).
- Two marketplace apps almost-fit but neither covers the full need.
- Compliance constraints prevent a third-party app from accessing certain content.

If you find yourself building, write the build justification down — it's the first thing future maintainers will ask.

## When NOT to install

- **An app with overlapping functionality** to one already installed. Two AI-content apps = confused editors.
- **An app you can't get permission to keep running for 12+ months.** Contractual / vendor risk.
- **An app with unclear data residency.** Some apps proxy through US-only servers; check the client's posture.
- **Anything for "evaluation only" without scoping how it gets removed.** Cleanup never happens; the app accumulates editor confusion.

## Pricing and licensing

Most Contentful-published apps are free with a Contentful subscription. Third-party apps frequently carry their own subscription (Smartling, Algolia, Bynder, Cloudinary, etc.) — verify pricing as part of the pursuit.

## Source

- Marketplace catalog: https://www.contentful.com/marketplace/apps/
- Contentful Apps GitHub (open-source examples and first-party app source): https://github.com/contentful/apps
- Last refreshed: 2026-05-09 via `web_fetch`. Re-pull when scoping new engagements.

## How skills should use this

- **`contentful-app-framework`** — cite this file under "Before building: check the marketplace." When the user asks "build a custom slug app," check this list first.
- **`contentful-personalization`** — Contentful Personalization (first-party) is the default; only suggest third-party (AB Tasty, Amplitude Experiment, Optimizely) when the client's stack requires it.
- **`contentful-localization`** — pick the TMS app from this list rather than inventing one.
- **`contentful-webhooks`** — webhook templates here are the canonical starting points; reference rather than rewriting from scratch.
- **`contentful-mcp-cli`** — when the agent suggests an integration, check the marketplace first via this file before falling back to a custom app or webhook script.
- **`contentful-content-model`** — when the model needs an external data wrapper (DAM image, video, ecommerce product, form), the marketplace app sets the field shape; the Slalom Demo space's `externalImage` / `wistiaVideo` / `coinProduct` / `salesforceForm` patterns are the in-vault references.
