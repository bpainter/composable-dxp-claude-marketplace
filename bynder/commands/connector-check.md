---
description: Audit Bynder Marketplace connector coverage — what's installed, what's actively used, what's stale, and which Tier 1 adoption-driver connectors are missing

# Project context
type: command
project: skills-library
plugin: bynder
tags: [type/command, plugin/bynder]
status: active
---

Use the `bynder-marketplace-connectors` skill to audit OOTB connector coverage on an existing Bynder deployment and recommend installs.

Faster, narrower than `/audit` — run this when the user's question is specifically "are we using the right connectors?" or "what would drive adoption faster?" rather than a full implementation audit.

The flow:

1. **Inventory installed connectors** — pull from Bynder portal admin (Settings → Integrations → Apps / Connectors).
2. **Pull connector usage from Analytics** — `bynder-foundations.md` references the Analytics API integration usage endpoint. Connectors with zero quarterly traffic are usually misconfigured or forgotten.
3. **Verify connector health** for each installed:
   - Latest release within last 12 months?
   - Verified-by-Bynder badge present?
   - Service-account auth still valid?
   - Bynder-side configuration (return-derivative, default filters) still aligned with current editorial intent?
   - Host-system field assignments still valid (content types haven't been retired)?
4. **Identify Tier 1 adoption-driver gaps** — every deployment should have:
   - Microsoft 365 (Word, PowerPoint, Outlook, Teams)
   - Slack
   - Adobe Creative Cloud (Photoshop, Illustrator, InDesign)
   - Figma
   These cost almost nothing to install and drive measurable adoption.
5. **Identify Tier 2 system gaps** — confirm CMS / commerce / marketing-automation / PIM systems in use have OOTB connectors installed where supported.
6. **For systems without an OOTB connector**, evaluate whether a custom UCV embed (`bynder-compact-view`) is justified by editorial flow.
7. **For systems with an OOTB connector that doesn't quite fit**, evaluate the layered pattern — install OOTB for baseline + custom UCV for gap-specific flows.

Produce a recommendation list with effort estimate (S/M/L) per item and expected adoption / integration impact.

Don't propose custom UCV before checking the Marketplace listing. Connectors evolve; what wasn't supported a year ago may be now.

Don't recommend installing every available connector — Tier 1 reflexively, Tier 2 when scope justifies, Tier 3 only on explicit fit.
