---
type: reference
project: skills-library
scope: plugin
plugin: algolia
tags: [type/reference, plugin/algolia, scope/foundational, topic/algolia, topic/cli]
status: active
---
# Algolia CLI cheat sheet

The Algolia CLI is the fastest path for one-off ops, scripted CI tasks, and any time the Dashboard would be too click-heavy. Pairs with the official Node SDK for anything more complex than a one-liner.

Install: `brew install algolia/algolia-cli/algolia` (or download from https://github.com/algolia/cli/releases). Auth via env vars (`ALGOLIA_APP_ID`, `ALGOLIA_API_KEY`) or `algolia auth login`.

Docs: https://www.algolia.com/doc/tools/cli/

## Auth and profiles

```bash
algolia auth login                      # interactive, stores in ~/.config/algolia/config.json
algolia profile list                    # show saved profiles
algolia profile add prod                # add a named profile
algolia --profile prod indices list     # use a specific profile
```

## Index ops

```bash
# Inspect
algolia indices list
algolia indices describe articles
algolia objects browse articles --output ndjson > articles.ndjson

# Create / clear / delete
algolia indices clear articles
algolia indices delete articles --confirm

# Atomic reindex from a file
algolia objects import articles --file ./articles.ndjson --replace

# Partial updates
algolia objects import articles --file ./price-changes.ndjson --partial

# Single record
algolia objects get articles SOME_OBJECT_ID
algolia objects delete articles SOME_OBJECT_ID --confirm
```

`--replace` uses `replaceAllObjects` under the hood — atomic, no empty-index window. Default to it for full reindexes.

## Settings

```bash
algolia settings export articles > articles.settings.json
algolia settings import articles --file articles.settings.json
algolia settings get articles | jq '.searchableAttributes'
```

Treat settings as code. Export → commit → review-as-PR → import. Don't let people edit production settings in the Dashboard without committing the export.

## Rules

```bash
algolia rules export articles > articles.rules.json
algolia rules import articles --file articles.rules.json --replace
algolia rules list articles
algolia rules get articles {ruleObjectID}
algolia rules search articles --query "free shipping"
```

## Synonyms

```bash
algolia synonyms export articles > articles.synonyms.json
algolia synonyms import articles --file articles.synonyms.json --replace
algolia synonyms list articles
```

## Search and debug

```bash
algolia search articles --query "composable cms"
algolia search articles --query "composable cms" --filters "type:article" --hitsPerPage 5

# Explain why a record ranks where it does
algolia search articles --query "composable cms" --getRankingInfo
```

## Replicas

```bash
algolia indices describe articles | jq '.replicas'

# Add a virtual replica via settings update
echo '{ "replicas": ["virtual(articles_price_asc)"] }' | algolia settings import articles --file -
```

## API keys

```bash
algolia keys list
algolia keys describe {keyID}
algolia keys add --acl search browse listIndexes
algolia keys delete {keyID} --confirm
```

For **secured API keys**, the CLI helps less than the SDK — secured keys are HMAC-derived per request from a parent search key, so generate them in your server runtime, not in the CLI.

## Operations and tasks

```bash
algolia operations wait articles {taskID}     # wait for an indexing task
algolia indices list-pending-tasks articles
```

## Common one-liners

```bash
# How many records in each index?
algolia indices list --output json | jq -r '.[] | "\(.entries)\t\(.name)"' | sort -nr

# Top 10 searches in the last 7 days (Analytics API via CLI)
algolia analytics top-searches articles --start-date $(date -v-7d +%Y-%m-%d)

# Top 10 no-result searches
algolia analytics top-searches articles --no-results

# Diff settings between staging and prod
diff <(algolia --profile staging settings export articles) \
     <(algolia --profile prod settings export articles)
```

## CI integration

Typical CI pattern:

```yaml
# .github/workflows/algolia-deploy-settings.yml (example)
- name: Apply settings
  run: algolia settings import articles --file ./algolia/articles.settings.json
  env:
    ALGOLIA_APP_ID: ${{ secrets.ALGOLIA_APP_ID }}
    ALGOLIA_API_KEY: ${{ secrets.ALGOLIA_ADMIN_KEY }}
```

Settings, synonyms, and rules each live in their own file under `./algolia/`. The same workflow runs against staging on PR, prod on merge.

## What the CLI is not great at

- Bulk record import for *very* large sources — the CLI imports a file, but the import is single-threaded and not pipelinable. For >1M records, write a Node script using the SDK or use the **Ingestion API**.
- Anything that requires custom transformation logic during import — preprocess to the file shape Algolia expects, then import.
- Live observability — use the Dashboard or the Monitoring API.
