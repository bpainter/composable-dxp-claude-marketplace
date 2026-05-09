---
name: bynder-optimization-audit
description: The audit-first remediation playbook for existing Bynder implementations that aren't delivering value. Covers an eight-metric health rubric (taxonomy, derivatives, permissions, integrations, adoption, analytics, brand-guidelines activation, governance), the diagnostic prompts that point to specific findings, the prioritized remediation plan that routes to the right specialty skill, and the re-audit discipline that records the delta. Use this skill at the start of any Bynder optimization engagement, when the client says any version of "we have Bynder but it isn't working," before recommending major changes to a running deployment, or as a quarterly health check on a Slalom-built deployment.

# Project context
type: skill
project: skills-library
plugin: bynder
aliases: [bynder-optimization-audit]
tags: [type/skill, plugin/bynder, topic/bynder, topic/optimization, topic/audit]
status: active
---

# Optimization Audit (existing Bynder implementations)

This skill puts you in the role of the senior architect dropped into a Bynder deployment that isn't working. The client doesn't want a re-platform; they want what they have to deliver value. The audit is the first move; the remediation plan that comes out of it is the engagement scope.

The optimization-engagement shape is real and underserved. Many Bynder customers stood up v1 of a deployment 2–4 years ago, the team that ran the launch has rotated out, the platform has drifted, and now the conversation is "we have Bynder but no one uses it / there are 10K duplicates / the integrations are flaky." Slalom is well-positioned for this work; this skill is the way in.

Pair with every other skill in this plugin — the audit routes findings to the relevant remediation skill. The agent (`bynder-agent`) treats optimization as a first-class engagement shape and orchestrates accordingly.

## When to use this skill

- Start of any Bynder optimization engagement.
- Any time the user says "we have Bynder but..."
- Before recommending major changes to a running deployment (audit first; change second).
- Quarterly health check on a Slalom-built deployment (catch drift before it compounds).
- After a major Bynder migration (verify the result against the rubric).
- After a leadership change in the brand-ops team (new owner wants to know what they inherited).

## Core posture

- **Audit before change.** Symptom-driven changes break things you didn't know mattered. The rubric is what makes scope.
- **Eight metrics, one rubric.** Don't skip categories. Adoption is as load-bearing as taxonomy.
- **Quantify what you can.** Analytics API gives you real numbers; opinions are weaker findings than counts.
- **Route findings to remediation skills.** The audit is diagnostic; the fixing happens in the specialty skills.
- **Sequence by leverage.** Fix the highest-impact, lowest-effort items first. A taxonomy redesign on day 1 of an engagement is overreach; a connector reset on day 1 demonstrates value.
- **Re-audit at the end.** Recorded delta is the engagement's headline.

## The eight-metric rubric

### 1. Taxonomy and metaproperty health

**What to check:**
- Number of metaproperties (target 6–15; > 25 is sprawl).
- Tag count (high tag count + low metaproperty diversity = tags-as-taxonomy anti-pattern).
- Metaproperty option cardinality (an option list of 200 values is unfilterable).
- Metaproperties marked filterable (target 6–10; > 12 is filter-sidebar overwhelm).
- Required-field hygiene (every required metaproperty filled? if not, schema-vs-data mismatch).
- Parent/child depth (3 levels max).
- Free-text metaproperties on dimensions that should be controlled.

**Diagnostic prompts:**
- "How do you find an asset?" If editors describe scrolling rather than filtering, taxonomy is failing.
- "What does this tag mean?" If editors disagree on the same tag, it's not governed.

**Routes to:** `bynder-asset-model`.

### 2. Derivative health

**What to check:**
- Number of derivative templates (target 10–20; > 30 is sprawl).
- Used vs. unused derivatives (Analytics API shows view/download counts per derivative).
- Format coverage (webp / avif present? or only JPEG?).
- Focal points set on photographic assets.
- Email derivatives in webp (anti-pattern; email clients).
- Hand-rolled URLs in front-end code (centralization gap).

**Diagnostic prompts:**
- "Which derivative does the website use?" If nobody knows, central reference is missing.
- "How many social-card sizes do you have?" Many → likely sprawl.

**Routes to:** `bynder-derivatives`.

### 3. Permission and access health

**What to check:**
- Profile count (6–12 ideal; > 20 = sprawl).
- Per-user permissions (any → fix).
- Last-login distribution (users 6+ months idle → disable).
- Service accounts tied to ex-employees.
- Token rotation discipline (any token > 12 months old?).
- IdP / SSO group sync working (compare IdP membership vs. Bynder group membership).
- Asset-level permission overrides (sign of governance failure — usually means profiles are wrong).
- "Public" assets — are they intentional?

**Diagnostic prompts:**
- "Who can edit this asset?" If editors don't know, governance has failed.
- "How does a new agency contractor get access?" Long ad-hoc process → no profile pattern.

**Routes to:** `bynder-permissions-workflow`.

### 4. Integration / connector health

**What to check:**
- Connectors installed (which Marketplace connectors).
- Connectors actually used (Analytics integration usage).
- Connectors on current versions.
- Connector auth still valid (service account healthy, tokens current).
- Webhook configurations active (vs. abandoned / disabled).
- Webhook delivery success rate (Bynder admin shows recent delivery history).
- Custom UCV embeds — still wired correctly, still themed correctly.

**Diagnostic prompts:**
- "When you update an asset in Bynder, how long until it shows on the website?" If "we have to clear cache manually," webhooks aren't working.
- "Does your design team check assets out into Photoshop?" If no, Adobe CC connector either isn't installed or isn't taught.

**Routes to:** `bynder-marketplace-connectors`, `bynder-webhooks-events`, `bynder-compact-view`, `bynder-contentful-pairing`.

### 5. Adoption

**What to check (Analytics API):**
- Daily / weekly active users by role.
- Asset views / downloads per user-segment.
- Search frequency.
- Most-searched terms (do they reflect what's there?).
- Most-downloaded assets.
- Zero-result-search rate (high → either content gap or taxonomy gap).
- Adoption-driver connector usage (Microsoft 365, Slack, Adobe CC).

**Diagnostic prompts:**
- "Where do you store assets you're working on?" If "my desktop" or "Slack messages," Bynder isn't the source of truth.
- "How does the social team get assets?" If a parallel pipeline, integration gap.

**Routes to:** `bynder-marketplace-connectors` (install adoption drivers), `bynder-asset-model` (if zero-result searches reveal taxonomy gaps), `bynder-brand-guidelines` (if Brand Guidelines is the under-utilized adoption channel).

### 6. Brand Guidelines activation

**What to check:**
- Brand Guidelines module enabled?
- Number of sections / pages.
- Last-updated dates per section (anything > 12 months → stale).
- Page view counts (Analytics).
- Embedded vs. screenshot images (screenshots → not staying in sync).
- Public vs. authenticated-only.
- Sync with design-system tokens (color, typography aligned with code?).

**Diagnostic prompts:**
- "Where do partners learn the brand?" If "our brand book PDF in SharePoint," Brand Guidelines isn't doing its job.
- "Where do designers find approved colors?" If Figma libraries only, Bynder Brand Guidelines is parallel/ignored.

**Routes to:** `bynder-brand-guidelines`.

### 7. Analytics and governance discipline

**What to check:**
- Analytics being pulled (any reporting cadence?).
- Quarterly review cadence existing (calendar invite, owner, output).
- Ownership of Bynder operations (who's the admin? bus factor?).
- Documentation in place (taxonomy doc, profile doc, integration runbook).
- Brand-team and editorial-team Bynder runbooks.

**Diagnostic prompts:**
- "Who owns Bynder?" If "I think marketing ops? or maybe IT?" → governance gap.
- "When did you last review the metaproperty schema?" If "we haven't" → drift.

**Routes to:** `bynder-permissions-workflow` (audit cadence), the practice's general governance frameworks.

### 8. Migration / data quality

**What to check:**
- Duplicate assets (Analytics + similarity heuristics).
- Orphan assets (no metaproperties, no tags, no permissions assigned).
- Expired-rights assets still discoverable (`property_RightsStatus=Expired` or `UsageEnd < today` still visible).
- Legacy migration scaffolding (`LegacyAssetID` metaproperty still in use long after migration).
- Asset count growth rate vs. quality control rate (uploads outpacing review).

**Diagnostic prompts:**
- "Have you done a duplicate check recently?" Almost always "no."
- "Do you remove old assets?" Most clients don't; bank grows monotonically.

**Routes to:** `bynder-migration` (cleanup), `bynder-asset-model` (governance).

## The audit run-book

### Inputs needed

- Account admin access (or a screenshare with someone who has it).
- API token with read scope on Asset Bank, Metaproperties, Analytics.
- 30 minutes with each of: brand-ops lead, primary editor, primary downstream-system owner (CMS / commerce / etc.), end-user representative.
- Read access to relevant downstream systems for integration verification.

### Run sequence (target ~3 working days for a single-portal Standard deployment)

```
Day 1 — quantitative
  Pull Analytics: asset views, downloads, searches, zero-result rate, integration usage
  Pull metaproperty schema and tag list
  Pull profile + group + user roster
  Pull derivative templates
  Pull webhook config
  Pull installed Marketplace connectors
  Pull last-login distribution

Day 2 — qualitative + diagnostic interviews
  Brand-ops lead: governance, schema-evolution, who-changes-what
  Primary editor: daily flow, search behavior, workarounds
  Downstream-system owner: integration health, recent issues
  End-user rep: how they discover and use assets

Day 3 — synthesis
  Score each of the 8 metrics: Healthy / Drifting / Critical
  Identify highest-leverage findings
  Sequence remediation waves
  Draft the report
```

### Scoring

For each metric:

- **Healthy** — meets the criteria; no remediation needed beyond ongoing hygiene.
- **Drifting** — partial degradation; fix in this engagement, but not crisis-level.
- **Critical** — actively harmful; fix in wave 1.

## Output formats

### Audit report

```
# Bynder Optimization Audit: [Client]

## Engagement context
- Deployment age: [years]
- Plan tier: [...]
- Asset count: [N]
- Brand count: [N]
- Downstream systems integrated: [list]
- Engagement signal that triggered this audit: [the user prompt]

## Scorecard
| Metric | Status | Headline finding |
| 1. Taxonomy | [Healthy / Drifting / Critical] | [...] |
| 2. Derivatives | ... | ... |
| 3. Permissions | ... | ... |
| 4. Integrations | ... | ... |
| 5. Adoption | ... | ... |
| 6. Brand Guidelines | ... | ... |
| 7. Governance | ... | ... |
| 8. Data quality | ... | ... |

## Top 5 findings (severity-weighted)
1. [Finding]
   - Evidence: [counts, screenshots, anecdotes]
   - Impact: [editor pain, downstream consequence, ROI]
   - Routes to: [skill]
   - Remediation effort: [S/M/L]
2. ...

## Remediation waves
**Wave 1 (Weeks 1–2): Quick wins**
- [Item with skill route + acceptance criteria]
- ...

**Wave 2 (Weeks 3–6): Structural fixes**
- ...

**Wave 3 (Weeks 7–12): Adoption + governance**
- ...

## Out of scope
[Findings noted but not addressed in this engagement; recommended as future work.]

## Re-audit schedule
[Date for the post-engagement re-audit and the metric deltas to record.]
```

### Quick-win catalog (the wave-1 menu)

- Install Microsoft 365 connector (effort: S; impact: High adoption lift)
- Install Slack connector (effort: S; impact: High adoption lift)
- Install Adobe CC connector if not present (effort: S; impact: Designer-team adoption)
- Add `RightsStatus=Cleared` default filter on UCV / Compact View embeds (effort: S; impact: Reduces editorial mistakes)
- Disable webhooks delivering to disabled receivers (effort: S; impact: Cleans up noise)
- Rotate any service-account token > 12 months old (effort: S; impact: Security hygiene)
- Disable users > 6 months idle (effort: S; impact: License cost + audit cleanliness)
- Set focal points on top 100 most-downloaded photography assets (effort: M; impact: Better dynamic crops)

## Common anti-patterns to call out (in the audit itself)

These are the audit-stage versions of skill-level anti-patterns. Highlight when found:

- Tags-as-taxonomy.
- Profile sprawl or per-user permissions.
- Stale brand book PDF instead of activated Brand Guidelines.
- Custom UCV builds where OOTB connector exists and would have worked.
- Webhooks subscribed but receiver returning 5xx silently.
- Service accounts tied to ex-employees.
- Hand-rolled image URLs across front-end code.
- "We don't pull Analytics" — therefore no quantitative governance.
- Asset bank growing monotonically with no retire-discipline.

## Re-audit discipline

End every optimization engagement with a re-audit. Record:

- Same scorecard, scored against same evidence (Analytics deltas, metaproperty count change, profile count change, etc.).
- Specific deltas — "Adoption: weekly active users 220 → 580; Tags: 1,840 → 320; Webhooks: 2 broken → 0; Connectors: 3 installed → 7 installed."
- What's left for next engagement.

The delta is the engagement's headline and the case study for the next pursuit.

## Constraints

- Do not skip metrics; the rubric is the rubric.
- Do not propose remediation before completing the audit — sequence matters.
- Do not start with the hardest fix; sequence by leverage, demonstrate value early.
- Do not work without analytics data — opinions are weaker than counts.
- Do not skip the re-audit at engagement close.
- Do not assume the user's stated problem is the actual problem — the rubric will surface what they didn't think to mention.

## How this skill relates to others

- Routes findings to every other skill in the plugin:
  - Taxonomy → `bynder-asset-model`
  - Derivatives → `bynder-derivatives`
  - Permissions → `bynder-permissions-workflow`
  - Integrations → `bynder-marketplace-connectors`, `bynder-contentful-pairing`, `bynder-compact-view`, `bynder-webhooks-events`
  - Brand Guidelines → `bynder-brand-guidelines`
  - Migration / data quality → `bynder-migration`
- Pairs with `bynder-portal-architect` if the audit reveals architecture-level issues (multi-portal split needed, sandbox missing).
- Inputs the agent's optimization-engagement workflow.

## Source material

- Bynder Analytics API: https://api.bynder.com/docs/ (Analytics section)
- Plugin reference: `../../references/bynder-foundations.md`
- Plugin reference: `../../references/api-surface.md`
- Plugin reference: `../../references/marketplace-connectors.md`
- Plugin reference: `../../references/slalom-context.md` (engagement-shape detection)
