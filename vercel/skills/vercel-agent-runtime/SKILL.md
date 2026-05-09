---
name: vercel-agent-runtime
description: Use Vercel's three agentic-infrastructure primitives together — Sandbox (sandboxed code execution from agent tools), Vercel Agent (hosted agentic runtime with built-in state and tools), and Workflows (durable execution for multi-step processes that survive restarts, retries, and human approvals). Use this skill any time agents need to execute generated code safely, when long-running multi-step processes need durability, when "this needs to run for hours / wait for a human / retry on failure" comes up, or when an existing serverless-function approach is straining under workflow complexity. Trigger on any agentic-infrastructure question on Vercel.

# Project context
type: skill
project: skills-library
plugin: vercel
aliases: [vercel-agent-runtime]
tags: [type/skill, plugin/vercel, topic/vercel, topic/agentic, topic/sandbox, topic/workflows]
status: active
---

# Vercel Agent Runtime — Sandbox + Agent + Workflows

Three agentic primitives that go together. **Sandbox** runs untrusted or LLM-generated code safely. **Vercel Agent** is the hosted runtime for agents-as-services with state and tools. **Workflows** is durable execution — functions that survive restarts, do retries, wait for humans, and run for days.

This skill covers all three. Pair with `vercel-ai-sdk` (LLM calls inside the agent), `vercel-ai-gateway` (model routing for the LLM), `vercel-fluid-compute` (function config — Workflows have different limits than vanilla functions), `vercel-storage` (state across steps), `vercel-observability` (trace agent / workflow execution), and `software-engineering-agentic-workflow-engineer` for the application-side multi-agent architecture.

## When to use this skill

- An agent needs to execute code (LLM-generated or user-supplied) safely.
- A multi-step process needs to survive function restarts.
- A workflow waits for a human approval step before continuing.
- A long-running job (minutes to hours) doesn't fit in a single function.
- Retry-with-backoff needs to be durable across restarts.
- An LLM tool needs to run heavyweight computation.
- The team is asking "should we build this as a chained Lambda or a real workflow?"

## Core posture

- **Sandbox is for code execution; Workflows is for orchestration.** They're complementary primitives, not alternatives.
- **Vercel Agent is the hosted-runtime form** when you want agents-as-services without building all the plumbing yourself.
- **Don't reinvent durability.** A chain of cron jobs + retries you've cobbled together is what Workflows replaces.
- **Default to Fluid functions when the task fits.** Reach for these primitives when the task genuinely doesn't.
- **Observability is non-negotiable.** Each primitive emits traces; wire them through.

## Vercel Sandbox

A sandboxed Node execution environment you can call from your code. Designed for agent tools — when an LLM generates code (or a user submits it) and you need to run it without exposing your real server.

```ts
import { Sandbox } from "@vercel/sandbox";

// Inside an AI SDK tool's `execute`:
const codeSandbox: tool({
  description: "Run Python or JavaScript code and return the output",
  parameters: z.object({
    runtime: z.enum(["python", "javascript"]),
    code: z.string(),
  }),
  execute: async ({ runtime, code }) => {
    const sandbox = await Sandbox.create({ runtime });
    const result = await sandbox.run(code, { timeout: 30_000 });
    return { stdout: result.stdout, stderr: result.stderr, exitCode: result.exitCode };
  },
});
```

Use cases:

- **Code interpreter for an AI assistant.** "Calculate the cost of these tiers" → LLM writes Python; Sandbox runs it; result feeds back.
- **User-submitted scripts.** A no-code tool where users define logic; runs in Sandbox.
- **Heavy data manipulation that doesn't belong on the request path.** Sandbox can run for longer and use more memory than a typical function.
- **Untrusted file processing.** Convert / inspect / transform a user upload without exposing your own runtime.

Don't use Sandbox for:

- Standard application code (it's heavier than just running it directly).
- Production-critical routes (Sandbox adds latency and cost; reserved for the niche).
- Persistent state across calls (Sandbox instances are ephemeral).

Verify current pricing and limits — Sandbox is metered separately from regular functions.

Docs: https://vercel.com/sandbox

## Vercel Agent

The hosted runtime for agents-as-services. Vercel Agent gives you:

- An agent process with built-in state.
- Tool integration (similar to AI SDK tools).
- Long-lived execution.
- Multi-turn conversation memory.
- Hosted endpoint per agent.

When to reach for Vercel Agent vs. building your own AI SDK + Workflows + storage stack:

- **Vercel Agent**: when you want an agentic endpoint with batteries included. Faster to ship.
- **AI SDK + your own state + Workflows**: when you need full control over orchestration, custom storage, or the agent shape doesn't fit Vercel Agent's model.

For Slalom defaults: try Vercel Agent first for hosted-agent use cases (a chat assistant for a client's product); fall back to the AI SDK + Workflows pattern for bespoke multi-agent orchestration.

Verify the API shape against current docs at https://vercel.com/agent — this is a newer surface and evolving.

## Vercel Workflows

Durable execution. A workflow is a function that:

- Survives function restarts (state checkpointed).
- Can wait for events (webhook arrivals, time delays, human approvals).
- Has automatic retry with backoff.
- Has its own observability surface (workflow inspector, step-by-step traces).

```ts
// Pseudocode shape — verify against current Workflows SDK
import { workflow, step } from "@vercel/workflows";

export const onboardCustomer = workflow("onboard-customer", async (input: { customerId: string }) => {
  const customer = await step("fetch", () => fetchCustomer(input.customerId));
  await step("send-welcome-email", () => sendEmail(customer.email, "welcome"));

  // Wait for human approval before continuing
  await step("wait-for-approval", () =>
    waitForEvent("manual-approval", { customerId: input.customerId, timeout: "7d" }));

  // Long-running computation — workflow survives function restarts
  const enrichment = await step("enrich", () =>
    enrichCustomerData(input.customerId));

  // Branch based on result
  if (enrichment.tier === "enterprise") {
    await step("notify-account-team", () => notifyAccountTeam(input.customerId));
  }

  await step("mark-onboarded", () => updateCustomer(input.customerId, { onboarded: true }));
});
```

What this gets you:

- **Each `step` is durable.** If the function dies during step 3, on resume the workflow picks up at step 3 with steps 1 and 2 already completed.
- **`waitForEvent` is durable.** Workflow can sleep for 7 days waiting for human approval. The function isn't running during the wait; it resumes when the event arrives.
- **Retry-with-backoff per step.** A failed step retries; you don't re-run the whole workflow.
- **Observable.** Workflow inspector shows you the step graph, status of each, the input/output, retry count.

When to reach for Workflows:

- Multi-step process longer than a single function execution.
- Human-in-the-loop approvals.
- Cron-driven sequences with state.
- Saga patterns (commit, compensate on failure).
- Long-running data pipelines triggered by events.

When to skip:

- Single-shot synchronous routes (just a function).
- Sub-second responses (Workflows have orchestration overhead).
- Stateless transformations (pure functions).

Docs: https://vercel.com/workflows

## How they fit together

Common patterns:

### Pattern 1: Agent uses Sandbox tool

```
User prompt → AI SDK chat → tool call: "run this Python"
                              ↓
                          Sandbox runs the code
                              ↓
                          Result feeds back to the LLM
                              ↓
                          LLM responds to user
```

Sandbox is a tool; the AI SDK orchestrates.

### Pattern 2: Workflow drives a multi-step agent

```
Trigger (webhook / cron) → Workflow starts
                            ↓
                          Step: gather context
                            ↓
                          Step: run agent (AI SDK)
                            ↓
                          Step: act on agent's decision (write to DB, send email)
                            ↓
                          Step: wait for outcome / human review
                            ↓
                          Step: complete
```

Workflow gives the agent durability and human-in-the-loop. The agent provides the reasoning.

### Pattern 3: Vercel Agent + Workflows for hosted multi-agent

```
Hosted agent (Vercel Agent) handles user conversation
   ↓
Triggers a Workflow for any process > 30s
   ↓
Workflow runs autonomously, calls back to agent on completion
   ↓
Agent surfaces result to user
```

Use when the agent needs to kick off long work without blocking the chat.

## Function configuration concerns

These primitives have different limits than vanilla Fluid functions:

- **Sandbox**: per-invocation timeout configurable; usually generous (minutes). Memory selectable. Pricing per second of Sandbox runtime.
- **Vercel Agent**: hosted runtime; pricing per agent-hour or per-event (verify current model).
- **Workflows**: workflow itself can run for days; each step is bounded by function limits.

For chat routes that orchestrate Workflow triggers, your AI SDK route still has standard function limits (`maxDuration`). The Workflow runs separately.

See `vercel-fluid-compute` for the underlying function config; the AI SDK route + Workflow combo is what Slalom uses.

## State and storage

Each primitive has different state defaults:

- **Sandbox**: stateless. Each `Sandbox.create()` is fresh. If you need state across calls, store it externally (Postgres, KV) and inject.
- **Vercel Agent**: built-in conversation state. Attach external storage for app data.
- **Workflows**: durable state in the workflow runtime. Step inputs/outputs persisted automatically. For app-level data, write to your DB during steps.

Common pattern: the workflow itself is the source of truth for "where in the process are we"; your application database is the source of truth for "what does the data look like." Don't duplicate.

## Observability

Each primitive emits traces visible in:

- The workflow inspector (per-step status, retries, durations).
- The agent dashboard (Vercel Agent).
- Function logs (Sandbox executions show up as function calls).

For end-to-end tracing across these + your app, configure OTEL — see `vercel-observability`. The output goes to your tracing backend (Honeycomb, Datadog, Tempo) where you can see the full flow from "user message" through "workflow step 5" to "response back."

## Common failure modes

- **Workflow steps with side effects on retry.** If step 3 retries, sending the email twice. Make steps idempotent or use a deduplication key.
- **Workflow state too big.** Each step's input/output is checkpointed. Don't pass megabytes through the workflow; pass references (an ID) and re-fetch in the next step.
- **Sandbox used as a regular function.** Costs more, slower. Only use Sandbox when you actually need isolation.
- **Vercel Agent + custom orchestration on top.** Often you want one or the other, not both. Pick the abstraction that fits.
- **No timeout on `waitForEvent`.** Workflow runs forever waiting for an approval that's never coming. Always set a timeout.
- **Workflow logic mutating shared resources without locks.** Two workflow instances modifying the same record. Use DB transactions or distributed locks (KV with TTL).
- **Logging full LLM responses in Workflow steps.** State checkpointing inflates fast. Log to OTEL / structured logs, not into step output.

## Output formats

### Workflow design doc

```
# Workflow: [Name]

## Trigger
- {webhook | cron | manual | event}

## Steps
1. {name}: {what it does, expected duration}
2. {name}: ...

## Wait points
- {step}: waits for {event}, timeout {duration}, fallback {action}

## Idempotency
- {step}: idempotent because {reason}
- {step}: deduplicated by {key}

## Failure handling
- {step}: retries {N} with backoff, then {action}

## State
- Workflow state: {what's checkpointed}
- App state: {what's in DB}

## Observability
- Trace target: {Honeycomb / Datadog}
- Alerts: {on-failure routing}

## SLA
- p95 completion: {duration}
- Cost per invocation: {estimate}
```

## How this skill relates to others

- LLM calls inside the agent → `vercel-ai-sdk`.
- Model routing for the LLM → `vercel-ai-gateway`.
- Function config for the orchestrating routes → `vercel-fluid-compute`.
- State across workflow steps → `vercel-storage`.
- Trace agent / workflow execution end-to-end → `vercel-observability`.
- Authentication and rate limits on agent endpoints → `vercel-security`.
- Multi-agent application architecture → `software-engineering-agentic-workflow-engineer`.
- Background-job alternative for simpler cases (queues without durability) → `software-engineering-backend-architect`.

## Source material

- Sandbox: https://vercel.com/sandbox
- Vercel Agent: https://vercel.com/agent
- Workflows: https://vercel.com/workflows
- AI SDK: https://ai-sdk.dev/
- Plugin reference: `../../references/api-surface.md`
