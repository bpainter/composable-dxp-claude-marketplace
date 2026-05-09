---
name: software-engineering-nextjs-config
description: Use this skill any time you're configuring environment variables, securing API access, writing or modifying proxy.ts (formerly middleware.ts), reviewing what's exposed to the client, or doing a pre-deploy security pass on a Next.js 16 app. Triggers include process.env, NEXT_PUBLIC_, .env.local, env validation with Zod, secret leakage, server-only / client-only packages, proxy.ts, the proxy() export, request matchers, edge vs nodejs runtime, cookies(), auth tokens, session cookies, Vercel deployment protection, exposed API keys, API route hardening, CORS, CSP, or "is it safe to ship this".

# Project context
type: skill
project: skills-library
plugin: software-engineering
aliases: [software-engineering-nextjs-config]
tags: [type/skill, plugin/software-engineering, topic/nextjs, topic/security, topic/config]
status: active
---

# Next.js 16 — Configuration and security

Env vars (and how to keep secrets out of the client bundle), `proxy.ts` (which replaces `middleware.ts` in Next.js 16), and the security review you should run before shipping any Next.js app.

## Environment variables

### The two flavors

| Prefix | Available in | Example |
|---|---|---|
| `NEXT_PUBLIC_` | **Server and client** (inlined into the JS bundle at build time) | `NEXT_PUBLIC_API_URL` |
| no prefix | **Server only** | `DATABASE_URL`, `STRIPE_SECRET_KEY` |

This is the most important rule: **anything in `NEXT_PUBLIC_*` is shipped to every visitor's browser.** Do not put secrets there. Ever.

### Storing them

| File | Loaded in | Committed to git? |
|---|---|---|
| `.env` | All envs (lowest priority) | Yes (only with non-sensitive defaults) |
| `.env.local` | Local dev (and tests) | **No** — gitignore it |
| `.env.development` / `.env.production` | The matching `NODE_ENV` | Optional |
| Vercel Project Env Vars | The deployed app | n/a (set in dashboard or CLI) |

### Validating env vars at boot

Validate with Zod so a misconfigured env fails the build, not a runtime fetch deep in production:

```ts
// lib/env.ts
import { z } from "zod";

const env = z.object({
  NEXT_PUBLIC_API_URL: z.string().url(),
  DATABASE_URL: z.string().url(),
  STRIPE_SECRET_KEY: z.string().min(1),
}).parse({
  NEXT_PUBLIC_API_URL: process.env.NEXT_PUBLIC_API_URL,
  DATABASE_URL: process.env.DATABASE_URL,
  STRIPE_SECRET_KEY: process.env.STRIPE_SECRET_KEY,
});

export default env;
```

Import `env` everywhere instead of touching `process.env` directly. The build will refuse to compile if a required var is missing.

### Don't leak server env vars to client components

```ts
// lib/api/client.ts (server only)
const TOKEN = process.env.STRIPE_SECRET_KEY;
```

If a client component imports this file, the build will succeed but `TOKEN` will be `undefined` at runtime — except when Next.js inlines it into the bundle, which depending on circumstances **can leak the secret**. Add `import "server-only"` at the top of any module that reads non-public env vars:

```ts
import "server-only";
const TOKEN = process.env.STRIPE_SECRET_KEY;
```

Now any accidental client import is a build error.

## `proxy.ts` (replaces `middleware.ts`)

In Next.js 16, **`middleware.ts` was renamed to `proxy.ts`**. Functionality is the same; the rename clarifies what it is — a network-edge proxy in front of your routes.

### Two important changes

1. **The file is `proxy.ts` and the export is `proxy`** (not `middleware`).
2. **The runtime is Node.js, not Edge.** You cannot opt back into Edge for `proxy.ts`. Edge runtime middleware is deprecated.

A codemod is available: `npx @next/codemod@latest middleware-to-proxy`.

### Anatomy

```ts
// proxy.ts (at the project root, next to next.config.ts)
import { NextRequest, NextResponse } from "next/server";

export function proxy(request: NextRequest) {
  // Examples of what proxy can do:
  // 1. Inspect/modify request headers
  // 2. Rewrite to a different URL (NextResponse.rewrite)
  // 3. Redirect (NextResponse.redirect)
  // 4. Set/read cookies
  // 5. Reject with NextResponse.json({...}, { status: 401 })

  return NextResponse.next();
}

export const config = {
  matcher: [
    // Apply to all routes except _next, static files, and APIs you want untouched
    "/((?!_next/static|_next/image|favicon.ico|api/health).*)",
  ],
};
```

### When to use it

- Auth check: redirect unauthenticated users to `/login`
- A/B test cookie assignment
- Geographic routing (`request.geo.country` → rewrite to localized variant)
- Bot blocking
- Adding security headers globally

### When NOT to use it

- Heavy database queries (it runs on every matched request — slow proxy = slow site)
- Anything that should be a Server Action or Route Handler
- Auth checks that need access to your full DB schema (do them in layouts/pages, where caching is friendlier)

### Cookie / session work

You **don't** need `proxy.ts` to manage session or cart cookies. Use server actions and Route Handlers — they have access to `cookies()` from `next/headers` and that's the right place. `proxy.ts` is for cross-cutting concerns above the routing layer.

## Cookie hardening

Any session-bearing cookie should be set with these attributes:

```ts
import { cookies } from "next/headers";

const cookieStore = await cookies();
cookieStore.set("session_token", token, {
  httpOnly: true,                                    // JS can't read it
  secure: process.env.NODE_ENV === "production",     // HTTPS only in prod
  sameSite: "lax",                                   // mitigates CSRF
  path: "/",
  maxAge: 60 * 60 * 24,                              // match server-side expiry
});
```

For cross-origin scenarios (third-party iframes, OAuth popups), `sameSite: "none"` + `secure: true` is required. Avoid unless you have a real reason.

## Headers and CSP

For most apps, set security headers globally via `next.config.ts`:

```ts
const nextConfig: NextConfig = {
  async headers() {
    return [{
      source: "/:path*",
      headers: [
        { key: "X-Frame-Options", value: "DENY" },
        { key: "X-Content-Type-Options", value: "nosniff" },
        { key: "Referrer-Policy", value: "strict-origin-when-cross-origin" },
        { key: "Strict-Transport-Security", value: "max-age=63072000; includeSubDomains; preload" },
        { key: "Permissions-Policy", value: "camera=(), microphone=(), geolocation=()" },
      ],
    }];
  },
};
```

A strict Content Security Policy is harder — it requires per-route nonce handling for inline scripts. Reach for it on regulated/high-value apps; skip it on landing pages.

Image domains must also be in `images.remotePatterns` (CSP-adjacent — Next.js's image optimizer enforces it).

## Pre-deploy security review

Run through this list before shipping:

1. **`.env.local` is gitignored.** `git ls-files | grep .env.local` should be empty.
2. **No secrets in `NEXT_PUBLIC_*`.** Only public URLs and feature flags belong there.
3. **Server-only modules import `"server-only"`.** Try to import them from a `"use client"` file — should fail at build.
4. **Server Actions validate input.** Every `formData.get(...)` is run through Zod or manual checks.
5. **Server Actions authorize.** A `removeItemAction(itemId)` must scope to the current user.
6. **Cookies are `httpOnly` + `secure` + `sameSite: "lax"`.**
7. **Error pages don't leak internals.** `error.message` is shown only in dev; production shows a generic message + `error.digest`.
8. **Security headers set** in `next.config.ts`.
9. **Auth flow tested with cookies disabled** (degrades gracefully, not silently).
10. **Rate limiting / bot mitigation** in place for any unauthenticated mutation endpoints.

## Common mistakes

- Reading `process.env.SECRET` in a component file imported by a `"use client"` file → secret either becomes `undefined` or worse, gets bundled.
- Using the old `middleware.ts` name in Next.js 16 → file is silently ignored. The codemod renames it for you.
- Writing a session cookie without `httpOnly` → any third-party script with XSS access can steal it.
- Putting auth checks in `proxy.ts` that depend on full DB lookups → every request pays the latency.
- Trusting the client `min`/`max`/`required` HTML attributes — clients lie. Validate server-side.

## How this skill relates to others

- Server actions that read cookies → [[software-engineering-nextjs-actions]]
- Server vs client boundary that determines what env vars are accessible → [[software-engineering-nextjs-server-client]]
- Plugin reference: `references/clean-code-foundations.md` (input validation, error handling)
- Plugin agent: `software-engineering-security-advisor` for the broader threat-modeling pass
- Vercel-platform-specific config (deployment protection, env scoping per environment, edge config) → `vercel-agent` in the adjacent `vercel` plugin

## References

- [Next.js: Renaming Middleware to Proxy](https://nextjs.org/docs/messages/middleware-to-proxy)
- [Next.js: Upgrading to Version 16](https://nextjs.org/docs/app/guides/upgrading/version-16)
- [Next.js: proxy.js](https://nextjs.org/docs/app/api-reference/file-conventions/proxy)
- [Next.js: Environment Variables](https://nextjs.org/docs/app/guides/environment-variables)
- [OWASP: Secure Cookie Attributes](https://owasp.org/www-community/controls/SecureCookieAttribute)
