---
name: software-engineering-nextjs-scaffold
description: Scaffold Next.js (App Router) pages, layouts, route handlers, server actions, and data-fetching patterns in TypeScript, matching the project conventions (Vercel deploy target, Tailwind, shadcn/ui, future Contentful integration). Use this skill any time the user wants to add a new page, route, layout, server action, API endpoint, dynamic segment, or data-fetching pattern, even when they say "create a new page," "add a route," "wire up a form submission," or "fetch X from the server." Trigger whenever new Next.js surface area is being added so the result lands in the right shape on first try.

# Project context
type: skill
project: skills-library
plugin: software-engineering
aliases: [software-engineering-nextjs-scaffold]
tags: [type/skill, plugin/software-engineering topic/software, topic/engineering]
status: active
---

# Next.js Scaffold

This skill produces Next.js App Router code in the shape the project expects. The point is to skip the "where should this go and what does it look like" debate every time a new page or route is added.

Defaults assumed (override if user says otherwise):
- Next.js 14+ with App Router
- TypeScript, strict mode
- Tailwind CSS for styling, shadcn/ui for primitives
- Vercel as deploy target
- Server Components by default; Client Components only when interactivity demands it
- Contentful as the eventual content source (Phase 2); Phase 1 content is co-located markdown or static data

## When to use this skill

- New page, route group, or layout.
- New server action.
- New route handler (`route.ts`) for an API endpoint.
- New dynamic route or catch-all.
- A form, fetch, or mutation that the user is about to write inline in a component.
- Re-shaping an existing route to match conventions.

## File and folder conventions

```
src/
  app/
    (marketing)/                # route group for unauthenticated marketing pages
      page.tsx                  # home
      layout.tsx
      formation/
        page.tsx
        [slug]/
          page.tsx
    (tools)/                    # route group for interactive tools
      formation-questionnaire/
        page.tsx
        actions.ts              # server actions
        _components/            # local-only components
      pro-forma/
        page.tsx
    api/
      health/
        route.ts
    layout.tsx                  # root layout
    not-found.tsx
    error.tsx
  components/
    ui/                         # shadcn/ui primitives (do not edit by hand)
    blocks/                     # composed marketing blocks (Hero, FAQ, etc.)
    forms/                      # reusable form patterns
  lib/
    contentful/                 # client + types (Phase 2)
    seo/                        # metadata helpers, schema generators
    utils.ts                    # cn(), etc.
  content/                      # Phase 1: co-located markdown and static data
  styles/
    globals.css
```

Conventions:
- **Underscore-prefixed folders** (e.g., `_components`) are private and not routable. Use them for components scoped to a single route.
- **Route groups** in parentheses (e.g., `(marketing)`) organize without affecting the URL.
- **Co-locate** server actions in an `actions.ts` next to the page that uses them, unless they are reused — then promote to `lib/`.

## Page template (Server Component default)

```tsx
// src/app/(marketing)/formation/page.tsx
import type { Metadata } from "next";
import { buildPageMetadata } from "@/lib/seo/metadata";

export const metadata: Metadata = buildPageMetadata({
  title: "Formation",
  description: "Plain-English guidance for forming your company.",
  path: "/formation",
});

export default async function FormationPage() {
  // Server Component: fetch data here directly. No useEffect.
  // const data = await getFormationOverview();

  return (
    <main className="mx-auto max-w-3xl px-6 py-16">
      <h1 className="text-4xl font-semibold tracking-tight">Formation</h1>
      <p className="mt-4 text-lg text-muted-foreground">
        {/* Lead with the answer (see founders-workbench-context) */}
      </p>
      {/* Sections */}
    </main>
  );
}
```

Rules:
- Default to `async` Server Components. Fetch data inline.
- Export `metadata` (or `generateMetadata` for dynamic routes) on every page.
- Use `@/` path alias, configured in `tsconfig.json`.
- No `"use client"` unless the file actually needs `useState`, `useEffect`, browser APIs, or event handlers.

## Dynamic route template

```tsx
// src/app/(marketing)/formation/[slug]/page.tsx
import type { Metadata } from "next";
import { notFound } from "next/navigation";
import { buildPageMetadata } from "@/lib/seo/metadata";

type Props = { params: { slug: string } };

export async function generateStaticParams() {
  // Phase 1: read from local content/. Phase 2: query Contentful.
  return [{ slug: "delaware-c-corp" }];
}

export async function generateMetadata({ params }: Props): Promise<Metadata> {
  const article = await getArticle(params.slug);
  if (!article) return {};
  return buildPageMetadata({
    title: article.title,
    description: article.summary,
    path: `/formation/${params.slug}`,
  });
}

export default async function ArticlePage({ params }: Props) {
  const article = await getArticle(params.slug);
  if (!article) notFound();

  return (
    <article className="mx-auto max-w-3xl px-6 py-16">
      <h1>{article.title}</h1>
      {/* render article body */}
    </article>
  );
}
```

## Layout template

```tsx
// src/app/(marketing)/layout.tsx
import { SiteHeader } from "@/components/blocks/site-header";
import { SiteFooter } from "@/components/blocks/site-footer";

export default function MarketingLayout({ children }: { children: React.ReactNode }) {
  return (
    <div className="flex min-h-screen flex-col">
      <SiteHeader />
      <div className="flex-1">{children}</div>
      <SiteFooter />
    </div>
  );
}
```

## Server Action template

```tsx
// src/app/(tools)/formation-questionnaire/actions.ts
"use server";

import { z } from "zod";
import { redirect } from "next/navigation";

const QuestionnaireSchema = z.object({
  founderName: z.string().min(1),
  email: z.string().email(),
  jurisdiction: z.enum(["DE", "CA", "NY", "OTHER"]),
  // ...
});

export type QuestionnaireResult =
  | { ok: true }
  | { ok: false; fieldErrors: Record<string, string[]> };

export async function submitQuestionnaire(
  _prev: QuestionnaireResult | null,
  formData: FormData,
): Promise<QuestionnaireResult> {
  const parsed = QuestionnaireSchema.safeParse(Object.fromEntries(formData));
  if (!parsed.success) {
    return { ok: false, fieldErrors: parsed.error.flatten().fieldErrors };
  }

  // Persist or forward. Phase 1: simple email/webhook. Phase 2: real backend.
  // await persist(parsed.data);

  redirect("/formation-questionnaire/thanks");
}
```

Rules:
- `"use server"` at the top of the file (or directive on the function).
- Validate every input with Zod. Never trust `FormData` shape.
- Return discriminated unions for client-side `useFormState` / `useActionState`.
- `redirect()` and `revalidatePath()` are imports, not return values.

## Client form bound to a Server Action

```tsx
// src/app/(tools)/formation-questionnaire/_components/questionnaire-form.tsx
"use client";

import { useActionState } from "react";
import { submitQuestionnaire } from "../actions";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";

export function QuestionnaireForm() {
  const [state, formAction, pending] = useActionState(submitQuestionnaire, null);

  return (
    <form action={formAction} className="space-y-4">
      <Input name="founderName" placeholder="Your name" />
      {state?.ok === false && state.fieldErrors.founderName && (
        <p className="text-sm text-destructive">{state.fieldErrors.founderName[0]}</p>
      )}
      <Input name="email" type="email" placeholder="Email" />
      <Button type="submit" disabled={pending}>
        {pending ? "Submitting…" : "Submit"}
      </Button>
    </form>
  );
}
```

## Route handler template (`route.ts`)

```ts
// src/app/api/health/route.ts
import { NextResponse } from "next/server";

export async function GET() {
  return NextResponse.json({ ok: true, ts: Date.now() });
}
```

Use route handlers for:
- Webhooks (Contentful publish, form submission relay).
- Health checks.
- Public JSON endpoints not consumed by the same Next.js app.

Do **not** use route handlers as a glorified data layer for your own pages — Server Components fetch data directly.

## Metadata helper

Centralize metadata composition in `src/lib/seo/metadata.ts` so OG, Twitter, canonical, and title-template logic isn't repeated per page. This is also where JSON-LD generation lives (see `strategy-geo-optimization`).

```ts
// src/lib/seo/metadata.ts
import type { Metadata } from "next";

const SITE_NAME = "{Project Name}";
const SITE_URL = "https://{project-domain}";

export function buildPageMetadata(args: {
  title: string;
  description: string;
  path: string;
}): Metadata {
  const url = `${SITE_URL}${args.path}`;
  return {
    title: { default: args.title, template: `%s · ${SITE_NAME}` },
    description: args.description,
    alternates: { canonical: url },
    openGraph: { title: args.title, description: args.description, url, siteName: SITE_NAME, type: "article" },
    twitter: { card: "summary_large_image", title: args.title, description: args.description },
  };
}
```

## Caching and revalidation

- Default in App Router: pages are statically rendered when possible.
- Use `export const revalidate = 3600` (seconds) on a page to opt into ISR.
- For Contentful-backed pages (Phase 2), prefer `revalidateTag()` from a webhook over time-based revalidation.
- Use `dynamic = "force-dynamic"` only when the page genuinely depends on per-request data (cookies, headers).

## Error and not-found

Every route group should have an `error.tsx` (Client Component, must be) and `not-found.tsx`. Don't let a thrown error fall through to Next's default page in production.

```tsx
// src/app/(marketing)/error.tsx
"use client";

export default function Error({ error, reset }: { error: Error; reset: () => void }) {
  return (
    <div className="mx-auto max-w-2xl px-6 py-24 text-center">
      <h1 className="text-2xl font-semibold">Something went wrong.</h1>
      <p className="mt-2 text-muted-foreground">{error.message}</p>
      <button className="mt-6 underline" onClick={reset}>Try again</button>
    </div>
  );
}
```

## Vercel-specific notes

- Set `NEXT_PUBLIC_SITE_URL` in env so `metadata` can build absolute URLs in preview vs. prod.
- Use Vercel's "Edge Config" only for true site-wide flags. Don't use it as a database.
- Image optimization is on by default; configure `images.remotePatterns` in `next.config.mjs` for Contentful's CDN.

## Common failure modes

- Reaching for a route handler when a Server Component fetch would do.
- Marking entire pages `"use client"` because one button needed `onClick`. Push the `"use client"` boundary down to the smallest interactive leaf.
- Validating only on the client side. Always validate in the server action.
- Forgetting `generateStaticParams` on dynamic routes that should be statically rendered.
- Hand-authoring per-page `<head>` tags instead of using the metadata API.
- Inline JSON-LD instead of a centralized helper. See `strategy-geo-optimization`.

## How this skill relates to others

- shadcn/ui primitives and theming → `software-engineering-shadcn-component`.
- Tailwind tokens and theming → forthcoming `software-engineering-tailwind-tokens` skill.
- Content modeling and the Contentful-side wiring → forthcoming `dev-contentful-model` and `dev-contentful-wrapper` skills.
- SEO metadata and structured data → `strategy-seo-brief`, `strategy-geo-optimization`.
