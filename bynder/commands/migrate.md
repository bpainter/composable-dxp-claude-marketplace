---
description: Plan and execute a migration into Bynder from another DAM (Widen, Brandfolder, Cloudinary, AEM Assets, MediaValet, Frontify) or from shared-drive chaos — extraction, mapping, throttled bulk import, downstream reference rewriting, cutover

# Project context
type: command
project: skills-library
plugin: bynder
tags: [type/command, plugin/bynder]
status: active
---

Use the `bynder-migration` skill to plan, execute, or recover a migration into Bynder.

Detect migration shape first:

- **Cross-DAM migration** — vendor API extraction is the best path; Widen / Brandfolder / Cloudinary / AEM Assets / MediaValet / Frontify all have API surfaces.
- **Shared-drive migration** — Box / SharePoint / Google Drive / Dropbox / network share. File system + sidecar metadata (EXIF / IPTC / XMP / CSV).
- **Bulk metaproperty change on a live deployment** — same migration discipline applies (rename a metaproperty, split parent options, backfill required field).
- **Portal restructure** — consolidating sub-portals or splitting a single portal. Treat as a migration from current state to target state.

The flow runs 8 phases:

1. **Discovery (1–2 weeks)** — inventory source, source metadata, downstream reference inventory, target schema design (`bynder-asset-model`), metadata mapping, excludes, partial-migrate set, sequencing.
2. **Extraction** — vendor API or filesystem crawl; produces a staging directory of asset binary + sidecar metadata JSON keyed by stable source ID.
3. **Mapping** — run the metadata mapper. Source field → Bynder metaproperty (or tag, or drop). This is where most engagement effort lives.
4. **Dry-run on sandbox** — `bynder-portal-architect` for sandbox plan; validate metadata maps to existing options, uploads succeed for representative samples, derivative re-render completes, permission profiles apply correctly.
5. **Production import** — throttled bulk via `bynder-js-sdk` (5–10 concurrency max) with idempotent ledger tracking `(sourceId, bynderId, status, attemptCount, lastError)`. Resumable on failure.
6. **Derivative re-render** — Bynder's render pipeline catches up over minutes-to-hours; schedule a derivative-readiness check before flipping downstream traffic.
7. **Downstream reference rewriting** — Contentful entries (route to `bynder-contentful-pairing`), marketing emails, social schedules, partner portals, sales decks. Each system has its own rewriting story.
8. **Cutover, verification, decommission** — soft freeze on source DAM, final delta extraction, downstream flip, soft freeze on Bynder for verification, source DAM read-only for 30–90 days before decommission.

End with `/audit` to confirm the new state passes the rubric.

Don't run production migration without a sandbox dry-run. Don't run without an idempotent ledger. Don't decommission source DAM in less than 30 days. Don't migrate without an editor-freeze window.
