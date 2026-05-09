---
name: vercel-ai-sdk
description: Build AI features with the Vercel AI SDK (ai-sdk.dev) — streaming text and structured output, model adapters across providers, tool calling, RSC integrations, useChat / useCompletion / useObject hooks, and the wiring patterns that pair the SDK cleanly with the AI Gateway, Fluid Compute, and Vercel-managed storage. Use this skill any time an AI feature is being built on Next.js, when the team is asking "how do we ship a chat UI", when streaming or tool calling is in scope, when RSC + AI is the question, or when an existing AI integration needs to be standardized. Trigger any time AI SDK or `ai` package usage is implicated.

# Project context
type: skill
project: skills-library
plugin: vercel
aliases: [vercel-ai-sdk]
tags: [type/skill, plugin/vercel, topic/vercel, topic/ai, topic/sdk]
status: active
---

# Vercel AI SDK

The Vercel AI SDK (`ai-sdk.dev`) is the developer-facing SDK for building AI features in Next.js (and other frameworks). Streaming-first, model-agnostic, RSC-aware. This skill owns the SDK integration patterns.

Pair with `vercel-ai-gateway` (route SDK calls through the Gateway for multi-provider + observability + cost control), `vercel-fluid-compute` (`maxDuration` configuration for streaming routes), `vercel-storage` (chat history, embeddings, RAG context), `vercel-observability` (token / cost dashboards), and `software-engineering-ai-engineer` for application-side AI architecture.

## When to use this skill

- Building a chat UI.
- Single-shot generations (summarize, classify, extract).
- Structured output (JSON schema → typed object).
- Tool calling (LLM invokes server functions).
- Generative UI (RSC streams components).
- Migrating from a hand-rolled OpenAI SDK integration.
- Standardizing an existing scattered AI integration.

## Core posture

- **Stream by default.** Modern AI UX expects token-by-token; the SDK is built for it.
- **Provider-agnostic.** Use `@ai-sdk/openai`, `@ai-sdk/anthropic`, etc. through the same `ai` core API. Switching providers is a one-line change.
- **Pair with the AI Gateway** (`vercel-ai-gateway`) on every Slalom build. Gives you observability + fallback + cost control without touching app code.
- **Tool calling is structured, not ad-hoc.** Define tools as Zod schemas; the SDK handles the LLM round-trip.
- **RSC integration is powerful but new.** Reach for it when the UX wins; don't over-engineer.
- **Token budgets are real.** Control input length, set max output, watch the dashboard.

## Install

```bash
pnpm add ai @ai-sdk/openai @ai-sdk/anthropic
# Plus zod for tool/structured-output schemas
pnpm add zod
```

Provider adapters install separately. Common: `@ai-sdk/openai`, `@ai-sdk/anthropic`, `@ai-sdk/google`, `@ai-sdk/mistral`, `@ai-sdk/xai`.

For AI Gateway routing, use `@ai-sdk/openai` adapter pointed at the Gateway's OpenAI-compatible endpoint, or the dedicated AI Gateway provider — see `vercel-ai-gateway`.

## Streaming text

```ts
// app/api/chat/route.ts
import { streamText } from "ai";
import { openai } from "@ai-sdk/openai";

export const maxDuration = 300; // function config — see vercel-fluid-compute

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: openai("gpt-4o"),
    system: "You are a helpful assistant for Slalom Composable DXP work.",
    messages,
    maxOutputTokens: 2000,
    temperature: 0.7,
  });

  return result.toDataStreamResponse();
}
```

Client side:

```tsx
// app/(chat)/chat/page.tsx
"use client";
import { useChat } from "ai/react";

export default function Chat() {
  const { messages, input, handleInputChange, handleSubmit } = useChat();
  return (
    <div>
      {messages.map((m) => (
        <div key={m.id}>{m.role}: {m.content}</div>
      ))}
      <form onSubmit={handleSubmit}>
        <input value={input} onChange={handleInputChange} />
      </form>
    </div>
  );
}
```

`useChat` handles all the streaming wiring — message append, in-flight state, completions, errors. For finer control, `streamText` exposes the raw stream.

## Single-shot generation

```ts
import { generateText } from "ai";

const { text } = await generateText({
  model: openai("gpt-4o-mini"),
  prompt: "Summarize this in one sentence: ...",
});
```

Use `generateText` when you don't need streaming (background jobs, server-side enrichment).

## Structured output

```ts
import { generateObject } from "ai";
import { z } from "zod";

const { object } = await generateObject({
  model: openai("gpt-4o"),
  schema: z.object({
    title: z.string(),
    tags: z.array(z.string()).max(5),
    audience: z.enum(["founder", "investor", "general-counsel"]),
    summary: z.string().max(280),
  }),
  prompt: `Extract metadata from this article: ${article}`,
});

// object is typed: { title: string; tags: string[]; audience: ...; summary: string }
```

`generateObject` enforces the Zod schema — the SDK retries with feedback if the LLM produces invalid output. Replaces hand-rolled "parse JSON, validate, retry" loops.

For streaming structured output, use `streamObject`:

```ts
const { partialObjectStream } = streamObject({
  model: openai("gpt-4o"),
  schema: ProductSchema,
  prompt: "...",
});

for await (const partial of partialObjectStream) {
  // partial is the in-progress object as fields stream in
  yield partial;
}
```

## Tool calling

```ts
import { streamText, tool } from "ai";
import { z } from "zod";
import { searchProducts } from "@/lib/products";
import { sendEmail } from "@/lib/email";

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: openai("gpt-4o"),
    messages,
    tools: {
      searchProducts: tool({
        description: "Search the product catalog for items matching a query",
        parameters: z.object({
          query: z.string(),
          limit: z.number().min(1).max(20).default(10),
        }),
        execute: async ({ query, limit }) => {
          return await searchProducts(query, limit);
        },
      }),
      sendEmail: tool({
        description: "Send an email to a customer",
        parameters: z.object({
          to: z.string().email(),
          subject: z.string(),
          body: z.string(),
        }),
        execute: async ({ to, subject, body }) => {
          await sendEmail(to, subject, body);
          return { sent: true };
        },
      }),
    },
    maxSteps: 5, // safety bound on tool-call iterations
  });

  return result.toDataStreamResponse();
}
```

The LLM picks tools based on conversation; the SDK runs the `execute`, feeds results back, the LLM continues. `maxSteps` prevents infinite loops.

For destructive tools (sendEmail, writeToDatabase), require human confirmation:

```ts
sendEmail: tool({
  description: "...",
  parameters: z.object({...}),
  // No execute — the LLM proposes the tool call, the UI surfaces it for human approval
}),
```

The `useChat` UI can render tool-call previews and let users approve before execution.

## RSC integration (`@ai-sdk/rsc`)

Generative UI: the LLM streams React components, not just text.

```tsx
// app/actions.tsx
"use server";

import { streamUI } from "@ai-sdk/rsc";
import { openai } from "@ai-sdk/openai";
import { ProductCard } from "@/components/blocks/product-card";

export async function generateAnswer(query: string) {
  const ui = await streamUI({
    model: openai("gpt-4o"),
    prompt: query,
    text: ({ content }) => <p>{content}</p>,
    tools: {
      showProduct: {
        description: "Show a product card",
        parameters: z.object({ id: z.string() }),
        generate: async function* ({ id }) {
          yield <ProductCardSkeleton />;
          const product = await getProduct(id);
          return <ProductCard {...product} />;
        },
      },
    },
  });
  return ui;
}
```

Server streams the rendered components; client receives them as RSC payloads. Powerful for AI assistants that need to show tables, charts, product cards, not just text.

When to reach for RSC integration:

- AI-driven dashboards.
- Conversational commerce (the assistant shows product cards inline).
- Document Q&A with structured extracts.
- Generative reports.

When to skip:

- Plain chat UI (`useChat` + text is simpler).
- Streaming-output workflows that don't need UI generation.

## RAG patterns

Retrieval-Augmented Generation pairs the AI SDK with a vector DB:

```ts
import { embed } from "ai";
import { openai } from "@ai-sdk/openai";

// 1. Embed the query
const { embedding } = await embed({
  model: openai.embedding("text-embedding-3-small"),
  value: userQuery,
});

// 2. Find similar documents (Pinecone / pgvector / Weaviate / etc.)
const docs = await vectorDB.query({ vector: embedding, topK: 5 });

// 3. Generate with context
const result = streamText({
  model: openai("gpt-4o"),
  system: `Use this context to answer: ${docs.map(d => d.text).join("\n\n")}`,
  prompt: userQuery,
});
```

For Slalom RAG defaults:

- **Embeddings**: `text-embedding-3-small` (cheap, fast) or `text-embedding-3-large` (better recall).
- **Vector DB**: Pinecone (managed, fast), pgvector on Vercel Postgres (when single-stack matters), Weaviate (when you need hybrid search).
- **Chunking**: 500–1500 token chunks with overlap. Tune per content shape.

For deep RAG patterns, see `software-engineering-ai-engineer`.

## Function configuration (Vercel-side)

AI streaming routes typically need:

```ts
// Per-route (App Router)
export const maxDuration = 300; // up to plan limit
export const runtime = "nodejs"; // Edge for tiny calls; Node for tool-heavy

// OR via vercel.json
{
  "functions": {
    "app/api/chat/route.ts": { "maxDuration": 300, "memory": 1024 }
  }
}
```

Why:

- Streaming may run for minutes on long-form generations or multi-tool flows.
- Default `maxDuration` (Pro: 60s) is too short for serious chat.
- Memory: 1024MB is fine for most LLM streaming; bump to 2048MB if doing image generation alongside.

See `vercel-fluid-compute`.

## Pairing with AI Gateway

Default Slalom posture: route every AI SDK call through the AI Gateway.

```ts
import { createOpenAI } from "@ai-sdk/openai";

const openai = createOpenAI({
  baseURL: process.env.AI_GATEWAY_URL,
  apiKey: process.env.AI_GATEWAY_KEY,
});

// Now every call through this client lands in the Gateway dashboard
```

Or use the AI Gateway provider directly (`@ai-sdk/gateway` or equivalent):

```ts
import { gateway } from "@ai-sdk/gateway";

const result = streamText({
  model: gateway("openai/gpt-4o"),
  // ...
});
```

Multi-provider routing, fallback, observability — all configured at the Gateway layer, not in app code. See `vercel-ai-gateway`.

## Cost shape

Per request, AI cost has three drivers:

1. **Input tokens.** Every message in `messages` + system + tool definitions. Long conversations balloon this.
2. **Output tokens.** Streamed response. `maxOutputTokens` caps this.
3. **Tool round-trips.** Each tool call adds an LLM-side decision + an execution. `maxSteps` caps this.

Optimization order:

- **Trim input.** Truncate old chat history; summarize when conversation gets long.
- **Cap output.** `maxOutputTokens` deliberately set per route.
- **Pick the right model.** `gpt-4o-mini` for routine; `gpt-4o` for hard cases. AI Gateway makes the swap easy.
- **Cache where deterministic.** Same prompt + temperature 0 → cache the result.
- **Streaming over polling.** No client retry loops eating tokens.

For visibility into cost, see `vercel-ai-gateway`.

## Common failure modes

- **Function timeout on long stream.** `maxDuration` too short. Bump it.
- **Tool calls infinite-looping.** `maxSteps` not set. Cap it.
- **JSON schema rejected by the LLM.** Schema too strict; tune via Zod descriptions or fallback to `generateText` + manual parse.
- **Hand-rolled JSON parsing.** Use `generateObject` instead.
- **`useChat` re-render loop.** Usually missing memoization on the messages array.
- **Cold start eating UX.** AI streaming routes hit the same Fluid Compute model as everything else. See `vercel-fluid-compute`.
- **Tokens leaking via `console.log`.** Don't log full prompts in production; PII risk.
- **No cost dashboard.** AI bills surprise. Wire AI Gateway dashboard before launch.
- **Provider downtime kills the feature.** Always have fallback via AI Gateway.

## Output formats

### AI feature spec

```
# AI Feature: [Name]

## What
{user-facing capability}

## Models
- Primary: {provider/model via Gateway}
- Fallback: {alternative}

## Patterns
- Streaming: {yes/no}
- Tool calling: {tools list, with destructive ones flagged}
- Structured output: {schemas}
- RAG: {if yes — embeddings model, vector DB}

## Function config
- maxDuration: {N}s
- memory: {MB}
- runtime: {nodejs | edge}

## Cost shape
- Expected tokens per call: {input ~N, output ~M}
- Volume: {calls per day}
- Estimated $/month: {N}
- Cap: {hard limit via Gateway}

## Observability
- Gateway dashboard: ✓
- App-side logging: structured JSON, no full prompt

## Failure modes & mitigations
- Provider down → fallback via Gateway
- Token cost spike → Gateway alerts, app-side rate limiting
- Tool error → graceful UI message, retry from user

## Risks
{prompt injection, PII, hallucination, others}
```

## How this skill relates to others

- AI calls flow through → `vercel-ai-gateway` (multi-provider, observability, cost).
- Function config for streaming → `vercel-fluid-compute`.
- Chat history / embeddings / RAG context → `vercel-storage`.
- Token / cost / latency dashboards → `vercel-observability`.
- Rate limiting / abuse protection → `vercel-security`.
- Sandboxed code execution from a tool → `vercel-agent-runtime`.
- Application-side AI architecture (model selection, prompt engineering, evals) → `software-engineering-ai-engineer`.
- Multi-agent orchestration → `software-engineering-agentic-workflow-engineer`.

## Source material

- Vercel AI SDK: https://ai-sdk.dev/
- AI Gateway: https://vercel.com/ai-gateway
- Vercel AI overview: https://vercel.com/ai
- Plugin reference: `../../references/api-surface.md`
