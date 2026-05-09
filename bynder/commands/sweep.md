---
description: Run a quarterly Bynder hygiene sweep — audit, then sequentially fix tag/derivative drift, permission decay, broken webhooks, stale brand-guideline pages, and connector misconfigurations

# Project context
type: command
project: skills-library
plugin: bynder
tags: [type/command, plugin/bynder]
status: active
---

Use the `bynder-agent` to run a periodic full hygiene pass on a Bynder deployment.

Different from `/audit`: this is for Slalom-built deployments on a quarterly review cadence, not a first-time audit on an unfamiliar implementation. The rubric is the same, the posture is "catch drift before it compounds" rather than "diagnose what's broken."

The flow runs ~60–90 minutes for a healthy deployment, longer for one that's been neglected:

1. **Run the audit** — `/audit` produces the scorecard against the eight metrics. Skip this step only if a fresh audit was completed in the last 30 days.
2. **Quick wins first** — go through the wave-1 menu from `bynder-optimization-audit/SKILL.md`:
   - Disable users idle > 6 months
   - Rotate any service-account token > 12 months old
   - Disable webhooks delivering to disabled receivers
   - Confirm Tier 1 adoption-driver connectors (Microsoft 365, Slack, Adobe CC, Figma) are installed and active
   - Set focal points on top-100 most-downloaded photo assets that are missing them
3. **Tag and metaproperty hygiene** — `bynder-asset-model`. Prune tag synonyms, retire unused options, document any new metaproperties added since last sweep.
4. **Derivative hygiene** — `bynder-derivatives`. Pull Analytics; identify unused derivatives; schedule deprecation; verify on-the-fly transformation presets are still aligned with front-end constants.
5. **Permission audit** — `bynder-permissions-workflow`. Profile-per-user creep, group membership drift, IdP sync delta, scope-rule alignment with current metaproperty values.
6. **Webhook delivery health** — `bynder-webhooks-events`. Pull recent delivery history from Bynder admin; surface failing receivers; confirm HMAC secrets are within rotation policy.
7. **Brand Guidelines freshness** — `bynder-brand-guidelines`. Last-updated dates per section; page view counts; design-system token drift if sync is configured.
8. **Re-audit and record delta** — re-run the scorecard, log the deltas, schedule the next sweep on the calendar.

Document findings in the engagement's quarterly review record. The delta from sweep to sweep is the long-running health metric.

Don't run sweep on a deployment that's never been audited — start with `/audit` for the baseline.
