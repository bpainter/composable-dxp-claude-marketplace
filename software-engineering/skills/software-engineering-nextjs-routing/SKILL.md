---
name: software-engineering-nextjs-routing
description: Use this skill any time you're creating, modifying, or reasoning about routes, layouts, or navigation in a Next.js 16 App Router project. Triggers include any work involving the app/ directory, page.tsx, layout.tsx, dynamic segments like [id] or [...slug], route groups (folder), parallel routes (@slot), intercepting routes ((..)folder), the Link component, useRouter, useParams, useSearchParams, generateStaticParams, or async params/searchParams. Also use when planning route structure for a new feature, designing nested layouts, or migrating from Pages Router. Pairs with software-engineering-nextjs-cache for caching strategy and software-engineering-nextjs-server-client for the server/client split.

# Project context
type: skill
project: skills-library
plugin: software-engineering
aliases: [software-engineering-nextjs-routing]
tags: [type/skill, plugin/software-engineering, topic/nextjs, topic/routing]
status: active
---

# Next.js 16 — Routing and layouts

Everything in the App Router file system: how routes are formed, how layouts compose, how navigation works, and how to read params and searchParams in Next.js 16.

## Mental model

The App Router maps the **`app/` directory tree to URLs**. Folders are segments. Files inside a folder define behavior for that segment.

```
app/
  layout.tsx       ← wraps every page (root layout, required)
  page.tsx         ← UI for "/"
  loading.tsx      ← Suspense fallback for "/"
  error.tsx        ← error boundary for "/"
  not-found.tsx    ← 404 UI for "/"
  products/
    layout.tsx     ← wraps every page under /products
    page.tsx       ← UI for "/products"
    [param]/
      page.tsx     ← UI for "/products/<anything>"
  (marketing)/     ← route group: doesn't appear in URL
    about/
      page.tsx     ← UI for "/about"
```

A request walks the tree from root downward, composing `layout.tsx` files outside-in, and the deepest matching `page.tsx` ends the chain.

## Core conventions

| File | Purpose | Default rendering |
|---|---|---|
| `layout.tsx` | Wraps children. Persists across navigation. **Cannot** receive `searchParams`. | Server component |
| `page.tsx` | Leaf UI for a route. Receives `params` + `searchParams`. | Server component |
| `template.tsx` | Like layout but re-mounts on navigation. | Server component |
| `loading.tsx` | Auto-Suspense fallback for the segment. | Server component |
| `error.tsx` | Error boundary. **Must** be a client component. | Client (`"use client"`) |
| `not-found.tsx` | 404 UI for the segment. Triggered by `notFound()`. | Server component |
| `route.ts` | Route Handler (HTTP methods). Replaces API Routes. | Server-only |

## Dynamic segments

Three flavors:

```
app/products/[param]/page.tsx       → /products/123, /products/black-tee
app/blog/[...slug]/page.tsx         → /blog/2026/01/post (catches all)
app/shop/[[...slug]]/page.tsx       → /shop and /shop/a/b (optional catch-all)
```

To pre-render dynamic segments at build time, export `generateStaticParams`:

```ts
export async function generateStaticParams() {
  const products = await getAllProducts();
  return products.map((p) => ({ param: p.slug }));
}
```

## Async `params` and `searchParams` (Next.js 16)

**This changed in v15 (`searchParams`) and v16 (`params`).** Both are now `Promise<T>`. You must await them in server components or `use()` them in client components.

### Server component (page or generateMetadata)

```ts
type Props = {
  params: Promise<{ param: string }>;
  searchParams: Promise<{ q?: string; category?: string }>;
};

export default async function Page({ params, searchParams }: Props) {
  const { param } = await params;
  const { q, category } = await searchParams;
  // ...
}

export async function generateMetadata({ params }: Props) {
  const { param } = await params;
  const product = await getProduct(param);
  return { title: product.name };
}
```

### Client component

```tsx
"use client";
import { use } from "react";

type Props = { searchParams: Promise<{ q?: string }> };

export function ClientFilters({ searchParams }: Props) {
  const { q } = use(searchParams);
  // ...
}
```

### Important rules

- **Layouts do not receive `searchParams`.** Only pages do. If a layout needs query state, read it in the page and pass it down.
- **`params` is shared up the tree.** A layout at `app/products/[param]/layout.tsx` receives the same `params` promise as the page below it.
- **Don't read `params` inside a `"use cache"` function.** Params are dynamic by definition; caching them defeats the point. Pass the resolved value in.

## Nested layouts

Layouts compose **outside-in**:

```
RootLayout
  └── (marketing)Layout       (route group, no URL impact)
        └── ProductsLayout    (renders for every /products/* route)
              └── ProductPage
```

Use this to share UI: a header in root layout, a sidebar in a section layout, page-specific UI in the page itself. Each layout's state survives navigation **within its subtree** — which is why filter sidebars in section layouts feel snappy.

## Route groups `(name)`

Folders wrapped in parens are **organizational only** — they don't add URL segments. Use them to:

- Apply a different layout to a subset of routes (e.g. `(marketing)` vs `(shop)`).
- Group related routes without polluting the URL.

## Parallel routes `@slot` and intercepting routes `(..)`

Advanced. Parallel routes let you render multiple pages into the same layout simultaneously (e.g. `@modal` + `@feed`). Intercepting routes let you load a route in a different layout context (e.g. open `/photo/123` as a modal over `/feed`).

For most apps, you don't need these. Modal-style overlays are usually simpler with shadcn `Sheet` or `Dialog`. Reach for parallel routes when you genuinely need two route trees rendered side by side and the back button should treat them independently.

## Navigation

### `<Link>`

The default for any in-app navigation. Performs **client-side transitions** with prefetching:

```tsx
import Link from "next/link";

<Link href="/products/black-tee" prefetch>Black T-Shirt</Link>
```

`prefetch` is `true` by default for Links in the viewport. Set `prefetch={false}` for low-priority links to save bandwidth.

### `useRouter` (programmatic)

```tsx
"use client";
import { useRouter } from "next/navigation";

const router = useRouter();
router.push("/search?q=mug");
router.replace("/cart");
router.refresh();
```

### URL as state

A pattern that comes up constantly: **don't put filter/search state in React state, put it in the URL**. The page becomes shareable, refresh-safe, and works without JS. Read with `searchParams` server-side; navigate with `router.push` from client components. Filtered list pages and search results are textbook examples.

## "Multi-app" routing

Multiple co-existing root experiences in one Next.js project — typically via route groups with their own root layouts:

```
app/
  (storefront)/
    layout.tsx     ← storefront chrome
    page.tsx
  (admin)/
    layout.tsx     ← admin chrome (different fonts, theme)
    dashboard/page.tsx
```

Each route group's layout effectively replaces the root layout for that subtree. Useful for marketing sites + apps, or storefront + admin in the same repo. For a single-experience product, this is overkill — but know it exists.

## Common mistakes

1. **Reading `params` synchronously** — throws in Next.js 16. Always `await`.
2. **Putting `searchParams` in a layout** — won't work. Move to the page.
3. **Forgetting `key` on `<Suspense>` when params change** — the same Suspense instance is reused; without a `key`, it won't show the fallback again on navigation.
4. **Using `<a>` for in-app links** — full reload, breaks transitions. Use `<Link>`.
5. **Generating params for every dynamic route** — only do this when you actually want SSG. Otherwise, render on demand.

## How this skill relates to others

- Caching, Suspense placement, streaming → [[software-engineering-nextjs-cache]]
- Server vs client component decisions → [[software-engineering-nextjs-server-client]]
- Server actions and forms → [[software-engineering-nextjs-actions]]
- error.tsx, not-found.tsx, loading.tsx → [[software-engineering-nextjs-errors]]
- Images, fonts, metadata, Web Vitals → [[software-engineering-nextjs-performance]]
- Env, proxy.ts, security → [[software-engineering-nextjs-config]]

## References

- Plugin-level: `references/architecture-patterns-2026.md` (when to colocate vs feature-based)
- This skill's references: `references/nextjs-typescript-best-practices.md`
- [Next.js: App Router file conventions](https://nextjs.org/docs/app/api-reference/file-conventions/page)
- [Next.js: Async params and searchParams in v16](https://nextjs.org/docs/messages/sync-dynamic-apis)
