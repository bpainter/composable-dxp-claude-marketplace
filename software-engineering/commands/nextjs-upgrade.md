---
description: Migrate a Next.js 14 or 15 codebase to Next.js 16 — async params, middleware-to-proxy rename, cacheComponents adoption, deprecated API sweep, with codemods where they exist and manual review where they don't

# Project context
type: command
project: skills-library
plugin: software-engineering
tags: [type/command, plugin/software-engineering, topic/nextjs, topic/migration]
status: active
---

Use the `software-engineering-agent` to drive a Next.js 14/15 → 16 upgrade. Don't run this against a codebase you don't have version control for and a green CI on.

Stages:

1. **Baseline.** Branch off `main`, ensure the suite is green, snapshot bundle size and the first page's TTFB. We'll compare against this at the end.
2. **Bump dependencies.** `pnpm up next@latest react@latest react-dom@latest`. Update `@types/react`, `@types/react-dom`, `eslint-config-next` to match. Run `pnpm install`.
3. **Run codemods, in order.**
   - `npx @next/codemod@latest middleware-to-proxy` — renames `middleware.ts` → `proxy.ts` and the export.
   - `npx @next/codemod@latest next-async-request-api` — converts synchronous `params` / `searchParams` access to `await`.
   - Review each codemod diff before committing. Codemods are conservative; some files will need a hand-fix.
4. **Verify the proxy runtime.** In Next.js 16, `proxy.ts` is Node runtime only — no `export const runtime = "edge"`. If the old middleware used Edge-only APIs, route them to a route handler with `runtime = "edge"` instead. From [[software-engineering-nextjs-config]].
5. **Audit `params` usage.** Grep for `params.` and `searchParams.` outside `await` blocks and `use()` calls. Every match is a bug. Type-check should catch most, but the codemod can miss generics. From [[software-engineering-nextjs-routing]].
6. **Decide on Cache Components.** Two paths:
   - **Adopt** (recommended for greenfield-shaped codebases): set `cacheComponents: true`, then walk every `fetch()` and decide explicit `"use cache"` placement. Use [[software-engineering-nextjs-cache]] decision matrix per endpoint.
   - **Defer** (large, in-flight codebases): keep the v15 caching defaults. Plan the migration as a separate workstream. Document the deferral with a date and an owner.
7. **Sweep deprecated APIs.** `NextApiRequest` / `NextApiResponse` (Pages Router only — flag any leak), `getServerSideProps` / `getStaticProps` (legacy), Edge runtime middleware (gone). Use `software-engineering-quality-engineer` for the sweep if the surface is large.
8. **Re-test.** Full suite + manual smoke through the critical user paths. Especially: any flow that depends on cookies (proxy.ts now runs Node runtime, behavior subtly differs), any dynamic OG image (`ImageResponse` runtime changes).
9. **Performance check.** Re-run Lighthouse / Speed Insights against the snapshot from stage 1. LCP and INP should be at parity or better. If worse, [[software-engineering-nextjs-performance]] for triage.
10. **Pre-deploy security review.** Run the checklist from [[software-engineering-nextjs-config]]. Cookie attributes, env-var leak check, `import "server-only"` on the right files.
11. **Stage and verify.** Deploy to a non-production environment first. Watch logs, error rates, and Web Vitals for at least one full traffic cycle before promoting to prod.

If the codebase is on Next.js 14, don't skip 15 — go 14 → 15 first (`searchParams` async), then 15 → 16. Two smaller PRs are safer than one large one.

Pair with `vercel-agent` (in the adjacent `vercel` plugin) if deploying to Vercel — the platform side may have its own migration items (Fluid Compute defaults, image optimization changes, env scoping).

Expected duration: 2–6 hours for a small/medium app, multi-day for a large one. The codemods do 70%; the remaining 30% is the work that matters.
