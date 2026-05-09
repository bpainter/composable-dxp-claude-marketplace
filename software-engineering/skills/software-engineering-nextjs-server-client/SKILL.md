---
name: software-engineering-nextjs-server-client
description: Use this skill any time you're deciding whether a component should run on the server or the client, drawing the boundary between them, or composing components across that boundary in a Next.js 16 app. Triggers include "use client", "use server" directives, passing props from server to client components, fetching data inside components, hydration errors, "Cannot import server-only code into a Client Component" errors, useState/useEffect being used in a Next.js component, or designing a new component's split. Also use when refactoring to push state down or pull data up.

# Project context
type: skill
project: skills-library
plugin: software-engineering
aliases: [software-engineering-nextjs-server-client]
tags: [type/skill, plugin/software-engineering, topic/nextjs, topic/architecture]
status: active
---

# Next.js 16 — Server / client architecture

The single most important mental shift in the App Router: **components are server components by default**. You opt into client behavior with `"use client"` at the top of a file.

## The two component types

| | Server component | Client component |
|---|---|---|
| **Default?** | Yes | No — opt in with `"use client"` |
| **Runs where?** | Node.js (server only) | Browser (and during server render for initial HTML) |
| **Can use** | `async/await`, `fetch`, env vars, DB drivers, `cookies()`, `headers()` | `useState`, `useEffect`, event handlers, browser APIs, refs |
| **Can't use** | Hooks, event handlers, browser APIs | `async`, server-only modules, env vars without `NEXT_PUBLIC_` prefix |
| **Bundle impact** | None — never sent to the client | Increases JS bundle |

## The directives

### `"use client"`

Goes at the **top of a file**, before any imports. Marks every export in that file as a client component, **and** every component imported from it transitively, until you cross another `"use client"` boundary.

```tsx
"use client";

import { useState } from "react";

export function QuantitySelector({ max }: { max: number }) {
  const [qty, setQty] = useState(1);
  // ...
}
```

### `"use server"`

Goes at the top of a file (marks all exports as Server Actions) or at the top of a function body (marks just that function). See [[software-engineering-nextjs-actions]] for the deep dive.

## Where to draw the boundary

**Default: server.** Move to client only when you need:

- React state (`useState`, `useReducer`)
- Effects (`useEffect`)
- Browser APIs (`window`, `localStorage`, `IntersectionObserver`)
- Event handlers (`onClick`, `onChange`, etc.)
- Class components / many third-party UI libraries

Everything else — fetching, layout, rendering markup — can and should stay server.

## Composition patterns

### Pattern 1: Push state down (the most important)

Don't put `"use client"` at the top of a big page. Push it as far down the tree as possible.

**Bad** — whole page becomes client; data fetching has to happen via `fetch` in `useEffect`:

```tsx
"use client";
import { useState, useEffect } from "react";

export default function ProductPage() {
  const [product, setProduct] = useState(null);
  useEffect(() => { fetch("/api/...").then(r => r.json()).then(setProduct); }, []);
  const [qty, setQty] = useState(1);
  // ...
}
```

**Good** — page stays server; only the interactive island is client:

```tsx
// app/products/[param]/page.tsx
import { QuantityClient } from "./quantity-client";

export default async function ProductPage({ params }) {
  const { param } = await params;
  const product = await getProduct(param);
  return (
    <main>
      <h1>{product.name}</h1>
      <p>{product.description}</p>
      <QuantityClient max={product.stock} productId={product.id} />
    </main>
  );
}
```

```tsx
// app/products/[param]/quantity-client.tsx
"use client";
import { useState } from "react";

export function QuantityClient({ max, productId }: { max: number; productId: string }) {
  const [qty, setQty] = useState(1);
  // ...
}
```

### Pattern 2: Server components as children of client components

A client component **can** render server components — but only if they're passed in as `children` or props, not imported directly.

```tsx
// providers.tsx (client)
"use client";
import { ThemeProvider } from "next-themes";
export function Providers({ children }: { children: React.ReactNode }) {
  return <ThemeProvider>{children}</ThemeProvider>;
}
```

```tsx
// layout.tsx (server)
import { Providers } from "./providers";
import { ServerHeader } from "./server-header";

export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        <Providers>
          <ServerHeader />   {/* server component as child of client provider */}
          {children}
        </Providers>
      </body>
    </html>
  );
}
```

This pattern lets you wrap the whole tree in client-side providers (theme, analytics, store) without forcing the entire tree to be client.

### Pattern 3: Server-only and client-only guards

To prevent accidental imports across the boundary, use the `server-only` and `client-only` packages:

```ts
// lib/db.ts
import "server-only";
// ... db driver setup
```

If anyone tries to import this file into a client component, the build fails. Same with `client-only` for browser-API-only code.

## Passing data across the boundary

### Props from server → client must be serializable

Anything you pass from a server component to a client component as a prop is serialized. **Functions, classes, Dates, Maps, Sets, etc. don't survive intact.**

Allowed: primitives, plain objects/arrays, `null`, `undefined`, JSX (treated as a special prop). Use server actions to pass functions through.

### Reading the same data on both sides

If both a server component and a client component need the same data, fetch it on the server and pass it down. Don't double-fetch from the client.

## Common errors and how to read them

- **"You're importing a component that needs `useState`. It only works in a Client Component"** → Add `"use client"` to the top of the file with the hook, or import that file from a client component.
- **"Cannot import server-only code into a Client Component"** → Something in a client component's import chain has `import "server-only"`. Find the leak (usually a shared lib file) and split it.
- **"Hydration failed because the initial UI does not match"** → A client component rendered different markup on server vs. client. Causes: random IDs, locale-dependent dates, `Math.random()`, reading `window` during render.

## A typical decision matrix

| Component | Type | Why |
|---|---|---|
| RootLayout, Header, Footer | Server | No interactivity needed; reads cookies for cart/session badges |
| Hero, marketing blocks | Server | Static markup |
| ProductCard | Server | Renders Link + Image |
| QuantitySelector | **Client** | useState for qty |
| Add to Cart button | **Client** (form action) | useFormStatus for pending UI |
| Search input | **Client** | onChange + debounce + router.push |
| Cart line item +/- buttons | Server (inside `<form>`) | Server actions; no useState needed |
| Mobile nav toggle | **Client** | useState for open/closed |

When in doubt: server. Add `"use client"` only when you hit a wall.

## How this skill relates to others

- Routing, layouts, navigation → [[software-engineering-nextjs-routing]]
- Caching, Suspense, streaming → [[software-engineering-nextjs-cache]]
- Server actions and forms → [[software-engineering-nextjs-actions]]
- Errors, loading, not-found → [[software-engineering-nextjs-errors]]

## References

- [Next.js: Server and Client Components](https://nextjs.org/docs/app/getting-started/server-and-client-components)
- [Next.js: Composition patterns](https://nextjs.org/docs/app/getting-started/server-and-client-components)
