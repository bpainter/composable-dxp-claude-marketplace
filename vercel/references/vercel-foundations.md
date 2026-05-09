---
type: reference
project: skills-library
scope: plugin
plugin: vercel
tags: [type/reference, plugin/vercel, scope/foundational, topic/vercel]
status: active
---
# Vercel foundations — the platform mental model every skill assumes

The vocabulary and architecture every Vercel skill in this plugin builds on. Read once, cite from every skill.

## The hierarchy

```
Account (Personal | Team)
  └── Team (Hobby | Pro | Enterprise) — billing, RBAC, SSO scope
        └── Project — one git repo (or one set of build settings)
              ├── Environments
              │     ├── Production           (the canonical deploy)
              │     ├── Preview              (one per non-prod git branch)
              │     └── Development          (vercel dev local; not a deployed env)
              ├── Domains                    (production + alias domains)
              ├── Environment Variables       (scoped per environment)
              ├── Deployments                (immutable, one per push)
              ├── Functions                  (Serverless / Edge / Fluid)
              ├── Storage bindings           (Blob, KV, Postgres, Edge Config)
              ├── Integrations               (marketplace add-ons)
              └── Settings (build, runtime, security, observability)
```

Things to internalize:

- **A Project maps to a git repo, not to a domain.** One repo can deploy to many environments under one project. Multi-domain or multi-region scenarios are configured inside one project, not across many.
- **Deployments are immutable.** Each git push produces a new deployment with a unique URL (`<project>-<hash>-<scope>.vercel.app`). Production is an alias that points at the latest production deployment. Rolling back means re-aliasing to a prior deployment — fast, atomic.
- **Preview deployments are URL-stable per branch.** A preview URL like `<project>-git-<branch>-<scope>.vercel.app` always points at the head of that branch. Pair this with Contentful preview URLs for content-side draft workflows.
- **Environment variables are environment-scoped.** A var in Production is not in Preview unless duplicated. Encrypt secrets via the dashboard or CLI; never commit them.
- **Domains are aliases.** The production domain (`example.com`) aliases to the latest production deployment. Custom alias domains (`staging.example.com`) can alias to a specific preview environment if you want a stable staging URL.

## Runtimes

In Next.js 16 on Vercel, the runtime story is:

- **Node (default).** Full Node ecosystem, Fluid Compute concurrency, longer cold-start than Edge but with much higher per-invocation throughput.
- **Edge runtime.** V8 isolates, near-zero cold start, smaller package compatibility, code-size budget. Best for tiny latency-sensitive paths (auth check, A/B routing, redirect logic).
- **Fluid Compute.** A Vercel-native concurrency model that lets a single Node Function instance serve many concurrent requests, dramatically lowering cold-start cost and per-invocation pricing. **Default for new projects on supported plans.** Not a separate runtime — a way the Node runtime executes.

Default posture: Node + Fluid Compute. Reach for Edge only when the call site has a measurable latency requirement and is package-compat-friendly.

`proxy.ts` (the Next.js 16 replacement for `middleware.ts`) is **Node-only**. The Edge runtime for proxy was deprecated.

## Environments

Three default environments per project:

| Environment | Source | When it deploys | Use |
|---|---|---|---|
| Production | git push to default branch (`main`) | On every push | The customer-facing deploy |
| Preview | git push to any non-default branch / PR | On every push | Per-branch QA, stakeholder review |
| Development | `vercel dev` locally | On demand | Local development with prod-like env |

Custom environments (Pro / Enterprise) let you add `staging`, `qa`, etc. with their own env-var sets. Use sparingly — environment proliferation makes env-var drift much harder to manage.

## The deploy lifecycle

```
git push
  ↓
Vercel webhook receives the event
  ↓
Build starts on Vercel build infrastructure
  - install deps
  - run build command (next build)
  - emit output (.next/, public/, function bundles)
  ↓
Deployment created (immutable, unique URL)
  ↓
If production branch: alias the production domain to this deployment
If preview branch: alias the preview-branch URL
  ↓
Speed Insights + Analytics start collecting on next visit
```

A failed build leaves the previous deployment in place — production never goes down from a bad build. A failed runtime (e.g., a function throwing) does affect production until you roll back via re-alias.

## Storage primitives

Four Vercel-native storage options. Each maps to a partner-managed service:

| Vercel | Underlying | What it is | Reach for it when |
|---|---|---|---|
| **Blob** | Vercel-managed (S3-compatible) | Object storage with a SDK + `next/image` integration | User uploads, CMS-uploaded media, downloadable assets |
| **KV** | Upstash Redis | Edge-replicated KV with Redis semantics | Sessions, feature flags, rate limit counters, hot reads |
| **Postgres** | Neon | Serverless Postgres (branching, autoscale) | Standard relational data, transactional writes |
| **Edge Config** | Vercel | Read-only-fast config served from the edge | Feature flags, redirects, A/B variants |

For external storage choices, see `vercel-storage`.

## Observability primitives

| Tool | What it tells you | Pricing |
|---|---|---|
| **Speed Insights** | Real-user Core Web Vitals (LCP, INP, CLS) | Per-event, generous free tier |
| **Web Analytics** | Pageviews, top pages, referrers (privacy-friendly) | Per-event, generous free tier |
| **Logs** | Function logs in the dashboard | Recent retention (24h–30d depending on plan) |
| **Log Drains** | Stream logs to external store (Datadog, Axiom, Honeycomb, S3) | Plan-dependent, often Enterprise |
| **OpenTelemetry** | Native trace export to OTLP-compatible backends | Plan-dependent |

For deeper coverage, see `vercel-observability`.

## Security primitives

| Feature | What it is | Plan |
|---|---|---|
| **Deployment Protection** | SSO / password / Bypass Tokens on non-public deploys | All plans |
| **WAF (Web Application Firewall)** | Custom firewall rules at the edge | Pro+ |
| **BotID** | Server-side bot identification | Pro+ |
| **Bot Management** | Rule-based bot allow / block / challenge | Pro+ |
| **Trusted IPs** | Allowlist origins for sensitive routes | Enterprise |
| **DDoS Mitigation** | Always-on at the edge | All plans |
| **SSO + SAML** | Org-level SSO | Enterprise |
| **HIPAA BAA** | Compliance posture | Enterprise (case-by-case) |

For deeper coverage, see `vercel-security`.

## Pricing model — what's metered

The categories that matter for cost on a typical Slalom build:

- **Bandwidth (egress).** Cached responses are usually fine; uncached, dynamic pages cost more. Image Optimization output counts here.
- **Function executions.** Per invocation. Fluid Compute concurrency dramatically reduces this. Long-running functions (`maxDuration`) cost more.
- **Image Optimization transforms.** Each unique (source, width, format, quality) is a transform. `sizes` discipline saves money.
- **Storage operations.** Blob ops, KV reads/writes, Postgres compute hours.
- **AI Gateway tokens.** Per-token, per-model. Multiplied if streaming + reasoning + tool calls.
- **Speed Insights / Analytics events.** Per event past the free tier.

Set a spend cap on Team accounts. Alert before you hit it.

## Plans (high-level)

| Plan | Slalom usage |
|---|---|
| **Hobby** | Personal projects, never client work |
| **Pro** | Standard Slalom engagements (single team, multiple projects) |
| **Enterprise** | Complex Slalom engagements (SSO, SAML, Trusted IPs, dedicated support, BAA, log drains, custom contract) |

Confirm the client's plan tier before scoping security or observability work — many features are gated.

## Build configuration surface

Most Vercel projects have a small set of knobs that decide build behavior:

- **`vercel.json`** — routing, rewrites, redirects, headers, function configuration. Minimal in most Slalom projects; Next.js handles most of this.
- **Project Settings (dashboard)** — framework preset, root directory, build command, install command, output directory, Node version, environment variables.
- **`next.config.mjs`** — Next.js-side configuration. Where `images.remotePatterns`, `i18n`, experimental flags live.
- **Environment variables** — managed in dashboard or via CLI / REST API. Encrypted.

## Source documents

- Vercel docs: https://vercel.com/docs
- Fluid Compute: https://vercel.com/fluid
- CDN: https://vercel.com/cdn
- Previews: https://vercel.com/products/previews
- Observability: https://vercel.com/products/observability
- Security overview: https://vercel.com/security
- AI: https://vercel.com/ai
- AI Gateway: https://vercel.com/ai-gateway
- Sandbox: https://vercel.com/sandbox
- Agent: https://vercel.com/agent
- Workflows: https://vercel.com/workflows
- v0: https://v0.app/
- Marketing sites solution: https://vercel.com/solutions/marketing-sites
- Web apps solution: https://vercel.com/solutions/web-apps
- REST API: https://vercel.com/docs/rest-api
