---
type: reference
project: skills-library
scope: plugin
plugin: contentful
tags: [type/reference, plugin/contentful, scope/foundational, topic/contentful]
status: active
---
# Slalom + Composable DXP context for Contentful work

This reference frames every skill in this plugin against Slalom's organizational model, the Composable DXP practice, and the Contentful partnership. Skills cite this file to keep recommendations grounded in how Slalom actually engages clients on Contentful work — not generic SaaS-vendor advice.

## Slalom in one paragraph

Slalom is a privately held, partner-owned consulting firm — ~12,000 people, 49 offices, "strategy to execution" positioning. The differentiator is that Slalom advises and builds; most consulting firms hand off a deck, Slalom hands off working software through Slalom Build. Engagement model is ~70–80% staff augmentation embedded in client teams plus ~20–30% scoped projects. Local-first delivery means same-time-zone teams and minimal travel.

## Composable DXP practice (Bermon's practice)

Inside Slalom's Enterprise capability. Sister practices cover Salesforce, Adobe Experience, ServiceNow, Anaplan / business planning, and Boomi / integration. Composable DXP focuses on:

- Modular DXP architectures wired by APIs and event streams, not monoliths
- Headless CMS, commerce, search, personalization, identity, integration
- MACH principles — Microservices, API-first, Cloud-native, Headless
- Reference stack: Contentful + Next.js + Vercel + Algolia + a CDP/personalization layer (often Contentful Personalization, formerly Ninetailed) + Bynder (DAM where needed)
- Delivery against Slalom Summit (Mobilize → Discover → Deliver → Transition → E&O → Close, 5 Risk Gates), Pursuit Excellence on the front end, Delivery Excellence as the quality framework

## Slalom × Contentful partnership

Contentful is a tier-1 technology partner for the practice. Slalom shows up as an implementation partner on enterprise Contentful pursuits and is one of the practical delivery vehicles for Contentful's vision of composable content platforms. That means:

- Skills should assume Contentful is a chosen partner, not one of many — but stay platform-agnostic enough to compare against Sanity, Storyblok, Strapi, Adobe AEM, etc. when the pursuit calls for it
- We don't pick fights with Contentful's positioning — we extend it with delivery rigor
- Personalization lives inside Contentful since the Ninetailed acquisition; we build on the Personalization API rather than recommending a separate vendor for AB/personalization unless there's a specific reason

## How this shapes Contentful skills

When a skill in this plugin produces a recommendation, frame it this way:

- **Default to composable**: the "right" answer presumes API-first, Topics & Assemblies modeling, and a headless front end. Don't relitigate that choice on every prompt.
- **Editor experience is a Slalom value**: a model that ships fast but breaks for editors is a failed engagement. Walkthroughs are not optional.
- **Migration discipline**: Slalom owns the script-everything posture. UI-only schema changes are not a Slalom answer. `contentful-migrations` is the Slalom way.
- **Multi-environment is the baseline**: even small clients get `master` + at least one non-prod environment with aliases. The deployment-pipeline pattern is the Slalom default.
- **Observability and webhooks**: every Slalom Contentful build ships with signed webhooks, on-demand revalidation, and an audit trail. Time-based revalidation is the safety net, not the strategy.
- **Build center alignment**: when scope grows, expect handoff to Slalom Build for the front-end work. Skills should produce artifacts that travel cleanly to a Build team — typed clients, codegen, story coverage.
- **Composable Accelerator influence**: where there's a session-bridging or staged-migration pattern from Salesforce SFRA / SiteGenesis to a headless front end, that pattern (Build-developed) is the reference.

## Engagement-level expectations

| Engagement level | Implications for Contentful work |
|---|---|
| Low Touch | Single environment, single locale, ~5–10 content types, light webhook setup, often staff-aug into an existing team |
| Standard | Master + 1–2 non-prod environments with aliases, full migration discipline, revalidation strategy, codegen pipeline, design-system aligned components, locale strategy named even if single-locale |
| Complex | Multi-space architecture, cross-space references, multi-locale, personalization, custom App Framework apps, advanced caching, observability dashboards, formal DQR cadence, multi-team governance |

The skills assume Standard by default. Bias up to Complex when the client is enterprise, multi-brand, multi-region, or regulated.

## What this isn't

- Not a Contentful product certification — for that, point at Contentful's official learning paths
- Not a Slalom delivery methodology reference — see `50_Knowledge/Frameworks/Slalom_Summit/` for that
- Not a vendor-neutral CMS comparison — these skills are opinionated for Contentful

For framework-level firm context, see `50_Knowledge/Frameworks/`. For Bermon's career bar and quantitative expectations, see `80_Skills_and_Agents/Memory/practice.md`.
