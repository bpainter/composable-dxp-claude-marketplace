---
name: bynder-permissions-workflow
description: Permission profiles, asset-level security, and the Bynder Workflow module — designing who-can-see-edit-approve-publish-what, scoping access to brand / region / metaproperty values, the Workflow module's campaign/job/task structure, review and approval cycles, and scheduled publishing. Covers the practical permission model (profiles, groups, asset-level overrides, sub-portal scoping), workflow stage design, the API surface for both, and the audit discipline that keeps permissions from drifting. Use this skill when designing permissions for a new deployment, fixing access drift on an existing deployment, rolling out the Workflow module, or integrating Bynder Workflow with an external project tool (Workfront, monday, Asana).

# Project context
type: skill
project: skills-library
plugin: bynder
aliases: [bynder-permissions-workflow]
tags: [type/skill, plugin/bynder, topic/bynder, topic/permissions, topic/workflow, topic/governance]
status: active
---

# Permissions and Workflow (Bynder governance)

Two related but distinct concerns: **permissions** (who can see, edit, download, approve, publish what) and **workflow** (the lifecycle of an asset from request through delivery). Both are governance layers. Both fail in similar ways — drift, exception-driven sprawl, decisions that nobody remembers making.

This skill puts you in the role of the senior architect who designs governance models that hold up over time. Default posture: profile-driven, group-mapped, audited quarterly, with workflow only where it actually adds value (creative-ops shops, regulated industries, agency/freelancer workflows).

Pair with `bynder-portal-architect` (permissions span portals or scope within them), `bynder-asset-model` (metaproperties drive permission filters), `bynder-marketplace-connectors` (Workflow integrates with external tools), and `bynder-optimization-audit` (permission drift is one of the audit's primary findings).

## When to use this skill

- Designing the permission model for a new Bynder deployment.
- Fixing permission drift on an existing deployment ("everyone has access to everything" or "no one can find anything they need").
- Rolling out the Bynder Workflow module.
- Integrating Bynder Workflow with Workfront / monday / Asana / Jira.
- Onboarding a new editorial team, agency, or freelancer cohort.
- Auditing access as part of a security review or quarterly governance.
- Designing scheduled-publishing flows.

## Core posture

- **Profile-driven, not user-driven.** Permissions belong to profiles (roles); users are assigned to groups; groups map to profiles. Don't grant individual user permissions.
- **Default-deny, not default-allow.** A new asset type, metaproperty, or brand should be invisible until a profile is granted access — not visible until somebody remembers to restrict it.
- **Metaproperty-scoped access is the leverage point.** "Editors can edit assets where `property_RightsStatus=Cleared`" gives you fine-grained control without proliferating profiles.
- **Workflow is optional.** Don't recommend it unless the client has a creative-ops shop, agency review cycles, or regulated content-approval requirements.
- **Audit quarterly.** Permission profiles drift; option memberships drift; group membership drifts. Quarterly review is non-negotiable.
- **Document the design.** Permissions designed in someone's head and clicked into the admin UI cannot be reproduced. Write it down.

## Permission model

### The hierarchy

```
User
  └─ assigned to → Group(s)
                    └─ assigned → Profile(s)
                                    ├─ Asset-bank rights (view/edit/delete/upload)
                                    ├─ Brand Guidelines rights
                                    ├─ Workflow rights
                                    ├─ Asset-type scoping
                                    ├─ Metaproperty-value scoping
                                    └─ Sub-portal scoping (if multi-portal)
```

Three rules:

1. **Don't grant rights directly to a user.** Always profile → group → user.
2. **Keep profile count manageable** — 6–12 for most deployments. More than that and you've fractured.
3. **Name profiles by capability, not by team.** `Brand Editor — Acme` is fine; `Acme Marketing Team` becomes meaningless when teams reorg.

### Standard profile spine

Most engagements end up with a recognizable set of profiles:

| Profile | Capability |
|---|---|
| `Viewer` | Browse, search, download approved derivatives. No edit. The widest profile; default for org-wide users. |
| `Editor — Brand` | Upload, edit metadata, manage tags, can edit assets where `property_Brand=X`. One per brand. |
| `Editor — Lead` | Editor + delete + bulk operations + manage collections. Senior editors. |
| `Brand Admin` | Editor — Lead + manage metaproperty options + manage Brand Guidelines. The brand-ops lead. |
| `Workflow Reviewer` | View jobs/tasks assigned for review; submit approval decisions. |
| `Workflow Owner` | Create campaigns/jobs/tasks; assign reviewers; close jobs. |
| `Agency / Freelancer` | Restricted Editor — limited to a specific campaign or collection; cannot delete. |
| `Account Admin` | Full account admin. 1–3 people, max. |
| `API Integration` | Service account for backend integrations; scoped to read or read+write specific asset types. |

Adapt to client governance, but resist the urge to ship 25 profiles.

### Scoping by metaproperty

The high-leverage move: profiles can scope by metaproperty value. Example:

```
Profile: Editor — North America
  - Edit rights: yes
  - Scope: property_Region IN [NA-US, NA-CA]
```

Now editors with this profile can only edit assets tagged for North America. The metaproperty schema (`bynder-asset-model`) is the substrate; the permission model is what makes it operational.

Common scoping patterns:

- **By Brand** (for multi-brand single-portal architectures).
- **By Region** (for regional editorial teams).
- **By RightsStatus** (`Cleared` only — agencies can edit cleared assets, can't touch restricted ones).
- **By Campaign** (agencies / freelancers scoped to a specific campaign).

### Asset-type scoping

A profile can be scoped to specific asset types:

```
Profile: Video Editor
  - Edit rights: yes
  - Asset types: video, audio
```

Useful when teams have specialty (video team vs. design team).

### Sub-portal scoping

In multi-sub-portal architectures (`bynder-portal-architect`), profiles can be scoped per sub-portal — an editor on the Acme Lite sub-portal doesn't appear in the Acme master portal admin.

### Group strategy

- **Map groups to org structure or function**, not to permission level. `Marketing — North America`, `Creative — EU`, `Agency — Wieden+Kennedy`. Group name should still make sense after a reorg.
- **One user can be in multiple groups.** Permissions union.
- **Group sync from IdP** — if the client has SAML/SSO with group claims, map IdP groups → Bynder groups for automatic membership management. Saves a class of "user left the company three months ago and still has access" incidents.

## Workflow module

The Bynder Workflow module is a separate licensed component, not enabled by default. It governs *creation* of assets — campaign requests, creative jobs, review/approval cycles, scheduled publishing.

### When to recommend Workflow

- Client has a dedicated creative-ops team managing intake from marketing → design.
- Asset creation involves multiple stakeholders (designer → creative director → brand approver → legal → publish).
- Regulated industry (pharma, financial services) where every asset needs documented approval.
- Agency-driven creative pipeline where the agency hands off to in-house for approval.
- Scheduled publishing matters (campaign launch dates, regional rollouts).

### When to skip Workflow

- Small marketing team where review happens in Slack / email.
- Client doesn't license the Workflow module (not all plans include it).
- Client already runs creative-ops in Workfront / monday / Asana and the integration cost > the value of native Workflow.

### Workflow concepts

```
Campaign — top-level container (e.g., "Q2 2026 Composable Launch")
  └── Job — unit of work (e.g., "Hero photography shoot")
        ├── Tasks — steps within a job (e.g., "Shoot scheduled", "Raw delivery", "Edit", "Brand review", "Legal review", "Final delivery to asset bank")
        └── Reviews — approval cycles attached to tasks
```

Each task has assignees, reviewers, deadlines, status. Reviews capture sign-off (approve/reject/request-changes).

### Workflow stage design

Standard stage pattern for a creative-asset job:

```
Stage 1: Intake — brief captured, scope confirmed, deadline set
Stage 2: Creation — designer / agency works
Stage 3: Internal review — creative director / brand
Stage 4: Approval — legal / compliance / brand
Stage 5: Delivery — published to asset bank with metadata applied
Stage 6: Post-launch — usage reporting, retire / archive
```

Tailor to industry. Pharma adds a medical-legal review stage. Regulated finance adds a disclosure-review stage.

### Scheduled publishing

Workflow tasks can include "publish on date X" — Bynder holds the asset in draft and surfaces it on the scheduled date. Useful for:

- Embargoed product launches.
- Regional rollout sequencing.
- Coordinated multi-channel launches.

### External integration (Workflow API + connectors)

If the client runs project management in Workfront / monday / Asana, integrate:

- Workfront → Bynder Workflow: bidirectional task sync. Marketplace connector exists; usually configure-rather-than-build.
- monday / Asana → Bynder: lighter integration via webhooks + Workflow API. Often a small custom integration.

See `bynder-marketplace-connectors`.

## Audit discipline

Quarterly:

1. **Pull the user list** — who has access, when did they last log in. Disable users with > 6 months idle.
2. **Pull the group memberships** — does each group still match a real org / function?
3. **Pull profile assignments per group** — has anyone been promoted / demoted out of pattern?
4. **Pull metaproperty-scope rules** — are scopes still aligned to current metaproperty values? (A profile scoped to `property_Region=EMEA-DE` breaks if `EMEA-DE` was renamed to `EU-DE`.)
5. **Pull the API integration tokens** — are there any tokens older than the rotation policy? Service accounts where the underlying user has left?
6. **Spot-check assets** — pull 10 random assets, verify the permission stack on each matches the design intent.

Document findings; remediate; close the loop. The Analytics API gives you most of the inputs.

## Output formats

### Permission design proposal

```
# Bynder Permission Model: [Client]

## Profile spine
| Profile | Rights | Asset-type scope | Metaproperty scope | Sub-portal scope |
| ... | ... | ... | ... | ... |

## Group structure
| Group | Function | Mapped profiles | IdP source (if SSO) |
| ... | ... | ... | ... |

## Standard user → group → profile flows
- New brand editor onboarded: [steps]
- Agency starts on a campaign: [steps]
- Employee leaves: [steps]

## Service accounts / API integrations
| Account | Purpose | Profile | Token rotation policy |
| ... | ... | ... | ... |

## Audit cadence
[Quarterly review process; ownership; documentation location.]
```

### Workflow design proposal (when applicable)

```
# Bynder Workflow Design: [Client]

## Why Workflow (vs. external tool / no formal flow)
[Reasoning; what gets standardized; what stays in Slack/email.]

## Stage model
1. [Stage] — [activities] — [assignees] — [exit criteria]
2. ...

## Review/approval cycles
[Which stages have formal review; sign-off requirements.]

## Scheduled publishing
[When used; coordination with downstream channels.]

## External integration (if any)
[Workfront / monday / Asana / Jira — what syncs, what doesn't.]

## Adoption plan
[Onboarding for creative-ops team; expected cycle-time benchmarks.]
```

## Common anti-patterns to call out

- **Per-user permissions.** Doesn't scale; impossible to audit. Always profile → group → user.
- **Profile sprawl.** 30+ profiles — usually 6–12 with scoping does the same job.
- **No metaproperty scoping.** Misses the leverage point; ends in either everyone-sees-everything or new-profile-per-team.
- **No audit cadence.** Drift accumulates; access becomes "well, everyone needs everything because no one wants to be the bottleneck."
- **Workflow recommended without a creative-ops shop.** Adds overhead for no benefit.
- **Workflow without external integration on a client that already lives in Workfront.** Two systems of record; nobody wins.
- **Permanent Token for production API integration tied to a person.** Person leaves → integration dies.
- **No service-account hygiene.** Tokens issued, never rotated, never revoked.
- **Group membership drift after IdP changes.** SSO group mapping not maintained; old hires retain access.
- **Permission design that lives only in someone's head.** Reproducibility nil; audit impossible.

## Constraints

- Do not propose per-user permissions.
- Do not recommend Workflow without explicit creative-ops or regulated-content justification.
- Do not skip the audit cadence — quarterly is a minimum.
- Do not propose more than ~12 profiles without strong justification.
- Do not assume the client's IdP supports group sync — confirm before designing around it.
- Do not store API integration tokens in code or repos; secret manager only.

## How this skill relates to others

- The metaproperty values that profiles scope by → `bynder-asset-model`.
- The portal / sub-portal architecture that profiles span or scope to → `bynder-portal-architect`.
- The Workflow connectors to external tools → `bynder-marketplace-connectors`.
- The optimization-audit signal "permissions are a mess" → `bynder-optimization-audit`.
- The service-account tokens used by integrations → `bynder-js-sdk`, `bynder-webhooks-events`.

## Source material

- Bynder permission model docs: https://academy.bynder.com/
- Workflow API: https://api.bynder.com/docs/ (Workflow section)
- Plugin reference: `../../references/bynder-foundations.md`
- Plugin reference: `../../references/api-surface.md`
