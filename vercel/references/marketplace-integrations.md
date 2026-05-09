---
type: reference
project: skills-library
scope: plugin
plugin: vercel
tags: [type/reference, plugin/vercel, scope/foundational, topic/marketplace, topic/integrations]
status: active
---
# Vercel Marketplace integrations — Slalom defaults

A curated map of the [Vercel Integrations Marketplace](https://vercel.com/integrations) for Slalom Composable DXP work. Re-pull anytime; the list grows fast.

The default posture: **install marketplace integrations to wire third parties cleanly into the Vercel project surface** rather than wiring them by hand. Each integration usually adds env vars, hooks, dashboards, and a uninstall path.

## Categories

Vercel's marketplace organizes integrations into:

- **AI & Machine Learning** — model providers, vector databases, AI tooling
- **CMS** — headless CMS providers
- **Commerce** — ecommerce backends
- **Database** — databases (often via Vercel-managed integrations)
- **Logging & Monitoring** — observability stacks
- **Authentication & Identity** — auth providers, SSO
- **Analytics** — third-party analytics beyond Vercel's
- **Development Tools** — DX, CI/CD, testing
- **Marketing & Productivity** — email, marketing automation
- **Search** — search-as-a-service

## Slalom-relevant integrations

Annotated by category — the ones to default-install vs. evaluate vs. skip on a typical engagement.

### CMS

| Integration | Notes |
|---|---|
| **Contentful** | Default install on Composable DXP builds. Wires Contentful env vars + revalidation hooks. See `contentful-react-wrapper`. |
| **Sanity** | Common alternative; wire similarly to Contentful. |
| **Storyblok** | Less common in enterprise; visual-editor-friendly. |
| **Builder.io** | When the client wants a visual page builder over a headless CMS. |

### Commerce

| Integration | Notes |
|---|---|
| **Shopify** | Default for DTC / lifestyle brands. |
| **commercetools** | Enterprise composable commerce default. Pair with `contentful-content-model`'s product patterns. |
| **BigCommerce** | Mid-market. |
| **Salesforce Commerce Cloud** | Slalom Composable Accelerator pattern; usually wired via SLAS / SCAPI rather than a stock integration. |

### Database

| Integration | Notes |
|---|---|
| **Vercel Postgres (Neon)** | First-party. Default for relational workloads. See `vercel-storage`. |
| **Vercel KV (Upstash Redis)** | First-party. Default for KV / cache. |
| **Vercel Blob** | First-party. Default for object storage. |
| **MongoDB Atlas** | When the client has it; document workloads. |
| **Supabase** | Postgres + auth + realtime, all-in-one. Good fit when auth is in scope. |
| **PlanetScale** | MySQL-compatible serverless DB; use when client requires MySQL. |
| **Turso** | Edge SQLite; niche but powerful for read-heavy workloads. |

### Logging & Monitoring

| Integration | Notes |
|---|---|
| **Datadog** | Default for enterprise clients with existing Datadog footprint. |
| **Sentry** | Default for application error tracking. |
| **Axiom** | Lightweight log + analytics; great Vercel integration. |
| **Honeycomb** | When the client wants high-cardinality observability. |
| **Logtail / Better Stack** | Smaller engagements. |
| **New Relic** | Enterprise APM. |

### Authentication & Identity

| Integration | Notes |
|---|---|
| **Clerk** | Default for B2B SaaS / internal apps. Complete UI + API. |
| **Auth0 (Okta)** | Enterprise. Common when client has Okta. |
| **WorkOS** | Enterprise SSO + SCIM. |
| **NextAuth (Auth.js)** *(library, not integration)* | Self-hosted; use when the client doesn't want an auth vendor. |

### AI & ML

| Integration | Notes |
|---|---|
| **OpenAI** | Default LLM provider for most builds; usually routed through AI Gateway. |
| **Anthropic** | Default for Claude-based features. |
| **Pinecone** | Default vector DB. |
| **Weaviate** | Alternative vector DB. |
| **Replicate** | Hosted model inference for non-LLM workloads (image, audio). |
| **ElevenLabs** | TTS. |
| **Hugging Face** | Hosted inference for niche models. |

For multi-provider routing, default to AI Gateway. See `vercel-ai-gateway`.

### Search

| Integration | Notes |
|---|---|
| **Algolia** | Slalom default search. Pair with Contentful webhook templates. |
| **Typesense** | Open-source alternative when budget or sovereignty matters. |
| **Meilisearch** | Lightweight self-hosted search. |
| **Coveo** | Enterprise B2B / commerce. |

### Marketing & Productivity

| Integration | Notes |
|---|---|
| **Resend** | Transactional email — Slalom default for newer builds. |
| **Postmark** | Alternative transactional. |
| **SendGrid** | Enterprise legacy. |
| **Loops** | Marketing email + transactional, dev-friendly. |
| **Stripe** | Payments. Default. |
| **Segment** | CDP (analytics fan-out). |

### Development Tools

| Integration | Notes |
|---|---|
| **GitHub / GitLab / Bitbucket** | Git provider connection. Pick the one the client uses. |
| **Slack** | Deploy notifications, alerts. |
| **Linear** | Issue tracking integration. |
| **Checkly** | Synthetic monitoring + browser checks. |
| **Cypress / Playwright Cloud** | E2E testing on previews. |

## Default-install set

Two opinionated defaults for Slalom engagements:

### Standard engagement

```
CMS:
  - Contentful

Storage:
  - Vercel Blob (or Bynder for DAM)
  - Vercel Postgres OR Supabase, depending on auth strategy

AI:
  - AI Gateway
  - OpenAI + Anthropic (via Gateway)

Search:
  - Algolia

Auth:
  - Clerk OR Auth0, depending on client

Email:
  - Resend

Observability:
  - Sentry
  - Speed Insights + Web Analytics (built-in)
  - Axiom OR Datadog (depending on client SIEM)

Notifications:
  - Slack
```

### Complex engagement

Same plus:

```
- Datadog (Log Drain to enterprise SIEM)
- WorkOS (enterprise SSO + SCIM)
- Checkly (synthetic monitoring across critical journeys)
- A vector DB (Pinecone) when AI search / RAG is in scope
- Turso or MongoDB Atlas if data shape demands it
```

## When NOT to install an integration

- **Overlapping with an existing integration** — two transactional email vendors = confusion + inconsistent template ownership.
- **No path to keep paying for it** — vendor risk; integrations accumulate.
- **Sovereignty / data-residency mismatch** — confirm where the integration's data lives.
- **Just for experimentation without a removal plan** — uninstalling is easy; cleanup of orphan env vars and webhooks is what gets forgotten.

## Pricing and licensing

Most integrations carry their own subscription. Vercel-billed integrations (Vercel Blob, KV, Postgres) consolidate billing into the Vercel invoice; third-party integrations bill separately. Verify pricing as part of pursuit scoping.

## Source

- Marketplace: https://vercel.com/integrations
- Last reviewed: 2026-05-09. Re-pull when scoping new engagements.

## How skills use this

- **`vercel-deploy-pipeline`** — when standing up a new project, walk this list and install the engagement-appropriate set.
- **`vercel-ai-sdk` / `vercel-ai-gateway`** — model and vector DB integrations live here.
- **`vercel-observability`** — Datadog / Axiom / Honeycomb integrations are the canonical wire-up paths.
- **`vercel-security`** — auth integrations (Clerk, Auth0, WorkOS) wire here, not in security itself.
- **`vercel-storage`** — Postgres / KV / Blob alternatives surface from this list.
- **`vercel-rest-api`** — automation scripts that install / remove integrations programmatically.
