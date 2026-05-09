---
type: reference
project: skills-library
scope: plugin
plugin: vercel
tags: [type/reference, plugin/vercel, scope/foundational, topic/cli]
status: active
---
# Vercel CLI cheat sheet

Quick reference for the [`vercel`](https://vercel.com/docs/cli) CLI. Skills cite this file rather than restating commands.

Install: `pnpm add -g vercel` (or `npx vercel` for one-offs).

## Auth

```bash
# Interactive login (opens browser).
vercel login

# Confirm identity.
vercel whoami

# Use a token (CI / scripts).
export VERCEL_TOKEN=...
vercel --token "$VERCEL_TOKEN" whoami

# Switch the active scope (Personal vs. a Team).
vercel switch
```

Tokens live at https://vercel.com/account/tokens. For CI, prefer scoped Team tokens that you can revoke independently.

## Project bootstrapping

```bash
# In a fresh repo: connect to a Vercel project (or create one).
vercel

# Link an existing local repo to an existing Vercel project (no deploy).
vercel link

# Pull project settings + env vars to .vercel/.env.development.local
vercel env pull
```

Defaults:

- `vercel link` writes `.vercel/project.json` (committed) and `.vercel/.env.*` (gitignored).
- `vercel env pull` is the canonical way to get a `.env.local` for `next dev` that mirrors production.

## Deploying

```bash
# Deploy current directory to a preview URL.
vercel

# Deploy to production (only if you intend to bypass the git workflow).
vercel --prod

# Deploy with a specific build target / scope / project.
vercel --scope my-team --project my-project

# Inspect a deployment.
vercel inspect <deployment-url>

# Remove an old deployment.
vercel rm <deployment-id-or-url>
```

For Slalom builds, **production deploys go through git pushes to the production branch**, not through the CLI. The CLI deploy is for emergency hotfixes or local validation. Document any CLI prod deploy in the team's release log.

## Local dev with prod-like envs

```bash
# Reproduces the Vercel build environment locally — closer to prod than `next build`.
vercel build

# Run a local server against the built output (uses Vercel's dev-server).
vercel dev
```

`vercel build` is the right tool for diagnosing build failures that don't reproduce under plain `next build`.

## Environment variables

```bash
# List env vars.
vercel env ls

# Add an env var (interactive prompts for environment + value).
vercel env add MY_KEY production

# Add to multiple environments at once.
vercel env add MY_KEY production preview

# Remove.
vercel env rm MY_KEY production

# Pull all env vars to a local file (target-specific).
vercel env pull --environment=production
vercel env pull --environment=preview
```

Conventions:

- Public client-side vars must be prefixed `NEXT_PUBLIC_` (Next.js convention).
- Secrets stay server-side; verify they aren't accidentally `NEXT_PUBLIC_*`.
- Prefer setting via dashboard for one-off keys; use CLI for scripted rollouts.

## Domains

```bash
# List domains under the active scope.
vercel domains ls

# Add a domain to a project.
vercel domains add example.com my-project

# Remove.
vercel domains rm example.com
```

Most domain work is faster in the dashboard. The CLI shines for scripted multi-project domain attaches.

## Logs

```bash
# Stream logs from a deployment.
vercel logs <deployment-url>

# Filter / format.
vercel logs <deployment-url> --since 1h --output json
```

For production observability, configure Log Drains in the dashboard rather than relying on CLI tail.

## Project / team management

```bash
# List projects in active scope.
vercel project ls

# Show project details.
vercel project inspect my-project

# Switch active scope (Personal vs. Team).
vercel switch
```

## CI usage patterns

GitHub Actions example:

```yaml
- uses: actions/checkout@v4
- run: pnpm install --frozen-lockfile
- name: Pull Vercel env
  run: pnpm dlx vercel pull --yes --environment=preview --token=${{ secrets.VERCEL_TOKEN }}
- name: Build
  run: pnpm dlx vercel build --token=${{ secrets.VERCEL_TOKEN }}
- name: Deploy preview
  run: pnpm dlx vercel deploy --prebuilt --token=${{ secrets.VERCEL_TOKEN }}
```

The `vercel pull` + `vercel build` + `vercel deploy --prebuilt` triad is the canonical scripted-deploy flow when you can't use the git integration directly.

## Common pitfalls

- **`vercel link` not run** → `vercel build` fails because there's no project context. Run `vercel link` (interactive) or set `VERCEL_PROJECT_ID` and `VERCEL_ORG_ID` in CI.
- **Wrong scope** → deploys land under the personal account instead of the Team. `vercel switch` or pass `--scope`.
- **Dotenv conflicts** → `vercel dev` reads `.vercel/.env.development.local`; `next dev` reads `.env.local`. Keep them in sync via `vercel env pull`.
- **`vercel logs` retention is short** → for incident investigation past the retention window, you need Log Drains. See `vercel-observability`.
- **CLI deploy bypassing git** → no PR review, no preview URL discoverability. Reserve for emergencies and document.
