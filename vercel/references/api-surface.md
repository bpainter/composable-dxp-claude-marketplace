---
type: reference
project: skills-library
scope: plugin
plugin: vercel
tags: [type/reference, plugin/vercel, scope/foundational, topic/vercel, topic/api]
status: active
---
# Vercel API surface — when to reach for which

Four programmable APIs in the Vercel ecosystem. Each skill in this plugin tells you which one to use; this reference tells you why.

## Decision matrix

| If you need to... | Use | Skill |
|---|---|---|
| Manage projects, deployments, env vars, domains, teams | **REST API** | `vercel-rest-api` |
| Build chat UIs, structured-output generation, tool calling | **Vercel AI SDK** | `vercel-ai-sdk` |
| Route across multiple LLM providers with fallback / observability | **AI Gateway** | `vercel-ai-gateway` |
| Generate UI from prompts; integrate v0 into a pipeline | **v0 API** | `vercel-v0` |
| Trigger a deploy from CI without git | **REST API** (deploy hook URL) | `vercel-deploy-pipeline` |
| Read function logs programmatically | **REST API** (Logs endpoint) or Log Drains | `vercel-observability` |
| Run a sandboxed code execution from an agent | **Sandbox SDK** | `vercel-agent-runtime` |
| Build a durable workflow with retries and human approval | **Workflows SDK** | `vercel-agent-runtime` |

## Vercel REST API

The administrative surface. Manage every Vercel resource programmatically — teams, projects, deployments, env vars, domains, integrations, log drains.

- Auth: Bearer token (Personal Access Token from your account, or Team token via OAuth).
- Base URL: `https://api.vercel.com/`
- Versioned per resource (`/v9/projects`, `/v13/deployments`, etc.). Always include the version.
- Returns JSON; pagination via `limit` + `next` cursor.
- Rate-limited per-account; specifics in headers (`X-RateLimit-*`).

Reach for it when:

- Standing up a new client project programmatically (one Slalom build = one canonical setup script).
- Wiring CI to deploy without git push (deploy hooks).
- Sweeping env vars across many projects.
- Audit / hygiene scripts (find projects without Speed Insights enabled, check for stale env vars).

Don't use it for runtime application work — it's the platform admin surface, not the app data plane.

Docs: https://vercel.com/docs/rest-api

## Vercel AI SDK (`ai-sdk.dev`)

The developer-facing SDK for building AI features in Next.js (and other frameworks). Streaming-first, model-agnostic, RSC-aware.

- npm: `ai`, `@ai-sdk/openai`, `@ai-sdk/anthropic`, `@ai-sdk/google`, etc.
- Server: `streamText`, `generateText`, `streamObject`, `generateObject`, tool calling.
- Client: `useChat`, `useCompletion`, `useObject` hooks, RSC integrations.
- Works against any model the provider adapters support; pair with the AI Gateway for routing.

Reach for it for:

- Streaming chat UIs.
- Single-shot generations (summarize, classify, extract).
- Structured output (JSON schema → typed object).
- Tool calling (the LLM invokes server functions).
- Generative UI (RSC streams UI components, not just text).

Don't use it for:

- Heavy training / fine-tuning workflows (use the model provider's training SDK).
- Pure embedding / RAG indexing (the AI SDK can call embeddings, but you'll want a vector DB SDK alongside).

Docs: https://ai-sdk.dev/

## AI Gateway

The model-routing edge layer. Wraps any AI request, sends it to the configured model provider, and gives you observability + cost control + fallback.

- One API surface; many providers (OpenAI, Anthropic, Google, Mistral, xAI, etc.).
- Routes by model name; you switch providers via config without changing code.
- Aggregated dashboards: token usage, cost, latency, errors.
- Fallback chains (try provider A, fall back to B on error).
- BYO API keys *or* Vercel-managed billing aggregation.

Pair with the AI SDK by setting the AI Gateway as the OpenAI-compatible base URL, or by using the dedicated AI Gateway SDK adapter.

Reach for it when:

- You're using more than one model provider.
- You want one cost dashboard across providers.
- You need provider fallback.
- You want per-team / per-project billing aggregation.

Docs: https://vercel.com/ai-gateway

## v0 API

The v0.app API for design-to-code generation. Programmatic access to the v0 model that produces React + Tailwind + shadcn output.

- npm / fetch endpoints; auth via API key.
- Inputs: prompt + optional image (Figma export, screenshot).
- Outputs: React components, sometimes whole pages, styled with Tailwind / shadcn primitives.
- Iterable — refine via follow-up prompts.

Reach for it when:

- Bootstrapping a new screen from a Figma export.
- Generating internal-tool UI fast.
- Prototyping for stakeholder feedback before Build picks it up.

Don't use it as a replacement for `software-engineering-design-implementation` or for production-grade design system work — v0 outputs are starting points, not final.

Docs: https://v0.app/docs/api

## SDKs (not exposed as external "APIs" but worth listing)

- **Vercel Sandbox** (`@vercel/sandbox`) — sandboxed Node execution from your code. Used for agent code-execution tools.
- **Workflows** (`@vercel/workflows`) — durable execution primitives. Define functions that survive restarts, retries, multi-day waits.
- **Vercel Agent** (the platform's agentic runtime) — exposes agents-as-services with built-in state and tool integration.
- **Vercel Blob** (`@vercel/blob`), **KV** (`@vercel/kv`), **Postgres** (`@vercel/postgres`), **Edge Config** (`@vercel/edge-config`) — storage SDKs covered in `vercel-storage`.

## Cross-cutting concerns

- **Tokens belong in env vars and Vercel-managed secrets, never in code.** Rotate on staff changes.
- **Personal tokens vs. Team tokens.** Personal tokens follow the user; Team tokens are shared. For automation that should outlive a person, use a Team integration token.
- **Rate limits are per token / per resource.** Sweeping operations need backoff.
- **Audit logs.** Pro+ plans expose audit logs via the dashboard and the REST API. Worth wiring into the SIEM for any production engagement.
