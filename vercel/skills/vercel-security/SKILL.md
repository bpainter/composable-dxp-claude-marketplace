---
name: vercel-security
description: Harden a Vercel project — Web Application Firewall (WAF) rules, BotID server-side bot identification, Bot Management policies, Deployment Protection (SSO / password / Bypass Tokens), Trusted IPs, security headers, secrets discipline, secure-by-default project posture. Use this skill when standing up a production project, when a security review is in flight, when bot traffic is becoming a problem, when DDoS or scraper activity spikes, or when an incident points at the platform layer. Trigger on any "is this secure" or "we got hit by bots" question on Vercel.

# Project context
type: skill
project: skills-library
plugin: vercel
aliases: [vercel-security]
tags: [type/skill, plugin/vercel, topic/vercel, topic/security]
status: active
---

# Vercel Security

Owns the platform-layer security posture. Pair with `vercel-deploy-pipeline` (Deployment Protection lives at project level), `vercel-cdn-edge` (rules apply at the edge), `vercel-observability` (security event logs and audit), `vercel-rest-api` (programmatic policy management), and `software-engineering-security-advisor` for the application-layer threat modeling that complements platform hardening.

## When to use this skill

- Standing up production for a Slalom engagement (Slalom defaults non-negotiable).
- Bot traffic is causing cost or noise.
- DDoS or scraper activity spike.
- Pre-launch security review.
- Auth provider integration (Clerk / Auth0 / WorkOS).
- Secrets handling audit.
- Trusted IPs / SAML SSO setup (Enterprise).

## Core posture

- **Secure by default.** WAF + BotID + Deployment Protection on every Slalom production project. Headers locked down. Spend caps in place.
- **Layered defense.** Vercel handles platform concerns; the app handles application concerns; the auth provider handles identity. Don't expect any one layer to do all the work.
- **Audit before launch.** Every Slalom build does a security checklist sweep before the production cut-over.
- **Rotate aggressively.** Tokens, secrets, Bypass Tokens — short-lived where possible, rotated on staff changes always.
- **Don't reinvent.** Use Vercel-native tools (WAF, BotID, Bot Management) before custom middleware.

## Vercel security primitives

| Feature | What | Plan |
|---|---|---|
| **Deployment Protection** | SSO / password / Bypass Tokens on non-public deploys | All plans |
| **WAF (Web Application Firewall)** | Custom firewall rules at the edge | Pro+ |
| **BotID** | Server-side bot identification | Pro+ |
| **Bot Management** | Allow / block / challenge bots based on rules | Pro+ |
| **Trusted IPs** | IP allowlist for sensitive routes / Production | Enterprise |
| **DDoS Mitigation** | Always-on at the edge | All plans |
| **SSO + SAML** | Org-level SSO for the dashboard | Enterprise |
| **HIPAA BAA** | Business Associate Agreement | Enterprise (case-by-case) |
| **Audit logs** | Who did what in the dashboard | Pro+ |
| **Secure Compute** | Dedicated compute pools | Enterprise |

Confirm the client's plan tier before scoping — many features are gated.

## Deployment Protection

Three modes, often combined:

### Vercel Authentication (SSO)

Org-account-gated access. Default ON for every non-public Slalom project. Members of the Vercel team see Preview URLs; everyone else hits a login wall.

Project Settings → Deployment Protection → Vercel Authentication.

### Password Protection

Static password on selected environments. For pre-launch Production where stakeholders need access without Vercel accounts.

### Bypass Tokens

Programmatic tokens that skip Deployment Protection. Use for:

- E2E tests (Cypress, Playwright) running in CI.
- Synthetic monitors (Checkly).
- External QA / screenshot tools.

Generate in Project Settings → Deployment Protection → Bypass for Automation. Token is a query param: `?x-vercel-protection-bypass=<token>`. Treat as a secret. Rotate quarterly.

### Trusted IPs (Enterprise)

IP allowlist for sensitive routes (admin, internal tools) or whole Production. Pair with WAF. Document the allowlist; review on staff changes.

## WAF (Web Application Firewall)

Custom firewall rules at the edge. Reach for it when:

- You need to block specific IPs / ranges.
- Specific user-agents need blocking (scrapers, vulnerability scanners).
- Geographic restrictions apply (some routes US-only).
- Rate limiting at the edge before requests hit your functions.
- Path-based blocking (`/admin` only from corporate IPs).

Rule structure:

```
IF (request matches condition) THEN action

Conditions: IP, country, user-agent, path, method, headers, query string
Actions: allow, deny, challenge (CAPTCHA), log
```

Slalom-default rules to add on launch:

```
DENY: country IN [list of high-fraud countries the client doesn't serve]
DENY: user-agent matches /(scrapy|httrack|wget|curl|python-requests)/i [unless legit use case]
RATE LIMIT: /api/* — 100 req/min per IP
RATE LIMIT: /api/auth/* — 10 req/min per IP
CHALLENGE: requests without User-Agent
DENY: requests with paths matching common attack signatures (.env, .git, /wp-admin)
```

Configure in Project Settings → Firewall. Rules can be configured in the dashboard or via REST API for repeatability across projects.

## BotID

Server-side bot identification. Vercel inspects each request and labels it as human, verified bot (Googlebot, Bingbot), suspicious bot, or malicious bot. The label is exposed via headers / SDK and you (or your WAF rules) decide what to do.

```ts
// Read BotID label in a route handler
export async function GET(req: Request) {
  const botKind = req.headers.get("x-vercel-botid-kind"); // "human" | "good_bot" | "suspicious" | "malicious"
  if (botKind === "malicious") {
    return new Response("Forbidden", { status: 403 });
  }
  // ...
}
```

BotID is cheap server-side detection. Pair with Bot Management for actioning.

## Bot Management

Rule-based bot control. Categories:

- **Allow good bots** (search engines, monitoring) — default allow with verification.
- **Block known bad bots** — vulnerability scanners, scrapers, credential stuffers.
- **Challenge ambiguous traffic** — JavaScript challenge or CAPTCHA before allowing access.
- **Rate limit by category** — bots get tighter limits than humans.

Slalom defaults:

```
Allow: verified search engines (Googlebot, Bingbot, etc.)
Allow: verified social platforms (Slackbot, Twitterbot for OG previews)
Block: scrapers, scanners, credential stuffers
Challenge: ambiguous bots not on allow / block list
Rate limit: bots → 30 req/min; humans → 300 req/min
```

For commerce / media / high-traffic sites, expect to tune Bot Management quarterly as new bot patterns emerge.

## Security headers

A baseline set of headers every Slalom project ships:

```js
// next.config.mjs
async function headers() {
  return [
    {
      source: "/(.*)",
      headers: [
        { key: "Strict-Transport-Security", value: "max-age=63072000; includeSubDomains; preload" },
        { key: "X-Frame-Options", value: "DENY" },
        { key: "X-Content-Type-Options", value: "nosniff" },
        { key: "Referrer-Policy", value: "strict-origin-when-cross-origin" },
        { key: "Permissions-Policy", value: "camera=(), microphone=(), geolocation=(), payment=()" },
        { key: "X-DNS-Prefetch-Control", value: "on" },
      ],
    },
  ];
}
```

For routes that embed third-party content (iframes, analytics):

- Audit `frame-ancestors` per route.
- Add a Content-Security-Policy (CSP) — start with report-only mode, then enforce.
- Pair with `vercel-cdn-edge` if cache headers also need to be coordinated.

## Secrets discipline

Patterns to enforce:

- **Vercel-managed secrets in env vars only.** Never in `vercel.json`, never in code, never in `NEXT_PUBLIC_*`.
- **Encrypted at rest.** Vercel does this; verify the dashboard masks values.
- **Short-lived tokens where possible.** Personal Access Tokens follow the user; integration tokens for automation.
- **Rotate on staff changes** — anyone who's left has had their tokens potentially scraped / cached.
- **`vercel env pull`** to get production-mirroring local env, not hand-copying.
- **Per-environment scoping.** Production secrets don't appear in Preview unless intentionally duplicated.
- **`NEXT_PUBLIC_*` audit** at every release. Anything prefixed `NEXT_PUBLIC_` is in the browser bundle. Trivial to leak a secret here.

For secret-sniffer tools (truffleHog, gitleaks), run in CI on the repo. Audit the Vercel env-var list quarterly.

## Auth integration

Vercel doesn't ship its own auth product. The defaults:

- **Clerk** — drop-in auth UI + APIs. Default for B2B SaaS / internal tools.
- **Auth0** — enterprise, often pre-existing.
- **WorkOS** — enterprise SSO + SCIM, when the client needs SAML.
- **NextAuth.js (Auth.js)** — self-hosted, library-shaped, pairs with any DB.
- **Supabase Auth** — when Supabase is the DB.

Slalom defaults:

- **Standard engagement** → Clerk.
- **Enterprise with existing Okta / Auth0** → use what they have.
- **Enterprise needs SAML / SCIM not in their existing stack** → WorkOS.

See `../../references/marketplace-integrations.md`.

## Pre-launch security checklist

Walk this before every Slalom production cut-over:

```
Account & access
☐ Production team has correct admins (not personal accounts)
☐ SSO required for dashboard access (Enterprise)
☐ Audit log review on a cadence
☐ Spend cap configured + alerts wired

Deployments
☐ Production branch protected (git provider)
☐ Preview deployments behind SSO or password
☐ Bypass Tokens documented and rotated
☐ No secrets in repo (gitleaks scan clean)
☐ No secrets in NEXT_PUBLIC_* (manual audit)

Functions & runtime
☐ maxDuration sized appropriately (no infinite-loop risk)
☐ Rate limiting on auth and write endpoints
☐ Input validation on all user-facing routes (Zod or similar)
☐ CORS locked down (no wildcard origin in production)

Edge / Network
☐ HTTPS enforced (HSTS header set)
☐ X-Frame-Options DENY (or per-route override)
☐ X-Content-Type-Options nosniff
☐ Referrer-Policy strict-origin-when-cross-origin
☐ Permissions-Policy locking down camera/mic/geolocation
☐ Content-Security-Policy (start in report-only)

WAF & Bots (Pro+)
☐ WAF enabled
☐ Baseline rules configured (rate limits, bad-bot block, geo if needed)
☐ BotID enabled
☐ Bot Management rules configured

Auth
☐ Auth provider integrated (Clerk / Auth0 / WorkOS / etc.)
☐ Session expiration sane (not 30-day default-tokens)
☐ MFA required for admin routes
☐ Logout invalidates server-side session

Storage
☐ Database connection limits not exhaustible by bot traffic
☐ Public Blob URLs reviewed (no accidental private data)
☐ KV TTLs set on all session-style keys

Observability
☐ Speed Insights + Analytics on
☐ Log drain to long-term store
☐ Alerts wired (deploy fail, error rate, spend, WAF events)
☐ Audit log → SIEM (Enterprise)

Backup & recovery
☐ Postgres branching tested for rollback
☐ Blob retention policy documented
☐ Postmortem template ready
```

## Common failure modes

- **Production deployed without WAF.** The first scraper costs more than a year of WAF gating.
- **Bypass Token in repo.** Preview deployment effectively public.
- **Secret with NEXT_PUBLIC_ prefix.** Shipped to browser. Often a service-role key for an analytics or DB platform.
- **Auth provider misconfigured.** Session cookie scope too wide (`*.vercel.app`) leaks to other apps.
- **CORS wildcard in production.** Any origin can hit your API.
- **No rate limit on auth endpoints.** Credential stuffing succeeds quietly.
- **CSP not enforced.** Report-only stays report-only forever; XSS risk persists.
- **Audit logs never reviewed.** Compromise gets noticed weeks later via SIEM, not on the platform.
- **WAF rules blocking legitimate traffic.** False-positive geo block kills international users; tune conservatively.

## Output formats

### Security posture doc

```
# Security Posture: [Project]

## Plan tier
{Hobby | Pro | Enterprise}

## Deployment Protection
- Production: {public | password | SSO}
- Preview: {SSO}
- Bypass Tokens: {list, rotation cadence}
- Trusted IPs: {if Enterprise}

## WAF
- Status: {on | off}
- Active rules: {summary}

## Bot Management
- BotID: {on | off}
- Allow list: {good bots}
- Block list: {bad bots}
- Challenge thresholds: {summary}

## Headers
- HSTS, X-Frame-Options, X-Content-Type-Options, Referrer-Policy, Permissions-Policy: {snippet or link}
- CSP: {report-only | enforced}

## Auth
- Provider: {Clerk | Auth0 | WorkOS | etc.}
- MFA on: {admin routes | all auth | none}
- Session expiry: {N hours / days}

## Secrets
- Rotation cadence: {quarterly}
- NEXT_PUBLIC_* audit: {date}
- gitleaks last clean: {date}

## Observability
- Drain: {destination}
- Alert routes: {list}

## Audit cadence
- {monthly / quarterly}
- Owner: {name}
```

## How this skill relates to others

- Deployment Protection lives at project level → `vercel-deploy-pipeline`.
- Edge-level rules apply via the same surface as cache rules → `vercel-cdn-edge`.
- Security event logging → `vercel-observability`.
- Programmatic policy management across projects → `vercel-rest-api`.
- Auth integrations → `../../references/marketplace-integrations.md`.
- Application-layer threat modeling, secure coding → `software-engineering-security-advisor`.
- Compliance / regulatory considerations → `software-engineering-security-advisor` + `software-engineering-cloud-architect`.

## Source material

- Security overview: https://vercel.com/security
- WAF: https://vercel.com/security/web-application-firewall
- BotID: https://vercel.com/botid
- Bot Management: https://vercel.com/security/bot-management
- Deployment Protection: https://vercel.com/docs/deployment-protection
- Plugin reference: `../../references/vercel-foundations.md`
- Plugin reference: `../../references/marketplace-integrations.md`
