---
type: reference
project: skills-library
scope: plugin
plugin: algolia
tags: [type/reference, plugin/algolia, scope/foundational, topic/algolia, topic/mcp]
status: active
---
# Algolia MCP cheat sheet

Algolia ships an official MCP server that exposes the search and indexing surface to LLMs as tools. Use it for agent-led ops (modeling, indexing, settings, rules, analytics queries) when the alternative is a CLI script the agent can't safely write.

Repo: https://github.com/algolia/mcp-node
Docs: https://www.algolia.com/doc/guides/algolia-ai/algolia-mcp/

## Install / run

```bash
# stdio mode for Claude Desktop / Claude Code
npx -y @algolia/mcp-node@latest

# Or run directly from the repo
git clone https://github.com/algolia/mcp-node && cd mcp-node && npm i && npm run start
```

Configure your client (`claude_desktop_config.json`, Cowork settings, etc.) with the app ID and an admin API key. Use a **scoped key** — the MCP gets exactly the ACLs the key allows.

## Recommended ACLs by use case

| Use case | ACLs |
|---|---|
| Read-only inspection (debugging) | `search`, `browse`, `listIndexes`, `analytics` |
| Settings + rules + synonyms management | `editSettings`, `settings`, `listIndexes` |
| Full content management | `addObject`, `deleteObject`, `editSettings`, `settings`, `listIndexes` |
| Production agent | Don't. Use a per-job scoped key generated for the run. |

## Tool surface (high level)

The MCP wraps the search-API client, so the tool surface mirrors the SDK:

- **Index** — `listIndices`, `getIndex`, `clearIndex`, `deleteIndex`
- **Records** — `searchObjects`, `browseObjects`, `getObject`, `saveObjects`, `partialUpdateObjects`, `deleteObjects`
- **Settings** — `getSettings`, `setSettings`
- **Rules** — `getRule`, `saveRule`, `searchRules`, `clearRules`
- **Synonyms** — `getSynonym`, `saveSynonym`, `searchSynonyms`, `clearSynonyms`
- **Replicas** — handled via `setSettings` (`replicas` array)
- **Analytics** — `getTopSearches`, `getNoResultsSearches`, `getTopHits` (read-only, useful for the agent to observe)
- **Recommend** — `getRecommendations` (read-only)

The exact tool names depend on the MCP version; run the client and inspect the tool list. Default to read-only ACLs while the agent gets oriented.

## Agent prompts that map well

These are the kinds of asks the MCP makes effortless. Each maps to a chain of two or three tool calls.

### Inspection
- "Explain the current relevance settings on the `articles` index in plain language."
- "What are the top 20 no-result searches in the last 30 days?"
- "Show me the records that match `query=composable cms` and explain why the top hit ranks where it does."

### Modeling
- "Compare the searchable attributes between `articles` and `articles_v2`."
- "Suggest a faceting strategy based on the attribute distribution in this index."
- "Find records over 10KB and recommend a flattening strategy."

### Settings + rules
- "Add a synonym group: `composable, headless, decoupled`."
- "Create a query rule that pins record `whitepaper-2026-composable-dxp` to position 1 for queries matching `composable dxp` or `headless cms`."
- "Add `popularity` to `customRanking` after the existing entries."

### Indexing (use carefully)
- "Reindex `articles` from this NDJSON file using `replaceAllObjects`."
- "Apply this partial-update batch to update `price` on these objectIDs."

### Diagnostics
- "Why does query `summit risk gates` return 0 results? Walk me through it: typo, synonym, missing record, or filter issue."
- "Run an A/B comparison between current settings and the proposed settings on a sample of recent queries."

## When NOT to use the MCP

- **Production runtime traffic.** The MCP is for ops, not for serving searches. Use a real client (`algoliasearch`, `instantsearch.js`, the Search API directly).
- **Bulk imports of >100k records.** Use the SDK / CLI / Ingestion API.
- **Anything you can't roll back.** The MCP can `clearIndex` and `deleteIndex` if the key allows it. Use a key that doesn't, unless the run truly needs it.
- **Live merchandising changes by non-engineers.** The Dashboard's rules editor has guardrails the MCP doesn't.

## Pairing with the CLI

The MCP and CLI overlap. Rough split:

- **MCP** for conversational, exploratory work where the LLM does the synthesis.
- **CLI** for scripted, reproducible work that lives in a repo and runs in CI.

Don't pick one religiously. Most teams end up using the MCP for diagnosis and the CLI for deployment.

## Slalom defaults

- **Per-engagement MCP keys** — scoped to the engagement's index prefix (e.g., `acmecorp_*`).
- **Read-only by default**; promote to write only for a specific session, then rotate.
- **Run log discipline** — every MCP-driven change goes into the engagement's `Decisions/` or `Run_Log/` notes. The MCP makes destructive changes easy; the run log makes them recoverable.
