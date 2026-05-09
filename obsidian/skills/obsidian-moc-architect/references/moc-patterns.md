# MOC Patterns

Common structural patterns for Maps of Content with worked examples.

## Pattern 1 — Linear / Journey MOC

For topics with a natural progression — learning paths, narrative arcs, sequences.

### Structure

```markdown
---
type: moc
status: refined
parent-moc: "[[Home]]"
---

# Zettelkasten Journey

The path I've taken through Zettelkasten, in roughly the order it made sense to me.

## Start here

If you're brand new, these three notes are enough:
- [[Atomic Notes Connect Better than Monoliths]]
- [[The Zettelkasten Workflow Has Three Stages]]
- [[Permanent Notes Are Where the Value Compounds]]

## Foundations

Once the basics click, these flesh out *why*:
- [[Folgezettel Was Niklas Luhmann's Linking Mechanism]]
- [[Literature Notes Are Different From Permanent Notes]]
- [[The Inbox Is Where Fleeting Notes Live]]

## Tensions and trade-offs

Where the system gets debated:
- [[Folgezettel vs Folders]]
- [[Atomicity Has Limits]]
- [[ZK in Digital Tools Loses Some of the Original]]

## Open questions

Things I'm still working out:
- [[How Big is Too Big for an Atomic Note]]
- [[Is a MOC a Permanent Note]]
```

### When to use

- Learning paths
- Reading orders
- Topics with a natural narrative
- Topics where the user has a clear sense of progression

---

## Pattern 2 — Categorical MOC

For topics with distinct sub-areas that don't have a natural order.

### Structure

```markdown
---
type: moc
status: refined
parent-moc: "[[Home]]"
---

# Productivity MOC

How I think about getting things done. This is my opinionated map, not an exhaustive list.

## Capture

Where ideas land before they're processed.
- [[Inbox Discipline Is Capture Discipline]]
- [[Voice Memos Are an Underrated Capture Channel]]
- [[The Capture Tool Should Match the Context]]

## Process

How captured items become structured.
- [[Process Inbox in Batches, Not Continuously]]
- [[Two-Minute Rule Is for Doing, Not Routing]]

## Plan

How structured items become commitments.
- [[Weekly Review Is the Keystone Habit]]
- [[Daily Plan Comes from Weekly Plan]]

## Execute

How commitments become outcomes.
- [[Single-Tasking Beats Context-Switching]]
- [[Calendar Is for Time, List Is for Tasks]]

## Reflect

How outcomes feed back into the system.
- [[Monthly Review Tightens the Loop]]
```

### When to use

- Domains with several distinct facets
- Mature topics where you've identified the dimensions
- Topics that benefit from "here are the parts" framing

---

## Pattern 3 — Question-Driven MOC

For topics shaped by open inquiries rather than known categories.

### Structure

```markdown
---
type: moc
status: draft
parent-moc: "[[Home]]"
---

# Habits MOC

I'm trying to figure out how habits actually work for me. This MOC is organized around the questions I'm chasing.

## Why do some habits stick and others don't?

- [[Habits That Stick Are Identity-Aligned]]
- [[Streaks Are Motivating Until They Aren't]]
- [[Replacing Beats Stopping]]

## How long does habit formation actually take?

- [[The 21-Days Claim Is a Myth]]
- [[Habits Form Faster Under Stress]]

## What's the difference between a habit and a routine?

- [[Habits Are Triggered, Routines Are Sequenced]]

## Open

Questions I haven't gone deep on:
- [[How Do Habits Compound Across Domains?]]
- [[Are Some People Just More Habit-Forming?]]
```

### When to use

- Topics still being worked out
- Areas where the right structure isn't yet clear
- Vaults where the user wants to surface their own inquiries

---

## Pattern 4 — Reading List MOC

For topics dominated by source notes rather than atomic notes.

### Structure

```markdown
---
type: moc
status: refined
parent-moc: "[[Home]]"
---

# Stoicism Reading

The books I've read on Stoicism, with my notes and rough recommendation.

## Strongly recommend

- [[Meditations]] (Aurelius) — start here
- [[A Guide to the Good Life]] (Irvine) — best modern intro

## Recommend

- [[Letters from a Stoic]] (Seneca)
- [[Discourses]] (Epictetus)

## Have read, didn't connect

- [[The Obstacle Is the Way]] (Holiday) — felt thin

## To read

- [[On the Shortness of Life]] (Seneca)
- [[The Inner Citadel]] (Hadot)

## Themes that emerged

- [[Premeditatio Malorum Is the Stoic Pre-Mortem]]
- [[Dichotomy of Control Is Foundational]]
- [[Memento Mori Reframes Time]]
```

### When to use

- Topics where you read more than you write
- Reading-heavy practices (history, philosophy, deep technical domains)
- When literature notes are the dominant note type

---

## Pattern 5 — Hub + Spokes MOC

For topics with a central concept that connects to many adjacents.

### Structure

```markdown
---
type: moc
status: refined
parent-moc: "[[Home]]"
---

# Atomic Notes MOC

## What atomic notes are

[[Atomic Notes Connect Better than Monoliths]] is the core claim. The supporting ideas:

- [[An Atomic Note Has One Idea]]
- [[Atomicity Is About Reusability, Not Brevity]]
- [[Claim-as-Title Forces Atomicity]]

## What they enable

- [[Atomic Notes Compose Into MOCs]]
- [[Atomic Notes Surface Unexpected Connections]]
- [[Backlinks on Atomic Notes Are Higher Signal]]

## Limits and tensions

- [[Atomicity Has Limits]] — when to *not* split
- [[Lit Notes Don't Need to Be Atomic]] — type-specific
- [[Daily Notes Are Anti-Atomic by Design]]

## Practice

- [[How I Refactor a Long Note Into Atomics]]
- [[Atomic Note Title Test]]
```

### When to use

- Topics where one central idea generates the rest
- "Concept families" with a clear core
- When you want the central idea to feel central in the map

---

## Choosing a pattern

| If your topic feels like...                            | Use                |
| ------------------------------------------------------ | ------------------ |
| A learning path or narrative                            | Linear / Journey   |
| A domain with distinct sub-areas                        | Categorical        |
| Open inquiries you're working through                   | Question-driven    |
| Mostly source notes (books, articles)                   | Reading list       |
| One central idea with many adjacents                    | Hub + spokes       |

It's fine to mix — most mature MOCs have categorical structure with a "Reading list" section and an "Open questions" section.

---

## Things every MOC should have

- **Frontmatter** — `type: moc`, `parent-moc:` (if not top-level), `status:`
- **Opening paragraph** — what this MOC covers and from what angle
- **Section headings** — at least 2, ideally 3–6
- **Commentary in each section** — 1–2 sentences of framing before the links
- **A "open questions" or "to read" or "draft" section** — keeps the map honest about what's unfinished

---

## Things to avoid

- **Pure link dumps** — no commentary, no framing, just bullets. That's an index, not a map.
- **Exhaustive coverage** — every note that could possibly belong. MOCs are *curated*. Use Dataview if you want exhaustive.
- **Flat alphabetical lists** — ignores your actual thinking structure.
- **Too many headings** — H4 and below is a sprawl signal. Time to split.
- **Stale MOCs** — last touched a year ago. Either update or retire.
- **MOCs as todo lists** — different note type. Mixed concerns get muddled.
- **MOCs in folders that hide them** — top-level MOCs should be findable from the home note in one hop.
