---
type: reference
project: skills-library
scope: plugin
plugin: bynder
tags: [type/reference, plugin/bynder, scope/foundational, topic/bynder]
status: active
---
# Slalom + Composable DXP context for Bynder work

This reference frames every skill in this plugin against Slalom's organizational model, the Composable DXP practice, and the Bynder partnership. Skills cite this file to keep recommendations grounded in how Slalom actually engages clients on Bynder work — not generic DAM-vendor advice.

## Slalom in one paragraph

Slalom is a privately held, partner-owned consulting firm — ~12,000 people, 49 offices, "strategy to execution" positioning. The differentiator is that Slalom advises and builds; most consulting firms hand off a deck, Slalom hands off working software through Slalom Build. Engagement model is ~70–80% staff augmentation embedded in client teams plus ~20–30% scoped projects. Local-first delivery means same-time-zone teams and minimal travel.

## Composable DXP practice (Bermon's practice)

Inside Slalom's Enterprise capability. Sister practices cover Salesforce, Adobe Experience, ServiceNow, Anaplan / business planning, and Boomi / integration. Composable DXP focuses on:

- Modular DXP architectures wired by APIs and event streams, not monoliths
- Headless CMS, commerce, search, personalization, identity, integration, and **DAM**
- MACH principles — Microservices, API-first, Cloud-native, Headless
- Reference stack: Contentful + Next.js + Vercel + Algolia + a CDP/personalization layer (Contentful Personalization, formerly Ninetailed) + **Bynder as the DAM where assets are a first-class concern**
- Delivery against Slalom Summit (Mobilize → Discover → Deliver → Transition → E&O → Close, 5 Risk Gates), Pursuit Excellence on the front end, Delivery Excellence as the quality framework

## Slalom × Bynder partnership

Bynder is a tier-1 technology partner for the practice. Slalom shows up as an implementation partner on enterprise Bynder pursuits — most commonly bundled with Contentful, sometimes as a stand-alone DAM stand-up, and increasingly as **Bynder optimization** engagements where the client has Bynder but isn't getting full value from it. That means:

- Skills should assume Bynder is a chosen partner, not one of many — but stay platform-aware enough to compare against Acquia DAM (Widen), Brandfolder, Cloudinary, Adobe AEM Assets, MediaValet, and Frontify when the pursuit calls for it
- We don't pick fights with Bynder's positioning — we extend it with delivery rigor and the integration discipline that DAM-only vendors can't ship alone
- Bynder + Contentful is the default composable pairing; the OOTB connector is the starting point but rarely the finishing one — Slalom usually builds custom Compact View flows, derivative selection UX, and webhook plumbing on top

## How this shapes Bynder skills

When a skill in this plugin produces a recommendation, frame it this way:

- **Default to composable**: the "right" answer presumes Bynder is the source of truth for assets, references flow through to a headless CMS, and the front end consumes derivatives via API or CDN URL. Don't relitigate that.
- **Editor / brand-team experience is a Slalom value**: a metaproperty schema that ships fast but breaks for the brand team is a failed engagement. Walkthroughs are not optional, and adoption metrics matter as much as technical correctness.
- **Taxonomy discipline**: Slalom owns the "model the metaproperty before you upload the asset" posture. Free-text, ungoverned metadata is not a Slalom answer.
- **Multi-environment is the baseline** when the client's plan supports it: at least one non-prod portal or sandbox before production. Where the plan doesn't support it, a documented import/export discipline.
- **Integration is the value driver**: assets in Bynder that don't flow to channels (CMS, commerce, social, sales enablement) are unrealized ROI. Every Slalom Bynder build ships with at least one downstream integration wired and verified.
- **Build center alignment**: when scope grows, expect handoff to Slalom Build for custom Compact View, custom marketplace apps, or migration tooling. Skills should produce artifacts that travel cleanly to a Build team — typed clients, scripted imports, clear ops runbooks.
- **Optimization is a real engagement shape**: many Bynder clients are running v1 of a deployment that has decayed (taxonomy sprawl, derivative ZIP bombs, permission drift, abandoned connectors). Slalom is well-positioned to come in and fix that — the `bynder-optimization-audit` skill exists for this.

## Engagement-level expectations

| Engagement level | Implications for Bynder work |
|---|---|
| Low Touch | Single account, single brand, ~5–10 metaproperties, light derivative set, OOTB connector to one downstream system, often staff-aug into an existing team |
| Standard | Single account with sub-portal split or a dev sandbox, full taxonomy design, derivative library aligned to the design system, OOTB connector + at least one custom integration via SDK, webhook-driven cache invalidation, Brand Guidelines module live |
| Complex | Multi-account / multi-brand, custom Compact View embeds in 2+ downstream systems, custom marketplace app, formal asset governance with workflow stages, advanced analytics and adoption reporting, migration from a legacy DAM, formal DQR cadence |
| Optimization | Existing Bynder implementation. Audit-first; remediation plan with prioritized waves; usually scoped 4–12 weeks; deliverables are taxonomy normalization, derivative cleanup, permissions reset, integration repair, and an adoption playback |

The skills assume Standard by default. Bias up to Complex when the client is enterprise, multi-brand, multi-region, or regulated; bias to Optimization when the client says any version of "we have Bynder but it isn't working."

## Optimization engagement signals

Watch for these in pursuit conversations — they almost always point to an optimization scope, not a greenfield:

- "We rolled out Bynder a few years ago but no one uses it"
- "We have thousands of duplicate assets"
- "Our editors download from Bynder, edit locally, then re-upload — the source of truth is fuzzy"
- "The Contentful connector kind of works but"
- "We can't tell which derivative to use for which channel"
- "Permissions are a mess; everyone has access to everything or no one has access to anything"
- "We can't measure asset usage"

Each of these maps directly to a skill or a sequence in `bynder-optimization-audit`.

## What this isn't

- Not a Bynder product certification — for that, point at Bynder Academy
- Not a Slalom delivery methodology reference — see `50_Knowledge/Frameworks/Slalom_Summit/` for that
- Not a vendor-neutral DAM comparison — these skills are opinionated for Bynder

For framework-level firm context, see `50_Knowledge/Frameworks/`. For Bermon's career bar and quantitative expectations, see `80_Skills_and_Agents/Memory/practice.md`.
