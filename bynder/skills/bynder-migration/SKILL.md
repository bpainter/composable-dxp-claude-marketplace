---
name: bynder-migration
description: Migrating into Bynder from another DAM (Acquia DAM/Widen, Brandfolder, Cloudinary, Adobe AEM Assets, MediaValet, Frontify) or from shared-drive chaos. Covers extraction strategies (vendor APIs, S3 / network-share crawls, manual exports), metadata mapping (source taxonomy → Bynder metaproperty schema), bulk import via bynder-js-sdk with throttled concurrency, derivative re-render, downstream reference rewriting (Contentful entries that point at old DAM URLs), rollback strategy, and editor coordination. Use this skill when planning, executing, or recovering a migration into Bynder — including bulk metaproperty changes that look like migrations even on an existing portal.

# Project context
type: skill
project: skills-library
plugin: bynder
aliases: [bynder-migration]
tags: [type/skill, plugin/bynder, topic/bynder, topic/migration, topic/import]
status: active
---

# Migration (into Bynder, or major restructuring within)

This skill puts you in the role of the engineer who's run a six-figure-asset migration and lived to talk about it. Default posture: migrations are scripts, not click-paths. Reproducible, dry-run-able, idempotent, with rollback. The discipline scales from "migrate 10K assets from Brandfolder" to "restructure the metaproperty schema on a live deployment with 200K assets."

The single most common mistake on Bynder migrations: under-scoping metadata mapping. Source taxonomies never line up cleanly with target metaproperty schemas. Plan the mapping before you write the import script.

Pair with `bynder-asset-model` (the target metaproperty schema), `bynder-portal-architect` (where the migration lands), `bynder-js-sdk` (the substrate for bulk import), `bynder-derivatives` (re-render after import), `bynder-permissions-workflow` (permission profiles applied to migrated assets), `bynder-contentful-pairing` (rewriting downstream Contentful references), and `bynder-optimization-audit` (the audit comes both before and after).

## When to use this skill

- Migrating assets into Bynder from another DAM (Widen / Acquia, Brandfolder, Cloudinary, AEM Assets, MediaValet, Frontify, Salsify-as-DAM, etc.).
- Migrating from "shared drive chaos" (Box, SharePoint, Google Drive, Dropbox, network-share folder trees) into Bynder.
- Bulk metaproperty changes on a live deployment that need migration discipline (rename a metaproperty, split one option into two, restructure a parent/child tree).
- Re-importing an existing portal into a new portal architecture (consolidating sub-portals, splitting a single portal into sub-portals).
- Recovery from a botched migration.

## Core posture

- **Migrations are scripts, not click-paths.** Reproducible, dry-run, idempotent, with rollback.
- **Map metadata before writing the import script.** A 200-line import script with a 20-line metadata mapper is upside down — the mapper is the script.
- **Throttle aggressively.** Bynder's CMA-equivalent rate limit is ~5–10 req/sec; your migration runs at 5 req/sec, not 50.
- **Idempotent re-runs.** A migration that breaks halfway through must be safely resumable. Use a "source ID → target asset ID" ledger.
- **Derivatives re-render after upload.** Plan for the lag — derivatives aren't instantly available.
- **Downstream references must rewrite atomically.** Contentful entries that reference old-DAM URLs need to update to Bynder URLs in coordinated cuts.
- **Editor coordination is part of the plan.** Editors who are uploading new assets during a migration window create conflict — either freeze, or partition.
- **Always run a dry-run on a sandbox / non-prod portal first.** Always.

## Migration phases

### Phase 0: Discovery (1–2 weeks)

Before any extraction:

1. **Inventory the source.** Total assets, asset types, total size, growth rate, last-modified distribution.
2. **Inventory source metadata.** What fields exist; cardinality per field; rights / approval state; obvious gaps.
3. **Inventory downstream references.** What systems point at source-DAM URLs (CMS entries, marketing emails, social-media accounts, partner portals, sales decks). Ranked by criticality.
4. **Identify the target schema.** Run `bynder-asset-model` to design the metaproperty schema in target Bynder.
5. **Build the metadata mapping.** Source field → Bynder metaproperty (or tag, or drop). Document every drop and every transform.
6. **Identify the Excludes.** Assets that won't migrate (corrupted files, expired-rights, duplicates, internal-test content). Document why and where they go (archive, delete).
7. **Identify the partial-migrate set.** Assets that migrate but with reduced metadata fidelity (because source metadata was missing / inconsistent).
8. **Sequence the rollout.** Big-bang vs. phased; phased usually wins for >50K asset migrations.

### Phase 1: Extraction

Extract from source. Strategy depends on source:

- **Vendor API (Widen, Brandfolder, Cloudinary, AEM Assets)** — best path. Asset binary + metadata in one shot.
- **S3 / network share crawl** — file system + sidecar metadata files (or extracted from EXIF / IPTC / XMP).
- **CSV export from a vendor's admin UI + manual binary download** — ugly but sometimes the only path. Fragile.
- **Migration-vendor service** — pays a vendor like AssetSync to handle extraction. Useful at scale; introduces a third party.

Output of extraction: a staging directory tree where each asset has a binary file and a sidecar `metadata.json` keyed to a stable source ID.

```
staging/
  source-id-001.jpg
  source-id-001.metadata.json
  source-id-002.mp4
  source-id-002.metadata.json
  ...
```

### Phase 2: Mapping

Run the metadata mapper on the staging directory:

```ts
// scripts/map-metadata.ts
for (const dir of stagingDirs) {
  const source = JSON.parse(readFile(dir + '/metadata.json'));
  const target: BynderUploadMetadata = {
    name: source.title,
    description: source.caption,
    tags: source.keywords?.slice(0, 20), // truncate; tags should be light
    metaproperty_AssetCategory: [optionId('AssetCategory', mapCategory(source.category))],
    metaproperty_Channel: source.channels?.map(c => optionId('Channel', mapChannel(c))) ?? [],
    metaproperty_Brand: [optionId('Brand', mapBrand(source.brand))],
    metaproperty_RightsStatus: [optionId('RightsStatus', mapRights(source.rights))],
    metaproperty_LegacyAssetID: [source.id], // scaffold; deprecate after verification
    // ... etc
  };
  writeFile(dir + '/bynder-target.json', JSON.stringify(target));
}
```

The mapper is where most engagement effort goes. Source taxonomies will not be clean. Build mapping tables, document fallbacks for "no value" cases, and surface unmapped values for manual review.

### Phase 3: Dry-run on sandbox

Run the import script against a sandbox or non-prod portal:

1. Validate metadata maps to existing options (no `unknown option` errors).
2. Validate uploads succeed for representative samples (large files, edge formats, special-character filenames).
3. Validate derivative re-render completes.
4. Validate permission profile applies correctly.
5. Validate spot-check assets render as expected in any UCV embed.

### Phase 4: Production import

```ts
// scripts/import.ts
import { bynder } from './client';
import { ledger } from './ledger';

const concurrency = 5;
const limit = pLimit(concurrency);

await Promise.allSettled(
  assets.map(asset =>
    limit(async () => {
      if (await ledger.alreadyMigrated(asset.sourceId)) return; // idempotent

      try {
        const result = await bynder.uploadAsset({
          filename: asset.filename,
          body: createReadStream(asset.path),
          contentType: asset.mime,
          data: asset.bynderTargetMetadata,
        });
        await ledger.recordSuccess(asset.sourceId, result.mediaid);
      } catch (err) {
        await ledger.recordFailure(asset.sourceId, err);
      }
    })
  )
);
```

The ledger (a SQLite file, a small Postgres table, a JSON file — whatever) tracks `(sourceId, bynderId, status, attemptCount, lastError)` so reruns skip what's done and retry what failed.

Concurrency: 5–10 typically. Watch for 429s; back off if they appear.

### Phase 5: Derivative re-render

After upload, derivatives don't instantly exist. Bynder's render pipeline catches up over minutes-to-hours depending on volume. For large migrations:

- Don't trust the upload response's derivative URLs immediately for production traffic.
- Schedule a derivative-readiness check (poll asset records, confirm thumbnail URLs return 200) before flipping downstream traffic.

### Phase 6: Downstream reference rewriting

The hardest phase. Whatever pointed at old-DAM URLs now needs to point at Bynder.

```
Contentful entries with embedded old-URLs in JSON fields
  → script that reads each entry's bynderAsset / json fields
  → looks up the source-id-or-old-url → bynderId mapping in the ledger
  → updates the entry's reference to the Bynder asset envelope
  → saves draft (don't publish — let editors review)
```

Variations for marketing emails (templates), social schedules (Hootsuite / Sprinklr), partner portals, sales decks — each has its own reference-rewriting story. Plan per system.

### Phase 7: Cutover and freeze

The migration window:

- **Soft freeze** on source DAM (no new uploads) for 24–72 hours before cutover. Editors notified.
- Final delta extraction + import to catch anything uploaded during the freeze.
- Downstream references flipped.
- Soft freeze on Bynder for 12–24 hours of editor verification.
- Source DAM read-only for 30–90 days as fallback before decommission.

### Phase 8: Verification and cleanup

- Spot-check assets across the catalog.
- Run `bynder-optimization-audit` on the result — does the new portal pass the rubric?
- Editor walkthrough sessions — confirm flow.
- Once confidence is high (typically 30 days), retire the `LegacyAssetID` metaproperty.
- Decommission source DAM (or move to read-only archive).

## Bulk metaproperty changes (mid-life migrations)

A "migration" doesn't only mean cross-DAM. A bulk metaproperty change on a live deployment is the same discipline.

Examples:

- Rename a metaproperty (`Channel` → `DistributionChannel`).
- Split a parent option into two children (`EMEA` → `EMEA-North` + `EMEA-South`).
- Add a required metaproperty to existing assets (backfill).
- Retire a metaproperty (with downstream consumers).

Pattern is identical:

1. Plan the mapping.
2. Build a script that walks affected assets and applies the change.
3. Dry-run on sandbox.
4. Throttle aggressively (writes are rate-limited).
5. Idempotent ledger.
6. Verify and audit.

## Output formats

### Migration plan

```
# Bynder Migration: [Source → Target]

## Source profile
- Source DAM: [name + plan tier]
- Total assets: [N]
- Total size: [TB]
- Asset-type distribution: [...]
- Last-modified distribution: [last 30/90/365 days, older]
- Source metadata fidelity: [high/medium/low]
- Known data-quality issues: [...]

## Downstream consumer inventory
| System | Critical? | Reference style | Rewrite strategy |
| ... | ... | ... | ... |

## Target profile
- Target portal: [URL]
- Metaproperty schema: [link to bynder-asset-model output]
- Derivative library: [link to bynder-derivatives output]
- Permission profiles applied: [list]

## Metadata mapping
[Source field → Bynder metaproperty (or tag, or drop). Including transforms and fallbacks.]

## Excludes
- [Set] — [count] — [why excluded] — [destination]

## Phasing
[Big-bang | phased; cutover sequence; freeze windows; rollback windows]

## Tooling
- Extraction: [script(s)]
- Mapping: [script(s)]
- Import: [script(s)]
- Reference rewriting: [script(s) per downstream system]
- Ledger: [where stored]

## Test plan
- Sandbox dry-run scope: [...]
- Spot-check criteria post-import: [...]
- Editor walkthrough script: [...]

## Rollback strategy
- If cutover fails: [restore steps]
- If post-cutover issues found: [decision tree — fix forward vs. roll back]

## Editor coordination
- Communications plan
- Freeze window
- Training sessions

## Decommission plan
- Source DAM read-only window: [duration]
- LegacyAssetID metaproperty retirement: [date]
```

## Common anti-patterns to call out

- **No metadata mapping pass.** Import script ships, source taxonomies don't fit Bynder schema, half the metadata lands as tags, schema decays in week 2.
- **No dry-run on sandbox.** First production discovery is at scale; scale incidents are 10× harder to debug.
- **No idempotent ledger.** Migration breaks halfway, restart re-imports duplicates.
- **Concurrency too high.** 50 concurrent uploads → 429 cascade → confusing partial state.
- **Derivative re-render not planned for.** Cutover happens, derivatives still rendering, downstream references serve broken URLs.
- **Downstream rewriting deferred.** "We'll update Contentful later" — editors see Bynder as the source of truth but Contentful still references old DAM URLs for months.
- **No editor freeze.** New assets uploaded during migration window land in source DAM, never make it to target.
- **Source DAM decommissioned too fast.** No fallback when post-migration issues found.
- **`LegacyAssetID` metaproperty kept forever.** Was a migration scaffold; should have a retirement date.
- **Migration treated as one-shot, not phased.** Big-bang on >50K assets risks production-grade incidents; phase by brand, region, or time-window.

## Constraints

- Do not skip the metadata mapping pass.
- Do not run production migrations without a sandbox dry-run.
- Do not run migrations without an idempotent ledger.
- Do not exceed ~10 concurrent writes against the API.
- Do not flip downstream references before derivative re-render confirms.
- Do not decommission source DAM in less than 30 days post-cutover.
- Do not migrate without an editor-freeze window.
- Do not assume metadata fidelity — confirm by sampling.

## How this skill relates to others

- The target metaproperty schema → `bynder-asset-model`.
- The portal architecture the migration lands in → `bynder-portal-architect`.
- The SDK substrate for the import → `bynder-js-sdk`.
- The derivative library to re-render against → `bynder-derivatives`.
- The permission profiles applied to migrated assets → `bynder-permissions-workflow`.
- The downstream Contentful references that need rewriting → `bynder-contentful-pairing`.
- The pre-migration audit and post-migration verification → `bynder-optimization-audit`.

## Source material

- Bynder API for bulk operations: https://api.bynder.com/docs/
- bynder-js-sdk: https://github.com/Bynder/bynder-js-sdk
- Plugin reference: `../../references/sdk-cheat-sheet.md`
- Plugin reference: `../../references/bynder-foundations.md`
- Plugin reference: `../../references/api-surface.md`
