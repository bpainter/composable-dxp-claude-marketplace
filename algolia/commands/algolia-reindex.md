---
description: Guided atomic reindex from the source-of-truth into an Algolia index. Safety rails: non-prod first, settings snapshot, replaceAllObjects, post-reindex validation, run log.

# Project context
type: command
project: skills-library
plugin: algolia
tags: [type/command, plugin/algolia]
status: active
---

Use the `algolia-agent` to run a guided full reindex. This is for **schema migrations, drift repair, and migrations from another search engine** — not for incremental ops (those run automatically via the indexing pipeline).

When to run:
- Record shape changed in `algolia-index-design`; every record needs reissue.
- Drift repair after a webhook outage or pipeline change.
- Migrating from a legacy search engine.
- Onboarding a new content type that needs initial population.

When NOT to run:
- Routine incremental sync — that's handled by `algolia-indexing-pipeline` webhooks.
- "Search feels stale" without evidence — run `/algolia-audit` first.

Pre-flight (mandatory):
- A scoped key with `addObject`, `editSettings`, `listIndexes` on the target index.
- A non-prod app to test against. **Never reindex prod first.**
- The mapping module from `algolia-contentful-integration` (or equivalent source mapping) is current.
- The current settings exported and committed (rollback insurance).

Run the seven stages.

1. **Snapshot current state** — export the existing settings, rules, and synonyms to local files. Note the current record count. This is the rollback insurance.
   ```
   algolia --profile {env} settings export {index} > /tmp/{index}.settings.before.json
   algolia --profile {env} rules export {index} > /tmp/{index}.rules.before.json
   algolia --profile {env} synonyms export {index} > /tmp/{index}.synonyms.before.json
   algolia --profile {env} indices describe {index}
   ```

2. **Run the mapping in non-prod** — execute the indexer (or backfill script) against the staging app. Confirm record count, record-size distribution, and a spot-check of representative entries.

3. **Validate against the source** — diff source-of-truth count vs. indexed count. Acceptable variance: zero for content-driven indices (every published article should be indexed). Investigate any gaps before proceeding.

4. **Verify search behavior in non-prod** — run a small set of canonical queries via MCP or the Dashboard. Compare ranking and hit shape against expectations. Surface any regressions to `algolia-relevance-tuning`.

5. **Atomic swap to prod** — only after stages 2–4 pass:
   ```
   algolia --profile prod objects import {index} --file ./records.ndjson --replace
   ```
   `--replace` uses `replaceAllObjects` — atomic, no empty-index window. Never use `clearIndex` followed by `saveObjects` for this; the empty-index window will hit production users.

6. **Post-reindex validation** — confirm record count, run the same canonical queries against prod, watch Search Analytics for the next hour. Look for spikes in no-result rate or drops in CTR.

7. **Run log** — append to `Runbooks/algolia-reindex-{index}-{date}.md`:
   - Trigger and motivation
   - Source state (entries indexed, locale set)
   - Pre-reindex record count vs. post
   - Settings/rules/synonyms unchanged (snapshot diff)
   - Any anomalies during stages 2–4 and how resolved
   - Post-reindex validation summary
   - Rollback procedure (re-import the snapshot files)

Confirm with the user before stage 5 — that's the destructive step.

If stage 6 surfaces a regression (canonical query returns wrong result, no-result rate spike), execute the rollback:
```
algolia --profile prod settings import {index} --file /tmp/{index}.settings.before.json
# re-import previous records from your last known-good NDJSON, with --replace
```

Don't run a reindex without a settings snapshot.
Don't skip non-prod validation. "It worked in staging" is the bar.
Don't reindex during high-traffic windows. Schedule for low-traffic if possible.
Don't reindex multiple indices in parallel without per-index validation. Sequence them.
