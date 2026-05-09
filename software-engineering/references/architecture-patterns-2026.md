---
type: reference
project: skills-library
scope: plugin
plugin: software-engineering
tags: [type/reference, plugin/software-engineering, topic/architecture]
status: active
inspired_by: "AmiirRahimi/architecture-patterns-collection (10 Next.js patterns)"
last_reviewed: 2026-05-09
---

# Architecture patterns (2026)

A senior engineer's working catalog of the patterns we actually reach for in 2026 web and platform work. Inspired by the [architecture-patterns-collection](https://github.com/AmiirRahimi/architecture-patterns-collection) reference repo, updated for Next.js 16, the composable / MACH stack, edge-first runtimes, and AI-augmented apps.

The point isn't to memorize patterns. It's to recognize **what problem each pattern solves**, **what it costs**, and **when it's the right shape** for the system in front of you. Most production codebases blend two or three of these.

## How to choose

Start with three questions:

1. **What is the system's volatility profile?** Frequent feature changes, slow business-rule changes, both? Match boundaries to the things that change at the same rate.
2. **What is the team shape?** Single squad / multi-squad / external integrators all push you to different boundaries.
3. **What is the deploy and scale model?** Edge-first serverless behaves differently from a Kubernetes monolith.

If two patterns seem to fit, pick the simpler one and earn the more complex pattern when the seams demand it.

## The catalog

### 1. Colocated structure (the simple default)

**Shape**: Files grouped by route or page; everything related lives next to the page that uses it. The Next.js App Router strongly encourages this with `_components/`, `actions.ts`, `loading.tsx` co-located inside route folders.

**Use when**:
- Small to medium apps (< ~50 routes).
- Single team, fast iteration, prototyping.
- The cost of hunting through layers exceeds the cost of occasional duplication.

**Avoid when**:
- Multiple teams own different parts of the system.
- The same domain logic is needed across unrelated routes.

**In Next.js**: this is the default. Reach for something else only when you feel actual pain.

### 2. Feature-based structure

**Shape**: Top-level folders by business feature (`features/checkout/`, `features/search/`), each holding its UI, logic, types, and tests. Routes import from features.

```
src/
  app/                  ← thin route shells
  features/
    checkout/
      components/
      lib/
      schemas.ts
      actions.ts
    search/
```

**Use when**:
- Multiple teams, each owning one or more features.
- The product roadmap is organized by feature.
- You want strong code locality and team autonomy without going as heavy as DDD.

**Avoid when**:
- The team is too small for the overhead.
- Your "features" are really just routes — colocated is fine.

**In Next.js 16**: pairs well with the App Router. The `app/` tree stays a thin routing layer; real code lives in `features/`. Route handlers and server actions become 5-line shells delegating to feature modules.

### 3. Layered architecture

**Shape**: Horizontal layers — Presentation → Application → Domain → Infrastructure — with strict directional dependencies (lower layers don't know about upper).

```
src/
  presentation/   ← UI components, route handlers
  application/    ← use cases, orchestration
  domain/         ← entities, value objects, business rules
  infrastructure/ ← DB clients, HTTP clients, caches
```

**Use when**:
- Long-lived enterprise systems.
- Business rules are complex and need to be insulated from framework churn.
- A regulated environment expects clear separation for audit.

**Avoid when**:
- The system is mostly CRUD — you'll write four files where one would do.
- The team isn't disciplined about respecting layer boundaries (it becomes archaeology).

### 4. Clean Architecture (Uncle Bob's variant)

**Shape**: Layered architecture taken to its logical extreme. Domain at the center, framework at the edge, dependency inversion enforced everywhere via interfaces.

```
domain/         ← entities, no framework knowledge
use-cases/      ← orchestration, depends only on domain
adapters/       ← interface implementations (DB, HTTP, UI)
frameworks/     ← Next.js, Express, Prisma — outermost ring
```

**Use when**:
- Multi-decade systems where the framework will outlive Next.js.
- Heavy testing pressure — pure domain logic with mocked adapters is trivial to test.
- Multiple delivery surfaces (web + mobile + CLI) sharing a domain.

**Avoid when**:
- You're shipping a marketing site or a B2B SaaS in <12 months. Overkill.
- The team hasn't internalized dependency inversion.

**Cost**: roughly 3× the file count of a colocated app. Earn it.

### 5. Domain-Driven Design (DDD)

**Shape**: Organize by bounded context (Order, Inventory, Catalog), with rich domain models, ubiquitous language, and explicit aggregate roots. Adjacent contexts communicate via published contracts (events or APIs).

**Use when**:
- Complex business domain — multiple stakeholders, deep business rules, language that confuses outsiders.
- You're modeling a real industry process (insurance claims, freight, healthcare encounters).
- The team is large enough that bounded contexts map to actual squads.

**Avoid when**:
- The domain is genuinely simple. CRUD-with-rules doesn't need DDD.
- You don't have access to subject-matter experts to model the language with.

**In 2026**: pairs naturally with event-driven and serverless backends. The "modular monolith" pattern below is often DDD without the microservices.

### 6. Modular monolith

**Shape**: A single deployable, but internally structured as independent modules with their own data, logic, and explicit module APIs. Modules don't share databases or import each other's internals.

```
modules/
  catalog/
    public-api.ts   ← the only thing other modules import
    internal/
    db.ts
  cart/
    public-api.ts
    internal/
```

**Use when**:
- You want microservice-style boundaries without microservice-style deployment overhead.
- A future split into services is plausible but not imminent.
- Multiple teams, but operations is one team and they want a single deploy unit.

**Avoid when**:
- The team is too small to maintain module discipline.
- You truly need independent deploy or scale per module — go to microservices.

**In 2026**: this is the dominant pattern for serious B2B SaaS. Cheaper than microservices, cleaner than a big ball of mud.

### 7. Hexagonal (Ports and Adapters)

**Shape**: Domain at the center; everything that talks to the outside world is an "adapter" implementing a "port" (interface) the domain defines.

```
domain/                       ← business logic, knows no IO
ports/                        ← interfaces: ProductRepo, EmailSender
adapters/
  http/                       ← Express, Next route handlers
  persistence/postgres.ts     ← implements ProductRepo
  email/sendgrid.ts           ← implements EmailSender
```

**Use when**:
- You expect to swap infra (DB, message broker, email) without touching domain.
- Heavy integration testing — adapters can be replaced with in-memory test doubles.
- Microservices where the wire protocol may change.

**Avoid when**:
- The "adapter" you're protecting against will never realistically change.
- The team treats interfaces as a checkbox rather than a discipline.

### 8. Atomic Design

**Shape**: A taxonomy for UI components: Atoms → Molecules → Organisms → Templates → Pages. Strictly a *component-organization* pattern, not a system architecture.

```
components/
  atoms/        ← Button, Input, Label
  molecules/    ← FormField, SearchBox
  organisms/    ← Header, ProductCard
  templates/    ← page layouts
  pages/        ← route-level composition
```

**Use when**:
- You're building a design system that needs to scale.
- Multiple teams need a shared mental model for component reuse.
- A design tool (Figma, etc.) already organizes this way and you want code parity.

**Avoid when**:
- The "atomic" boundaries cause more arguments than clarity ("is `LoginForm` an organism or a template?").
- A flat structure with shadcn-style primitives is enough.

**In 2026**: most teams have moved toward "primitives + composed blocks + page assemblies" — a flatter version of Atomic Design. shadcn/ui pushes this hard.

### 9. Traditional MVC

**Shape**: Model-View-Controller separation. Controllers handle requests, Models hold data and business logic, Views render output.

**Use when**:
- The system maps cleanly to CRUD operations.
- You're working in a framework that defaults to MVC (Rails, Laravel).
- The team is fluent in MVC and the cost of switching mental models exceeds the gain.

**Avoid when**:
- You're in Next.js / React / modern component-driven UI. MVC was designed before reactive UI; it doesn't translate cleanly.
- Models become "fat" or controllers become "thick."

**In 2026 Next.js**: don't reach for this. It misshapes naturally for the App Router.

### 10. Serverless-first / event-driven

**Shape**: Functions are the primary unit of compute, triggered by HTTP, queue messages, schedules, or domain events. State lives in managed stores (KV, blob, document DB). Inter-function communication is async via queues or event buses.

**Use when**:
- Variable, spiky load — pay only for what you use.
- The team is small and doesn't want to operate Kubernetes.
- The product is naturally event-driven (notifications, webhooks, ETL, async fanout).

**Avoid when**:
- Cold starts will hurt UX and you can't tolerate them (long-running connections, ML inference with large models).
- The function count grows beyond what a small team can keep mental track of without a good catalog.

**In 2026**: Vercel **Fluid Compute** (single function instance handling many concurrent requests) collapsed much of the cold-start objection for the Vercel platform. AWS Lambda SnapStart and similar tech does the same elsewhere. Serverless-first is the right default for new web work unless you have a clear reason against it.

## Patterns we didn't list (and why)

- **Onion Architecture** — same animal as Clean Architecture, different vendor. Pick one vocabulary and stick to it.
- **Three-tier architecture** — superseded by layered + serverless or layered + modular monolith for new work.
- **Microservices** — a deployment topology more than an architecture pattern. Reach for it when team boundaries and deploy independence demand it; not as a default. The decade-long lesson: most "microservices" should have been a modular monolith.

## Composable / MACH overlay

The Slalom Composable DXP practice runs on **MACH** — Microservices, API-first, Cloud-native, Headless. That's not a separate pattern; it's a constraint set:
- Each capability is a microservice (often modular monolith inside).
- All interactions are API-mediated (REST, GraphQL, gRPC, AsyncAPI).
- Cloud-native = serverless or containerized, declarative IaC.
- Headless = the experience layer (Next.js, mobile, kiosks) is decoupled from content and commerce engines.

In a MACH system, the experience layer is typically **Feature-based + Colocated**, the content backend is **DDD + Modular monolith**, and the commerce engine is somebody else's problem (commercetools, Shopify, BigCommerce, Salesforce Commerce).

## Decision matrix

| If your system is… | Reach for… |
|---|---|
| < 50 routes, single team | Colocated |
| Multi-team product, organized by feature | Feature-based |
| Complex domain, multiple bounded contexts | DDD + Modular monolith |
| Spiky load, async fanout, lots of webhooks | Serverless-first |
| Need to swap DB / queue / email vendor cleanly | Hexagonal |
| Multi-decade system, heavy regulation | Clean Architecture |
| MACH composable with multiple experience surfaces | Feature-based for Next.js + DDD on the platform side |

## When patterns blend (most of the time)

A typical 2026 production app has:
- A **feature-based** Next.js front end
- Talking to a **modular monolith** API
- Whose modules are internally **layered**
- Deployed in a **serverless-first** topology
- With **hexagonal** seams at the integrations that matter (auth, payment, search)

That's not architectural promiscuity — that's matching each subsystem's pattern to its actual constraints.

## Common failure modes

- **Picking a pattern by reputation**, not by problem fit. "We do Clean Architecture" doesn't survive contact with a CRUD admin panel.
- **Inheriting boundaries from frameworks** instead of from the domain. The folder structure that ships with `create-next-app` is a starting point, not an architecture.
- **Premature DDD** — doing event sourcing on a system whose business rules fit on a Post-it.
- **Pretending you're modular when you're not** — modular monolith without enforced module APIs decays into a regular monolith with extra steps.

## How this reference relates to the skills

- `software-engineering-composable-architect` — applies these patterns to MACH digital platforms.
- `software-engineering-backend-architect` — applies layered, hexagonal, modular monolith, and serverless-first to backend services.
- `software-engineering-cloud-architect` — applies serverless-first and modular monolith to cloud target selection.
- `software-engineering-nextjs-*` (the seven Next.js skills) — apply colocated and feature-based inside the App Router.

When in doubt, pick the simpler pattern and earn the more complex one.
