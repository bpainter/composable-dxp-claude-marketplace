---
type: reference
project: skills-library
scope: plugin
plugin: vercel
tags: [type/reference, plugin/vercel, scope/foundational, topic/vercel]
status: active
---
# Slalom + Composable DXP context for Vercel work

This reference frames every skill in this plugin against Slalom's organizational model, the Composable DXP practice, and the Vercel partnership. Skills cite this file to keep recommendations grounded in how Slalom actually engages clients on Vercel work — not generic "deploy to Vercel" advice.

## Slalom in one paragraph

Slalom is a privately held, partner-owned consulting firm — ~12,000 people, 49 offices, "strategy to execution" positioning. The differentiator: Slalom advises and builds. Engagement model is ~70–80% staff augmentation embedded in client teams plus ~20–30% scoped projects. Local-first, same-time-zone teams.

## Composable DXP practice (Bermon's practice)

Inside Slalom's Enterprise capability. Sister practices cover Salesforce, Adobe Experience, ServiceNow, Anaplan / business planning, and Boomi / integration. Composable DXP focuses on:

- Modular DXP architectures wired by APIs and event streams, not monoliths
- Headless CMS, commerce, search, personalization, identity, integration
- MACH principles — Microservices, API-first, Cloud-native, Headless
- Reference stack: **Next.js + Vercel** + Contentful + Algolia + a CDP/personalization layer + Bynder (DAM where needed)
- Delivery against Slalom Summit (Mobilize → Discover → Deliver → Transition → E&O → Close, 5 Risk Gates)

## Slalom × Vercel partnership

Vercel is a tier-1 platform partner for the practice. The default deploy target on Slalom Composable DXP builds is Vercel. That means:

- Skills should assume Vercel is a chosen partner, not one of many — but stay platform-aware enough to compare against AWS Amplify, Netlify, Cloudflare Pages, or self-hosted Node when the pursuit demands it
- We don't relitigate the Vercel choice every engagement; we extend it with delivery rigor (observability, security headers, cost guardrails, deploy protection)
- The Slalom front-end pattern is **Next.js App Router on Vercel with Fluid Compute**. Departures from this default need a stated reason

## How this shapes Vercel skills

When a skill in this plugin produces a recommendation, frame it this way:

- **Default to Vercel-native primitives** when they're a clean fit (Speed Insights, Image Optimization, Edge Config). Bypass to external services (imgix, Datadog, Cloudflare) when the cost, scale, or capability gap is named explicitly.
- **Multi-environment is the baseline.** Production + Preview + Development from day one. Domain-aliased preview deployments. No "deploy from a developer's laptop" patterns.
- **Cost guardrails are not optional.** Bandwidth, Image Optimization transforms, Function executions, AI Gateway tokens — all metered. Set spend caps and alerts before launch.
- **Security defaults are explicit, not implicit.** WAF + BotID + Deployment Protection (SSO on Preview, password / Bypass Tokens for non-public Production) on every Slalom build. Don't ship without them.
- **Observability is wired in week 1.** Speed Insights, Web Analytics, log ingestion to a long-term store. The dashboard is one of the four artifacts handed off to the client at Transition.
- **AI features go through the AI Gateway** when there's any chance of multi-provider routing, fallback, or BYO-key scenarios. Don't hard-wire to one model SDK.
- **Build center alignment**: when scope grows, expect handoff to Slalom Build for the front-end work. Skills should produce artifacts that travel cleanly to a Build team — `vercel.json` checked in, env vars documented, observability dashboards reproducible.
- **Composable Accelerator influence**: where there's a session-bridging or staged-migration pattern from Salesforce SFRA / SiteGenesis to a headless front end, that pattern (Build-developed) deploys to Vercel. The agent here knows the pattern.

## Engagement-level expectations

| Engagement level | Implications for Vercel work |
|---|---|
| Low Touch | Personal or small Team plan, single project, default observability, basic Deployment Protection |
| Standard | Team plan, full Production / Preview / Development env split, Speed Insights + Web Analytics, signed webhook handlers, WAF + BotID enabled, alert routing, cost spend cap |
| Complex | Enterprise plan, Trusted IPs, SSO + SAML, multiple projects with shared org-level settings, Log Drains to client SIEM, multi-region considerations, formal DR plan, custom v0 / Sandbox / Agent integrations |

The skills assume Standard by default. Bias up to Complex when the client is enterprise, regulated, or running multiple brands on shared infrastructure.

## What this isn't

- Not a Vercel certification path — for that, point at Vercel's official learning paths
- Not a Slalom delivery methodology reference — see `50_Knowledge/Frameworks/Slalom_Summit/` for that
- Not a vendor-neutral platform comparison — these skills are opinionated for Vercel

For framework-level firm context, see `50_Knowledge/Frameworks/`. For Bermon's career bar and quantitative expectations, see `80_Skills_and_Agents/Memory/practice.md`.
