---
description: Add a new Next.js 16 feature with the right cross-skill sequencing — route + server/client split + caching + actions + errors + perf, in the order that produces a coherent, well-shaped feature on first try

# Project context
type: command
project: skills-library
plugin: software-engineering
tags: [type/command, plugin/software-engineering, topic/nextjs, topic/feature-development]
status: active
---

Use the `software-engineering-agent` to sequence the seven Next.js skills against a new feature. The point is to hit each concern in the right order, so we don't end up retrofitting caching after the route is half-built or discovering the form needed validation after we shipped.

Before the work starts, the agent confirms three things from the user:
- **Feature name and route shape** — single page or multi-page flow? Static or dynamic? URL-as-state, or React state?
- **Data sources** — what gets fetched, what's user-scoped vs shared, what's mutated.
- **Interactivity** — forms? buttons that mutate? optimistic UI?

Then sequence:

1. **Route shape** — [[software-engineering-nextjs-routing]] decides where files live, layout composition, dynamic segment names, async `params` / `searchParams` typing.
2. **Server / client boundary** — [[software-engineering-nextjs-server-client]] draws the line. Default to server; push `"use client"` to the smallest interactive island. Identify which sub-components need hooks and split them out.
3. **Caching strategy** — [[software-engineering-nextjs-cache]] decides per-endpoint: `"use cache"` or not, `cacheLife`, tags, where Suspense boundaries go and what `key` props they need. **Cache the static, stream the dynamic.** Don't cache user-scoped data.
4. **Mutations and forms** — [[software-engineering-nextjs-actions]] for any form, button, or interaction that mutates server state. Always Zod-validate the FormData. Always authorize. Always `revalidatePath` or `revalidateTag` after a successful mutation.
5. **Error and loading surfaces** — [[software-engineering-nextjs-errors]] adds segment-level `error.tsx`, `not-found.tsx`, and `loading.tsx` (or `<Suspense>`) where failure or latency could otherwise blank the page.
6. **Performance pass** — [[software-engineering-nextjs-performance]] sets `priority` on the LCP image, configures `sizes`, picks a font strategy, generates dynamic OG if the feature has shareable URLs.
7. **Config / security pass** — [[software-engineering-nextjs-config]] reviews any new env vars (server-only? `NEXT_PUBLIC_`?), any new `proxy.ts` rules (auth gating? geo routing?), and any cookies the feature sets (httpOnly + secure + sameSite).

Skip stages the feature genuinely doesn't touch. A pure read-only static page may legitimately skip stages 4 and 7. A list page with no interactive bits skips 2 and 4. Don't skip 5 — every feature deserves an `error.tsx` and a `loading.tsx`.

Hand the resulting plan to the human before writing code. Show:
- Files to create/modify (paths)
- Caching contract per fetch
- Suspense boundary placement and `key` props
- Server actions with their Zod schemas
- Error / not-found / loading surfaces
- Web Vitals levers used

Then build. The point of the upfront plan is that the seven concerns are addressed by the time you start typing, not after the PR.

Don't use this for tiny tweaks ("add a Cancel button to the existing form") — that's a direct skill call. This is for a feature that crosses three or more concerns.
