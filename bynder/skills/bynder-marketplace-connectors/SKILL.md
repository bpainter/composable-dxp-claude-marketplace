---
name: bynder-marketplace-connectors
description: Bynder Marketplace OOTB connectors — when to install rather than build. Covers the connector catalog (Contentful, Adobe Creative Cloud, Salesforce, Sitecore, AEM, WordPress, Drupal, Microsoft 365, Slack, Hootsuite, Workfront, Akeneo, and more), the build-vs-buy decision, the install pattern (both-sides configuration), the gotchas (versioning, permission separation, lack of webhook replication), and the layering pattern where OOTB handles the baseline and custom Compact View handles the gaps. Use this skill any time the engagement implies integrating Bynder with a downstream system — always before recommending a custom build, and explicitly when adding adoption-driver connectors (Microsoft 365, Slack, Adobe CC) to an existing implementation.

# Project context
type: skill
project: skills-library
plugin: bynder
aliases: [bynder-marketplace-connectors]
tags: [type/skill, plugin/bynder, topic/bynder, topic/marketplace, topic/connectors, topic/integration]
status: active
---

# Marketplace Connectors (OOTB-first integration discipline)

This skill puts you in the role of the senior architect who reflexively checks the Bynder Marketplace before recommending a custom build. Default posture: the Marketplace is real software, mostly maintained, and gets the integration to a working baseline in hours rather than weeks. Custom Compact View embeds are the answer for what the connectors don't do — not the answer for everything.

The Marketplace at https://marketplace.bynder.com/en-US/home is the catalog. Skills cite it; this skill operationalizes the build-vs-buy decision and the install pattern.

Pair with `bynder-compact-view` (the custom-build alternative), `bynder-contentful-pairing` (the most-installed connector), `bynder-permissions-workflow` (connectors inherit Bynder permissions), and `bynder-optimization-audit` (connector misconfig is one of the audit's primary findings).

## When to use this skill

- The engagement implies integrating Bynder with a downstream system. Always check the Marketplace first.
- Adding adoption-driver connectors (Microsoft 365, Slack, Adobe CC) to an existing deployment.
- Diagnosing a misbehaving OOTB connector.
- Deciding whether to layer a custom Compact View embed on top of an OOTB connector.
- Auditing existing connectors during an optimization engagement.

## Core posture

- **Marketplace first.** Always. The OOTB connector does 80% of what most custom builds end up doing, costs a fraction of the engineering time, and stays maintained by Bynder or the partner.
- **Configure both sides.** Half the install is in Bynder (portal config, return-derivative, default filters, API user). Half is in the host system (app installation, field assignment, scopes). Skipping the Bynder side is the most common install failure mode.
- **Layer custom on top, don't replace.** When an OOTB connector handles 80% of editorial flow but misses a key 20%, install OOTB for the baseline and build a custom UCV embed for the gap.
- **Connectors don't replicate webhooks.** They handle the picker UX. Downstream cache invalidation is still your job (`bynder-webhooks-events`).
- **Connector permissions and host permissions are separate models.** A user with Bynder access to asset X may not have host-system access to the place X gets embedded. Audit both stacks together.
- **Adoption-driver connectors are leverage.** Microsoft 365, Slack, Adobe CC — these connectors put Bynder in the daily-use surface of every employee. Install them reflexively.

## The connector catalog (high level)

The detailed catalog is in `../../references/marketplace-connectors.md`. This skill works at the decision-framework level.

### Tier 1: install reflexively (high adoption value, low install cost)

| Connector | Why |
|---|---|
| Microsoft 365 (Word, PowerPoint, Outlook, Teams) | Puts Bynder asset search in the daily-use surface. Highest-leverage adoption move. |
| Slack | Same idea, Slack-native. |
| Adobe Creative Cloud (Photoshop, Illustrator, InDesign) | Designers stop downloading-then-uploading. Demonstrates DAM ROI to creative team immediately. |
| Figma | Designer-favorite. Drag-and-drop Bynder assets into Figma frames. |

These cost almost nothing to install and drive measurable adoption. Recommend them on every engagement regardless of greenfield / optimization shape.

### Tier 2: install when scope justifies (CMS, commerce, marketing automation)

| Connector | When |
|---|---|
| Contentful | Default for our practice. See `bynder-contentful-pairing`. |
| Sitecore (XP / XM Cloud) | When client is on Sitecore. |
| Adobe Experience Manager | When client uses AEM as page-authoring with Bynder as enterprise DAM (talk through asset-source-of-truth before installing). |
| Drupal / WordPress | When the client's marketing site lives there. |
| Salesforce Commerce Cloud / commercetools / Shopify | When commerce is in scope. |
| HubSpot / Marketo / Salesforce Marketing Cloud | When the marketing-automation stack is in scope. |
| Akeneo / Salsify / inRiver (PIM) | When PIM is part of the data spine. |

These are real install-and-configure exercises (not click-once), but they're an order of magnitude cheaper than building custom.

### Tier 3: install when explicit fit (workflow, social, niche)

| Connector | When |
|---|---|
| Adobe Workfront | When client's creative-ops runs on Workfront. |
| monday.com / Asana | When project management runs there. |
| Hootsuite / Sprinklr | When social team uses these tools. |

### Always evaluate

For any host system the client uses heavily, check the Marketplace before sizing custom work. The catalog evolves; a connector that didn't exist last quarter may exist this quarter.

## The build-vs-buy framework

```
For host system X:
  Q1: Is there an OOTB Marketplace connector for X?
       └─ No → custom UCV embed (bynder-compact-view); plan accordingly
       └─ Yes → Q2

  Q2: Is the connector actively maintained (last release within 12 months,
      verified-by-Bynder badge, working installs at peer clients)?
       └─ No → consider custom UCV; don't build on a stale connector
       └─ Yes → Q3

  Q3: Does the connector's default behavior cover ≥ 80% of the editorial
      flow (asset picker, returned derivative, default filters, allowed
      asset types)?
       └─ Yes → install OOTB; don't build custom unless gaps emerge later
       └─ Partial → install OOTB + layer custom UCV embed for the gap-specific
                   flows (e.g., a bulk-publish dashboard alongside the
                   standard CMS field control)
       └─ No → custom UCV embed; the connector adds operational overhead
              without value
```

## The install pattern (both-sides configuration)

Every connector has two halves. Skipping either half leaves the integration generic and the editorial flow loose.

### Bynder side

```
Bynder portal admin →
  1. Create / confirm an API user account for the integration (service
     account, not a person).
  2. Issue a Permanent Token or OAuth Client Credentials with scopes
     limited to what the connector actually needs (typically asset:read
     + meta.assetbank:read; possibly :write for connectors that update
     metadata).
  3. Configure Compact View defaults for the integration (return-
     derivative, default filters, allowed asset types). This is per-
     integration config — Contentful and Salesforce can have different
     UCV defaults.
  4. Set up any sub-portal scoping if the integration should only see
     a subset of brands.
  5. Note the Bynder-side identifiers (portal URL, API user, default-
     filter option UUIDs) — the host-side install will ask for these.
```

### Host system side

```
Host system →
  1. Install the connector / app.
  2. Authenticate against Bynder (service-account credentials issued
     above, or OAuth flow if the connector supports it).
  3. Map the connector's field control to the right host-system field
     type (Contentful: JSON object field with Bynder asset control;
     SFCC: content slot or product image; etc.).
  4. Walk an editor through using the field control.
  5. Document the install for runbook purposes.
```

Skip step 4 at your peril.

## Connector audit — what to check on an existing install

For optimization engagements where connectors are in scope:

1. **Are they actually being used?** Pull Analytics API integration usage. Connectors with zero quarterly traffic are usually misconfigured or forgotten.
2. **Are they on a current version?** Check the Marketplace listing for the latest connector release; check what's installed.
3. **Is the auth still valid?** Service-account tokens expire / get rotated; tokens bound to ex-employees stop working.
4. **Are the Bynder-side defaults correct?** Often the connector was installed years ago and the portal config has drifted from current editorial intent.
5. **Are the host-system field assignments still right?** Content types evolve; a Bynder asset field may be on a content type nobody uses anymore.
6. **Are there permission gaps?** Users who should be able to use the picker can't; users who shouldn't can.
7. **Are webhooks wired?** Connectors don't replicate them; check that downstream-cache-invalidation works for asset updates from this connector's flow.

## Output formats

### Connector install plan

```
# Bynder Connector: [Host system]

## Why install (vs. custom UCV)
[Per the build-vs-buy framework.]

## Bynder side
- Service account: [name]
- Token type: [Permanent | OAuth Client Credentials]
- Scopes: [list]
- Compact View defaults:
  - Return derivative: [name]
  - Default filters: [metaproperty option list]
  - Allowed asset types: [list]
- Sub-portal scope (if multi-portal): [...]

## Host system side
- App / connector version: [version]
- Authentication: [credentials reference]
- Field assignments: [content type / object → field name → control]

## Editor walkthrough
- [Pick asset → save → publish → verify]

## Custom layering (if any)
[What custom UCV embed is layered on top, for which flows.]

## Webhook strategy
[Confirmed: connectors don't replicate webhooks. Pointer to bynder-webhooks-events plan.]

## Audit cadence
[Quarterly check-list per connector.]
```

### Connector audit findings

```
# Connector Audit: [Client]

## Connectors in scope
| Connector | Installed | Active? | Version | Last meaningful use |
| ... | ... | ... | ... | ... |

## Findings
- [Connector] — [issue] — [severity] — [proposed fix]

## Adoption-driver gaps
[Tier-1 connectors not installed; expected adoption lift if installed.]

## Recommended remediation waves
1. [Wave 1] — [scope, effort, expected impact]
2. ...
```

## Common anti-patterns to call out

- **Building custom when an OOTB connector exists.** Engineering cost, maintenance cost, brittleness — all avoidable.
- **Installing OOTB but skipping the Bynder side.** Generic picker, no default filters, editors confused, permissions sloppy.
- **Replacing a maintained OOTB connector with custom because of one missing feature.** Layer instead.
- **Forgetting that connectors don't wire webhooks.** Asset updates don't propagate; downstream goes stale; users lose trust.
- **Service account tied to a person.** Person leaves, integration dies.
- **Tier-1 adoption connectors not installed on optimization engagements.** Highest leverage left on the table.
- **Stale connectors trusted as current.** Marketplace listings don't always reflect maintenance status; verify on engagement start.
- **Connector permission scope copy-pasted from documentation.** Scopes should be the minimum needed, not the documentation default.
- **No quarterly connector audit.** Drift accumulates; misconfigs compound.

## Constraints

- Do not propose custom UCV before checking the Marketplace.
- Do not skip Bynder-side configuration on connector installs.
- Do not assume connectors handle webhook-driven sync.
- Do not install with broad token scopes — minimum needed.
- Do not install with a person-bound permanent token in production.
- Do not install Tier 2/3 connectors before installing Tier 1 adoption drivers (unless the engagement explicitly excludes them).

## How this skill relates to others

- The custom-build alternative when no connector fits → `bynder-compact-view`.
- The most-installed connector pattern → `bynder-contentful-pairing`.
- The webhook plumbing connectors don't handle → `bynder-webhooks-events`.
- The permission profiles for service accounts → `bynder-permissions-workflow`.
- The audit signal "broken connector" → `bynder-optimization-audit`.

## Source material

- Bynder Marketplace: https://marketplace.bynder.com/en-US/home
- Plugin reference: `../../references/marketplace-connectors.md` (full catalog with use cases)
- Plugin reference: `../../references/bynder-foundations.md`
- Plugin reference: `../../references/api-surface.md`
