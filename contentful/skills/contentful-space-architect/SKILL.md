---
name: contentful-space-architect
description: Design and operate Contentful spaces, environments, and environment aliases — pick the right space topology for the engagement, set up the deployment-pipeline pattern, plan cross-space references when a multi-space architecture is justified, and lay down the conventions that the rest of the plugin's skills rely on. Use this skill when starting a new client engagement, when a single space has gotten brittle and people are asking about splitting it, when the team is going from "edit master directly" to a real release pipeline, when environment aliases are about to be set up, or when cross-space references come up. Trigger any time the question is "where should this content live and how do we promote it through environments".

# Project context
type: skill
project: skills-library
plugin: contentful
aliases: [contentful-space-architect]
tags: [type/skill, plugin/contentful, topic/contentful, topic/architecture]
status: active
---

# Contentful Space Architect

This skill owns the topology layer — organizations, spaces, environments, environment aliases, and the deployment pipeline pattern that ships changes safely. The choices here become constraints on every other skill: the model lives inside an environment, migrations target an environment, the wrapper reads from an alias.

Pair with `contentful-migrations` (the operational arm of any environment change), `contentful-content-model` (the schema that has to travel cleanly between environments), `contentful-webhooks` (per-environment configuration), and `software-engineering-composable-architect` for the larger system view.

## When to use this skill

- Setting up Contentful for a new engagement.
- A single-environment client wants a real release pipeline.
- The team is asking about environment aliases and what they replace.
- Multi-brand or multi-region work where one space may not be enough.
- "We need to share content across these two products" → cross-space references.
- Auditing an existing setup that grew organically and is now hard to release against.

## Core posture

- **One organization, the right number of spaces.** Default to one space. Add more only when there's a real boundary (separate billing, separate roles, separate brand with no shared content).
- **Always run a deployment pipeline, even on small projects.** `master` is the alias. `master-staging` and feature environments hang behind it. The minute you have two people editing, you need the discipline.
- **Aliases are the cut-over mechanism.** Tokens read from aliases. Build the new environment, swap the alias, the front end sees the new state. Old environment is the rollback target.
- **Cross-space references are an answer to a specific problem, not a default.** If the answer to "why two spaces" isn't named in one sentence, it's one space.

## Topology decisions

### One space vs. many

| One space | Many spaces |
|---|---|
| Same brand, same audience, same editorial team | Distinct brands or businesses with separate editorial governance |
| Shared content (a `Person` who appears on multiple sites) | Truly disjoint content |
| Same compliance / data-residency posture | Different regulatory or sovereignty needs |
| Default for ~80% of engagements | Multi-brand portfolios; M&A transitionals; partner-vs-customer-facing splits |

The honest test: if removing a space would make day-to-day work easier, you have one space too many. If keeping them together creates ongoing political or governance friction, split.

When you do split, plan for cross-space references *before* the second space exists. Adding them retroactively to a relationship that should have been a single content type is more painful than it sounds.

### Environments inside a space

Every space ships with `master`. Add environments for:

- **`master-staging`** (or `staging`) — the alias-protected pre-prod target. Always behind an alias.
- **Release environments** — `release-2026-04-22` style, time-stamped, the body the alias points at. Created fresh for each release.
- **Feature environments** — `feat-multi-locale`, `feat-checkout-rework`. Branched from current master, used for in-flight work.
- **Sandbox environments** — `dev-bermon`, `dev-team`, ephemeral. Wipe weekly.

Environment limits live on the plan. Free-tier spaces cap at a small number; enterprise plans usually permit dozens. Confirm the cap before you design the strategy.

### Environment aliases

An alias is a name that points at an environment. Tokens read from the alias name, not the environment ID directly. The release pattern:

```
Today:
  master  ──► release-2026-04-22
  staging ──► release-2026-04-29

Cut day:
  master  ──► release-2026-04-29     (alias swap)
  staging ──► release-2026-05-06     (new clone, ready for the next release)

Old release-2026-04-22 sits idle as the rollback target until the next release ships clean.
```

Conventions to enforce:

- **Aliases are stable names.** Don't rename them. `master` and `staging` for life.
- **Environments behind aliases are time-stamped.** Never reuse an environment name; create a fresh clone each time.
- **Release environments expire.** After two clean releases past, delete the older one. They're not free.
- **Tokens read from aliases.** Direct-environment tokens exist for special cases (running migrations against `release-2026-04-22` before swap), but production reads always go through the alias.

### When aliases aren't available

Older plans or some legacy spaces don't have aliases enabled. The fallback is **token rotation** — generate a new Delivery token for each environment, deploy the front end with the new token, decommission the old. Slower, more error-prone, but workable. Push the client to enable aliases at renewal.

## Deployment pipeline (the canonical Slalom flow)

```
Developer / agent makes a schema change in a feature env (script-driven, see contentful-migrations)
       │
       ▼
Migration script committed to repo, reviewed, merged
       │
       ▼
CI applies migration to release env (cloned from current master)
       │
       ▼
Front-end PR + Storybook updates merged
       │
       ▼
QA against staging alias
       │
       ▼
Alias swap (master → new release env)
       │
       ▼
Smoke check; previous release env retained as rollback
```

Things to wire once and forget:

- **CI job that runs migrations** against the release env (see `contentful-migrations`).
- **Vercel preview deployments** point at the staging alias, not master.
- **Webhooks** are configured per environment (see `contentful-webhooks`). When you create a release env, the webhook config from master copies along; verify it on the new env before swap.
- **Audit logging** turned on at the org level (enterprise plans). Schema changes attribute to a person/CI account.

## Cross-space references

When you do split content across spaces, Contentful supports cross-space references in the Delivery, Preview, and GraphQL APIs (not CMA — you write into each space directly).

Constraints:

- Spaces must be in the same organization.
- The feature is gated by plan; confirm it's enabled.
- The reference resolves at read time; the dependency goes one way per relationship.
- Environment-aware: a cross-space reference resolves against a specified environment in the target space. Plan environment alignment carefully (see https://www.contentful.com/developers/docs/concepts/environment-support-for-cross-space-references/).

Modeling guidance:

- Pick a clear ownership direction. The space that owns the content type owns the entries. Other spaces consume.
- Don't fan-out: avoid having Space A reference Space B which references Space C. Two-hop reads get expensive.
- Keep the cross-space content small and stable. People, products, taxonomy terms, brand assets — yes. Page assemblies — almost never.

## Roles and permissions topology

Roles and team membership live at the space level. Recommended baseline:

- **Admin** — practice leads only.
- **Developer** — full access to environments, content types, webhooks. Can run migrations in non-prod.
- **Editor** — content authoring in the editor UI. Can publish in non-prod, requires review/scheduled action in prod.
- **Reviewer** — read-only + comments + scheduled actions approval.
- **Service accounts** — one PAT per system that needs to write (front-end build, Algolia sync, MCP server). Scoped where the plan supports it.

Use SSO + SCIM (see plugin foundations) for enterprise clients to keep the user roster honest.

## Output formats

### Topology proposal

```
# Contentful Topology: [Client]

## Space plan
- {space_id} ({brand/business}) — {what it owns}
  Why one space and not many: {one sentence}

## Environment plan
| Alias | Points at (today) | Purpose | Lifetime |
|---|---|---|---|
| master | release-2026-04-22 | production | perpetual alias |
| staging | release-2026-04-29 | pre-prod QA | perpetual alias |

## Release flow
{deployment pipeline diagram}

## Roles
| Role | Members | Capabilities |
|---|---|---|

## Service accounts
| Account | Token scope | Used by |
|---|---|---|

## Cross-space references (if any)
{which content types, which direction, why}

## Risks
{plan-cap, governance, training, rollback}
```

### Release runbook (one-page)

```
1. Branch release env from current master:
   contentful space environment create --source master --environment-id release-{date}
2. Apply migrations against release env (CI job: deploy-migration.yml).
3. Deploy front end PR with feature flag pointing reads at the release env directly.
4. Repoint staging alias at the release env.
5. QA on staging alias URL.
6. Repoint master alias at release env (cut-over).
7. Smoke test against production URL.
8. Wait 24h, then delete previous release env.
```

## Common anti-patterns

- **Editing `master` directly.** Even on small projects, this guarantees an untraceable schema change and a "why did the prod build break" Friday.
- **Not using aliases.** Hard-coded environment IDs in the front-end env file mean every release rotates tokens.
- **Long-lived feature environments.** A `feat-x` env that's lived for six months has its own divergent schema and merge conflicts. Set a 4-week soft expiry.
- **One service account for everything.** Compromise the front-end token and you've compromised the migration pipeline. Partition by purpose.
- **Cross-space references for political reasons.** "Marketing won't let us touch their content" is not an architecture; it's a roles problem. Solve roles first.
- **Never deleting old release envs.** Plans cap environment count; you'll hit the limit at the worst time.

## How this skill relates to others

- The migration discipline that hangs off this topology → `contentful-migrations`.
- The schema that travels through environments → `contentful-content-model`.
- Per-environment webhook config → `contentful-webhooks`.
- Locale strategy at the space level → `contentful-localization`.
- Personalization deployment posture → `contentful-personalization`.
- App definitions, which are org-scoped but install per space → `contentful-app-framework`.
- Tooling glue → `contentful-mcp-cli`.

## Source material

- Multiple environments: https://www.contentful.com/developers/docs/concepts/multiple-environments/
- Environment aliases: https://www.contentful.com/developers/docs/concepts/environment-aliases/
- Cross-space references: https://www.contentful.com/developers/docs/concepts/environment-support-for-cross-space-references/
- Best practices for environments and aliases: https://www.contentful.com/developers/docs/concepts/environments-and-environment-aliases-best-practices/
- Deployment pipeline: https://www.contentful.com/developers/docs/concepts/deployment-pipeline/
- Enterprise observability: https://www.contentful.com/developers/docs/concepts/enterprise-observability/
- Plugin reference: `../../references/contentful-foundations.md`
