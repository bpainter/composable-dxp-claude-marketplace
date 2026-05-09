---
name: software-engineering-nextjs-errors
description: Use this skill any time you're designing or troubleshooting how a Next.js 16 app handles failures, missing resources, or loading states. Triggers include error.tsx, global-error.tsx, not-found.tsx, the notFound() function from next/navigation, the redirect() function, throwing errors from server components, ErrorBoundary, "Something went wrong" UI, 404 pages, segment-level error fallbacks, retry/reset patterns, or any work where you need to decide between "throw notFound()" vs. "return null" vs. "render an error UI".

# Project context
type: skill
project: skills-library
plugin: software-engineering
aliases: [software-engineering-nextjs-errors]
tags: [type/skill, plugin/software-engineering, topic/nextjs, topic/errors]
status: active
---

# Next.js 16 — Errors and not-found surfaces

Next.js gives you **four file conventions** to handle the unhappy path. Use them; don't reinvent.

| File | When it shows | Must be |
|---|---|---|
| `not-found.tsx` | `notFound()` is called in this segment | Server component (typically) |
| `error.tsx` | An uncaught error is thrown in this segment | Client component (`"use client"`) |
| `global-error.tsx` | An uncaught error in the root layout itself | Client component |
| `loading.tsx` | Auto-Suspense fallback during navigation | Server component |

Place them at the segment level where you want them to handle errors for that segment and below.

## not-found.tsx

Triggered explicitly. You decide *when* to call `notFound()`:

```ts
// app/products/[param]/page.tsx
import { notFound } from "next/navigation";

export default async function ProductPage({ params }) {
  const { param } = await params;
  const product = await getProduct(param);
  if (!product) notFound();   // throws under the hood; nothing renders below
  return <ProductView product={product} />;
}
```

The closest `not-found.tsx` up the tree renders. If none exists, `app/not-found.tsx` is the fallback.

```tsx
// app/products/[param]/not-found.tsx
import Link from "next/link";

export default function ProductNotFound() {
  return (
    <section>
      <h1>Product not found</h1>
      <p>We can't find that one. Try browsing the catalog.</p>
      <Link href="/search">Browse products</Link>
    </section>
  );
}
```

### `notFound()` returns HTTP 404

The response from a `notFound()` call is `404 Not Found`, not `200 OK` with a 404 page. Important for SEO and indexers.

## error.tsx

Catches any uncaught error thrown during rendering of the segment. **Must be a client component** (it uses React's `componentDidCatch` machinery under the hood).

```tsx
// app/error.tsx
"use client";

import { useEffect } from "react";

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  useEffect(() => {
    // Log to your observability tool. Don't log raw errors to console in prod.
    console.error(error);
  }, [error]);

  return (
    <section>
      <h1>Something went wrong.</h1>
      <button onClick={() => reset()}>Try again</button>
    </section>
  );
}
```

Two things to know:

1. **`error.tsx` doesn't catch errors in the layout it sits next to** — only in the page and below. To catch a layout's own errors you need `error.tsx` one level up, or `global-error.tsx` for the root.
2. **`error.digest`** is a server-side error ID. Surface it in the UI (dev only) or to your logger. The actual stack trace is hidden from users in production.

### Per-segment error.tsx

Useful when one section of your app should degrade gracefully without taking down the rest. Example:

```
app/
  error.tsx                   ← whole-app fallback
  search/
    page.tsx
    error.tsx                 ← only renders if /search throws
```

If the search page throws, the header and footer still render — only the search content swaps to the error UI.

## global-error.tsx

Renders **its own `<html>` and `<body>` tags** because the root layout failed:

```tsx
"use client";

export default function GlobalError({ error, reset }) {
  return (
    <html>
      <body>
        <h1>App crashed</h1>
        <button onClick={() => reset()}>Reload</button>
      </body>
    </html>
  );
}
```

Only triggered when the **root layout itself** throws. Rare, but worth having.

## global-not-found.tsx (optional)

A single 404 page for unmatched routes anywhere in the app. Bypasses your normal layout pipeline, so it must define its own `<html>`, `<body>`, fonts, and styles. Use only if you have very specific 404 needs that the standard `app/not-found.tsx` can't handle.

## When to use which

| Situation | Tool |
|---|---|
| User typed a URL that doesn't match any route | `app/not-found.tsx` (auto) |
| Resource doesn't exist (e.g. unknown product slug) | `notFound()` from your page → segment `not-found.tsx` |
| Network call failed | Throw → caught by nearest `error.tsx` |
| Validation error in a server action | Return `{ error: "..." }`, surface with `useActionState` |
| Loading delay on navigation | `loading.tsx` |
| Loading delay on a slow component | `<Suspense fallback={...}>` (more granular) |

## Loading vs. Suspense

- **`loading.tsx`** = file-convention shorthand for "wrap this whole segment in `<Suspense>` while it loads."
- **`<Suspense>`** = explicit, finer-grained boundary inside a page.

Use both:

```tsx
// app/search/loading.tsx       ← shows during nav from /cart → /search
export default function Loading() { return <ResultsSkeleton />; }

// app/search/page.tsx          ← within the page, refine further
export default async function SearchPage({ searchParams }) {
  const { q, category } = await searchParams;
  return (
    <main>
      <SearchInput defaultQ={q} />
      <CategoryFilter defaultValue={category} />
      <Suspense key={`${q}-${category}`} fallback={<ResultsSkeleton />}>
        <SearchResults q={q} category={category} />
      </Suspense>
    </main>
  );
}
```

## Common mistakes

- **Forgetting `"use client"` on `error.tsx`** — it won't compile, and the error message is misleading.
- **Returning JSX for "not found" instead of calling `notFound()`** — produces 200 OK with a 404-looking page, which is bad for SEO and indexers.
- **Logging raw error objects to the user** — leaks stack traces in production. Use `error.digest` for support, not `error.message`.
- **No `loading.tsx` or `<Suspense>` around dynamic data** — page hangs blank while data resolves.

## How this skill relates to others

- File conventions, where to place each file → [[software-engineering-nextjs-routing]]
- Suspense boundaries that pair with loading.tsx → [[software-engineering-nextjs-cache]]
- Server-action validation errors that don't go through error.tsx → [[software-engineering-nextjs-actions]]
- Why error.tsx must be a client component → [[software-engineering-nextjs-server-client]]

## References

- [Next.js: not-found.js](https://nextjs.org/docs/app/api-reference/file-conventions/not-found)
- [Next.js: error.js](https://nextjs.org/docs/app/api-reference/file-conventions/error)
- [Next.js: loading.js](https://nextjs.org/docs/app/api-reference/file-conventions/loading)
