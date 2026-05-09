---
description: Bootstrap a new Bynder deployment — portal architecture, metaproperty schema, derivatives, permissions, brand guidelines, integrations, with optional Contentful pairing

# Project context
type: command
project: skills-library
plugin: bynder
tags: [type/command, plugin/bynder]
status: active
---

Use the `bynder-agent` to orchestrate a greenfield Bynder stand-up. Pick the engagement shape first:

- **Greenfield Bynder + Contentful** (most common Slalom pattern) — full sequence below, ending in `bynder-contentful-pairing`.
- **Greenfield Bynder-only** — same upstream sequence; downstream depends on which non-Contentful system consumes assets.

The flow runs 7 stages:

1. **Portal architecture** — `bynder-portal-architect` decides single portal vs. sub-portals vs. multi-account, custom domain, sandbox plan, cross-system mapping.
2. **Asset model** — `bynder-asset-model` designs the metaproperty taxonomy (6–10 facets), naming conventions, tag policy, asset-type scoping. Brand-team walkthrough is required.
3. **Brand Guidelines** — `bynder-brand-guidelines` stands up the module with the standard 8-section outline, embedded asset strategy, sync-with-design-system plan.
4. **Derivatives** — `bynder-derivatives` defines the pre-rendered set (10–20 max) aligned to design-system breakpoints, plus the dynamic-transformation preset map.
5. **Permissions and workflow** — `bynder-permissions-workflow` builds the 6–12 profile spine, group structure, IdP/SSO mapping, optional Workflow module setup.
6. **Integrations** — `bynder-marketplace-connectors` installs the Tier 1 adoption drivers (Microsoft 365, Slack, Adobe CC, Figma) and any Tier 2 systems in scope. For Contentful, route to `bynder-contentful-pairing` for OOTB install + custom UCV layering plan.
7. **Webhook plumbing** — `bynder-webhooks-events` wires asset-event sync to downstream caches. Required for any CMS pairing.

Pick the engagement level first (Low Touch / Standard / Complex per `references/slalom-context.md`) — this calibrates the choices in each stage.

Don't skip stages. Each builds on the previous. If the user wants to skip Stage 5 ("we'll figure permissions out later"), push back — permission drift on a deployment without a v1 model becomes the optimization engagement two years later.

After bootstrap completes, schedule the first `/sweep` for ~90 days out as a validation pass.

Don't run bootstrap on a deployment that already has thousands of assets and active conventions — that's an optimization engagement. Use `/audit` instead.
