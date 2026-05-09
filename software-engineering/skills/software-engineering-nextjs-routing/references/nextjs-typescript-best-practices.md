---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
skill: software-engineering-nextjs-routing
tags: [type/reference, plugin/software-engineering, topic/nextjs, topic/typescript]
status: active
last_reviewed: 2026-05-09
---

# Next.js + TypeScript best practices (May 2026)

Working conventions for the Next.js 16 + TypeScript stack. The fundamentals (typing components, contexts, API responses) are covered in any "intro to Next.js + TS" article — this reference focuses on the patterns that matter at the senior level and the specific shapes the App Router expects.

For deeper JS / TS mental models, see the plugin-level `references/modern-javascript-essentials.md`.

## Project setup

Default to TypeScript strict mode. The `create-next-app --typescript` flag does this; verify in `tsconfig.json`:

```jsonc
{
  "compilerOptions": {
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "noImplicitOverride": true,
    "moduleResolution": "Bundler",
    "module": "ESNext",
    "target": "ES2024",
    "lib": ["dom", "dom.iterable", "ES2024"],
    "jsx": "preserve",
    "paths": { "@/*": ["./src/*"] }
  }
}
```

`noUncheckedIndexedAccess: true` is the highest-leverage flag — it forces you to handle `undefined` from array/object index access. Catches a class of bugs at compile time.

## Typing pages, layouts, and route handlers

### Page with async params and searchParams (Next.js 16)

```ts
// app/products/[param]/page.tsx
import type { Metadata } from "next";

type Props = {
  params: Promise<{ param: string }>;
  searchParams: Promise<{ ref?: string }>;
};

export async function generateMetadata({ params }: Props): Promise<Metadata> {
  const { param } = await params;
  const product = await getProduct(param);
  return { title: product.name };
}

export default async function Page({ params, searchParams }: Props) {
  const { param } = await params;
  const { ref } = await searchParams;
  // ...
}
```

Don't use `NextPage` — it's deprecated in App Router contexts. Type pages directly with the `Props` shape.

### Layout

```ts
// app/(marketing)/layout.tsx
type Props = {
  children: React.ReactNode;
  params: Promise<{ /* shared dynamic segments */ }>;
};

export default async function MarketingLayout({ children }: Props) {
  return <div className="...">{children}</div>;
}
```

Layouts never receive `searchParams`. If TypeScript shows it as a prop somewhere, you've grabbed the wrong type.

### Route handler

```ts
// app/api/products/route.ts
import { type NextRequest, NextResponse } from "next/server";

export async function GET(request: NextRequest) {
  const q = request.nextUrl.searchParams.get("q");
  const products = await searchProducts(q ?? "");
  return NextResponse.json(products);
}

export async function POST(request: NextRequest) {
  const body = await request.json();
  // validate with Zod
  return NextResponse.json({ ok: true });
}
```

Use `NextRequest` / `NextResponse` (from `next/server`), not the older `NextApiRequest` / `NextApiResponse` (those are Pages Router).

## Server Actions: type-safe forms

```ts
// app/(account)/actions.ts
"use server";

import { z } from "zod";

const UpdateProfileSchema = z.object({
  displayName: z.string().min(1).max(80),
  email: z.string().email(),
});

export type UpdateProfileResult =
  | { ok: true }
  | { ok: false; fieldErrors: Record<string, string[]> };

export async function updateProfileAction(
  _prev: UpdateProfileResult | null,
  formData: FormData,
): Promise<UpdateProfileResult> {
  const parsed = UpdateProfileSchema.safeParse(Object.fromEntries(formData));
  if (!parsed.success) {
    return { ok: false, fieldErrors: parsed.error.flatten().fieldErrors };
  }
  await persist(parsed.data);
  return { ok: true };
}
```

Pair with `useActionState` on the client:

```tsx
"use client";
import { useActionState } from "react";
import { updateProfileAction, type UpdateProfileResult } from "./actions";

const [state, action, pending] = useActionState<UpdateProfileResult | null, FormData>(
  updateProfileAction,
  null,
);
```

The discriminated union (`ok: true` vs `ok: false`) gives you exhaustive narrowing in JSX. Avoid generic `Result<T, E>` types when a domain-specific shape with named fields reads better.

## Validate at every trust boundary with Zod

Three places where input crosses from "client/world" to "your code":

1. **Server actions** — every `formData.get(...)` is `unknown`. Run through Zod.
2. **Route handlers** — `request.json()` returns `unknown`. Run through Zod.
3. **External API responses** — the `fetch` you trusted last quarter changed shape this quarter. Run through Zod.

Define schemas in a `schemas.ts` co-located with the action, or in `lib/schemas/` if reused. Export the inferred types:

```ts
export const Product = z.object({
  id: z.string(),
  name: z.string(),
  price: z.number().int().nonnegative(),
});
export type Product = z.infer<typeof Product>;
```

Then `import type { Product } from "@/lib/schemas"` everywhere — single source of truth, runtime + compile-time agreement.

## Discriminated unions for state

Whenever the same value space has two shapes (loading vs loaded, ok vs error, server vs client), reach for a discriminated union:

```ts
type Async<T> =
  | { status: "idle" }
  | { status: "loading" }
  | { status: "success"; data: T }
  | { status: "error"; error: Error };
```

In JSX: `if (state.status === "success") { state.data /* narrowed */ }`. The `default:` branch in a switch becomes `assertNever(state)`, giving you an exhaustiveness check.

## `unknown` over `any`

`any` disables type checking. `unknown` requires you to narrow before use. In a 2026 codebase, **`any` should appear only as an annotated escape hatch** with a comment explaining why.

```ts
// Bad
function parseConfig(raw: any) { return raw.entries; }

// Good
function parseConfig(raw: unknown) {
  if (typeof raw === "object" && raw !== null && "entries" in raw) {
    return (raw as { entries: unknown }).entries;
  }
  throw new Error("Invalid config");
}

// Better: validate with Zod
function parseConfig(raw: unknown) { return ConfigSchema.parse(raw); }
```

## `as const` and `satisfies`

`as const` narrows literal types to their literal value:

```ts
const ROUTES = ["/", "/products", "/cart"] as const;
type Route = typeof ROUTES[number]; // "/" | "/products" | "/cart"
```

`satisfies` validates without widening:

```ts
const COLORS = {
  primary: "#0070f3",
  danger: "#e00",
  success: "#0a0",
} satisfies Record<string, `#${string}`>;

// Type of COLORS.primary is still the literal "#0070f3", not string
```

Use `satisfies` for config objects you want to type-check without losing literal narrowness. Use `as const` for arrays/objects you want fully readonly.

## Type-only imports

```ts
import type { Metadata } from "next";
import { type NextRequest, NextResponse } from "next/server";
```

Type-only imports are erased at compile time, so they never affect bundle size. The `verbatimModuleSyntax: true` tsconfig flag enforces this.

## Avoid: `React.FC`

```ts
// Don't
const Greeting: React.FC<{ name: string }> = ({ name }) => <div>{name}</div>;

// Do
function Greeting({ name }: { name: string }) {
  return <div>{name}</div>;
}
```

`React.FC` adds an implicit `children` prop in older React types, makes generics awkward, and isn't needed for inference. Plain function declarations read better and have no caveats.

## Optional chaining and nullish coalescing

```ts
const username = user?.profile?.displayName ?? "Guest";
const ttl = config.ttlSeconds ?? 60;
```

Use `?.` and `??` over `&&` / `||` for null/undefined defaults. `||` treats `0` and `""` as falsy, which is rarely what you want.

## Type-safe testing

```ts
import { render, screen } from "@testing-library/react";
import { Greeting } from "./Greeting";

test("renders greeting with name", () => {
  render(<Greeting name="Alice" />);
  expect(screen.getByText(/Hello, Alice/)).toBeInTheDocument();
});
```

Run tests against the same `tsconfig.json` strictness as production code — no looser config in tests.

## Type-safe environment variables

See the plugin-level pattern in `software-engineering-nextjs-config`. Boilerplate:

```ts
// lib/env.ts
import { z } from "zod";

const env = z.object({
  NEXT_PUBLIC_API_URL: z.string().url(),
  DATABASE_URL: z.string().url(),
}).parse(process.env);

export default env;
```

Import `env` everywhere instead of touching `process.env` directly. Build fails on missing or malformed config.

## Common failure modes

- **Typing `params` as a plain object** — in Next.js 16 it's a `Promise<T>`. The compiler flags it, but only if you've updated `next` to a version that ships the new types.
- **Reaching for `React.FC` or `NextPage`** — neither belongs in a 2026 App Router codebase.
- **`any` to silence the compiler** — almost always means a missing Zod schema at a trust boundary.
- **Mutating arrays/objects in React state** — TS lets you, React doesn't notice the change. Default to immutable patterns.
- **Forgetting `import type`** — type-only imports prevent bundle bloat from runtime imports of pure-type modules.

## References

- [Next.js TypeScript docs](https://nextjs.org/docs/app/api-reference/config/typescript)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html)
- [tkdodo/blog: TypeScript posts](https://tkdodo.eu/blog/all) — high-signal patterns for React + TS
- Plugin-level `references/modern-javascript-essentials.md` for the language-level fundamentals
