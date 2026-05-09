---
name: vercel-deploy-pipeline
description: Stand up and operate the Vercel deploy pipeline — project setup, the Production / Preview / Development environment trio, custom environments, domains and aliases, Deployment Protection (SSO / password / Bypass Tokens), build configuration, rollback via re-aliasing, and the deploy-hook patterns that drive CI-from-non-git sources. Use this skill when a new Vercel project is being created, when "set up our preview deployments" comes up, when staging deploys need a stable URL, when domain configuration is in question, when a deploy fails and the cause isn't obvious, or when rollback is on the table. Trigger on any "how do we ship to Vercel" question.

# Project context
type: skill
project: skills-library
plugin: vercel
aliases: [vercel-deploy-pipeline]
tags: [type/skill, plugin/vercel, topic/vercel, topic/deploy]
status: active
---

# Vercel Deploy Pipeline

Owns the deploy lifecycle on Vercel — from "we don't have a project yet" through "production rollback in 30 seconds." Pair with `vercel-fluid-compute` (runtime decisions land here), `vercel-cdn-edge` (cache invalidation on deploy), `vercel-security` (Deployment Protection), `vercel-observability` (post-deploy verification), and `vercel-rest-api` (programmatic operations).

## When to use this skill

- Standing up a new Vercel project for a client engagement.
- Configuring Preview deployments and per-branch URLs.
- Adding a stable staging-domain alias.
- Wiring Deployment Protection (SSO, password, Bypass Tokens).
- Diagnosing a build failure that doesn't reproduce locally.
- Rolling back a bad production deploy.
- Setting up deploy hooks for non-git triggers (CMS publish, scheduled job).

## Core posture

- **Git is the system of record.** Production deploys come from git pushes to the production branch. CLI deploys are for emergencies.
- **Three environments minimum** on day one: Production, Preview, Development. Custom environments are additive, not replacements.
- **Domains are aliases**, not part of the immutable deployment. Rollback = re-alias.
- **Deployment Protection is non-optional** for non-public projects. Slalom ships every preview behind SSO or Bypass Tokens.
- **Build settings live in the dashboard or `vercel.json`**, both checked. Don't depend on dashboard-only state for reproducibility.

## Project setup (the canonical Slalom flow)

```
1. Confirm scope
   - Personal account vs. Team. For client work: always Team.
   - Plan tier (Hobby / Pro / Enterprise) — drives feature availability.
   - Who pays, who has admin, who has deploy access.

2. Connect git
   - GitHub / GitLab / Bitbucket repo.
   - Pick the framework preset (Next.js for ~90% of Slalom work).
   - Verify Root Directory if monorepo (e.g., `apps/web`).
   - Verify Build Command, Install Command, Output Directory.

3. Environments
   - Production: pinned to default branch (`main`).
   - Preview: every non-default branch / PR.
   - Development: local via `vercel dev`.
   - Add staging-aliased Preview domain (see "Domains" below).

4. Environment variables
   - Server-only secrets in Production + Preview (encrypted).
   - `NEXT_PUBLIC_*` for client-side public config.
   - Use `vercel env pull` to sync to local `.env`.
   - NEVER commit secrets. NEVER expose `*_TOKEN` or `*_SECRET` as `NEXT_PUBLIC_`.

5. Domains
   - Production domain (`example.com`) → DNS-verified, attached to Production environment.
   - Apex vs. www — pick one canonical, redirect the other.
   - `staging.example.com` aliased to a stable Preview URL pattern.

6. Deployment Protection
   - Production: public, OR password-protected if pre-launch.
   - Preview: SSO (Vercel Authentication) — gates all preview URLs to org members.
   - Bypass Tokens for E2E test runners and external QA tools.

7. Observability + Security
   - Speed Insights + Web Analytics enabled.
   - WAF + BotID enabled (Pro+).
   - Spend cap configured.

8. Document
   - .vercel/project.json committed.
   - README contains "How to deploy", "How to access preview", "Who has admin."
```

## Environments in detail

| Environment | Source | Lifetime | Use |
|---|---|---|---|
| **Production** | Push to `main` | Long-lived deployment alias | Customer-facing |
| **Preview** | Push to any non-default branch / PR | One per branch, refreshed on push | Stakeholder review, QA |
| **Development** | `vercel dev` locally | Ephemeral, per-developer | Local development |
| **Custom** (Pro+) | Manual config (`staging`, `qa`) | Long-lived alias | Stable QA / UAT URL |

**Custom environments**: useful when Preview URLs aren't stable enough (e.g., external QA team needs a single URL). Configure in Project Settings → Environments. Treat each custom environment as having its own env-var set; copy carefully from Preview.

## Domain configuration

```
Domains
├── example.com               → Production (alias)
├── www.example.com           → 301 redirect to apex
├── staging.example.com       → custom-environment alias (Pro+) OR latest Preview
└── *.example.com (wildcard)  → for branch-named subdomains (rare)
```

Patterns:

- **Apex + www redirect.** Pick one canonical (apex or www). Vercel handles the redirect at the edge.
- **Staging alias.** A stable URL like `staging.example.com` aliased to a custom environment OR to a "latest preview" pattern. Stakeholders bookmark this.
- **Preview-branch URLs.** `<project>-git-<branch>-<scope>.vercel.app`. Predictable per-branch; useful for PR-based stakeholder review.
- **Wildcard.** Reserved cases — multi-tenant SaaS, branch-named demos. Most engagements don't need it.

DNS:

- A or AAAA records to Vercel's IPs, OR
- CNAME to `cname.vercel-dns.com`, OR
- Vercel manages the zone (registrar transfer).

For Slalom builds, prefer the CNAME pattern unless the client is using Vercel as their DNS provider. Document the DNS state in the runbook.

## Deployment Protection

Three modes, often combined:

### Vercel Authentication (SSO)

Org-account-gated access to Preview deployments. Default ON for non-public projects. Members of the Vercel team see Preview URLs; everyone else gets a login wall.

Enable in Project Settings → Deployment Protection → Vercel Authentication.

### Password Protection

Static password on selected environments. Useful for pre-launch Production where the client wants stakeholders to access without giving them Vercel accounts.

Enable in Project Settings → Deployment Protection → Password Protection.

### Bypass Tokens

Programmatic-access tokens that skip Deployment Protection. Use for:

- E2E tests running in CI.
- External QA tools (Checkly synthetic monitors).
- Pre-launch screenshot tools.

Generate in Project Settings → Deployment Protection → Bypass for Automation. Token is a query param: `?x-vercel-protection-bypass=<token>`. Treat as a secret — leaks publish your preview.

### Trusted IPs (Enterprise)

IP allowlist for sensitive routes or Production. Pair with WAF for layered defense. See `vercel-security`.

## Build configuration

Most Slalom projects need only modest configuration:

`vercel.json` (when needed — many projects don't have one):

```json
{
  "$schema": "https://openapi.vercel.sh/vercel.json",
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        { "key": "X-Frame-Options", "value": "DENY" },
        { "key": "X-Content-Type-Options", "value": "nosniff" },
        { "key": "Referrer-Policy", "value": "strict-origin-when-cross-origin" }
      ]
    }
  ],
  "rewrites": [
    { "source": "/api/proxy/:path*", "destination": "https://api.example.com/:path*" }
  ],
  "redirects": [
    { "source": "/old-path", "destination": "/new-path", "permanent": true }
  ]
}
```

Headers, rewrites, redirects can also live in `next.config.mjs` — pick one place per concern. For Slalom builds, prefer `next.config.mjs` for app-level concerns and `vercel.json` for platform-level (e.g., security headers, function config).

Function configuration:

```json
{
  "functions": {
    "app/api/heavy/route.ts": {
      "maxDuration": 60,
      "memory": 1024
    }
  }
}
```

`maxDuration` defaults vary by plan; long-running functions (LLM streaming, video processing) often need explicit higher values.

## Build failure debugging

Order of operations when a deploy fails:

1. **Read the build log from the dashboard.** Find the first error, not the cascade.
2. **Diff against the last good build.**
   - Different env vars? (Compare Production vs. Preview).
   - Different commit? Check the diff for new dependencies.
   - Different framework version? (`next` upgrade in `package.json`).
3. **Reproduce locally with `vercel build`.** This runs the Vercel build environment more faithfully than `next build`.
4. **Common causes:**

| Symptom | Likely cause |
|---|---|
| `Module not found: sharp` | Native dep failing; ensure `sharp` is in dependencies, not devDependencies |
| Function exceeds size limit | Large dep (e.g., Puppeteer) bundled into a function |
| `Image Optimization` error | Domain not in `images.remotePatterns` |
| `proxy.ts` errors | Old `middleware.ts` not renamed after Next.js 16 upgrade |
| Env var undefined | Set in Production but not Preview, or vice versa |
| Build succeeds but 404s in Production | Routing config drift; check `next.config` redirects/rewrites |

## Rollback

Vercel rollback is alias re-pointing — not a re-build:

```
Dashboard → Deployments → Find prior good deployment → "Promote to Production"
```

Or via CLI:

```bash
vercel promote <prior-deployment-url-or-id> --token=$VERCEL_TOKEN
```

Or via REST API (`vercel-rest-api`).

Implications:

- Rollback is atomic and fast (~seconds).
- The deployment binary is unchanged — same code, same env vars at deploy time.
- If env vars have changed since the prior deploy, the rolled-back code may behave differently than it did originally. Verify env-var diff before promoting.
- Don't rely on rollback for env-var-driven incidents — change the env var instead.

## Deploy hooks (non-git triggers)

Deploy hooks are URLs that, when POSTed to, trigger a deploy of a specific branch:

```
POST https://api.vercel.com/v1/integrations/deploy/{project-id}/{hook-id}
```

Use cases:

- **Contentful publish** triggers a rebuild (alternative to ISR / on-demand revalidation).
- **Scheduled refresh** for sites with time-sensitive content.
- **External CI** triggers a Vercel deploy.

Configure in Project Settings → Git → Deploy Hooks. The hook URL is sensitive — treat as a secret.

For ISR / on-demand revalidation patterns, prefer those over deploy hooks; they're cheaper and faster. Deploy hooks are for "rebuild from scratch" scenarios.

## Environment variable discipline

Patterns to enforce:

- **Encrypt sensitive vars.** Vercel encrypts at rest; the dashboard masks them in UI.
- **Scope per environment.** Don't blanket-set across all environments unless the value is genuinely shared.
- **`NEXT_PUBLIC_*` audit.** Anything prefixed `NEXT_PUBLIC_` is shipped to the browser. Audit before launch.
- **Sync to local via `vercel env pull`.** Avoid hand-editing `.env.local` for Production-mirror config.
- **Rotate on staff changes.** Personal Access Tokens follow the user; integration tokens don't. Use integration tokens for automation that should outlive a person.

## Output formats

### Project setup runbook

```
# Vercel Project Setup: [Project Name]

## Account & Team
- Team: {team-id}
- Plan: {Hobby | Pro | Enterprise}
- Admins: {list}
- Deploy access: {list}

## Project
- Project ID: {id}
- Repo: {git URL}
- Framework: {Next.js / other}
- Root directory: {path or repo root}
- Production branch: {main}

## Environments
| Env | Source | Domain | Protection |
|---|---|---|---|
| Production | main | example.com | public |
| Preview | non-main branches | <project>-git-<branch>.vercel.app | SSO |
| Custom: staging | (alias to latest preview / custom env) | staging.example.com | SSO |

## Environment variables
| Name | Production | Preview | Notes |
|---|---|---|---|
| ... | ✓ | ✓ | server-only |

## Observability
- Speed Insights: {enabled}
- Web Analytics: {enabled}
- Log drain: {Datadog / Axiom / etc., or none}

## Security
- WAF: {enabled / rules}
- BotID: {enabled}
- Bot Management: {rules}
- Trusted IPs: {if Enterprise}

## Cost
- Spend cap: {$N}
- Alerts: {addresses}

## Owners
- Slalom: {names}
- Client: {names}
```

### Deploy postmortem

```
# Deploy Incident: [Date] [One-line summary]

## Symptom
{what was broken}

## Detection
{how the issue surfaced — alert, customer report, monitoring}

## Root cause
{what changed and why it broke}

## Fix
{what we did}

## Prevention
{what we'll add to the runbook / CI / monitoring to catch this earlier}

## Timeline
- T+0: incident
- T+N: detected
- T+M: rolled back
- T+P: root cause identified
- T+Q: fix deployed
```

## Common pitfalls

- **Hobby plan for client work.** No Team controls, no SSO, no audit log. Use Pro minimum.
- **Apex domain not migrated.** Stuck on a www-only or vice versa, breaking direct visits or canonical SEO.
- **Preview deployments wide-open.** Anyone with a URL can see pre-release content. Always SSO or password.
- **Bypass Token in code.** Token committed; preview is effectively public. Use env vars and rotate.
- **Custom environment proliferation.** `staging`, `qa`, `uat`, `pre-prod` all with different env-var sets that drift. Keep to ≤2 custom environments.
- **No spend cap.** A runaway loop or DDoS prints a five-figure invoice. Always set a cap, even on Pro.
- **Deploy hooks treated as public.** Anyone with the URL can trigger a deploy.
- **Build config drift.** Dashboard settings and `vercel.json` disagree. Pick one source of truth (preferably `vercel.json` + `next.config.mjs` checked into the repo).
- **Environment variable copy-paste between environments.** Production token leaks into Preview because someone duplicated for convenience. Use the dashboard's per-environment toggle deliberately.

## How this skill relates to others

- Function runtime decisions on top of this pipeline → `vercel-fluid-compute`.
- Cache strategy and image optimization on deployed assets → `vercel-cdn-edge`.
- Storage bindings configured at the project level → `vercel-storage`.
- Observability wired post-deploy → `vercel-observability`.
- Security hardening on top of Deployment Protection → `vercel-security`.
- Programmatic project / deploy automation → `vercel-rest-api`.
- Marketplace integrations that wire env vars and webhooks → `../../references/marketplace-integrations.md`.

## Source material

- Vercel deployments: https://vercel.com/docs/deployments
- Previews: https://vercel.com/products/previews
- Deployment Protection: https://vercel.com/docs/deployment-protection
- Domains: https://vercel.com/docs/domains
- CLI: https://vercel.com/docs/cli
- Plugin reference: `../../references/vercel-foundations.md`
- Plugin reference: `../../references/cli-cheat-sheet.md`
