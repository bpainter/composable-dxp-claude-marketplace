---
type: readme
project: skills-library
scope: plugin
plugin: vercel
tags: [type/readme, plugin/vercel]
status: active
---
# vercel

**Platform-implementation skills for shipping on Vercel**

Sits adjacent to `software-engineering`. The software-engineering plugin owns the framework (Next.js routing, server/client, cache, actions); this plugin owns the platform (deploys, edge, storage, observability, security, AI primitives, agentic runtime, REST API). Most engagements pull from both.

## Skills (11)

### Deploy and runtime

| Skill | What it does |
|---|---|
| [[vercel-deploy-pipeline]] | Project setup, Production / Preview / Development environments, domains, Deployment Protection, build configuration, rollback. |
| [[vercel-fluid-compute]] | Fluid Compute, Node vs. Edge runtimes in Next.js 16, function configuration, cold-start mitigation, concurrency. |
| [[vercel-cdn-edge]] | Edge Network, CDN cache, ISR / PPR behavior on Vercel, image optimization metering, bandwidth guardrails. |

### Storage and data

| Skill | What it does |
|---|---|
| [[vercel-storage]] | Blob, KV, Postgres, Edge Config — when to use each, integration patterns, cost shape, alternatives. |

### Operate and secure

| Skill | What it does |
|---|---|
| [[vercel-observability]] | Speed Insights, Web Analytics, Logs, Drains, OTEL integrations (Datadog, Honeycomb, Axiom). |
| [[vercel-security]] | WAF, BotID, Bot Management, Deployment Protection, Trusted IPs, headers, secrets discipline. |

### AI and agentic

| Skill | What it does |
|---|---|
| [[vercel-ai-sdk]] | The Vercel AI SDK — streaming UIs, model adapters, tool calling, structured output, RSC patterns. |
| [[vercel-ai-gateway]] | AI Gateway — multi-provider routing, fallback, cost control, BYO keys vs. gateway billing. |
| [[vercel-v0]] | v0.app design-to-code workflow, prompt patterns, the v0 API, fit with shadcn / Tailwind. |
| [[vercel-agent-runtime]] | Sandbox (sandboxed code execution), Vercel Agent (agentic runtime), Workflows (durable execution). |

### Programmatic operations

| Skill | What it does |
|---|---|
| [[vercel-rest-api]] | Vercel REST API for managing teams, projects, deployments, env vars, domains. Automation patterns. |

## Agent

**`vercel-agent`** — Senior Vercel-platform specialist who knows when to use each skill in this plugin and how to sequence them across deploy, runtime, storage, observability, security, and the AI / agentic surface. Use this agent when work is Vercel-specific (not when it's general Next.js — route those to `software-engineering-agent`).

## Shared references

Every skill in this plugin can cite these category-level references at `references/`:

- **`slalom-context.md`** — Slalom organizational context, Composable DXP practice focus, partner relationship with Vercel
- **`vercel-foundations.md`** — Account / team / project hierarchy, environments, runtimes, the platform mental model
- **`api-surface.md`** — REST API, AI SDK, AI Gateway, v0 API at-a-glance with when-to-use guidance
- **`cli-cheat-sheet.md`** — Vercel CLI commands grouped by workflow (auth, deploy, env, project, log)
- **`marketplace-integrations.md`** — Curated map of Vercel Marketplace integrations for Slalom defaults

## Conventions

- Each skill has a `SKILL.md` with YAML frontmatter and structured sections.
- Tone across all skills: direct, builder-oriented, opinionated. Match Bermon's `preferences.md`.
- All skills assume **Slalom's Composable DXP practice** is the operating context — Next.js + Vercel is the default deploy stack; alternatives only when called out.

## Pairs with

- **`software-engineering`** — owns Next.js 16 deep dives (`-nextjs-routing`, `-nextjs-server-client`, `-nextjs-cache`, `-nextjs-actions`, `-nextjs-errors`, `-nextjs-performance`, `-nextjs-config`). When a question is about the framework, route there; when it's about the platform, stay here.
- **`contentful`** — webhooks revalidate Vercel-cached pages; preview tokens flow into Vercel preview deployments
- **`design`** — `design-handoff` outputs feed into v0 prompts when design-to-code is in play
- **`project-management`** — release planning aligns with the Vercel deploy cadence

## Part of the second-brain marketplace

See the [marketplace README](../README.md) for the full architecture.

## License

MIT.
