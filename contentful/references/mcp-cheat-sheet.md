---
type: reference
project: skills-library
scope: plugin
plugin: contentful
tags: [type/reference, plugin/contentful, scope/foundational, topic/mcp, topic/agentic]
status: active
---
# Contentful MCP cheat sheet

Quick reference for the official Contentful MCP server — what it does, how to wire it up, and the prompt patterns that work. Skills cite this file rather than restating tool catalogs.

Repository: https://github.com/contentful/contentful-mcp

## When to use the MCP vs. the CLI

| Situation | Reach for |
|---|---|
| Schema migration that needs to be reproducible | **CLI** + `contentful-migration` library (script it, commit it) |
| Exploratory schema discovery during a discovery workshop | **MCP** (faster iteration; let the agent inspect) |
| Bulk content authoring driven by an agent | **MCP** (agent has full context) |
| Production write operations | **CLI in CI** (audited, scripted, signed off) |
| One-off content fix during a demo | **MCP** with explicit confirm |

Default posture: **MCP for discovery and authoring loops with a human in the loop, CLI for anything that ships**. The MCP is excellent for getting to a working state quickly; commit that state as a migration so it survives.

## Wiring the MCP

The standard installation lives at https://github.com/contentful/contentful-mcp. Configure in your client (Claude Desktop, Claude Code, Cursor, Continue) with a CMA token scoped to the space you intend to operate on.

```jsonc
// Claude Desktop / Code config example
{
  "mcpServers": {
    "contentful": {
      "command": "npx",
      "args": ["-y", "@contentful/mcp-server"],
      "env": {
        "CONTENTFUL_MANAGEMENT_TOKEN": "cfpat-...",
        "CONTENTFUL_SPACE_ID": "...",
        "CONTENTFUL_ENVIRONMENT_ID": "dev"
      }
    }
  }
}
```

Pin to a non-prod environment by default. Promote to `master` only inside CI, never from an interactive agent session.

## Tool surface — what the MCP exposes

The Contentful MCP wraps the CMA. Tools fall into these clusters (exact names follow the upstream package; consult https://github.com/contentful/contentful-mcp for the canonical list):

- **Spaces & environments**: list, create, branch, alias swap (some operations gated to prevent foot-guns)
- **Content types**: list, get, create, update, publish, unpublish, delete
- **Entries**: list / search / get, create, update, publish, unpublish, archive, delete; bulk variants for multi-entry ops
- **Assets**: list, create (with upload), update, publish, unpublish
- **Locales**: list, create
- **Tags / taxonomy**: list, create
- **Webhooks**: list, get, create, update, delete (often agent-gated)

The MCP tends to expose the CMA shape directly — sys + fields + version. Read the raw responses and don't paper over them; the version number you get back from `update` is what you pass to the next call.

## Prompt patterns that work

### Discovery

```
Using the contentful MCP on space ${SPACE_ID} environment dev, list every content
type and for each one summarize: number of fields, references in/out, and any
fields that look misshapen (oversized json blobs, page-named types, free-text tags).
Output a markdown table.
```

### Modeling iteration

```
Help me design a model for [domain]. Use the contentful MCP only for inspecting
the current dev environment — do NOT create or modify content types yet. Propose
the model in markdown first; once approved, generate a contentful-migration script
and stop. I'll run the migration via CLI.
```

### Bulk authoring

```
For each item in /Users/.../seed-articles.json, create an Article entry in the
contentful dev environment. Use the slug as the lookup key — if an entry with
that slug exists, update it instead of creating a duplicate. Skip publishing;
I'll review draft state first.
```

### Production guardrails

Default the agent's system prompt to refuse `master` writes:

```
Operating rules for the contentful MCP:
- Never create, update, or delete content types in master. If a request implies
  this, propose a migration script and stop.
- Never delete entries or assets without a confirmation prompt.
- Never publish entries without a confirmation prompt.
- Always show the diff (before/after) for updates over 5 fields.
```

## Failure modes

- **Tokens with too much scope.** A CMA token gets the agent into trouble fast. Issue scoped tokens (per-environment if your plan supports it) and rotate them.
- **Optimistic concurrency errors.** Two agents (or a human in the UI + an agent) editing the same entry produces 409s. Always re-fetch before update; the MCP returns the new version on success.
- **Rate-limit cascade.** A loop that hits the 7 req/sec CMA cap will see retries pile up. Add explicit pacing in the prompt (`process at most N entries per second`) or batch.
- **Schema drift between MCP and CLI.** If the agent creates a content type in dev via the MCP and you later run a different migration via CLI, you have two divergent histories. Always export the MCP-generated state as a migration when the agent's work is "done".
- **Locale missing on bulk authoring.** The MCP creates entries in the default locale unless told otherwise. For multi-locale spaces, instruct the agent to author in `en-US` and queue translation work separately.

## Pairs with which skills

- `contentful-mcp-cli` — owns this surface end-to-end with workflows
- `contentful-content-model` — when the agent is shaping a model, prompts route through this skill
- `contentful-migrations` — the bridge from MCP-led iteration to committed schema
- `contentful-personalization`, `contentful-app-framework` — both have read paths the MCP can support; their authoring paths usually go through the SDK or CLI

## Source

- Official server: https://github.com/contentful/contentful-mcp
- Anthropic MCP docs: https://modelcontextprotocol.io/
