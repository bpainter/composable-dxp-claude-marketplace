---
description: Run a Bynder optimization audit — eight metrics (taxonomy, derivatives, permissions, integrations, adoption, brand-guidelines, governance, data quality) with a prioritized remediation plan

# Project context
type: command
project: skills-library
plugin: bynder
tags: [type/command, plugin/bynder]
status: active
---

Use the `bynder-optimization-audit` skill to run the audit-first remediation playbook for an existing Bynder implementation.

This is the right command when the user says any version of "we have Bynder but it isn't working" — duplicates, low adoption, broken connectors, taxonomy decay, permission drift, stale brand guidelines.

The flow runs three days (target):

1. **Day 1 — quantitative pull**: Analytics (views/downloads/searches/zero-result rate/integration usage), metaproperty schema, tag list, profile + group + user roster, derivative templates, webhook config, installed Marketplace connectors, last-login distribution.
2. **Day 2 — qualitative interviews**: brand-ops lead, primary editor, downstream-system owner, end-user representative.
3. **Day 3 — synthesis**: score each of the 8 metrics (Healthy / Drifting / Critical), identify highest-leverage findings, sequence remediation waves, draft the report.

Score each metric and route findings to the relevant remediation skill:

- Taxonomy / metaproperty health → `bynder-asset-model`
- Derivative health → `bynder-derivatives`
- Permission and access → `bynder-permissions-workflow`
- Integration / connector health → `bynder-marketplace-connectors`, `bynder-contentful-pairing`, `bynder-compact-view`, `bynder-webhooks-events`
- Adoption → `bynder-marketplace-connectors` (install adoption drivers), `bynder-asset-model` (taxonomy gaps)
- Brand Guidelines activation → `bynder-brand-guidelines`
- Analytics and governance → `bynder-permissions-workflow`
- Migration / data quality → `bynder-migration`

Sequence the remediation by leverage. Wave 1 is quick wins (install Microsoft 365 / Slack / Adobe CC connectors, rotate stale tokens, disable idle users, set focal points on top-100 photo assets). Wave 2 is structural fixes (taxonomy normalization, derivative cleanup, permissions reset). Wave 3 is adoption and governance.

Don't execute fixes during the audit itself — produce the plan, then route per-item to specialty skills.

End the engagement with a re-audit. Recorded delta is the headline.
