---
type: readme
project: skills-library
scope: plugin
plugin: software-engineering
tags: [type/readme, plugin/software-engineering]
status: active
---
# software-engineering

**Architecture, frontend, backend, mobile, AI/ML, DevOps, security, data, design systems, content modeling, and Next.js 16 deep dives**

## Skills (28)

Organized by what you'd reach for first.

### Architecture personas

| Skill | What it does |
|---|---|
| [[software-engineering-composable-architect]] | Composable enterprise architect designing modern, API-first digital platforms. The MACH/composable practice lead. |
| [[software-engineering-cloud-architect]] | Designs scalable, secure, and cost-efficient cloud architectures across AWS, Azure, and GCP. |
| [[software-engineering-backend-architect]] | Designs scalable, maintainable backend systems — API-first design, microservices, event-driven systems, and cloud-native approaches. |

### Build personas

| Skill | What it does |
|---|---|
| [[software-engineering-frontend-developer]] | Builds fast, accessible, responsive web applications using React, TypeScript, and modern tooling. |
| [[software-engineering-mobile-app-developer]] | Builds high-performance, production-ready mobile applications for iOS and Android using native, cross-platform, and hybrid approaches. |
| [[software-engineering-design-system-engineer]] | Builds and maintains production design systems in code using Storybook, shadcn/ui, and Tailwind CSS. |
| [[software-engineering-devops-engineer]] | Designs, automates, and secures modern CI/CD pipelines and infrastructure-as-code across AWS, Azure, and GCP. |
| [[software-engineering-quality-engineer]] | Ensures quality across component-driven, headless digital platforms using Contentful, Next.js, Vercel, and Storybook. |

### AI / data / security

| Skill | What it does |
|---|---|
| [[software-engineering-ai-engineer]] | Designs, trains, and deploys ML systems from research to production — transformers, LLMs, computer vision, forecasting. |
| [[software-engineering-agentic-workflow-engineer]] | Designs, builds, and optimizes intelligent multi-agent AI systems using OpenAI Agents, LangGraph, and Claude's toolchain. |
| [[software-engineering-data-engineer]] | Designs and builds data infrastructure, pipelines, and platforms that enable analytics and AI at scale. |
| [[software-engineering-security-advisor]] | Enterprise cybersecurity advisor for consulting engagements. |
| [[software-engineering-technical-writer]] | Technical documentation architect and developer-experience writer. |

### Next.js 16 deep dives

| Skill | What it does |
|---|---|
| [[software-engineering-nextjs-routing]] | App Router file conventions, dynamic segments, async `params` / `searchParams`, layouts, navigation, route groups, parallel/intercepting routes. |
| [[software-engineering-nextjs-server-client]] | Drawing the `"use client"` boundary, composition patterns, when to push state down, server-only / client-only guards. |
| [[software-engineering-nextjs-cache]] | `"use cache"`, Cache Components, `cacheLife`, `cacheTag`, Suspense placement, streaming, request waterfalls. |
| [[software-engineering-nextjs-actions]] | Server actions, forms, `useActionState`, `useFormStatus`, `useOptimistic`, `revalidatePath` / `revalidateTag`. |
| [[software-engineering-nextjs-errors]] | `error.tsx`, `not-found.tsx`, `loading.tsx`, `notFound()`, segment-level error fallbacks. |
| [[software-engineering-nextjs-performance]] | `next/image`, `next/font`, `next/script`, metadata API, dynamic OG images, Core Web Vitals. |
| [[software-engineering-nextjs-config]] | Env vars + Zod validation, `proxy.ts` (replaces `middleware.ts`), cookie hardening, security headers, pre-deploy review. |

### Task-focused workflows (Next.js + shadcn + Tailwind + Contentful stack)

| Skill | What it does |
|---|---|
| [[software-engineering-nextjs-scaffold]] | Scaffolds Next.js (App Router) pages, layouts, route handlers, server actions, and data-fetching patterns in TypeScript. |
| [[software-engineering-shadcn-component]] | Adds, customizes, and composes shadcn/ui components the right way — install via CLI, theme via CSS variables, extend via wrapping. |
| [[software-engineering-shadcn-registry-author]] | Builds, versions, and distributes a custom shadcn registry — package team-specific components for cross-project install. |
| [[software-engineering-pro-blocks-composer]] | Composes full pages and dashboards from shadcndesign Pro Blocks and equivalent libraries (shadcnblocks, shadcn.io, shadcn studio). |
| [[software-engineering-tailwind-tokens]] | Wires design tokens into Tailwind CSS and CSS variables — color, spacing, typography, radius, elevation, motion. |
| [[software-engineering-component-spec]] | Documents a UI component for a design system — anatomy, props, variants, states, do/don't, accessibility, examples, changelog. |
| [[software-engineering-design-implementation]] | Translates a design artifact (Figma screen, component, or flow) into production-ready code with visual parity and design-system fidelity. |
| [[software-engineering-a11y-audit]] | Audits a screen, component, or flow for WCAG 2.2 AA conformance and remediates the findings — keyboard, focus, semantics, contrast, motion, touch targets. |

> Contentful-specific skills (content modeling, React wiring, GraphQL, migrations, webhooks, personalization, etc.) live in the adjacent **`contentful`** plugin. See [[../contentful/README|the contentful plugin]].

> Vercel-platform skills (deploy pipeline, Fluid Compute, CDN/edge, storage, observability, security, AI SDK, AI Gateway, v0, Sandbox/Agent/Workflows, REST API) live in the adjacent **`vercel`** plugin. See [[../vercel/README|the vercel plugin]].

## Agent

- **`software-engineering-agent`** — Senior engineering practitioner who knows when to use each skill in this plugin and how to sequence them. Default entry point for framework-level engineering work. Hand off Vercel-platform work to `vercel-agent` and Contentful work to `contentful-agent`.

## Commands (4)

Slash commands that orchestrate the seven Next.js skills against repeatable workflows. Each runs through `software-engineering-agent`, hits each concern in the right order, and produces a plan before executing.

| Command | What it does |
|---|---|
| `/nextjs-bootstrap` | Set up a new Next.js 16 project with our conventions baked in from day one — App Router, TS strict, `cacheComponents: true`, `proxy.ts`, env validation, security headers, shadcn + Tailwind, fonts, analytics. Hands off to `vercel-agent` for the platform side when deploying to Vercel. |
| `/nextjs-feature` | Add a new feature with the right cross-skill sequencing — route shape → server/client → caching → actions → errors → performance → config. Produces a plan before any code lands. |
| `/nextjs-upgrade` | Migrate a Next.js 14 or 15 codebase to 16 — codemods (`middleware-to-proxy`, `next-async-request-api`), Cache Components decision, deprecated API sweep, perf-and-security re-verification. |
| `/nextjs-audit` | Audit an existing Next.js 16 codebase against May 2026 best practices — seven checks, scored healthy / warning / critical, with a leverage-ordered fix plan routed to the relevant skill. |

## Shared references

Every skill in this plugin can cite these category-level references at `references/`:

- **`slalom-context.md`** — Slalom organizational context, service pillars, MACH/composable priorities
- **`clean-code-foundations.md`** — Code-level best practices applicable to every engineering skill: meaningful names, function design, comments discipline, formatting, error handling
- **`shadcn-ecosystem.md`** — Full map of the shadcn/ui ecosystem: core components, MCP server, custom registries, pro-blocks libraries, agent skills, and integration patterns
- **`tailwind-semantic-naming.md`** — The project-wide Tailwind convention: utility-first for one-offs, `@layer components` semantic class names with parent-child-grandchild hierarchy for reusable patterns
- **`modern-javascript-essentials.md`** — JavaScript mental model through ES2026 (scope/closures, prototypes, types/coercion, asynchrony, modules, iteration, Set/Map, Temporal, TypeScript fundamentals). Inspired by *YDKJS Yet* (2nd ed.); kept current
- **`architecture-patterns-2026.md`** — When to reach for colocated, feature-based, layered, Clean Architecture, DDD, modular monolith, hexagonal, Atomic Design, traditional MVC, or serverless-first. With the MACH/composable overlay used by Slalom's Composable DXP practice

## Conventions

- Each skill has a `SKILL.md` with YAML frontmatter and structured sections.
- Each skill has a `references/` folder with skill-specific reference files (where present).
- Task-focused workflow skills are written for the Next.js + shadcn + Tailwind + Contentful stack and may not generalize beyond it.
- Tone across all skills: direct, practical, builder-oriented.

## Pairs with

- **`contentful`** — `contentful-content-model`, `contentful-react-wrapper`, `contentful-graphql`, and the rest of the platform-implementation skills. The composable-architect persona here sets system shape; that plugin owns Contentful-specific implementation.
- **`vercel`** — `vercel-deploy-pipeline`, `vercel-fluid-compute`, `vercel-cdn-edge`, `vercel-ai-sdk`, and the rest of the Vercel-platform skills. This plugin owns the framework; that plugin owns the platform.
- **`design`** — `design-handoff` and `design-screen` outputs feed `software-engineering-design-implementation`
- **`cx`** — `cx-information-architect` outputs feed `contentful-content-model` (in the contentful plugin)
- **`project-management`** — sprint and user-story outputs drive engineering execution
- **`consulting`** — `consulting-management-consultant` and architecture personas pair on engagement structure

## Part of the composable-dxp marketplace

See the [marketplace README](../README.md) for the full architecture.

## License

MIT.
