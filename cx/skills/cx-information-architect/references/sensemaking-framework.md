---
type: reference
project: skills-library
scope: skill
plugin: cx
parent: "[[cx-information-architect]]"
tags: [type/reference, plugin/cx, scope/skill, topic/information-architecture, source/covert]
status: active
---
# Sensemaking — Abby Covert's framework

Drawn from Abby Covert, *How to Make Sense of Any Mess* (2014). The most useful field guide to information architecture as a *practice* (not a deliverable). Each step has a job to do; skipping any of them produces an IA that is correct in form and broken in use.

## The seven steps

```
1. Identify the mess        — name what's confusing and to whom
2. State your intent        — what should be true after the work
3. Face reality             — inventory what exists, what's broken
4. Choose a direction       — pick the path that aligns to intent
5. Measure the distance     — how far is reality from intent?
6. Play with structure      — design taxonomy, ontology, choreography
7. Prepare to adjust        — IA is not a deliverable; it lives
```

This isn't a waterfall. It's a discipline; you cycle through it as the work progresses.

## 1. Identify the mess

A mess is "any situation where something is confusing or full of difficulty." Covert's three causes:

1. Too much information.
2. Not enough information.
3. Not the right information.
4. Some combination.

The first job is naming the mess concretely. "Our content is a mess" is not a mess statement. "Customers can't find pricing on mobile, and our editors duplicate the same FAQ in three places" is.

**Output:** a one-paragraph mess statement that names *who is confused, when, and about what*.

## 2. State your intent

Intent is what should be true after the work. It is not the deliverable.

**Format:**

```
For [stakeholder]
[verb] [object] [outcome]
[so that] [downstream business / user impact]
```

**Example:**

```
For prospects evaluating composable DXP platforms,
make pricing, capability, and support information findable in two clicks
so that we stop losing late-stage deals to competitors with clearer sites.
```

Intent statements rule out work. If a proposal doesn't move the intent, it's not in scope. Without an intent statement, the team builds a beautifully-modeled IA that nobody asked for.

## 3. Face reality

The current-state inventory. What exists, where, who owns it, how is it labeled, who uses it, what's broken.

For content systems, this typically includes:

- **Content inventory.** Every page / asset / record. Spreadsheet form. Owner, type, channel, last-updated, page-views.
- **Vocabulary audit.** What labels are in use today? Where do they conflict? (Often the same concept has 4–6 names across departments.)
- **System inventory.** Every system holding content. CMS, DAM, marketing automation, CRM, knowledge base, intranet.
- **Stakeholder map.** Who creates, approves, consumes, depends on this content.
- **Pain inventory.** Where users get stuck (analytics, support tickets, search-failure logs).

**Watch out:** Facing reality is uncomfortable; teams skip to design. A future-state IA built without facing reality is a wishlist. The work below is mostly the work.

## 4. Choose a direction

A direction is a strategic posture toward the mess. Examples:

- **Consolidate** — collapse multiple sources into one canonical source.
- **Federate** — let multiple sources exist; build navigation that crosses them.
- **Split** — separate audiences that share a current source but have different needs.
- **Restructure** — keep the content; redesign the relationships.
- **Retire** — remove content that no longer earns its keep.

Most messes need a primary direction with secondary moves. Naming it explicitly prevents the team from doing all five at once and finishing none.

## 5. Measure the distance

Quantify the gap between reality and intent. Sometimes a count (1,200 pages → goal of 400). Sometimes a quality measure (0% of pages have structured metadata → goal of 100%). Sometimes a behavior measure (search success rate 28% → goal of 65%).

**Why this matters:** "Improving the IA" is not a goal. "Reducing time-to-information from 4.2 minutes to under 90 seconds" is. The measure makes scope tractable.

## 6. Play with structure

The design work most teams jump to first. Covert's three layers of structure:

### Taxonomy
What things are called and how they're grouped. Categories, tags, controlled vocabularies.

- **Faceted vs. hierarchical.** Most modern IA leans faceted (multiple attributes per item). Hierarchical works for stable, exclusive categorization.
- **Mutually exclusive vs. overlapping.** Categories should be MECE where possible; if not, document the overlap rule.
- **Naming.** Use the customer's words, not the company's. Test names with customers before committing.

### Ontology
What things mean and how they relate. The relationships between content types.

- A "blog post" and an "article" — same thing or different? Decide before modeling.
- An "author" — is it a person record, a string field, or both?
- A "topic" — is it a tag, a content type, a channel?

Ontology decisions are the most consequential and the least visible. Get them wrong and every downstream taxonomy is built on sand.

### Choreography
How things flow over time. The sequences and rules that govern how content moves through the system.

- Editorial workflow — draft → review → approve → publish.
- Lifecycle — when does content auto-archive? When does it require review?
- Cross-channel sequencing — when content updates in CMS, what happens in email, search, app?

Most CMS implementations get taxonomy roughly right and ignore choreography until ops breaks.

## 7. Prepare to adjust

IA is not a one-time deliverable. Reality moves. Channels appear. Audiences shift. Plan for:

- **Quarterly review of the vocabulary audit.** New terms enter; old ones drift.
- **Annual content inventory refresh.** What's been added; what's grown stale; what's grown popular.
- **Search-log review.** What users searched for and didn't find — the most concrete signal of gaps.
- **Migration discipline.** When the IA changes, redirects, training, and editorial guidelines change with it.

## Sensemaking checks

Before signing off any IA deliverable, run these:

- **Intent test.** Does the IA move the intent statement? If not, why does it exist?
- **Vocabulary test.** Do the labels match the customer's words (verified with research, not guessed)?
- **Reality test.** Will the editorial team be able to maintain this without developer intervention?
- **Distance test.** Has the gap between reality and intent shrunk measurably?
- **Adjustment test.** Is there a defined cadence for review and revision?

## Why this framework belongs here

Most IA work in Composable DXP engagements jumps from "we need a content model" to a Contentful spec. That bypasses 1–5 and the work fails in production. The architecture is technically correct and operationally unmaintainable.

Covert's framework is the discipline that pushes the IA conversation upstream — into intent, reality, direction, distance — before the modeling work begins.

## Source

- Covert, A. *How to Make Sense of Any Mess: Information Architecture for Everybody*. Self-published, 2014.
- Covert, A. abbycovert.com — additional templates and worksheets.
