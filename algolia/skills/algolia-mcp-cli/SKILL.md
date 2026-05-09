---
name: algolia-mcp-cli
description: >
  Practical Algolia MCP server and CLI workflows — when to reach for which, agent-led indexing and relevance work, scripted ops in CI, safety rails, and the Slalom defaults for keys and run logs. Use this skill when an LLM agent needs to operate against an Algolia app (read settings, propose synonyms, simulate ranking, apply rules), when scripting reproducible ops in CI (settings/rules/synonyms-as-code), or when deciding which interface fits a given task. The MCP shines for conversational diagnosis; the CLI shines for repeatable deploys.

# Project context
type: skill
project: skills-library
plugin: algolia
aliases: [algolia-mcp-cli]
tags: [type/skill, plugin/algolia, topic/algolia, topic/mcp, topic/cli, topic/agentic]
status: active
---

This skill puts you in the role of an engineer (or an LLM agent) deciding which interface fits a given Algolia op. Default posture: **MCP for conversational diagnosis and one-off tweaks; CLI for reproducible, version-controlled, CI-deployed work.**

Pair with `algolia-relevance-tuning` (the changes you make via these tools), `algolia-indexing-pipeline` (the bulk write paths), `algolia-api-keys-security` (the keys these tools use), and `references/cli-cheat-sheet.md` / `references/mcp-cheat-sheet.md` for command-level details.

## When to use this skill

- Agent-led ops session — you want an LLM to propose synonyms, simulate a query rule, or audit settings.
- Standing up Algolia ops in CI — settings/rules/synonyms-as-code, deploy on merge.
- Deciding between MCP and CLI for a task.
- Scripting a one-off (bulk-apply 30 synonyms, add `popularity` to `customRanking` across 5 indices).
- Debugging "the search returns weird results" with the MCP doing the legwork.
- Hardening agent prompts so they don't blow up the index.

## Core posture

- **MCP for conversation, CLI for code.** They overlap; the right pick is whichever ends up in version control.
- **Per-engagement scoped keys.** Never share keys across engagements. Never share keys across humans.
- **Read-only by default.** Promote to write only for the specific session. Demote (or expire) immediately after.
- **Run log discipline.** Every MCP-driven change to a non-prod app — write what changed, why, and the rollback. The MCP makes destructive ops easy; the run log makes them recoverable.
- **Production stays in CI.** Agents diagnose in non-prod; CI deploys to prod after human review. Don't let the agent push to prod directly.

## When to use the MCP

The Algolia MCP server (https://github.com/algolia/mcp-node) exposes the search-API client to LLM agents. Reach for it when:

- You want **conversational synthesis** over the index — "which queries return zero results and have a good content-side fix?" The agent reads the analytics, the records, the settings, and produces a plan.
- You're **diagnosing relevance** — "why does record X rank below record Y for query Q?" The agent fetches the ranking info per record and walks through the eight ranking dimensions.
- You're **proposing synonyms or rules** based on patterns the agent reads from search analytics.
- You're doing **one-off tweaks** in a non-prod app and the alternative is a CLI script you'd write once and never run again.

When NOT to use the MCP:

- **Repeatable ops** that should live in version control. Use the CLI in CI.
- **Bulk imports** of >100k records. Use the SDK / CLI / Ingestion API.
- **Anything you can't roll back** without backups. The MCP can `clearIndex` and `deleteIndex` if the key allows it.

### MCP setup

```bash
# Install (Node-based; runs over stdio for Claude Desktop / Code / Cowork)
npx -y @algolia/mcp-node@latest
```

Configure in the client (Claude Desktop's `claude_desktop_config.json`, or Cowork settings):

```json
{
  "mcpServers": {
    "algolia": {
      "command": "npx",
      "args": ["-y", "@algolia/mcp-node@latest"],
      "env": {
        "ALGOLIA_APPLICATION_ID": "ABC123",
        "ALGOLIA_API_KEY": "<scoped-read-key-with-search-browse-listIndexes-analytics>"
      }
    }
  }
}
```

Use a **scoped key**, not the admin key. The MCP gets exactly the ACLs the key allows. See `algolia-api-keys-security` for the recommended ACLs by use case.

### High-leverage MCP prompts

These map well — each is one or two tool calls behind the scenes:

**Inspection / diagnosis:**
- "Explain the current `searchableAttributes`, `customRanking`, and `ranking` on the `articles` index, in plain language."
- "Show me the top 20 no-result searches in the past 30 days. For each, suggest whether it's a synonym gap or a content gap and why."
- "Run the query `composable cms` on the `articles` index. Walk me through why the top 3 hits rank where they do — typo, words, attribute, custom, etc."
- "Find records over 10 KB and recommend a flattening strategy."
- "Compare settings between `articles_staging` and `articles` (prod). Surface the diffs that matter for relevance."

**Modeling:**
- "Suggest a faceting strategy for the `products` index based on the attribute distribution in the records."
- "Propose a `_distinct_key` strategy to collapse color/size variants based on the records I'm looking at."

**Settings / rules / synonyms (write — only with a scoped key that allows it):**
- "Add a synonym group: `composable, headless, decoupled` (multi-way) to `articles`."
- "Create a query rule on `articles` that pins `whitepaper-2026-composable-dxp` to position 1 for queries `composable dxp`, `headless cms`."
- "Add `popularity` to `customRanking` on `articles`, after `publishedAt_unix`."

**Indexing (use carefully):**
- "Reindex `articles` from this NDJSON file using `replaceAllObjects`."
- "Apply this partial-update batch to update `popularity` on these 200 objectIDs."

### MCP guardrails — what to put in the agent prompt

Put guardrails in the agent's system prompt or per-task instructions:

- "Never call `clearIndex` or `deleteIndex`."
- "Confirm every write op with me before executing — show the proposed change first."
- "Always check current settings before proposing changes."
- "Pair every relevance change with a clear hypothesis and a measurement plan."
- "Never apply changes to indices ending in `_prod` directly. Use the staging app."

The MCP is a powerful tool. Treating it as autonomous-by-default is how you discover the run log mattered.

## When to use the CLI

The Algolia CLI (https://github.com/algolia/cli) is the fastest path for repeatable, scriptable ops. Reach for it when:

- **Settings, rules, and synonyms are version-controlled.** Export → commit → review → import in CI.
- **Index migrations** (atomic reindex from a file).
- **CI deploys** of the above on merge to main.
- **Auditing** — diff settings between staging and prod.
- **Scheduled jobs** — nightly re-export to detect Dashboard drift.

When NOT to use the CLI:

- **Conversational diagnosis** — the MCP is faster.
- **Bulk imports of >1M records** — write a Node script that uses the SDK with proper batching and error handling.

### CLI setup

```bash
brew install algolia/algolia-cli/algolia
algolia auth login
# OR per-profile, per-environment
algolia profile add prod
ALGOLIA_APP_ID=... ALGOLIA_API_KEY=... algolia --profile prod indices list
```

Stash the API key in the engagement's secret store; never commit.

### Settings-as-code workflow

```
algolia/
├── articles.settings.json
├── articles.rules.json
├── articles.synonyms.json
├── glossary_terms.settings.json
├── ...
└── deploy.sh
```

`deploy.sh`:

```bash
#!/usr/bin/env bash
set -euo pipefail

PROFILE="${1:-staging}"
INDICES=(articles glossary_terms)

for IDX in "${INDICES[@]}"; do
  algolia --profile "$PROFILE" settings import "$IDX" --file "algolia/$IDX.settings.json"
  algolia --profile "$PROFILE" rules import "$IDX" --file "algolia/$IDX.rules.json" --replace
  algolia --profile "$PROFILE" synonyms import "$IDX" --file "algolia/$IDX.synonyms.json" --replace
done
```

CI workflow (GitHub Actions):

```yaml
# .github/workflows/algolia-deploy.yml
name: Algolia deploy
on:
  push:
    branches: [main]
    paths: ['algolia/**']
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: brew install algolia/algolia-cli/algolia
      - name: Deploy to staging
        run: ./algolia/deploy.sh staging
        env:
          ALGOLIA_APP_ID: ${{ secrets.ALGOLIA_STAGING_APP_ID }}
          ALGOLIA_API_KEY: ${{ secrets.ALGOLIA_SETTINGS_DEPLOY_KEY }}
      - name: Deploy to prod (with approval)
        if: github.event_name == 'workflow_dispatch'
        run: ./algolia/deploy.sh prod
        env:
          ALGOLIA_APP_ID: ${{ secrets.ALGOLIA_PROD_APP_ID }}
          ALGOLIA_API_KEY: ${{ secrets.ALGOLIA_PROD_DEPLOY_KEY }}
```

Staging deploy on merge; prod deploy on manual trigger.

### Drift detection

Schedule a daily diff:

```bash
algolia --profile prod settings export articles > /tmp/prod.settings.json
diff algolia/articles.settings.json /tmp/prod.settings.json
```

If the diff is non-empty, someone edited prod settings in the Dashboard. Either reconcile (commit the changes) or re-deploy from source (revert the Dashboard changes). Either way, surface the drift to the team.

## MCP + CLI together — the typical engagement workflow

```
Discovery / diagnosis (MCP, scoped read-only key)
  │
  ▼
Propose change (LLM-drafted in conversation)
  │
  ▼
Test in non-prod app (MCP write or CLI to staging)
  │
  ▼
Validate (MCP runs queries, reads analytics)
  │
  ▼
Commit to repo (settings.json, rules.json, synonyms.json updated)
  │
  ▼
PR review (humans review the JSON diff)
  │
  ▼
Merge → CI deploys to prod (CLI in GitHub Actions)
```

This split is intentional. MCP gets you to the answer fast; CI ensures prod stays reproducible.

## Slalom defaults

- **One scoped key per engagement** — keys live in 1Password's engagement vault.
- **MCP keys are read-only** by default. Promote to write only for a specific session, ephemeral.
- **Settings, rules, synonyms in `algolia/` at repo root.** Same convention across all engagements.
- **CI deploys to staging on merge to main; prod deploys are manual triggers** (sometimes scheduled, but always approved).
- **Run log per session** — when the MCP makes changes, the agent records the diff in `60_Digital_Manufacturing/{Factory}/WIP/{run}/run_log.md` or the engagement's relevance log. This lets future-Bermon (or future-other-agent) reconstruct what happened.

## Output formats

### MCP session run log entry

```
[2026-05-09T15:30:00Z] Algolia MCP session — articles relevance audit
  app: slalom-acme-staging
  key: scoped (search, browse, listIndexes, analytics, editSettings)

Findings:
  - 12 no-result searches with content gaps (list attached)
  - 3 no-result searches fixable via synonyms (proposed)
  - searchableAttributes order looks right
  - customRanking missing popularity — recommend add

Changes applied (staging only):
  - Added synonyms: [composable, headless, decoupled], [risk-gate, rg, gate-review]
  - customRanking: ['desc(publishedAt_unix)'] → ['desc(popularity)', 'desc(publishedAt_unix)']

Validation:
  - Query "composable cms" — top hit unchanged, position 4 hit moved to 2
  - Query "rg2" now returns Risk Gate 2 article (previously 0 results)

Next steps:
  - Commit settings + synonyms to algolia/ in repo
  - PR for review
  - Promote to prod via CI
```

### CI deploy plan entry

```
# Algolia CLI deploy plan: [Engagement]

## Repo layout
algolia/
├── articles.settings.json
├── articles.rules.json
├── articles.synonyms.json
└── ...

## Profiles
- staging: app slalom-{client}-staging, key ALGOLIA_SETTINGS_DEPLOY_STAGING
- prod: app slalom-{client}-prod, key ALGOLIA_SETTINGS_DEPLOY_PROD

## Triggers
- Staging: on push to main, paths algolia/**
- Prod: workflow_dispatch (manual)

## Approvals
- Prod requires PR + review (CODEOWNERS on algolia/)
- Prod requires environment protection rule in GitHub Actions

## Rollback
- Re-export prior settings before deploy (`algolia settings export`)
- Re-import to revert
- Document procedure in Runbooks/algolia-deploy-rollback.md
```

## Common anti-patterns

- **MCP with the admin key.** Wide blast radius. Use scoped keys.
- **MCP making changes to prod directly.** Bypasses review.
- **Settings drift** — Dashboard edits not mirrored in repo. Detect with daily diffs.
- **CLI deploys without dry-run.** `--dry-run` exists; use it on PRs.
- **No run log when the MCP changes things.** Forget what was done; can't roll back confidently.
- **Hand-rolled CI scripts that call the SDK.** The CLI is purpose-built; don't reinvent.
- **Sharing MCP keys across engagements.** A leak in one compromises all.
- **Long-lived write keys for the MCP.** Use time-bound (`validUntil`) where possible.

## Constraints

- Don't put admin keys in MCP configs.
- Don't run the MCP against prod for write ops without explicit human approval.
- Don't commit the Algolia API key, ever. Use env vars.
- Don't deploy settings without exporting current state first (rollback insurance).
- Don't skip the run log on MCP sessions that mutate state.

## How this skill relates to others

- The changes these tools apply → `algolia-relevance-tuning`, `algolia-indexing-pipeline`.
- The keys these tools need → `algolia-api-keys-security`.
- Command-level reference → `references/cli-cheat-sheet.md`, `references/mcp-cheat-sheet.md`.
- The Contentful-side equivalent (Contentful CLI + MCP) → `contentful-mcp-cli` in the contentful plugin.

## Source material

- Algolia CLI: https://github.com/algolia/cli
- Algolia CLI docs: https://www.algolia.com/doc/tools/cli/
- Algolia MCP server: https://github.com/algolia/mcp-node
- Algolia MCP docs: https://www.algolia.com/doc/guides/algolia-ai/algolia-mcp/
- Plugin reference: `../../references/cli-cheat-sheet.md`
- Plugin reference: `../../references/mcp-cheat-sheet.md`
