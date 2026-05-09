---
description: Audit an existing Next.js 16 codebase against May 2026 best practices — seven checks across routing, server/client, caching, server actions, errors/loading, performance, and config/security, with a prioritized fix plan

# Project context
type: command
project: skills-library
plugin: software-engineering
tags: [type/command, plugin/software-engineering, topic/nextjs, topic/audit, topic/code-review]
status: active
---

Use the `software-engineering-agent` to run a structured audit of an existing Next.js 16 codebase. The output is a scored report with a prioritized fix plan, not a refactor — we produce the plan, then route per-item to the relevant skill.

Run the seven checks in order. Score each healthy / warning / critical. Cite specific file paths and line numbers in findings.

1. **Routing and layouts** — see [[software-engineering-nextjs-routing]].
   - Are `params` and `searchParams` awaited everywhere (no synchronous access)?
   - Layouts free of `searchParams`?
   - Dynamic routes use `generateStaticParams` only when SSG is genuinely wanted?
   - In-app navigation uses `<Link>`, not `<a>`?
   - Filter / search state in the URL or in React state?

2. **Server / client architecture** — see [[software-engineering-nextjs-server-client]].
   - `"use client"` pushed to the smallest leaf, or stamped at the top of large pages?
   - Server-only modules guarded with `import "server-only"`?
   - Any `useEffect` + `fetch` patterns that should be server fetches?
   - Hydration error count in production logs?

3. **Caching and Suspense** — see [[software-engineering-nextjs-cache]].
   - `cacheComponents: true` in `next.config.ts`?
   - Per-endpoint caching matches volatility — static cached, user-scoped not, request-scoped streamed?
   - Suspense boundaries around dynamic content?
   - Suspense boundaries on URL-state-dependent content have a `key` prop?
   - Any `cookies()` / `headers()` / `await params` inside a `"use cache"` function (build error, but easy to slip past in review)?

4. **Server actions and mutations** — see [[software-engineering-nextjs-actions]].
   - Every `formData.get(...)` validated through Zod or manual checks?
   - Every action authorizes the operation against the current user/session?
   - `revalidatePath` or `revalidateTag` called after every successful mutation?
   - No secrets returned in action results?
   - `redirect()` is the last statement (not mid-function)?

5. **Errors and loading** — see [[software-engineering-nextjs-errors]].
   - Root `error.tsx`, `not-found.tsx`, `loading.tsx` all present?
   - `error.tsx` is `"use client"`?
   - 404 cases use `notFound()` (returning HTTP 404), not JSX returning 200?
   - `error.message` shown to the user only in dev (production shows generic + `error.digest`)?
   - Per-segment error/loading surfaces where graceful degradation matters?

6. **Performance** — see [[software-engineering-nextjs-performance]].
   - `next/image` used everywhere, with `width`/`height` (or `fill` + sized parent)?
   - `priority` only on the LCP image?
   - `sizes` on every non-fixed-width image?
   - `next/font` used for all webfonts (no CSS `@import` or `<link>` hand-rolled)?
   - `next/script` with `afterInteractive` (or rare justified `beforeInteractive`)?
   - Metadata API used (no hand-authored `<head>` tags)?
   - LCP ≤ 2.5s, INP ≤ 200ms, CLS ≤ 0.1 in field data?

7. **Config and security** — see [[software-engineering-nextjs-config]].
   - `.env.local` gitignored?
   - No secrets in `NEXT_PUBLIC_*`?
   - Server-only modules import `"server-only"`?
   - Cookies set with `httpOnly` + `secure` + `sameSite: "lax"`?
   - Security headers set in `next.config.ts`?
   - Env vars validated through a Zod schema at boot, not read raw with `process.env.X`?
   - File is `proxy.ts` (not the legacy `middleware.ts`)?

Then produce the fix plan, ordered by leverage:
- **Critical** — security holes, broken caching that leaks user data, missing error boundaries that take down the app.
- **High** — missing Suspense `key` props, missing input validation, missing `import "server-only"` on secret-reading modules, `priority` on non-LCP images.
- **Medium** — generalist hardening: env validation, security headers, cookie attributes.
- **Defer** — stylistic cleanups, naming consistency, opportunistic performance gains.

Each fix-plan item lists: the finding, the file paths, the specific skill that owns the remediation, and the rough effort.

Don't execute fixes during the audit. Hand the report to the human, get approval per item, then route to the relevant `software-engineering-nextjs-*` skill for the actual work.

If the audit reveals the codebase is on Next.js 14/15, stop the audit and run `/nextjs-upgrade` first — auditing pre-16 code against 16 best practices generates noise.

Expected duration: 30–90 minutes for a small/medium app. Pair with `software-engineering-quality-engineer` for the test-coverage and a11y dimensions.
