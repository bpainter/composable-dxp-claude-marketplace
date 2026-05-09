---
name: bynder-portal-architect
description: Portal, sub-portal, account, and environment architecture for Bynder — when to use a single portal vs. multiple, how to split sub-portals for multi-brand, custom domains, dev/test/prod patterns where the plan supports separate environments, and the trade-offs between strong isolation and shared-asset-pool flexibility. Covers portal naming, branding, custom CSS, login customization, and the cross-system mapping (Bynder portal × Contentful environment × CDN). Use this skill when standing up a brand-new Bynder account, splitting an existing single-brand deployment into multi-brand, planning a sandbox / non-prod environment, or migrating from one portal architecture to another.

# Project context
type: skill
project: skills-library
plugin: bynder
aliases: [bynder-portal-architect]
tags: [type/skill, plugin/bynder, topic/bynder, topic/architecture]
status: active
---

# Portal Architect (Bynder accounts, sub-portals, environments)

This skill puts you in the role of a senior architect who's stood up Bynder for clients ranging from "single brand, single market" to "five brands, three regions, two business units." Default posture: simpler is better. Most clients are single-portal; most multi-portal architectures are over-engineered until proven necessary. Sub-portals are the right multi-brand answer in almost every case.

Bynder's account model: one account → one or more portals (the customer-facing surface) → optional sub-portals (brand or regional cuts within the same account) → asset banks, brand guidelines, workflows, etc. Custom domains, custom themes, and login customization layer on top.

Pair with `bynder-asset-model` (the metaproperty schema lives in the portal), `bynder-permissions-workflow` (profiles span portals or scope to one), `bynder-brand-guidelines` (multi-brand often means multi-guideline), `bynder-contentful-pairing` (the cross-system mapping), and `bynder-migration` if the engagement is restructuring an existing portal architecture.

## When to use this skill

- Standing up a brand-new Bynder account.
- Splitting a single-brand deployment into multi-brand.
- Consolidating multiple brand silos that grew separately.
- Planning a sandbox / non-prod environment alongside production.
- Choosing between sub-portal split, single portal + brand metaproperty, or multi-account architecture.
- Setting up custom domains and login customization.
- Mapping Bynder portals to downstream environments (Contentful spaces, CDN buckets, etc.).
- Migrating from one portal architecture to another.

## Core posture

- **Single portal until proven otherwise.** Most clients work fine with one portal. Multi-portal adds operational overhead.
- **Sub-portals are usually the right multi-brand answer.** Same account, isolated brand surfaces, shared admin and shared analytics, separate guideline trees. Strong-enough isolation for most clients.
- **Multi-account is rare and expensive.** Use only when contractual / legal isolation is required (separate business units with separate billing, an acquired company being run independently).
- **Custom domains drive adoption.** `assets.acme.com` looks like an Acme product; `acme.bynder.com` looks like an external vendor.
- **Sandbox is plan-dependent.** Not every Bynder plan includes a non-prod environment. Where it doesn't, mimic via permissions and a `Sandbox` collection.
- **Document the cross-system mapping.** Bynder portal × Contentful environment × CDN bucket × analytics property — write it down once, verify quarterly.

## The portal architecture decision tree

```
Q1: Does the client have one brand or multiple?
   └─ One brand → Single portal. Done.
   └─ Multiple brands → Q2

Q2: Do brands share editorial governance, asset cross-pollination,
    and a single brand-ops team?
   └─ Yes → Single portal + Brand metaproperty (+ permission profiles
            scoped per brand for editor-side isolation if needed)
   └─ No → Q3

Q3: Are brands run by independent teams with separate guidelines,
    separate asset taxonomies, and minimal asset reuse?
   └─ Yes → Sub-portals (one account, multiple sub-portal surfaces)
   └─ Mostly yes but billing must be separate / acquired company
     in handoff → Multi-account

Q4: Does production need a sandbox / non-prod environment?
   └─ Plan includes sandbox → use it
   └─ Plan does not → simulate via permission profiles + Sandbox
                       collection
```

## Single portal — the default

For the vast majority of engagements:

```
Acme Corp
  └── Portal: assets.acme.com (custom domain)
        ├── Asset Bank
        ├── Brand Guidelines
        ├── Workflow (if licensed)
        ├── Analytics
        └── Users / Profiles (one set, one identity model)
```

Pros: simplest operational model. One admin team, one analytics property, one brand-guideline tree, one asset-bank search surface.

Cons: doesn't isolate brand surfaces if the client really does run brands as separate businesses.

When this fits: one brand. One marketing org. One creative team. Asset reuse across products / regions / channels is normal.

## Single portal + Brand metaproperty — the soft multi-brand answer

For multi-brand clients where brands share governance:

```
Acme Holdings
  └── Portal: assets.acme.com
        ├── Asset Bank
        │     └── Assets tagged with property_Brand ∈ {Acme, Acme Lite, Acme Industrial}
        ├── Brand Guidelines (one tree, with sections per brand)
        └── Permission profiles
              ├── Brand: Acme — full asset access
              ├── Brand: Acme Lite — restricted to property_Brand=Acme Lite
              └── ...
```

Pros: shared asset pool for brand-cross-pollination (Acme corporate photography reusable across sub-brands), single admin team, simpler analytics.

Cons: brand teams see each other's filter results in the search sidebar — can feel cluttered. Brand Guidelines is one tree, which can get long.

When this fits: holding-company structures where brands ladder up to a corporate identity. Multiple product lines under one umbrella brand. Regional variations of one global brand.

## Sub-portals — the strong multi-brand answer

```
Acme Holdings (account)
  ├── Sub-portal: assets.acme.com (Acme master brand)
  │     ├── Asset Bank
  │     ├── Brand Guidelines
  │     └── ...
  ├── Sub-portal: assets.acmelite.com (Acme Lite, separate consumer brand)
  │     ├── Asset Bank
  │     ├── Brand Guidelines
  │     └── ...
  └── Sub-portal: assets.acme-industrial.com (B2B division)
        └── ...
```

Pros: each brand has a clean surface, independent guidelines, independent search experience. Editors only see their brand by default. Analytics per brand.

Cons: cross-brand asset reuse requires explicit duplication or cross-portal link (clunky). Three admin contexts to maintain. Three places to update if a global change is needed.

When this fits: brands run as separate businesses with separate teams, separate guidelines, minimal asset reuse. Acquired-but-not-consolidated brands. B2B / B2C splits.

## Multi-account — the rare strong-isolation answer

Two separate Bynder accounts, separate billing, separate everything. Use only when:

- Legal / contractual isolation is required.
- Acquired company will be divested or run independently.
- Different business units have different billing entities.

The integration cost is real — separate APIs, separate tokens, separate admin teams. Almost every "we need separate accounts" conversation ends in "let's do sub-portals" once the cost is visible.

## Sandbox / non-prod

Not every Bynder plan includes a sandbox. When the plan does:

```
Production portal:    assets.acme.com  (full asset library, prod permissions)
Sandbox portal:       sandbox-assets.acme.com  (subset, dev/test users only)
```

Use the sandbox for:

- Migration dry-runs (bulk import, metaproperty changes).
- Testing custom Compact View embeds.
- Webhook receiver testing.
- Onboarding new editorial staff.

Replication from prod to sandbox is usually one-way and manual / scripted. Bynder doesn't have native cross-portal sync.

When the plan doesn't include sandbox:

- Use a `Sandbox` collection within the production portal.
- Permission-gate it so only dev/test users can access.
- Test integrations against assets in this collection only.
- Document clearly so editors don't accidentally publish from it.

## Custom domains

Production should always be on a custom domain — `assets.{client}.com` or `dam.{client}.com`. Bynder's default `{client}.bynder.com` looks like an external vendor; the custom domain looks like a client product.

Setup:

1. Configure the domain in the Bynder portal admin.
2. Set up DNS (CNAME to Bynder's CDN domain).
3. SSL is managed by Bynder (Let's Encrypt or similar).
4. Test with users on different networks.

Cost: usually included in mid-tier plans and up; confirm.

## Theme and login customization

Apply the brand to the portal:

- Logo (top-left, login screen).
- Primary color (links, buttons, accents).
- Login background image.
- Login messaging (welcome text, terms).
- Custom CSS for advanced theming.

Slalom default: match the client's primary brand color, ship a custom login background that's asset-bank-derived (using a hero from the bank). Don't over-theme — Bynder's default UI is good; custom CSS that fights the underlying layout breaks on every Bynder release.

## Cross-system mapping

Document the mapping between Bynder and downstream systems. Example for a single-brand Bynder + Contentful client with prod + non-prod:

| Surface | Production | Non-prod |
|---|---|---|
| Bynder portal | `assets.acme.com` | Same portal, `Sandbox` collection |
| Bynder API endpoint | `acme.bynder.com/api/` | Same |
| Bynder permission profile | `Acme — Editor (Production)` | `Acme — Sandbox` |
| Contentful space | `acme-production` | Same |
| Contentful environment alias | `master` | `staging` |
| CDN | `cdn.acme.com` | `cdn-staging.acme.com` |
| Front end | `acme.com` | `staging.acme.com` |

The most common production incidents on Bynder + Contentful pairings come from environment mismatch — a non-prod Contentful pulling production assets, a sandbox token reaching production. Document the mapping; verify quarterly.

## Output formats

### Portal architecture proposal

```
# Bynder Portal Architecture: [Client]

## Decision summary
[Single portal | Single + Brand metaproperty | Sub-portals | Multi-account]
[Why; what was considered; what was traded off.]

## Portal structure
[Tree view: account → portal(s) → sub-portal(s)]

## Brand strategy
[How brand surfaces map to portals / metaproperties / permissions.]

## Custom domain plan
[Domains; DNS; SSL; rollout sequence.]

## Theme / login customization
[Logo, colors, login background, messaging.]

## Sandbox plan
[Plan-included sandbox | Sandbox collection | Other.]

## Cross-system mapping
| Surface | Production | Non-prod |
| ... | ... | ... |

## Migration (if existing)
[Current state; target state; sequenced steps; risks.]

## Operational handoff
[Admin team; documentation location; quarterly review.]
```

## Common anti-patterns to call out

- **Multi-account when sub-portals would suffice.** Real cost; rare benefit.
- **Sub-portals when single + Brand metaproperty would suffice.** Operational overhead per brand for clients that share governance.
- **Single portal when sub-portals are obviously needed.** Brand teams stepping on each other in the search UI; guidelines bleeding across brands.
- **Default `{client}.bynder.com` domain in production.** Looks like vendor; reduces adoption.
- **No sandbox plan, no `Sandbox` collection.** Devs end up testing against production assets.
- **Cross-system mapping not documented.** Environment mismatch incidents.
- **Heavy custom CSS that fights Bynder's layout.** Breaks on every Bynder release.
- **Using the API user token for theme administration.** Different scope; admin operations require admin permissions.

## Constraints

- Do not propose multi-account without confirming the contractual / billing requirement.
- Do not propose sub-portals without confirming brands have separate teams and minimal asset reuse.
- Do not propose a sandbox plan without confirming what the customer's Bynder license includes.
- Do not skip the cross-system mapping document.
- Do not theme so heavily that future Bynder releases will break the portal.

## How this skill relates to others

- The metaproperty schema that lives in the portal → `bynder-asset-model`.
- The permission profiles that map editors to portal scope → `bynder-permissions-workflow`.
- The Brand Guidelines tree (per portal or per brand) → `bynder-brand-guidelines`.
- The Contentful integration that depends on the cross-system mapping → `bynder-contentful-pairing`.
- Migration between portal architectures → `bynder-migration`.

## Source material

- Bynder admin / customization docs: https://academy.bynder.com/
- Plugin reference: `../../references/bynder-foundations.md`
- Plugin reference: `../../references/api-surface.md`
