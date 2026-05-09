---
description: Bootstrap a new Next.js 16 project with our conventions — App Router, TS strict, Tailwind, shadcn, cacheComponents on, proxy.ts security headers, env validation, and the seven Next.js skills' defaults baked in from day one

# Project context
type: command
project: skills-library
plugin: software-engineering
tags: [type/command, plugin/software-engineering, topic/nextjs, topic/bootstrap]
status: active
---

Use the `software-engineering-agent` to scaffold a new Next.js 16 project with the seven Next.js skills' defaults wired in from day one. Don't pick a piece of this; do all of it.

Stages (run in order):

1. **Confirm prerequisites.** Node 22+, pnpm or bun, git initialized, deploy target named (Vercel, AWS, self-hosted). If deploy target is Vercel, queue `vercel-agent` (in the sibling `vercel` plugin) for stage 8.
2. **Scaffold the project.** `pnpm create next-app@latest` with `--typescript`, `--app`, `--tailwind`, `--eslint`, `--src-dir`, `--import-alias "@/*"`. Verify it boots.
3. **Tighten `tsconfig.json`** — set `strict: true`, `noUncheckedIndexedAccess: true`, `noImplicitOverride: true`, `verbatimModuleSyntax: true`. See `software-engineering-nextjs-routing/references/nextjs-typescript-best-practices.md`.
4. **Enable Cache Components.** Set `cacheComponents: true` in `next.config.ts`. Hand off to [[software-engineering-nextjs-cache]] for the per-endpoint caching plan.
5. **Wire env validation.** Create `lib/env.ts` with a Zod schema. Add `import "server-only"` to anything reading non-public env. From [[software-engineering-nextjs-config]].
6. **Stand up `proxy.ts`** (not `middleware.ts`) at the project root. Default to a no-op `proxy` returning `NextResponse.next()` with a sensible matcher. From [[software-engineering-nextjs-config]].
7. **Add security headers** in `next.config.ts` — X-Frame-Options, X-Content-Type-Options, Referrer-Policy, Strict-Transport-Security. From [[software-engineering-nextjs-config]].
8. **Scaffold the route shells** — root `layout.tsx`, `not-found.tsx`, `error.tsx`, `loading.tsx`. From [[software-engineering-nextjs-routing]] and [[software-engineering-nextjs-errors]]. Add the metadata API helpers (`buildPageMetadata`).
9. **Wire the design system** — add shadcn/ui via the CLI, configure Tailwind tokens. Hand off to [[software-engineering-shadcn-component]] and [[software-engineering-tailwind-tokens]].
10. **Install fonts and analytics scaffolding** — `next/font` for the primary type, `@vercel/speed-insights` (if Vercel) or `web-vitals` placeholder. From [[software-engineering-nextjs-performance]].
11. **Vercel-platform setup** (if applicable) — hand off to `vercel-agent` in the adjacent `vercel` plugin for project creation, env scoping (Production/Preview/Development), Deployment Protection, domain wiring, Vercel Blob/KV/Postgres provisioning if needed.

Stop and ask only if the user names a constraint that breaks a default (e.g., "we can't enable cacheComponents because we're stuck on Next 15 LTS until Q3" → switch to `/nextjs-upgrade` first).

Don't run this on an existing codebase — that's `/nextjs-upgrade`. This is greenfield only.

Expected duration: 30–60 minutes for a focused setup. Output is a runnable project with all seven concerns already addressed at the foundation, so the team isn't retrofitting them in week 4.
