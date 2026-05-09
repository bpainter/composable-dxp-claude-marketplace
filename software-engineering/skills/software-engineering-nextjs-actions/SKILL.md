---
name: software-engineering-nextjs-actions
description: Use this skill any time you're handling form submissions, mutations, or any user-triggered server-side work in a Next.js 16 app. Triggers include "use server", Server Actions, action prop on a form, FormData, useFormStatus, useActionState, useFormState, revalidatePath, revalidateTag, redirect from next/navigation, optimistic updates with useOptimistic, or any "submit", "save", "delete", "update" interaction. Also use when wiring buttons that need to mutate server state (add to cart, like, follow, vote).

# Project context
type: skill
project: skills-library
plugin: software-engineering
aliases: [software-engineering-nextjs-actions]
tags: [type/skill, plugin/software-engineering, topic/nextjs, topic/forms, topic/mutations]
status: active
---

# Next.js 16 — Server Actions and forms

Server Actions are how you mutate server state from the UI **without writing API routes**. They're the canonical way to handle forms, button-driven mutations, and any user action that should trigger server work.

## What a Server Action is

An async function that:
- Has `"use server"` either at the top of its file or as the first statement in its body.
- Runs **only on the server**, even when invoked from a client component.
- Can be passed as the `action` prop to a `<form>`, called from `formAction`, or invoked from a click handler.
- Returns serializable data back to the client.

## Two ways to declare

### File-level (the default for shared actions)

```ts
// app/cart/actions.ts
"use server";

import { revalidatePath } from "next/cache";
import { cookies } from "next/headers";

export async function addToCartAction(formData: FormData) {
  const productId = String(formData.get("productId"));
  const quantity = Number(formData.get("quantity"));

  const token = await ensureCartToken();
  await fetch(`${API}/cart`, {
    method: "POST",
    headers: {
      Authorization: `Bearer ${process.env.API_TOKEN}`,
      "x-cart-token": token,
      "Content-Type": "application/json",
    },
    body: JSON.stringify({ productId, quantity }),
  });

  revalidatePath("/cart");
  revalidatePath("/", "layout"); // refresh the header badge
}
```

### Inline (for one-off actions co-located with a server component)

```tsx
export default async function ProductPage({ params }) {
  const { param } = await params;
  const product = await getProduct(param);

  async function addOne() {
    "use server";
    await addItem(product.id, 1);
    revalidatePath("/cart");
  }

  return <form action={addOne}><button>Quick add</button></form>;
}
```

## Wiring forms

The vanilla, no-JS-required pattern:

```tsx
import { addToCartAction } from "./actions";

export function AddToCartForm({ productId }: { productId: string }) {
  return (
    <form action={addToCartAction}>
      <input type="hidden" name="productId" value={productId} />
      <input type="number" name="quantity" defaultValue={1} min={1} />
      <button type="submit">Add to cart</button>
    </form>
  );
}
```

This works **without any client-side JavaScript**. If JS loads, Next.js progressively enhances to avoid a full navigation.

## Pending UI

Use `useFormStatus` from `react-dom` for per-form pending state:

```tsx
"use client";
import { useFormStatus } from "react-dom";

export function SubmitButton({ children }: { children: React.ReactNode }) {
  const { pending } = useFormStatus();
  return (
    <button type="submit" disabled={pending}>
      {pending ? "Adding..." : children}
    </button>
  );
}
```

`useFormStatus` only works **inside** a `<form>`. The button itself must be a client component, but the form can be rendered from a server component.

## Returning data and validation errors

Use `useActionState` (formerly `useFormState`) to get the action's return value:

```ts
// actions.ts
"use server";

import { z } from "zod";

const Schema = z.object({
  quantity: z.coerce.number().int().min(1).max(99),
  productId: z.string().min(1),
});

export type State = { ok: true } | { ok: false; fieldErrors: Record<string, string[]> };

export async function addToCartAction(_prev: State | null, formData: FormData): Promise<State> {
  const parsed = Schema.safeParse(Object.fromEntries(formData));
  if (!parsed.success) {
    return { ok: false, fieldErrors: parsed.error.flatten().fieldErrors };
  }
  // ... do work
  return { ok: true };
}
```

```tsx
"use client";
import { useActionState } from "react";
import { addToCartAction, type State } from "./actions";

export function Form() {
  const [state, action, pending] = useActionState<State | null, FormData>(addToCartAction, null);
  return (
    <form action={action}>
      <input name="quantity" />
      {state?.ok === false && state.fieldErrors.quantity?.[0] && (
        <p className="text-red-500">{state.fieldErrors.quantity[0]}</p>
      )}
      <SubmitButton>Add</SubmitButton>
    </form>
  );
}
```

## Calling actions from non-form click handlers

For "Remove" buttons or "+/-" steppers where a `<form>` feels heavy:

1. **Wrap each button in its own tiny form:**
   ```tsx
   <form action={removeItemAction.bind(null, item.id)}>
     <button>Remove</button>
   </form>
   ```

2. **Or use `formAction`** on a button inside an outer form:
   ```tsx
   <button formAction={removeItemAction.bind(null, item.id)}>Remove</button>
   ```

3. **Or call from an `onClick` in a client component**:
   ```tsx
   "use client";
   import { removeItemAction } from "./actions";
   const onClick = async () => { await removeItemAction(item.id); };
   ```

## Revalidation: making the UI catch up

After a mutation, you almost always need to invalidate cached UI. Two functions:

- `revalidatePath(path, type?)` — invalidates by URL. `type` is `"layout"` or `"page"` (default).
- `revalidateTag(tag)` — invalidates anything tagged with that string via `cacheTag()`.

```ts
revalidatePath("/cart");                // /cart route's data
revalidatePath("/", "layout");          // any layout that shows cart count
```

Or with tags (cleaner if you tag your fetches):

```ts
revalidateTag(`cart:${cartToken}`);
```

## Redirect after mutation

```ts
import { redirect } from "next/navigation";

export async function checkoutAction() {
  "use server";
  const orderId = await createOrder();
  redirect(`/orders/${orderId}`);
}
```

`redirect()` throws internally — it must be the last statement in your action.

## Optimistic updates

For instant feedback before the server responds:

```tsx
"use client";
import { useOptimistic } from "react";

export function CartItem({ item }: { item: CartItem }) {
  const [optimisticQty, setOptimisticQty] = useOptimistic(item.quantity);
  async function update(newQty: number) {
    setOptimisticQty(newQty);
    await updateQuantityAction(item.id, newQty);
  }
  return <span>{optimisticQty}</span>;
}
```

Use sparingly. The basic `useFormStatus` pending UI covers most cases.

## Security checklist

Server Actions are public endpoints, even when you didn't define a route. Treat every action as untrusted input:

1. **Validate every field.** Use Zod or manual checks. Don't trust `formData.get("quantity")`.
2. **Authorize before mutating.** Check session, scope to the current user.
3. **Don't return secrets.** The return value is shipped to the client.
4. **Don't accept arbitrary IDs without scoping.** A `removeItemAction(itemId)` should only remove items in *this* user's cart.

## Common mistakes

- Skipping Zod / manual validation because the form had `min`/`max` HTML attributes — clients lie.
- Returning the action result by mutating outer state instead of using `useActionState`.
- Forgetting to `revalidatePath` after a mutation, then debugging "why is the UI stale" for an hour.
- Using `redirect()` mid-function — it must be the last statement.
- Sending secrets back from the action so the client can "use them" — they're now in the page source.

## How this skill relates to others

- Form layout, server vs client structure → [[software-engineering-nextjs-server-client]]
- Caching the data the action invalidates → [[software-engineering-nextjs-cache]]
- Validation patterns and Zod schemas → see `software-engineering-nextjs-routing/references/nextjs-typescript-best-practices.md`
- Auth and cookie hardening → [[software-engineering-nextjs-config]]

## References

- [Next.js: Updating data with Server Actions](https://nextjs.org/docs/app/getting-started/updating-data)
- [Next.js: revalidatePath](https://nextjs.org/docs/app/api-reference/functions/revalidatePath)
- [React: useActionState](https://react.dev/reference/react/useActionState)
- [React: useOptimistic](https://react.dev/reference/react/useOptimistic)
