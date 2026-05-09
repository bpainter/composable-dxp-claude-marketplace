---
name: marketing-seo-brief
description: Produce SEO content briefs that a writer can hand off and execute against — primary and secondary keywords, search intent, SERP feature targets, competitor coverage, outline with H-tag hierarchy, internal linking plan, and on-page SEO checklist. Use this skill any time the user wants to plan, scope, or brief a piece of content for organic search, even if they say "outline a post," "what should we write about X," or "make this rank." Trigger whenever the work is upstream of writing a piece of content intended to attract organic traffic, so the writer doesn't have to invent the SEO scaffolding themselves.

# Project context
type: skill
project: skills-library
plugin: marketing
aliases: [marketing-seo-brief]
tags: [type/skill, plugin/marketing topic/marketing, topic/content]
status: active
---

# SEO Content Brief

The deliverable here is a **brief**, not the article. A good brief lets a different writer (human or AI) produce a piece that has a real chance to rank, without re-doing keyword and SERP analysis. Think of yourself as the editor scoping the assignment.

Pair this skill with `strategy-geo-optimization` — most modern content needs to perform on both classical search and AI-driven discovery, and the structural choices overlap but aren't identical.

## When to use this skill

- The user asks for a content brief, content outline, or "plan a post."
- The user asks "what should we say about X" for a topic that will live on the site.
- A new content idea is being scoped before it's assigned.
- An existing piece is underperforming and needs a re-brief, not just edits.

## What to gather before drafting the brief

If the user hasn't given you these, ask — a brief without them is guesswork:

1. **Topic and angle.** Not just "vesting" but "what founders should know about vesting before signing their first term sheet."
2. **Audience.** Specific persona, not "everyone." Reach for the project's persona references.
3. **Goal.** Awareness, decision-stage education, or conversion (questionnaire, contact)?
4. **Existing coverage on the site.** What related pages exist, so the brief can specify internal links and avoid cannibalization.
5. **Known constraints.** Word count target, publish date, legal review window, author.

## Brief structure

Output the brief as a markdown document with these sections, in this order. Keep it tight — a brief that's longer than the article is a smell.

```
# Content Brief: [Working title]

## Snapshot
- **Topic:** [one line]
- **Primary audience:** [persona]
- **Stage of journey:** [awareness / consideration / decision]
- **Goal of the page:** [what should the reader do next]
- **Estimated length:** [range]
- **Author / Reviewer:** [names or roles]

## Search intent
[1-2 sentences: what is the searcher actually trying to do? Decide between options? Understand a term? Execute a task? This frames every other choice.]

## Keywords
- **Primary keyword:** [one phrase, the one we are explicitly targeting]
- **Secondary keywords:** [3-7 phrases, semantically related, used in subheads and body]
- **Long-tail / question variants:** [3-5 actual questions people search, useful for FAQ section and GEO]

## SERP analysis
[3-5 bullet points on what is currently ranking. What format wins — listicle, explainer, decision tree? What features show — featured snippet, People Also Ask, video carousel? What angle is missing that we can own?]

## Competitor coverage
[2-4 specific competing pages, with the gap or weakness we'll exploit. E.g., "Cooley GO covers vesting but doesn't address founder-vs-employee differences" or "Stripe Atlas explainer is dense; ours can be more scannable."]

## Outline
[H1, H2s, H3s. Every H2 should answer a question or advance the decision. Include a 1-line note under each heading describing what that section delivers. Lead with the answer; deep context comes later.]

H1: [working title]
- [opening framing — three-line opener: hook, primary claim, why-it-matters]

H2: [first question/section]
- [what this section delivers]

H2: [second]
- [...]

[Include FAQ section with 4-6 question-styled H3s where the question matches a real search query — these double as GEO anchors and feed PAA features.]

## On-page SEO requirements
- **Title tag:** [≤60 chars, primary keyword, brand suffix]
- **Meta description:** [140-155 chars, includes primary keyword and a hook]
- **URL slug:** [short, hyphenated, primary keyword]
- **H1:** [includes primary keyword naturally]
- **Image needs:** [count, with alt text guidance]
- **Schema:** [Article + FAQPage + BreadcrumbList; if how-to, HowTo]

## Internal linking
- **Inbound:** [3-5 existing pages that should link TO this one]
- **Outbound:** [3-5 existing pages this one should link to]
- [Note any pillar/cluster relationship]

## External authority links
[1-3 high-authority external links to reference — primary sources (regulatory bodies, official documentation, peer-reviewed research) preferred for any factual or legal-adjacent content. AI-generated content should never cite case law, statute, or technical specs without human verification.]

## Notes for the writer
- [tone notes — e.g., "lead with the binary answer, then qualify"]
- [legal / brand / regulatory guardrail flags — route to project-specific reference]
- [GEO notes — see `strategy-geo-optimization` — claim density, attribution-friendly phrasing]
- [length budget reminder]

## Success criteria
- [primary KPI: rankings for primary keyword in 90 days]
- [secondary: organic traffic, engaged time, conversion to questionnaire]
```

## Rules of thumb

**One primary keyword per page.** Two means neither will rank well. If two intents are pulling at the page, split it.

**Subheads earn their place.** Every H2 should map to a question someone is actually asking. If a heading is just a label ("Background"), it's not pulling its weight — rewrite to a question or a concrete sub-promise.

**Lead with the answer, then context.** SEO and founder-content philosophy align here: the reader (and the search engine) should know what the page is about in the first 100 words.

**Internal linking is a system, not an afterthought.** When briefing one page, identify the cluster it belongs to. Tag pillar vs. supporting and reflect that in inbound/outbound links.

**Don't keyword-stuff the brief.** A brief that lists 20 keywords is a brief that produces unfocused content. Pick one primary, a tight secondary set, and questions.

## When ranking matters less than discovery

If the page is targeted primarily at AI/LLM citation rather than classical search (e.g., a definitional glossary entry or a frequently-cited explainer), follow `strategy-geo-optimization` more heavily and treat SEO requirements as the floor, not the focus.

## Common failure modes

- Brief written without ever looking at the SERP. Always check what's actually ranking.
- "Search intent" copied from the keyword name. Be specific.
- Outlines that march through history before answering the question.
- 12 H2s on a 600-word page (or 2 H2s on a 2,500-word page).
- Missing internal links — content that ranks alone is content that loses to clusters.
- Forgetting that legal-adjacent content goes through legal review, which may rewrite key phrases. Sequence: brief → draft → legal review → SEO polish, not the other way.
