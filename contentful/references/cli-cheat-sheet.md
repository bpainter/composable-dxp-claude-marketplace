---
type: reference
project: skills-library
scope: plugin
plugin: contentful
tags: [type/reference, plugin/contentful, scope/foundational, topic/cli]
status: active
---
# Contentful CLI cheat sheet

Quick reference for the [`contentful-cli`](https://github.com/contentful/contentful-cli) commands you'll reach for most. Skills cite this file to avoid restating commands.

Install: `npm install -g contentful-cli` (or use `pnpm dlx`/`npx` for one-offs).

## Auth

```bash
# Authenticate. Drops a token into ~/.contentfulrc.json.
contentful login

# Confirm who you are.
contentful whoami

# Use a Personal Access Token instead of OAuth (CI / scripts).
export CONTENTFUL_MANAGEMENT_TOKEN=cfpat-...
contentful --management-token "$CONTENTFUL_MANAGEMENT_TOKEN" whoami
```

PATs are listed under https://app.contentful.com/account/profile/cma_tokens. Treat them like passwords — never commit them.

## Spaces and environments

```bash
# List spaces in your org.
contentful space list

# Set the active space for subsequent commands.
contentful space use --space-id <SPACE_ID>

# List environments in the active space.
contentful space environment list

# Create a new environment by branching from another.
contentful space environment create \
  --environment-id release-2026-04-22 \
  --name "release-2026-04-22" \
  --source master

# Delete an environment.
contentful space environment delete --environment-id old-release

# List environment aliases.
contentful space environment-alias list

# Update an alias to point at a new environment (the deploy step).
contentful space environment-alias update \
  --alias-id master \
  --target-environment release-2026-04-22
```

## Content types and migrations

The CLI does not generate migration scripts for you. Use `contentful-migration` (the library) for the actual schema changes; the CLI runs them.

```bash
# Run a migration script against an environment.
contentful space migration \
  --space-id <SPACE_ID> \
  --environment-id release-2026-04-22 \
  ./migrations/2026-04-22_add_seo_fields.js

# Dry-run? Set --yes to skip the prompt; for true dry-run, the contentful-migration
# library exposes a programmatic `dryRun` mode — see contentful-migrations skill.
```

For ad-hoc inspection:

```bash
# Export an environment to a JSON file (content types + entries + assets).
contentful space export \
  --space-id <SPACE_ID> \
  --environment-id master \
  --content-file ./exports/master.json

# Import an export into another env (e.g., seeding a feature env).
contentful space import \
  --space-id <SPACE_ID> \
  --environment-id feature-x \
  --content-file ./exports/master.json
```

`export` / `import` are blunt instruments — full content moves, no diff. For schema-only or surgical changes, use migrations.

## Generating content type types

If you're using `contentful-typescript-codegen` rather than `graphql-codegen`:

```bash
pnpm dlx contentful-typescript-codegen \
  --output src/lib/contentful/__generated__/contentful.d.ts
```

Reads from your active space/environment. Pair with a `prebuild` script.

## App framework

```bash
# Bootstrap a new app.
contentful app create-app-definition

# List apps in the org.
contentful app list

# Install an app definition into a space.
contentful app install --app-definition <APP_DEFINITION_ID>
```

See `contentful-app-framework` for the full lifecycle.

## Webhooks

The CLI does not have first-class webhook commands historically — most teams manage these in the UI or via the CMA + a small Node script. If you need it scripted, use the `contentful-management` SDK directly. See `contentful-webhooks`.

## Bulk content operations

For bulk edits on entries, prefer one of:

1. A migration script (schema + data together, version-controlled).
2. A small Node script using `contentful-management` SDK with backoff.
3. The `contentful-cli` `space export` → edit JSON → `space import` pattern (only when the change is a full replacement).

Don't loop curl-against-the-CMA in a shell script. You will hit the 7 req/sec limit and corrupt state.

## CI usage patterns

```yaml
# .github/workflows/contentful-migration.yml
- name: Apply migration
  env:
    CONTENTFUL_MANAGEMENT_TOKEN: ${{ secrets.CONTENTFUL_MANAGEMENT_TOKEN }}
  run: |
    pnpm dlx contentful-cli@^3 \
      space migration \
      --management-token "$CONTENTFUL_MANAGEMENT_TOKEN" \
      --space-id ${{ vars.CONTENTFUL_SPACE_ID }} \
      --environment-id ${{ inputs.environment }} \
      --yes \
      ./migrations/${{ inputs.migration_file }}
```

## Common pitfalls

- **No `--environment-id` flag** → defaults to `master`. Always be explicit.
- **Token confusion**: `--management-token` for CMA operations (most CLI commands), `--access-token` for Delivery/Preview reads. The CLI errors clearly but the messages aren't always obvious on first read.
- **`contentful login` writes plaintext to `~/.contentfulrc.json`.** On shared dev machines, prefer env-var PATs.
- **`space export` is heavy.** It pulls every entry. Use `--query-entries '{"content_type": "..."}'` to scope.
- **Locale-specific imports** require care: an export carries all locales, and importing into a space without those locales fails. Add the locales first.
