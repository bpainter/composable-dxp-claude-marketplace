---
name: vercel-ai-gateway
description: Use the Vercel AI Gateway as the single edge layer in front of every AI provider — multi-provider routing across OpenAI / Anthropic / Google / Mistral / xAI / etc., fallback chains for provider outages, aggregated observability (token usage, cost, latency, errors), cost guardrails, and BYO-keys vs. Vercel-billed aggregation. Use this skill any time AI features are being built or extended on Vercel, when "we keep hitting OpenAI rate limits" comes up, when multiple model providers are in play, when a cost dashboard is needed, or when a fallback strategy is being designed. Trigger on any AI routing or AI cost question.

# Project context
type: skill
project: skills-library
plugin: vercel
aliases: [vercel-ai-gateway]
tags: [type/skill, plugin/vercel, topic/vercel, topic/ai, topic/gateway]
status: active
---

# Vercel AI Gateway

The AI Gateway sits between your app and AI providers. One API surface; many providers; aggregated observability + fallback + cost control.

This skill owns the Gateway integration. Pair with `vercel-ai-sdk` (the SDK that calls the Gateway), `vercel-fluid-compute` (function config for streaming), `vercel-observability` (the Gateway's dashboard is a key surface), `vercel-security` (rate limiting and key management), and `software-engineering-ai-engineer` for application-side model selection.

## When to use this skill

- Greenfield AI feature — plan the Gateway integration before code.
- Existing direct-to-provider integration — migrate behind the Gateway.
- Multiple model providers in play — consolidate visibility.
- Provider outages causing user-facing failures — add fallback.
- Cost spike investigation — Gateway dashboard is where you look.
- BYO-keys vs. consolidated billing decision.

## Core posture

- **Default to Gateway routing on every Slalom AI feature.** Even with one provider today, the Gateway gives you instant observability and an easy fallback path tomorrow.
- **One configuration surface.** Models, fallback chains, rate limits, cost caps — all in the Gateway dashboard, not scattered through app code.
- **BYO keys for enterprise clients with existing contracts**, Vercel-billed aggregation for smaller engagements where consolidated invoicing simplifies things.
- **Treat the Gateway URL as a secret.** If someone has it, they can spend tokens on your account.

## What the Gateway gives you

| Capability | How |
|---|---|
| **Multi-provider** | Route by model name. `gpt-4o` → OpenAI; `claude-sonnet-4` → Anthropic; one API. |
| **Fallback** | Try primary; on error, try secondary. Configurable chains per route. |
| **Observability** | Aggregated dashboard: tokens, cost, latency, errors, by provider / model / project / time. |
| **Cost control** | Spend caps, alerts, per-key budgets. |
| **Rate limiting** | Provider rate limits hidden behind Gateway smoothing. |
| **Caching** | Optional response caching for deterministic prompts (configurable). |
| **Logs** | Per-request log of input / output / metadata. |
| **BYO keys** | Bring provider keys; Gateway routes through them. Or use Vercel-billed aggregation. |

## Wiring the Gateway

Two patterns:

### Pattern 1: AI SDK + Gateway-as-OpenAI-compat

The Gateway exposes an OpenAI-compatible endpoint. Use any AI SDK provider that targets OpenAI's API and point it at the Gateway:

```ts
// lib/ai/client.ts
import { createOpenAI } from "@ai-sdk/openai";

export const openai = createOpenAI({
  baseURL: process.env.AI_GATEWAY_URL,    // e.g., https://gateway.ai.cloudflare.com/<your-id>
  apiKey: process.env.AI_GATEWAY_API_KEY,  // your Gateway key, NOT a provider key
});

// Now every call routes through the Gateway:
const result = await streamText({
  model: openai("gpt-4o"),
  // ...
});
```

The Gateway sees the model name (`gpt-4o`), looks up the configured provider (OpenAI), routes there, and tracks the call.

### Pattern 2: Dedicated Gateway provider

The Vercel AI SDK's Gateway adapter:

```ts
import { gateway } from "@ai-sdk/gateway";

const result = await streamText({
  model: gateway("openai/gpt-4o"),
  // ...
});

// Switch providers by model name:
const r2 = await streamText({
  model: gateway("anthropic/claude-sonnet-4"),
  // ...
});
```

Provider-prefixed model names (`openai/...`, `anthropic/...`) are explicit. Easier to grep for in code than the OpenAI-compat baseURL pattern.

## Model routing

Every AI call has a model name. The Gateway maps that name to one of:

- A specific provider's model (`openai/gpt-4o` → OpenAI's gpt-4o).
- A custom alias you've defined (`fast` → `openai/gpt-4o-mini`).
- A fallback chain (`production-chat` → primary `claude-sonnet-4`, fallback `gpt-4o`).

Configuration is in the Gateway dashboard. Aliases let you keep app code stable while iterating on provider choice:

```
Gateway alias: "production-chat"
  Primary: anthropic/claude-sonnet-4
  Fallback 1: openai/gpt-4o
  Fallback 2: openai/gpt-4o-mini
```

```ts
// app code references the alias, never the provider directly
const result = streamText({ model: gateway("production-chat"), messages });
```

Switch primary providers without redeploying app code. This is the operational win.

## Fallback strategy

Configure fallback per route or per alias. Patterns:

- **Quality-equivalent fallback.** Primary `claude-sonnet-4` → fallback `gpt-4o`. Degraded but usable on Anthropic outage.
- **Cheaper-on-fallback.** Primary `gpt-4o` → fallback `gpt-4o-mini`. Cost-saving when primary is throttled.
- **Multi-region.** Primary `azure-openai/eu` → fallback `azure-openai/us`. For data-residency sensitivity.

Trigger conditions:

- HTTP errors (5xx).
- Rate limit (429).
- Timeout.
- Custom (via Gateway logic).

Test fallback paths in non-prod before relying on them.

## Cost observability

The Gateway dashboard surfaces:

- **Token usage by provider** — input + output, per model, per time window.
- **Cost** — aggregated and per-provider, with currency conversion.
- **Latency** — p50 / p95 / p99 per provider.
- **Error rate** — by provider and error type.
- **Per-project / per-key breakdown** — useful for multi-team or multi-tenant.

For Slalom defaults: dashboard reviewed weekly during active dev, and on a monthly cadence post-launch. Set alerts on:

- Cost burn rate (e.g., $X projected month-end vs. budget).
- Error rate spike (provider issue).
- Latency p95 regression (model degradation).

## Cost control

Knobs:

- **Spend caps.** Hard limit; over-cap requests fail. Configure per project / key.
- **Rate limits.** Per-key requests/sec or tokens/min. Smooth provider rate-limit cliffs.
- **Per-model cost caps.** Limit `gpt-4o` to $X/day, force fallback to `gpt-4o-mini`.

Slalom defaults:

- Spend cap on every Production project.
- Alert at 50%, 80%, 100% of monthly budget.
- Per-environment caps (Production budget separate from Preview).

## Rate limiting

The Gateway smooths provider rate limits — routes traffic across multiple keys, queues briefly during bursts, and surfaces 429s only when you've hit your *own* configured ceiling.

Two layers:

- **Gateway-side**: per-key configurable rate limits.
- **Provider-side**: each provider has its own limits (e.g., OpenAI tier-2 rate). Gateway respects the upstream limit and queues / fails accordingly.

App code rarely needs to handle rate limits when the Gateway is in front. Errors surface as standard errors via the AI SDK.

## BYO keys vs. Vercel-billed aggregation

Two billing models:

### BYO (Bring Your Own keys)

Enter your OpenAI / Anthropic / etc. API key in the Gateway dashboard. Gateway uses it for routing. **You're billed by each provider directly.** Gateway is "free" (or very low cost) — you pay for the routing infrastructure, not the tokens.

Reach for BYO when:

- Enterprise client has existing provider contracts (volume discounts).
- Compliance requires direct provider relationships.
- Multi-app accounting needs per-app provider breakdowns.

### Vercel-billed aggregation

Vercel issues you a single Gateway key. **Vercel bills you for tokens consumed** (passed through, plus Gateway margin). Simpler invoicing.

Reach for aggregation when:

- Smaller engagement, not yet at provider volume tiers.
- Consolidated billing simplifies finance.
- Faster setup (no per-provider account setup).

For Slalom defaults: aggregation for early-stage builds, BYO once volume justifies negotiating provider contracts.

## Caching

The Gateway can optionally cache responses for deterministic prompts (same input + same model + temperature 0). When enabled:

- Cached responses skip the provider entirely. Latency near-zero, cost zero.
- Cache key is a hash of the request shape. Anything that varies invalidates.

Reach for it when:

- Embedding generation on the same input (deterministic, common to repeat).
- Classification with stable taxonomies.
- "Featured FAQs" generated from a stable corpus.

Skip when:

- Streaming chat (stateful, conversational; never cache-friendly).
- Anything user-facing that should feel fresh.
- Anything with `temperature > 0` (intentionally non-deterministic).

## Migration from direct-to-provider

For an existing app calling OpenAI directly:

```diff
- import OpenAI from "openai";
- const openai = new OpenAI({ apiKey: process.env.OPENAI_API_KEY });
+ import { createOpenAI } from "@ai-sdk/openai";
+ const openai = createOpenAI({
+   baseURL: process.env.AI_GATEWAY_URL,
+   apiKey: process.env.AI_GATEWAY_API_KEY,
+ });
```

Or for the AI SDK with Gateway:

```diff
- import { openai } from "@ai-sdk/openai";
+ import { gateway } from "@ai-sdk/gateway";
- const result = streamText({ model: openai("gpt-4o"), ... });
+ const result = streamText({ model: gateway("openai/gpt-4o"), ... });
```

Verify in the Gateway dashboard that calls are landing. Decommission the direct-provider env vars after a quiet period.

## Common failure modes

- **Gateway URL committed to repo.** It's a secret. Scrub from code; rotate the Gateway key.
- **No fallback configured.** Provider outage → user-facing 500s. Configure at least one fallback per production model.
- **Spend cap not set.** Runaway prompt loop or DDoS prints a five-figure bill.
- **Aggregation billing surprise at scale.** When you hit provider-volume tiers, BYO usually beats aggregation. Re-evaluate quarterly.
- **Caching enabled on streaming chat.** Different users see same response. Disable caching for chat routes.
- **Direct-to-provider calls bypass the Gateway.** A stray import (`@ai-sdk/openai` directly) sneaks past. Audit `vercel-ai-gateway` config in CI: lint for non-Gateway imports.
- **No alerts on the Gateway dashboard.** Token spike goes unnoticed for days.

## Output formats

### Gateway configuration plan

```
# AI Gateway: [Project]

## Aliases
| Alias | Primary | Fallback 1 | Fallback 2 | Use case |
|---|---|---|---|---|
| production-chat | anthropic/claude-sonnet-4 | openai/gpt-4o | openai/gpt-4o-mini | Customer-facing chat |
| structured-extract | openai/gpt-4o | anthropic/claude-sonnet-4 | (fail) | Server-side extraction |
| embeddings | openai/text-embedding-3-small | (none) | (fail) | RAG / semantic search |

## Billing
- Mode: {BYO | Aggregated}
- Provider keys (BYO): {list, with rotation cadence}

## Rate limits
| Key | RPM | TPM | Notes |
|---|---|---|---|

## Spend cap
- Total: {$N/month}
- Alerts: {addresses, thresholds}

## Caching
- Enabled for: {aliases / routes}
- Disabled for: {aliases / routes}

## Migration plan
- {if migrating from direct provider}
```

## How this skill relates to others

- The SDK that calls the Gateway → `vercel-ai-sdk`.
- Function config for AI streaming routes → `vercel-fluid-compute`.
- Cost / token / latency dashboards → `vercel-observability`.
- Rate limit + abuse protection complementary to Gateway → `vercel-security`.
- App-side model selection / prompt engineering → `software-engineering-ai-engineer`.
- Multi-agent orchestration on top of the Gateway → `software-engineering-agentic-workflow-engineer`.

## Source material

- AI Gateway: https://vercel.com/ai-gateway
- Vercel AI overview: https://vercel.com/ai
- AI SDK: https://ai-sdk.dev/
- Plugin reference: `../../references/api-surface.md`
