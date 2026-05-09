---
description: Health check on an existing Algolia app — settings drift, no-result queries, missing Insights, record-size health, key hygiene, plan-tier fit. Produces a prioritized fix plan.

# Project context
type: command
project: skills-library
plugin: algolia
tags: [type/command, plugin/algolia]
status: active
---

Use the `algolia-agent` to run a periodic Algolia health audit. This is the equivalent of the obsidian `/audit` — read-only, produces a prioritized fix plan, doesn't apply changes.

Cadence: ~30 days post-bootstrap, then quarterly. Also run on engagement handover or before any major relevance change.

Pre-flight: confirm a scoped read-only key for the MCP/CLI (`search`, `browse`, `listIndexes`, `analytics`).

Run the eight checks. For each, score healthy / warning / critical against the thresholds noted.

1. **Insights wiring** — are `view`, `click`, and `conversion` events flowing? Per index. Critical if any indexed surface has zero events in the last 7 days. Use `algolia-analytics-events` for the taxonomy lens. Tooling: Algolia Analytics → Events tab, or the Analytics API.

2. **Search Analytics — top searches and no-results** — pull the top 50 searches and top 20 no-result searches from the past 30 days. Healthy: <5% no-result rate on top searches. Warning: 5–10%. Critical: >10%. Each no-result query gets triaged into "synonym gap" (fixable in `algolia-relevance-tuning`) or "content gap" (escalate to the contentful agent).

3. **CTR and click position** — mean click position across all searches. Healthy: ≤2.5. Warning: 2.5–4. Critical: >4 (relevance is reordering away from position 1). Surface low-CTR queries (CTR <20%) on the top-100 list.

4. **Settings drift** — diff each index's live settings against `algolia/{index}.settings.json` in the repo. Healthy: zero diffs. Warning: minor formatting only. Critical: any meaningful change in `searchableAttributes`, `customRanking`, `ranking`, or `attributesForFaceting`. If drifted, decide: reconcile (commit Dashboard changes) or revert (re-deploy from source).

5. **Record-size health** — sample records per index. Healthy: max <8 KB (free) or <80 KB (paid). Warning: max approaching the plan limit. Critical: any record at or over the limit. Escalate to `algolia-index-design` for flattening strategy.

6. **Key hygiene** — list all keys per app. Healthy: each key has a description, scoped ACLs, and rotation evidence in the last 90 days. Warning: missing descriptions or unrotated for 6+ months. Critical: an admin-level key in `NEXT_PUBLIC_*` env vars, or a key shared across environments. Surface to `algolia-api-keys-security` for rotation.

7. **Plan-tier fit** — confirm the active plan supports the features in use (Personalization on Grow+, NeuralSearch on Premium). Warning if features are configured but plan-gated off. Critical if quotas are at >80% utilization.

8. **Replica / sort-order health** — confirm replicas exist for any sort orders the front end exposes. Critical if a UI sort selector points to a missing replica. Cheap fix.

For each finding, score and append to the engagement's audit log under `Runbooks/algolia-audit-{date}.md` (or the equivalent engagement notes location).

Produce a prioritized fix plan, ordered by leverage:

- **Foundational**: Insights wiring (Stage 1), key security (Stage 6).
- **High signal-to-noise**: no-result queries fixable by synonyms (Stage 2), settings drift reconciliation (Stage 4).
- **Structural**: record flattening (Stage 5), missing replicas (Stage 8).
- **Defer**: low-CTR query investigation (Stage 3) typically goes to `/algolia-relevance-review`.

Compare against the previous audit log entry if one exists. Surface trends.

Wait for user approval per fix-plan item before handing off to the right specialty skill.

Don't execute fixes during the audit. Produce the plan; route per item.

Don't run this against prod without a read-only key. The `analytics` ACL is enough for everything except settings export, which needs `settings`.
