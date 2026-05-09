---
name: vercel-rest-api
description: Use the Vercel REST API to manage projects, deployments, env vars, domains, integrations, and teams programmatically — for repeatable client-onboarding scripts, CI deploy hooks, env-var sweeps across many projects, log fetches, audit-log integration, and any "we need to do this in 30 places" automation. Use this skill when scripted operations on the Vercel platform are in scope, when standing up a multi-project repeatable setup, when the dashboard is too slow for the volume of work, or when CI integration needs more than a deploy hook. Trigger on any "automate this on Vercel" question.

# Project context
type: skill
project: skills-library
plugin: vercel
aliases: [vercel-rest-api]
tags: [type/skill, plugin/vercel, topic/vercel, topic/api, topic/automation]
status: active
---

# Vercel REST API

Owns the programmatic admin surface. Pair with `vercel-deploy-pipeline` (most ops here are project-scoped), `vercel-observability` (log fetches and dashboard ops), `vercel-security` (token discipline), and the CLI cheat sheet (the CLI wraps a subset of the REST API; use the API directly when you need beyond that subset).

## When to use this skill

- Standing up a new client project programmatically.
- Sweeping env vars across many projects (rotation, addition, removal).
- Triggering deploys from non-git CI.
- Fetching logs programmatically for incident analysis or compliance.
- Integrating with the client's CMDB or service catalog.
- Exporting audit logs to a SIEM.
- Building a Slalom internal tool that operates Vercel on behalf of teams.

## Core posture

- **Reach for the dashboard for one-off ops.** REST API is for repeatable / scripted / multi-resource ops.
- **Use Team tokens for automation, not Personal Access Tokens.** PATs follow the user; integration tokens don't.
- **Backoff and retries are required.** The API is rate-limited.
- **Idempotency matters.** Scripts that re-run should produce the same end state, not duplicates.
- **Never log token values.** Tokens in logs land in shared dashboards land in compromise.

## Auth

```
Authorization: Bearer <token>
```

Tokens come from:

- **Personal Access Token** — https://vercel.com/account/tokens. Tied to a user.
- **Team integration token** — created when you install an integration, scoped to the team.
- **OAuth** — for apps acting on behalf of users.

For Slalom automation, prefer Team-scoped tokens managed via integrations rather than PATs from individual accounts.

## Base structure

```
Base URL: https://api.vercel.com/

Versioned per resource:
  /v9/projects
  /v13/deployments
  /v3/teams
  /v9/projects/<id>/env
  ...

Pagination: ?limit=N&until=<cursor>
Filters: query params per resource (?teamId=...)

Response envelopes vary by version; current versions return arrays + pagination cursors.
```

Always pass the team scope (`?teamId=team_xxx`) when operating in a Team — without it, requests run against your Personal account.

## Common operations

### List / get projects

```ts
async function listProjects(token: string, teamId: string) {
  const res = await fetch(`https://api.vercel.com/v9/projects?teamId=${teamId}&limit=100`, {
    headers: { Authorization: `Bearer ${token}` },
  });
  if (!res.ok) throw new Error(await res.text());
  return res.json();
}
```

Pagination: response includes `pagination.next`; loop until null.

### Create a project

```ts
async function createProject(token: string, teamId: string, name: string, repo: { type: "github"; repo: string }) {
  const res = await fetch(`https://api.vercel.com/v11/projects?teamId=${teamId}`, {
    method: "POST",
    headers: {
      Authorization: `Bearer ${token}`,
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      name,
      framework: "nextjs",
      gitRepository: repo,
      buildCommand: "next build",
      installCommand: "pnpm install --frozen-lockfile",
      outputDirectory: ".next",
    }),
  });
  if (!res.ok) throw new Error(await res.text());
  return res.json();
}
```

### Set environment variable

```ts
async function setEnv(token: string, teamId: string, projectId: string, key: string, value: string, target: ("production" | "preview" | "development")[]) {
  const res = await fetch(`https://api.vercel.com/v10/projects/${projectId}/env?teamId=${teamId}`, {
    method: "POST",
    headers: { Authorization: `Bearer ${token}`, "Content-Type": "application/json" },
    body: JSON.stringify({
      key,
      value,
      target,
      type: "encrypted",
    }),
  });
  if (!res.ok && res.status !== 409 /* already exists */) throw new Error(await res.text());
  return res.ok ? res.json() : null;
}
```

For idempotent "set or update," check existence first or handle 409 with an update call.

### Trigger a deploy

```ts
async function triggerDeploy(token: string, teamId: string, projectId: string) {
  const res = await fetch(`https://api.vercel.com/v13/deployments?teamId=${teamId}`, {
    method: "POST",
    headers: { Authorization: `Bearer ${token}`, "Content-Type": "application/json" },
    body: JSON.stringify({
      name: "manual-trigger",
      project: projectId,
      target: "production",
      gitSource: { type: "github", ref: "main", repoId: "..." },
    }),
  });
  if (!res.ok) throw new Error(await res.text());
  return res.json();
}
```

For most teams, **deploy hooks** (a per-project URL you POST to) are simpler than the REST API for triggering deploys from a non-git source. Reserve the REST API for deploys with custom configuration.

### Promote a deployment (rollback)

```ts
async function promoteDeployment(token: string, teamId: string, deploymentId: string) {
  const res = await fetch(`https://api.vercel.com/v9/projects/.../promote/${deploymentId}?teamId=${teamId}`, {
    method: "POST",
    headers: { Authorization: `Bearer ${token}` },
  });
  return res.json();
}
```

The CLI's `vercel promote` wraps this. For scripted incident response, the REST API gives finer control.

### Fetch logs

```ts
async function fetchLogs(token: string, teamId: string, deploymentId: string) {
  const res = await fetch(`https://api.vercel.com/v3/deployments/${deploymentId}/events?teamId=${teamId}`, {
    headers: { Authorization: `Bearer ${token}` },
  });
  return res.json();
}
```

Real-time logs use Server-Sent Events on `/events?follow=1`. For long-term retention, use Log Drains (see `vercel-observability`).

### Manage domains

```ts
async function addDomain(token: string, teamId: string, projectId: string, domain: string) {
  const res = await fetch(`https://api.vercel.com/v10/projects/${projectId}/domains?teamId=${teamId}`, {
    method: "POST",
    headers: { Authorization: `Bearer ${token}`, "Content-Type": "application/json" },
    body: JSON.stringify({ name: domain }),
  });
  return res.json();
}
```

### Audit logs (Pro+)

```ts
async function fetchAuditLog(token: string, teamId: string, since: Date) {
  const res = await fetch(`https://api.vercel.com/v1/teams/${teamId}/audit-log?since=${since.toISOString()}`, {
    headers: { Authorization: `Bearer ${token}` },
  });
  return res.json();
}
```

Wire this into a SIEM or scheduled job to push to long-term storage.

## Common Slalom scripts

### New-project bootstrap

```
1. Create project (vercel-rest-api)
2. Add domains (vercel-rest-api)
3. Set env vars (vercel-rest-api)
4. Configure Deployment Protection (dashboard or REST)
5. Install marketplace integrations (one per integration's API)
6. Verify build succeeds
7. Configure log drain (vercel-rest-api)
```

A Slalom-internal `slalom-vercel-bootstrap` script that takes a YAML config and applies all of the above is a high-leverage tool for client onboarding.

### Env var rotation

```
1. List all projects in scope
2. For each, find env vars matching pattern (e.g., DATABASE_URL)
3. Update value
4. Trigger redeploy
5. Verify new version is live
6. Mark rotation complete
```

Stagger redeploys to avoid mass simultaneous restarts on shared databases.

### Pre-launch hygiene sweep

```
For each project in client's team:
  - Deployment Protection state (audit)
  - Speed Insights enabled?
  - Web Analytics enabled?
  - Spend cap set?
  - Production env vars audit (no NEXT_PUBLIC_ leakage of secrets)
  - WAF + BotID on?
Output a report.
```

Excellent for client handoff readiness checks.

## Rate limits

Per-token limits exist; specifics in response headers (`X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset`).

Backoff pattern:

```ts
async function withRetry<T>(fn: () => Promise<T>, maxAttempts = 5): Promise<T> {
  for (let attempt = 1; attempt <= maxAttempts; attempt++) {
    try {
      return await fn();
    } catch (err: any) {
      if (attempt === maxAttempts) throw err;
      const status = err?.response?.status;
      if (status === 429) {
        // Use Retry-After if present
        const retryAfter = Number(err.response.headers.get("retry-after") ?? 5);
        await sleep(retryAfter * 1000);
        continue;
      }
      if (status >= 500) {
        await sleep(2 ** attempt * 1000);
        continue;
      }
      throw err;
    }
  }
  throw new Error("unreachable");
}
```

For sweeping operations, add explicit pacing — don't rely on retries to handle volume.

## SDKs

Vercel publishes a TypeScript SDK at `@vercel/sdk` (and similar for other languages). It wraps the REST API with typed helpers:

```ts
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({ bearerToken: process.env.VERCEL_TOKEN });
const projects = await vercel.projects.getProjects({ teamId, limit: 100 });
```

For one-off scripts, raw `fetch` is fine. For larger automation tools, the SDK saves typing and provides built-in pagination helpers.

## Common failure modes

- **Hardcoded token in repo.** Compromise propagates instantly. Use env vars, rotate on commit if leaked.
- **No team scope.** Operations land on the personal account. Audit `teamId` in every call.
- **Pagination missed.** First 100 projects look fine; the 101st never gets touched. Always paginate.
- **Idempotency missing.** Re-running a setup script creates duplicate env vars or domain conflicts. Check first or handle 409.
- **Rate-limit cascade.** Loop without backoff; 429 triggers retry; retries amplify the problem. Always backoff.
- **Logging full responses.** Tokens, secrets, URLs end up in logs. Sanitize before logging.
- **API version drift.** Endpoints version per resource; v9 may be deprecated by v11 by the time you re-run the script. Pin versions and update intentionally.

## Output formats

### Automation script spec

```
# Vercel Automation: [Name]

## Goal
{what the script does and why}

## Inputs
- Token: {Team integration token, scope}
- Config: {YAML / JSON shape}

## Operations
1. {API call} → {expected outcome}
2. ...

## Idempotency
- {How re-runs behave}

## Error handling
- {Per error class}

## Rate limit handling
- {Backoff strategy}

## Audit
- Logs to: {destination}
- Tracks: {operations performed, success/failure}

## Schedule
- {one-off | daily | on event}

## Cleanup
- {What to do if the script is decommissioned — token rotation, etc.}
```

## How this skill relates to others

- Project-level operations are the most common API target → `vercel-deploy-pipeline`.
- Log fetches and audit log shipping → `vercel-observability`.
- Token rotation and security hygiene → `vercel-security`.
- The CLI is the lighter-weight wrapper for many of these operations → `../../references/cli-cheat-sheet.md`.
- Marketplace integration installation often has its own API per integration → `../../references/marketplace-integrations.md`.
- For multi-project Slalom hygiene scripts, this skill is the engine; the agent (`vercel-agent`) drives the workflow.

## Source material

- REST API reference: https://vercel.com/docs/rest-api
- TypeScript SDK: https://www.npmjs.com/package/@vercel/sdk
- CLI source (wraps the API): https://github.com/vercel/vercel/tree/main/packages/cli
- Plugin reference: `../../references/api-surface.md`
- Plugin reference: `../../references/cli-cheat-sheet.md`
