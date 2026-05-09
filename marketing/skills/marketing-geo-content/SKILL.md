---
name: marketing-geo-content
description: Structure content so AI assistants and answer engines (ChatGPT, Claude, Perplexity, Google AI Overviews, Bing Copilot) can extract, attribute, and cite it cleanly. Covers claim density, question-led structure, schema markup, citation hygiene, and "answer chunks" that survive being lifted out of context. Use this skill any time the user wants content to perform in AI/LLM-driven discovery, even if they say "make this AI-friendly," "rank in ChatGPT," "answer engine optimization," or "GEO." Trigger whenever content is being drafted or restructured for an audience that may arrive via an AI summary instead of a classical search result, so the page is built to be cited rather than just clicked.

# Project context
type: skill
project: skills-library
plugin: marketing
aliases: [marketing-geo-content]
tags: [type/skill, plugin/marketing topic/marketing, topic/content]
status: active
---

# Generative Engine Optimization (GEO)

GEO is the discipline of making content that an LLM-driven answer engine can confidently quote. The goal is no longer just "rank on Google" — it is "show up as the cited source inside an AI answer." The content patterns that win are different enough from classical SEO that they need their own skill.

Pair this with `strategy-seo-brief` for the keyword and SERP layer. They are complementary, not competing.

## What changes when the reader is an LLM

An answer engine doesn't read your page top to bottom and decide if it likes you. It pulls **chunks** — usually a paragraph or a list — and either uses them to compose an answer or quotes them with attribution. Three implications:

1. **Chunks must stand alone.** A paragraph that depends on the previous one for context will be lifted, mis-summarized, and cited badly.
2. **Specific, attributable claims beat smooth prose.** "Most companies use 4-year vesting with a 1-year cliff" is more liftable than "Vesting schedules vary, but tend to follow common patterns."
3. **The model is looking for trust signals.** Author bylines, primary-source citations, recency, structured data — all of these tip the model toward citing you.

## Structural patterns

### Question-led headings

Phrase H2s and H3s as questions a real person would ask. Models match queries to question-shaped headings far more readily than to label-shaped ones.

| Don't | Do |
|---|---|
| Vesting basics | What is founder vesting? |
| Equity considerations | How much equity should a co-founder get? |
| Tax implications | When should I file an 83(b) election? |

### Answer-first paragraphs

Each section begins with a 1-2 sentence direct answer, then follows with context. The first sentence is the chunk most likely to be lifted — it should be self-contained.

> **What is founder vesting?**
>
> Founder vesting is a schedule that determines when a founder's equity actually becomes theirs to keep — typically 4 years with a 1-year cliff. It exists so that a co-founder who leaves after six months doesn't walk away with a quarter of the company.

The first sentence answers the question. The second adds the trust-building "why" without requiring it.

### Liftable list patterns

Models love numbered or bulleted lists when each item is a complete claim, not a sentence fragment.

```
The most common founder vesting terms are:

1. Four-year total vesting period.
2. One-year cliff (no vesting until month 12, then 25% vests at once).
3. Monthly vesting after the cliff.
4. Acceleration on certain triggers, often double-trigger on acquisition.
```

Each line could be lifted alone and still mean something.

### "Most / some / nobody" frequency anchors

Frequency-anchored statements are highly liftable because they read as factual market norms. They also stay safe for any audience that requires general statements rather than prescriptive advice (regulated industries, legal-adjacent content, healthcare).

> **Most** mid-market enterprises adopt composable DXP in stages — starting with a single channel rather than a full replatform. **Some** lead with content (CMS-first); **some** lead with commerce. **Almost nobody** ships best-of-breed across every layer in the first phase.

### TL;DRs and key-takeaways blocks

A bullet block at the top of the page summarizing the 3-5 main claims. This is the single most-cited block on most well-optimized pages.

### Tables for comparisons

Two-axis comparisons (entity types, financing instruments, vesting variations) belong in tables. Models extract them cleanly and reproduce them faithfully.

### FAQ section at the bottom

A 4-8 question FAQ section using `FAQPage` schema. These get pulled into Google's AI Overviews and "People Also Ask" features, and they double as anchors for direct LLM citation.

## Claim density

Aim for content where most paragraphs contain a verifiable claim. Avoid prose that is connective tissue without substance ("In today's startup ecosystem, founders face many choices.").

A useful self-check: if you removed every sentence that doesn't either (a) state a fact, (b) make a claim about what is typical, or (c) define a term — would the page still hold up? If yes, you have good claim density. If half the page disappears, rewrite.

## Citation hygiene

Models cite sources they trust. Build trust via:

- **Author bylines on every article**, with a real bio, role, and credentials. Anonymous content gets cited less.
- **Primary-source links** for any factual claim — IRS publications, SEC releases, Delaware Code, court opinions. Wikipedia and other listicles do not count.
- **Date stamps**: published date and last-updated date. Recency is a strong trust signal.
- **Internal cross-links** that demonstrate topical depth (clusters > orphan pages).
- **External outbound links** to authoritative sources. Closed-garden content (no outbound links) is read as lower authority.

For any regulated or legally-adjacent content (legal, healthcare, finance), AI-assisted authoring is prone to hallucinating citations — every citation gets human verification before publish. Never let an AI-drafted citation into production unverified.

## Structured data

Use schema.org JSON-LD on every published page. The combinations that matter most for founder-facing content:

- `Article` (or `BlogPosting`) — author, datePublished, dateModified, headline, description.
- `FAQPage` — for the FAQ section. Each Q&A becomes a Question + Answer node.
- `BreadcrumbList` — site hierarchy.
- `HowTo` — only when the content is genuinely a step-by-step procedure (e.g., "How to file an 83(b) election").
- `Organization` — site-wide, includes legalName, sameAs links.
- `Person` — for author entities, with credentials and affiliation.
- `WebPage` with `speakable` annotations — flag the most important sentences for voice/snippet extraction.

If working with Contentful, model these as part of the content type so authors don't have to hand-author JSON-LD.

## Multi-format hedging

LLMs ingest content from many surfaces. Where reasonable, also produce:

- A clean RSS / Atom feed for syndication.
- An llms.txt file describing the site for AI crawlers (emerging convention).
- Open Graph and Twitter Card metadata for any social/quote contexts.
- Plain-text or markdown variants of key pages, for AI training/citation pipelines that prefer them.

These are scaffolding, not a substitute for content quality, but they remove friction for the engines that want to ingest you.

## Common failure modes

- **Storytelling lead.** Opening with an anecdote about a fictional founder. Search engines and models both want the answer up top.
- **Walls of prose with no extractable units.** Two paragraphs of nuance with no liftable claim.
- **Heading labels instead of questions.** "Considerations" is dead weight. "What should I consider before…" is liftable.
- **No author byline.** Drops trust signal hard.
- **Stale dates.** A page last updated in 2021 won't get cited about 2026 financing trends.
- **Hallucinated citations from AI drafts.** Always verify; non-negotiable for legal, healthcare, finance, and any regulated domain.
- **Unfaithful TL;DR.** TL;DR claims that aren't supported by the body. Models will lift the TL;DR, get burned, and stop citing you.

## Quick GEO checklist

Run this before publish:

- [ ] H1 reflects the searcher/asker's question or task.
- [ ] H2s are questions, not labels.
- [ ] Each section starts with an answer-first sentence that stands alone.
- [ ] Page has a TL;DR or key-takeaways block.
- [ ] At least one liftable list or comparison table.
- [ ] FAQ section with 4-8 question-styled entries.
- [ ] FAQPage + Article + BreadcrumbList JSON-LD present.
- [ ] Author byline with real bio.
- [ ] Published and last-updated dates visible.
- [ ] Primary-source citations for factual claims.
- [ ] Frequency-anchored phrasing ("most," "some," "rarely") in place of vague qualifiers.
- [ ] Page passes the project's voice / brand / legal-review self-check.

## How this skill relates to others

- Keyword targeting and SERP analysis → `strategy-seo-brief`.
- Voice and audience for the target reader → project-specific brand and audience references.
- Disclaimer and citation safety → for legal-adjacent content, route through the engagement's legal review.
- Content modeling for Contentful (so structured data is built into authoring, not added at the end) → forthcoming `dev-contentful-model` skill.
