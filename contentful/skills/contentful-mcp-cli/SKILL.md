---
name: contentful-mcp-cli
description: Use the Contentful MCP server and the Contentful CLI together as the agent + automation surface for delivery work — when to reach for which, the prompt patterns that get high-leverage results, the safety rails that keep MCP-driven exploration from corrupting production, and the bridge from MCP-led iteration to committed migration scripts. Use this skill any time agent-driven Contentful operations are in play, when the team is choosing between MCP, CLI, or SDK for an automation, when scripted environment setup is being planned, or when bulk content authoring is needed. Trigger on phrases like "use the Contentful MCP," "let the agent set up the model," "automate this with the CLI," "bulk import," or "scripted env config."

# Project context
type: skill
project: skills-library
plugin: contentful
aliases: [contentful-mcp-cli]
tags: [type/skill, plugin/contentful, topic/contentful, topic/mcp, topic/cli, topic/agentic]
status: active
---

# Contentful MCP & CLI Workflows

Three ways to drive Contentful from outside the UI: the **Contentful MCP server** (agent-friendly, conversational, exploratory), the **Contentful CLI** (deterministic, scriptable, CI-ready), and the **`contentful-management` SDK** (the underlying API surface; reach for when MCP/CLI don't fit).

This skill is about the agent + automation pattern that ties them together. Pair with `contentful-migrations` (the destination of any MCP-led iteration), `contentful-content-model` (what the agent shapes), `contentful-space-architect` (the topology agents operate on), `contentful-webhooks` (what the agent verifies fires), and `contentful-app-framework` (when the agentic UI lives in Contentful itself).

## When to use this skill

- Setting up the Contentful MCP server for a Slalom delivery team.
- Choosing between MCP, CLI, and SDK for an automation.
- Designing prompts for agent-led content modeling or authoring.
- Bulk content imports that don't fit the CLI's `import` shape.
- Building a CI-driven Contentful operations pipeline.
- Establishing safety rails so an agent doesn't blow up `master`.

## Core posture

- **MCP for human-in-the-loop iteration; CLI for ship.** Agent explores, human approves; CLI in CI applies the committed result.
- **Never operate on `master` from a chat window.** Default the agent to a non-prod environment. `master` writes only happen via CI on a reviewed PR.
- **Scope tokens by purpose.** A discovery token (CMA, scoped to dev environment if your plan supports it) is different from a CI write token (CMA, scoped to release env). Different keys; rotate independently.
- **Commit MCP outputs as migrations.** Whenever an agent produces a working schema state, capture it as a `contentful-migration` script. The MCP session is the pencil; the migration is the ink.
- **Prefer reads over writes for unsupervised agent work.** Agents triaging, summarizing, or auditing are great candidates. Authoring with no human review is not.

## Tool choice matrix

| Situation | Reach for |
|---|---|
| Discovery: "What does this content model look like?" | MCP |
| One-off schema iteration during a workshop | MCP |
| "Generate 30 seed articles from this CSV" with review | MCP |
| Schema migration that ships to production | CLI + `contentful-migration` library |
| Setting up environments / aliases | CLI |
| Bulk export / import | CLI |
| Webhook configuration | SDK script (committed) |
| Custom programmatic operation that doesn't fit migrations | SDK script |
| Auditing existing content for issues | MCP (read-only prompts) |

## MCP setup

The official Contentful MCP server: https://github.com/contentful/contentful-mcp.

Configuration in your client (Claude Desktop / Code, Cursor, Continue, or any MCP-compatible client):

```jsonc
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

Defaults to enforce:

- **Pin to a non-prod environment.** `CONTENTFUL_ENVIRONMENT_ID=dev` (or a feature env).
- **Use a scoped CMA token.** If your plan supports per-environment tokens, use them.
- **Document the agent's operating rules in the system prompt** (see "Safety rails" below).

The MCP exposes the CMA shape as tools — list/get/create/update/publish for entries, content types, assets, locales, tags, webhooks. See `references/mcp-cheat-sheet.md` for the per-tool surface and prompt patterns.

## Prompt patterns

### Discovery (read-only)

```
Connect to the contentful MCP. List every content type in the current
environment. For each one, summarize: machine ID, display name, number
of fields, fields that look misshapen (page-named types, oversized JSON
blobs, free-text tags). Output a markdown table. Don't write anything.
```

### Modeling iteration

```
We're designing a model for [domain]. Use the MCP only to inspect the
current state. Propose the model in markdown first. Once I approve,
generate a contentful-migration script and stop. I'll run the migration
via CLI in CI. Do not create or modify content types in this session.
```

### Auditing

```
Connect to the MCP. Walk every entry of type Article. Flag entries that:
1. Have no body content (empty Rich Text).
2. Reference an unpublished asset.
3. Have no value on the seoDescription field.
4. Were last updated more than 18 months ago.

Output a table grouped by issue. Don't fix anything; just report.
```

### Bulk authoring (with human review)

```
For each item in /Users/.../seed-articles.json, create a draft Article
entry. Use the slug as the lookup key — if an entry with that slug
exists, update it; do not duplicate. Skip publishing. After each
batch of 10, summarize what changed and pause for confirmation.
```

### Bridging to migrations

```
We've iterated on the model in dev. Generate a contentful-migration
script that captures the current state of:
- ContentType: glossaryTerm
- ContentType: page
- ContentType: callToAction

Output it as a single .cjs file ready to commit. Do not write anything
to Contentful in this session.
```

The "do not write" guard is critical. The agent reads the live state from dev and emits the migration script.

## CLI in CI

The CLI is the production-side runner. The pattern from `contentful-migrations` and `contentful-space-architect` summarized:

```yaml
# .github/workflows/contentful.yml
name: Contentful operations
on:
  workflow_dispatch:
    inputs:
      operation:
        type: choice
        options: [migrate, env-create, env-promote]
      environment:
        type: string
        required: true

jobs:
  run:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: pnpm/action-setup@v3
      - uses: actions/setup-node@v4
        with: { node-version: 20, cache: pnpm }
      - run: pnpm install --frozen-lockfile

      - name: Apply pending migrations
        if: ${{ inputs.operation == 'migrate' }}
        env:
          CONTENTFUL_MANAGEMENT_TOKEN: ${{ secrets.CONTENTFUL_CMA_CI }}
        run: pnpm tsx scripts/apply-pending-migrations.ts --env ${{ inputs.environment }}

      - name: Create release environment
        if: ${{ inputs.operation == 'env-create' }}
        run: |
          pnpm dlx contentful-cli@^3 \
            --management-token "$CONTENTFUL_MANAGEMENT_TOKEN" \
            space environment create \
            --space-id ${{ vars.CONTENTFUL_SPACE_ID }} \
            --environment-id ${{ inputs.environment }} \
            --source master

      - name: Promote alias
        if: ${{ inputs.operation == 'env-promote' }}
        run: |
          pnpm dlx contentful-cli@^3 \
            --management-token "$CONTENTFUL_MANAGEMENT_TOKEN" \
            space environment-alias update \
            --alias-id master \
            --target-environment ${{ inputs.environment }}
```

The CI token (`CONTENTFUL_CMA_CI`) is **separate** from the MCP token. Different keys; different rotation; different audit trail.

## SDK scripts (the third option)

For operations that don't fit the CLI's built-in commands (webhook setup, complex bulk operations, cross-content-type joins), write a small Node script using `contentful-management` SDK:

```ts
// scripts/setup-webhooks.ts
import { createClient } from "contentful-management";

const args = process.argv.slice(2);
const env = args[0] ?? "dev";

const client = createClient({ accessToken: process.env.CONTENTFUL_MANAGEMENT_TOKEN! });
const space = await client.getSpace(process.env.CONTENTFUL_SPACE_ID!);

// Idempotent: get-or-create
const id = `revalidate-${env}`;
let webhook;
try {
  webhook = await space.getWebhook(id);
} catch {
  webhook = await space.createWebhookWithId(id, {
    /* config */
  });
}

console.log(`Webhook ${id} ready: ${webhook.url}`);
```

Run via the CLI's same auth pattern (env vars, CI). Commit alongside migrations.

## Safety rails

Default rules to ship in every agent system prompt that has CMA write access:

1. **No write operations against `master` or any production-aliased environment.** If a request implies one, propose a migration script and stop.
2. **No deletes without explicit confirmation.** Even on dev — agents misinterpret. Always confirm before delete.
3. **No publishes without explicit confirmation.** Drafts are reversible; publishes propagate.
4. **Show diffs before updates over 5 fields.** Easy way to catch a misread instruction.
5. **No webhook deletion without naming it explicitly.** Rebuilding a webhook config is annoying.
6. **No locale add/remove from MCP.** That's a migration.
7. **Token scope honored.** If the token is dev-scoped, the agent works only in dev.

Embed these in the agent system prompt, and verify by occasional adversarial testing.

## Bridging the gap: from MCP iteration to committed delivery

The discipline:

```
1. Agent in MCP iterates schema in dev environment until "done".
2. Agent emits a contentful-migration script that produces the same state
   from the prior committed state. Reviewer reads the script.
3. Migration committed to repo, PR review.
4. CI runs migration on a fresh release env (cloned from current master).
5. QA against staging alias.
6. Alias swap. (See contentful-space-architect.)
7. The dev environment is reset (cloned fresh from new master) for the
   next iteration.
```

Without this discipline, the MCP iteration is invisible — schema lives in dev with no record of how it got there. Production never inherits cleanly.

## Audit and observability

Every CMA token write produces an audit entry. Enterprise plans expose this; smaller plans require ad-hoc logging.

For Slalom delivery, instrument:

- **Per-token activity dashboards.** A spike in dev-token writes during a workshop is fine; a spike in CI-token writes outside a deploy window is not.
- **Webhook delivery logs.** A migration that breaks webhook filters shows up here.
- **MCP session transcripts.** Save the prompt/response history for any session that wrote to Contentful, for postmortem and future reference.

## Common pitfalls

- **Single CMA token reused everywhere.** Compromise one path, compromise all paths.
- **MCP token with `master` access.** The agent will eventually be told to "fix this real quick" against prod. Don't.
- **Agent-driven schema changes that never become migrations.** Dev drifts; the next person who clones it inherits an opaque state.
- **CLI scripts in personal `~/.contentfulrc.json`-authenticated state.** Works on your laptop, not in CI; not auditable. Always env-var-driven.
- **Bulk loops without rate-limit awareness.** 7 req/sec CMA cap; loops without `await sleep(150)` get throttled and retry-loop.
- **MCP prompts that assume too much agency.** "Improve the model" is too vague; agents make non-compositional changes. Be specific.
- **No teardown of feature MCP environments.** Devs clone an env, iterate, walk away. Plans cap environment count; quarterly cleanup.

## Output formats

### Operation plan

```
# Contentful Operation: [Name]

## Goal
{what changes}

## Tool
- MCP / CLI / SDK script — and why

## Environment
- {dev | staging | release-{date}}
- Token: {scope}

## Steps
1. ...

## Safety rails
- {what the agent / script must NOT do}

## Outputs to commit
- migrations/...
- scripts/...

## Verification
- Webhook fires on test publish
- Front-end revalidation propagates
- {other}
```

## How this skill relates to others

- The destination of any MCP-led schema iteration → `contentful-migrations`.
- What the agent is shaping → `contentful-content-model`.
- The topology the operation runs against → `contentful-space-architect`.
- Webhook config the agent might verify or build → `contentful-webhooks`.
- Bulk content authoring with personalization variant patterns → `contentful-personalization`.
- App Framework apps that wrap an agentic UI inside Contentful itself → `contentful-app-framework`.
- Marketplace apps that the agent might suggest installing instead of building → `references/marketplace-apps.md`.

## Source material

- Contentful MCP: https://github.com/contentful/contentful-mcp
- Contentful CLI: https://github.com/contentful/contentful-cli
- contentful-management SDK: https://github.com/contentful/contentful-management.js
- MCP protocol: https://modelcontextprotocol.io/
- Plugin references:
  - `../../references/cli-cheat-sheet.md`
  - `../../references/mcp-cheat-sheet.md`
