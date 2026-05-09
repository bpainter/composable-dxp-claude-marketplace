---
description: Stand up Algolia for a new engagement — applications per environment, indices, keys with least-privilege ACLs, settings-as-code repo layout, Insights wiring, and the Contentful indexer scaffold.

# Project context
type: command
project: skills-library
plugin: algolia
tags: [type/command, plugin/algolia]
status: active
---

Use the `algolia-agent` to set up a new Algolia integration end-to-end. This is the greenfield runbook for a Composable DXP engagement.

Pick the engagement context first — Contentful + Next.js (default), commerce-on-Shopify, or other source. The choice calibrates Stage 4 onward.

The flow runs 8 stages, each delegating to the right skill:

1. **Applications + plan** — confirm the active plan tier (Build / Grow / Premium) and provision per-environment apps (`slalom-{client}-prod`, `-staging`, `-dev`). Premium-only features (NeuralSearch, AI Browse) are flagged for Stage 7. See `references/slalom-context.md` for the partner-portal sandbox path.

2. **Index design** — `algolia-index-design` decides per-type indices vs. federated, locale fan-out, replicas (default virtual), distinct strategy. Output: an attribute matrix per index, committed to the engagement's docs.

3. **Records and source mapping** — for Contentful sources, `algolia-contentful-integration` picks Marketplace app vs. custom Vercel-Function indexer. For other sources, see `references/integrations-map.md`. Output: a mapping module per content type.

4. **Indexing pipeline** — `algolia-indexing-pipeline` scaffolds the webhook handler, signature verification, atomic-reindex script, and scheduled drift-repair cron. Default tier 2 (Vercel Function + Contentful webhook).

5. **Keys** — `algolia-api-keys-security` mints scoped keys per workload (indexer, settings-deploy, browser search-only, MCP read). Stores them in the engagement's secret store. Documents the secured-API-key pattern if multi-tenant.

6. **Settings, rules, synonyms — as code** — establish `algolia/` at repo root with `*.settings.json`, `*.rules.json`, `*.synonyms.json` per index. CI workflow deploys staging on merge, prod on manual trigger. See `algolia-mcp-cli` for the deploy script and `references/cli-cheat-sheet.md` for the CLI commands.

7. **Front-end wiring** — `algolia-instantsearch-react` for the SERP, `algolia-autocomplete` for the global header, `algolia-search-client` for any custom or RSC surface. Hand-off to `software-engineering-frontend-developer` for the design-system primitives.

8. **Insights + analytics baseline** — `algolia-analytics-events` writes the event taxonomy (event names, `userToken` policy, queryID attribution). Wire on day one. Defer Personalization, Recommend, NeuralSearch to a later A/B once events have flowed for ≥30 days.

Expected duration: a focused 1–2 day kickoff, plus 1–2 weeks of incremental wiring before the SERP goes live.

Don't skip Stage 8. Without Insights, Recommend and Personalization can't train, A/B tests can't measure, and Search Analytics flies blind.

Don't enable AI features (NeuralSearch, AI Browse, Personalization) during bootstrap. Capture them as a Phase 2 backlog item that runs after the events history is sufficient.

After bootstrap completes, the integration is ready to operate. The first `/algolia-audit` should run ~30 days later to validate the events are flowing and surface any settings drift.

If the engagement already has Algolia in production with active records, this is a restructure, not a bootstrap. Use `/algolia-audit` and `/algolia-relevance-review` instead.
