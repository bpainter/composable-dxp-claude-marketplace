---
name: contentful-migrations
description: Reproducible Contentful schema migrations using the Contentful CLI and the contentful-migration library. Authoring scripts, dry-running them on a non-prod environment, promoting through aliases, and recovering when something goes sideways. Use this skill any time a schema change needs to ship — adding a content type, renaming a field, changing a validation, deleting a field, splitting one type into two, backfilling data — or any time someone is about to "edit the content type in the UI." Trigger on phrases like "migrate the model," "add a field," "rename slug to handle," "we changed the model in dev and need to apply to prod," "we need a rollback plan," or whenever the operational arm of a model change is the question.

# Project context
type: skill
project: skills-library
plugin: contentful
aliases: [contentful-migrations]
tags: [type/skill, plugin/contentful, topic/contentful, topic/migrations, topic/cli]
status: active
---

# Contentful Migrations

Schema changes get scripted. Always. The Contentful UI is fine for exploration; it is not fine as the source of truth for what your environments look like. Every change that ships goes through a migration script that lives in the repo, gets reviewed, runs in CI, and can be replayed.

This skill owns that discipline. Pair with `contentful-space-architect` (the environment topology you're migrating across), `contentful-content-model` (the schema being changed), `contentful-mcp-cli` (when an agent is the one writing the script), and `contentful-graphql` + `contentful-react-wrapper` for the front-end side of the change.

## When to use this skill

- A model change has been agreed and now needs to ship.
- Someone is about to edit a content type in the UI and call it done.
- Promoting a feature env's schema to staging or master.
- Rolling back a bad change.
- Adding required validations to existing content (a backfill).
- Splitting a content type, renaming, or deleting fields.
- Adding a locale to a space.
- Changing roles or webhook configuration as part of a release (those live alongside schema scripts).

## Core posture

- **Scripted, version-controlled, reviewable.** UI edits are exploratory; scripts are the deliverable.
- **Forward + back.** Every migration has a reverse. Some reverses are destructive; document them.
- **Dry-run first.** The contentful-migration library has dry-run; use it. Then run on a non-prod env. Then promote.
- **Schema and data move together.** A required-field add isn't done until existing entries have a value.
- **Author migrations small.** One concern per script. Sequencing many small ones is easier than untangling a monolithic one.
- **Production migrations run in CI, not from a developer's laptop.** Audit trail, repeatability, no "it worked on my machine."

## The toolchain

Two libraries cooperate:

- **`contentful-migration`** — the imperative migration DSL. You write `migration.createContentType(...)`, `migration.editContentType(...)`, `migration.transformEntries(...)`. Programmatic, idempotent-friendly, includes a dry-run mode.
- **`contentful-cli`** — the runner. `contentful space migration ./script.js` applies a script to an environment.

For agent-led work, the **Contentful MCP server** (see `contentful-mcp-cli`) can drive CMA operations directly. Treat MCP-driven changes as exploratory; commit them as a migration script when the work is "done."

Setup:

```bash
pnpm add -D contentful-migration contentful-cli
```

Project layout:

```
migrations/
  2026-04-22_001_add_seo_fields.cjs
  2026-04-22_002_split_hero_into_topic_and_assembly.cjs
  2026-04-29_001_add_audience_field.cjs
  README.md       # numbering and conventions
```

Filenames are date-sorted. Numbering inside a date breaks ties. The `_001` is conventional, not enforced.

## Anatomy of a migration script

```js
// migrations/2026-04-22_001_add_seo_fields.cjs
module.exports = function (migration, context) {
  const article = migration.editContentType("article");

  article
    .createField("seoTitle")
    .name("SEO Title")
    .type("Symbol")
    .required(false)
    .validations([{ size: { max: 70 } }]);

  article
    .createField("seoDescription")
    .name("SEO Description")
    .type("Text")
    .required(false)
    .validations([{ size: { max: 160 } }]);

  article.changeFieldControl("seoTitle", "builtin", "singleLine", {
    helpText: "Title tag and OpenGraph title. Aim for under 60 characters.",
  });
  article.changeFieldControl("seoDescription", "builtin", "markdown", {
    helpText: "Meta description and OpenGraph description. Plain text, ~155 chars.",
  });
};
```

A few rules baked in:

- **`module.exports`** — the script is a function the runner invokes with a `migration` builder and a `context` (space, env, fetch helpers).
- **Idempotent thinking, not idempotent in fact.** The library doesn't no-op on re-run; it errors if a field already exists. We get reproducibility by tracking which scripts have run (CI does this; see "Tracking applied migrations" below).
- **Help text is part of the migration.** Editor experience is a deliverable.

### Editing a field

```js
module.exports = function (migration) {
  const product = migration.editContentType("product");

  // Add a new validation to an existing field.
  product.editField("slug").validations([
    { regexp: { pattern: "^[a-z0-9][a-z0-9-]*$", flags: "" } },
    { unique: true },
  ]);
};
```

### Renaming a field (the safe pattern)

Renaming is two migrations and a code release in between, because the field name is a part of the API surface that the front end queries against:

```js
// migrations/2026-04-29_001_add_handle_field.cjs
module.exports = function (migration) {
  const product = migration.editContentType("product");
  product
    .createField("handle")
    .name("Handle")
    .type("Symbol")
    .validations([{ regexp: { pattern: "^[a-z0-9][a-z0-9-]*$" } }, { unique: true }]);
};
```

Then a backfill:

```js
// migrations/2026-04-29_002_backfill_handle_from_slug.cjs
module.exports = function (migration) {
  migration.transformEntries({
    contentType: "product",
    from: ["slug"],
    to: ["handle"],
    transformEntryForLocale: (fromFields, locale) => {
      if (locale !== "en-US") return; // backfill once on the default locale
      return { handle: fromFields.slug?.["en-US"] };
    },
  });
};
```

Then the front end ships a release that reads `handle` (with a fallback to `slug` if needed). Then:

```js
// migrations/2026-05-06_001_drop_slug_from_product.cjs
module.exports = function (migration) {
  const product = migration.editContentType("product");
  product.deleteField("slug");
};
```

Three scripts, each small and reversible enough.

### Deleting a field

```js
module.exports = function (migration) {
  const product = migration.editContentType("product");
  product.deleteField("legacyTaxonomy");
};
```

A delete is destructive. Ensure:

1. No queries reference the field (`grep` the repo).
2. No webhooks filter on the field.
3. No dashboards or downstream consumers (Algolia indexer, analytics enrichment) read it.

### Adding a required field with a default

A required field on existing entries breaks publishing until backfilled.

```js
module.exports = async function (migration) {
  const article = migration.editContentType("article");

  // Step 1: add as optional with a default-value behavior
  article
    .createField("audience")
    .name("Audience")
    .type("Symbol")
    .required(false)
    .validations([{ in: ["founder", "investor", "general-counsel"] }]);
  article.changeFieldControl("audience", "builtin", "dropdown", {
    helpText: "Primary audience for this article.",
  });
};
```

Then a backfill migration to set values, then a follow-up that flips it to required.

### Adding a locale

```js
module.exports = function (migration) {
  migration.createLocale("fr-FR")
    .name("French (France)")
    .fallbackCode("en-US")
    .optional(true);
};
```

Locales are space-level. After the add, see `contentful-localization` for the field-flag and translation-workflow steps.

## Dry-running

The library exposes `--dry-run` on the CLI runner:

```bash
contentful space migration \
  --space-id $CONTENTFUL_SPACE_ID \
  --environment-id dev \
  --yes \
  --dry-run \
  ./migrations/2026-04-22_001_add_seo_fields.cjs
```

Dry run prints what *would* happen — content type creates, field changes — without writing. It does not simulate `transformEntries` against real entries; for that, run on a feature env first.

## Tracking applied migrations

The CLI itself does not maintain a "migrations applied" table. You have two reasonable options:

1. **Filename-prefixed numeric ordering + CI tracking.** CI maintains a JSON or git-tag record of "migrations applied to {env}." A new script is appended; CI runs only the new ones.
2. **Self-marking migrations.** Each script reads/writes a `_migrations` content type that records its own application. Heavier, but environment-portable.

For most engagements, option 1 is enough. A small CI helper:

```yaml
# .github/workflows/contentful-migrate.yml
name: Apply Contentful migrations
on:
  workflow_dispatch:
    inputs:
      environment:
        type: choice
        options: [dev, staging, "release-{date}"]
        required: true
jobs:
  migrate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: pnpm/action-setup@v3
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: pnpm
      - run: pnpm install --frozen-lockfile
      - name: Apply pending migrations
        env:
          CONTENTFUL_MANAGEMENT_TOKEN: ${{ secrets.CONTENTFUL_MANAGEMENT_TOKEN }}
          CONTENTFUL_SPACE_ID: ${{ vars.CONTENTFUL_SPACE_ID }}
          ENV: ${{ inputs.environment }}
        run: pnpm tsx scripts/apply-pending-migrations.ts
```

The `apply-pending-migrations.ts` reads `migrations/_applied/{env}.json`, diffs against the `migrations/` directory, and runs the new ones in order via `contentful-cli` programmatically (or shells out). On success, append to the manifest and commit.

## Promotion across environments

Standard pipeline:

```
1. Author migration script in a feature branch.
2. PR review.
3. CI applies to a feature env (named after the branch / PR).
4. QA against feature env.
5. Merge PR.
6. CI applies to staging-aliased release env.
7. Final QA.
8. Alias swap (master → new release env). See contentful-space-architect.
9. CI marks the migration as applied to master.
```

Two things that go wrong here:

- **Out-of-order migrations across envs.** Two PRs that both add a field to `article`, merged in one order but applied to envs in another order. Solution: serialize migrations on master with strict numbering; rebase feature branches before merge.
- **A migration that depends on entry data created in another migration.** Resist. If you must, run them as a single transactional script (the library wraps many operations; on error it doesn't auto-rollback the partial state, so prefer two clean scripts with manual ordering in the manifest).

## Rolling back

Three patterns, in order of preference:

1. **Forward fix.** Author a new migration that undoes the bad change. Cheaper to reason about than a rollback.
2. **Alias swap.** If the bad migration is on a release env that hasn't been swapped to master, point the alias at the previous release env. The bad env stays in place; it just isn't reading.
3. **Restore from backup.** Contentful enterprise plans support backup/restore. Out-of-band; coordinate with Contentful support.

Whatever you do, never edit content types in the UI to fix a bad migration. You're guaranteed to lose track.

## Migrating data, not just schema

`transformEntries` is the entry-data API. It runs server-side on Contentful, page-by-page. Useful patterns:

- **Backfill a new field from an existing one.** (See rename example above.)
- **Split one field into two.** Split a `fullName` Symbol into `firstName` / `lastName`.
- **Combine fields into a JSON object.** Wrap a set of related fields into a single nested object before deleting them.
- **Locale-aware transforms.** `transformEntryForLocale` runs once per locale; check the locale code to do the right thing.

For data changes that don't fit `transformEntries` (e.g., joining data across content types), write a small Node script using `contentful-management` SDK with backoff:

```ts
import { createClient } from "contentful-management";

const client = createClient({ accessToken: process.env.CONTENTFUL_MANAGEMENT_TOKEN! });
const env = await client
  .getSpace(process.env.CONTENTFUL_SPACE_ID!)
  .then((s) => s.getEnvironment(process.env.CONTENTFUL_ENVIRONMENT!));

let skip = 0;
const limit = 100;
while (true) {
  const page = await env.getEntries({ content_type: "article", skip, limit });
  for (const entry of page.items) {
    // ...mutate fields...
    await entry.update();
    await entry.publish();
    await sleep(150); // stay under 7 req/sec
  }
  if (page.items.length < limit) break;
  skip += limit;
}
```

The `await sleep(150)` is the rate-limit honoree. Pull it into a helper; never write the loop without it.

## Output formats

### Migration plan

```
# Migration: [Name]

## What changes
- [Schema] {content type / field changes}
- [Data] {backfill / transform changes}
- [Locales] {if applicable}

## Steps
1. Migration script: {filename} — schema change
2. Migration script: {filename} — data backfill (if needed)
3. Front-end PR: {what reads change}
4. Migration script: {filename} — final cleanup (drop deprecated field, flip required)

## Sequencing
- Apply 1 to feature env → verify
- Front-end change deployed reading new field with fallback
- Apply 2 + 3 to feature env → verify
- Promote to staging → QA
- Promote to master → drop deprecated, run cleanup

## Reverse plan
- For each forward step, the inverse: ...

## Risks
- {what could go wrong; how it's caught}
```

## Common pitfalls

- **Editing the model in two environments at once.** Two devs both add a field to `article` in their own dev envs, both write a migration. Merge order matters.
- **Required fields without backfill.** Editors hit "publish" and get blocked across the entire content type.
- **Transforms without dry-run.** Always run on a feature env with realistic data before staging.
- **Migrations that publish entries.** Sometimes correct, often surprising. If a transform should publish, do it explicitly and document.
- **No CI guardrails.** A laptop running `contentful space migration ... --environment master` is a foot-gun. Block it via roles or by not handing out CMA tokens with master access.
- **Forgetting webhooks on new envs.** Cloning an env doesn't always carry webhook config the way you'd expect. Verify per release env.
- **Large transforms on master without coordination.** Editors get optimistic-concurrency errors mid-publish. Run during low-traffic windows.

## How this skill relates to others

- The schema being migrated → `contentful-content-model`.
- The environment topology you're moving through → `contentful-space-architect`.
- Webhook config that often changes alongside schema → `contentful-webhooks`.
- Front-end changes that ship in lockstep → `contentful-react-wrapper`, `contentful-graphql`.
- Locale changes → `contentful-localization`.
- Agent-driven schema iteration that gets committed as a script → `contentful-mcp-cli`.

## Source material

- contentful-migration library: https://github.com/contentful/contentful-migration
- Contentful CLI: https://github.com/contentful/contentful-cli
- CMA reference: https://www.contentful.com/developers/docs/references/content-management-api/
- Plugin reference: `../../references/cli-cheat-sheet.md`
