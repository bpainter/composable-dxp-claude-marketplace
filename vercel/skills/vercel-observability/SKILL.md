---
name: vercel-observability
description: Wire end-to-end observability on a Vercel project — Speed Insights for real-user Core Web Vitals, Web Analytics for traffic, the dashboard's function logs and the OpenTelemetry / Log Drains exits to long-term stores (Datadog, Honeycomb, Axiom, S3), alerting, dashboards, and incident response. Use this skill when standing up a new project, when "we have no visibility" comes up, when a slow page lands without root cause, when an incident needs forensic logs past Vercel's retention, or when an SRE-style review is in flight. Trigger any time the question is how do we see what's happening on Vercel.

# Project context
type: skill
project: skills-library
plugin: vercel
aliases: [vercel-observability]
tags: [type/skill, plugin/vercel, topic/vercel, topic/observability]
status: active
---

# Vercel Observability

Owns the see-what's-happening layer. Pair with `vercel-deploy-pipeline` (turn on Speed Insights / Analytics on day one), `vercel-fluid-compute` (function logs), `vercel-cdn-edge` (cache hit / miss observability), `vercel-security` (audit logs), and `vercel-rest-api` (programmatic log fetches and dashboard automation).

## When to use this skill

- Greenfield project — wiring observability from week one.
- "We don't know what's happening on the site."
- Speed Insights and Web Analytics enablement.
- Setting up Log Drains to a long-term store.
- Incident response — pulling logs past Vercel's retention.
- OpenTelemetry tracing across functions and integrations.
- Cost / SRE review of an existing project.

## Core posture

- **Speed Insights + Web Analytics on every Slalom production project.** From day one. Non-negotiable.
- **Long-term log retention via Drain.** Vercel's dashboard logs are short-lived; ship them to Datadog / Axiom / Honeycomb for forensics.
- **OTEL when it's available.** Tracing across services beats siloed log search.
- **Alerts before launch.** Configure alerts during build, not after the first incident.
- **Cost is observable too.** Set spend caps, alert on burn rate, don't wait for the bill.

## What Vercel gives you natively

| Tool | What it tells you | Where to enable |
|---|---|---|
| **Speed Insights** | Real-user Core Web Vitals (LCP, INP, CLS) per route | Project Settings → Speed Insights, install `@vercel/speed-insights` |
| **Web Analytics** | Pageviews, top pages, referrers, top countries | Project Settings → Web Analytics, install `@vercel/analytics` |
| **Function Logs** | Recent function execution logs in dashboard | Always on; retention plan-dependent |
| **Build Logs** | Build output per deployment | Always on |
| **Audit Logs** (Pro+) | Who did what in the dashboard | Settings → Audit |

What it doesn't give you natively (without integration):

- Distributed tracing across non-Vercel services.
- Long-term log retention for forensics.
- Custom metric dashboards beyond Speed Insights / Analytics.
- Alerting on arbitrary log patterns.

## Speed Insights — wiring

```tsx
// app/layout.tsx
import { SpeedInsights } from "@vercel/speed-insights/next";

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body>{children}<SpeedInsights /></body>
    </html>
  );
}
```

Once installed, the dashboard shows:

- **LCP** — Largest Contentful Paint, p75 per route.
- **INP** — Interaction to Next Paint (replaces FID), p75 per route.
- **CLS** — Cumulative Layout Shift, p75 per route.
- **TTFB** — Time to First Byte, p75 per route.
- **FCP** — First Contentful Paint.

Per-route breakdowns make it easy to spot the culprit. Pair with `software-engineering-nextjs-performance` for the framework-side fixes.

Pricing: per-event past the free tier; scales with traffic. For high-traffic sites, sample (10–25% sample rate often enough).

## Web Analytics — wiring

```tsx
import { Analytics } from "@vercel/analytics/next";

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>{children}<Analytics /></body>
    </html>
  );
}
```

Dashboard surfaces:

- Pageviews / sessions / visitors.
- Top pages.
- Top referrers.
- Country breakdown.
- Custom events (`track('signup', { plan: 'pro' })`).

Privacy-friendly — doesn't use cookies; aggregates anonymized data. For richer behavioral analytics, integrate Segment / Amplitude / GA4 alongside.

## Function logs

The dashboard's Functions tab shows recent function executions with:

- Status code.
- Duration.
- Memory used.
- Region.
- Function output / errors.

Retention varies by plan (24h on Hobby up to 30+ days on Enterprise). For incident forensics or long-term analysis, ship to a Drain.

`console.log` / `console.error` from your code shows up in function logs. Use structured logging (JSON) for downstream consumability:

```ts
function logEvent(level: "info" | "warn" | "error", event: string, data: Record<string, unknown>) {
  console.log(JSON.stringify({ ts: new Date().toISOString(), level, event, ...data }));
}
```

Structured logs land cleanly in Datadog / Axiom / Honeycomb without parsing tricks.

## Log Drains

Log Drains stream all your project logs to an external destination. Available for:

- **Datadog**
- **Axiom**
- **Honeycomb**
- **S3** (raw archive)
- **HTTPS** (custom endpoint)
- Many others via marketplace integrations

Configure in Project Settings → Logs → Drains. Each drain has a delivery format (JSON / NDJSON) and optional filtering.

Slalom defaults:

- **Standard engagement** → Axiom (cheap, fast, integrates well).
- **Enterprise with existing Datadog** → Datadog.
- **Enterprise with existing SIEM (Splunk, Sumo)** → HTTPS drain + custom endpoint that forwards.

Pricing: drain itself is plan-gated (typically Pro+ or Enterprise). Destination costs separately.

## OpenTelemetry

Vercel supports native OTEL trace export. Configure via:

- The `@vercel/otel` package (Vercel-published).
- Standard OTEL exporters pointed at OTLP-compatible backends.

```ts
// instrumentation.ts (Next.js convention)
import { registerOTel } from "@vercel/otel";

export function register() {
  registerOTel({ serviceName: "my-app" });
}
```

This wires automatic instrumentation for `fetch`, route handlers, server components, and server actions. Traces flow to your configured OTLP backend (Honeycomb, Datadog, Tempo, etc.).

Reach for OTEL when:

- Tracing spans multiple services (Vercel + external API + database).
- You want flame-graph performance views, not just metrics.
- Your team is already OTEL-familiar.

Skip OTEL if:

- Single-service Vercel app + Speed Insights + a basic Drain is enough.
- Team isn't ready to operate a tracing stack.

## Alerts

Vercel-native alerts:

- **Speed Insights threshold alerts** (Pro+).
- **Spend alerts** at $X budget thresholds.
- **Failed deployment** notifications via Slack / email.

For deeper alerting (on-call rotations, multi-channel routing), pair with:

- **PagerDuty** — page on incidents.
- **Datadog / Honeycomb / Axiom monitors** — alert on log patterns or metric anomalies.
- **Checkly** — synthetic monitoring (does this URL respond? does this user journey work?).

Alerts to configure on day one for any Slalom build:

| Alert | Threshold | Route |
|---|---|---|
| Failed production deploy | any | Slack #engineering |
| Speed Insights LCP regression | >2.5s p75 sustained | Slack |
| Spend on track to exceed cap | 80% of budget | Slack + email |
| Function error rate | >1% sustained 5min | PagerDuty |
| WAF rule trips | spike threshold | Security Slack channel |
| Synthetic monitor failure | 2/3 consecutive | PagerDuty |

## Dashboards

A reasonable Slalom default dashboard set, regardless of where you ship logs:

1. **Real-user perf** — LCP / INP / CLS by route, last 7d, p75.
2. **Traffic** — pageviews, top routes, country breakdown.
3. **Function health** — error rate, p50/p95/p99 duration by route.
4. **Cache hit rate** — % of requests served from CDN cache vs. origin.
5. **Cost burn** — bandwidth, function GB-s, image transforms, AI Gateway tokens, against budget.
6. **Storage operations** — Postgres queries, KV ops, Blob bandwidth.
7. **Security** — WAF blocks, BotID flags, auth failures.

Each dashboard has clear ownership, alert thresholds, and review cadence.

## Cost observability

Vercel's billing dashboard breaks costs into:

- Bandwidth.
- Function executions (invocations + GB-s).
- Image Optimization transforms.
- AI Gateway tokens.
- Storage GB-months and operations.
- Other (Drains, Speed Insights events past free tier, etc.).

Patterns:

- Set a spend cap on the Team account.
- Alert at 50%, 80%, 100% of monthly budget.
- Per-project usage breakdown in the org admin view.
- For Slalom engagements, include cost dashboard in the Transition deliverable so the client can self-manage post-launch.

## Incident response playbook

```
1. Acknowledge alert / page.
2. Check the Vercel status page.
3. Check the most recent deploy — is this regression-shaped?
   - If yes: rollback (re-alias prior production deployment). See vercel-deploy-pipeline.
4. Pull function logs from the affected route (dashboard or via Drain).
5. Pull traces (OTEL) for the affected user / request if available.
6. Check cache hit rate — sudden drop = surge to origin.
7. Check spend — DDoS amplifies bandwidth cost.
8. Check WAF / BotID — bot attack signatures.
9. Triage: contain (rollback / WAF rule), then investigate root cause.
10. Postmortem with timeline. Add the alert / runbook step that would have caught it earlier.
```

For full incident discipline, see `software-engineering-devops-engineer`.

## Common failure modes

- **Speed Insights / Analytics not installed.** Most common gap on rushed launches. Five minutes to fix.
- **Logs only in dashboard, no Drain.** Incident lands two weeks later; logs gone.
- **Structured logging missing.** `console.log("user " + id + " did " + thing)` is hard to query in Axiom. Switch to JSON.
- **Alert noise.** Too many alerts → page fatigue → real alert ignored. Tune thresholds quarterly.
- **No spend cap.** A loop or DDoS prints a five-figure invoice; alerts fired but nobody acted.
- **OTEL configured but no backend running.** Trace data goes nowhere; dev assumes tracing is "on."
- **Synthetic monitors check the homepage only.** Real broken state is on `/checkout/step-3`. Cover critical user journeys.
- **Audit logs ignored.** Pro+ has them; nobody reads them. Wire to SIEM or skim quarterly.

## Output formats

### Observability runbook

```
# Observability Runbook: [Project]

## Wired
- Speed Insights: ✓
- Web Analytics: ✓
- Function logs: dashboard (24h) + Drain to Axiom (90d)
- OTEL: enabled, backend Honeycomb
- Audit log: drained to client SIEM

## Dashboards (links)
- Real-user perf: ...
- Traffic: ...
- Function health: ...
- Cache hit rate: ...
- Cost burn: ...

## Alerts (configured)
| Alert | Threshold | Route | Owner |
|---|---|---|---|

## Synthetic monitors
| Journey | Tool | Frequency |
|---|---|---|

## On-call
- Primary: {rotation}
- Escalation: {who}

## Known issues / runbooks
- {symptom → action}
```

## How this skill relates to others

- Day-one wiring at project setup → `vercel-deploy-pipeline`.
- Function-runtime metrics for cold-start / duration analysis → `vercel-fluid-compute`.
- Cache hit rate observability → `vercel-cdn-edge`.
- Storage ops metrics → `vercel-storage`.
- AI cost dashboards (tokens, providers) → `vercel-ai-gateway`.
- Security event observability → `vercel-security`.
- Programmatic dashboard / monitor creation → `vercel-rest-api`.
- Application-side performance work → `software-engineering-nextjs-performance`.
- DevOps / SRE discipline broadly → `software-engineering-devops-engineer`.

## Source material

- Observability overview: https://vercel.com/products/observability
- Speed Insights: https://vercel.com/docs/speed-insights
- Web Analytics: https://vercel.com/docs/analytics
- Logs: https://vercel.com/docs/logs
- Log Drains: https://vercel.com/docs/log-drains
- OpenTelemetry: https://vercel.com/docs/otel
- Plugin reference: `../../references/vercel-foundations.md`
- Plugin reference: `../../references/marketplace-integrations.md`
