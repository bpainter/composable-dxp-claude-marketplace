---
name: consulting-humanize
description: Rewrite consultant-jargon and AI-generated prose into clear, direct, professionally friendly English at roughly a high-school reading level. Strips em-dashes and en-dashes, breaks up long sentences, removes common AI tells (comparative "not X but Y" patterns, summary closers that start with "This...", hedging stacks, listicle bloat), and gets to the point. Use this skill any time the user wants writing that sounds human, drafts that need to be cleaned up, content that reads as overwritten, or output from another AI that needs to ship to a real audience, even when they say "make this clearer," "tighten this up," "rewrite this," or "this sounds AI." Trigger whenever prose needs to read like it was written by a person who respects the reader's time, not a consultant trying to sound smart or an LLM trying to sound thoughtful.

# Project context
type: skill
project: skills-library
plugin: consulting
aliases: [consulting-humanize]
tags: [type/skill, plugin/consulting topic/consulting, topic/strategy]
status: active
---

# Humanize

Most consulting writing and most AI writing fails for the same reason. It tries to sound impressive instead of being clear. This skill is for stripping that out.

The target voice is professional and friendly. Direct. Reads like someone who knows the topic and respects your time. Aim for roughly a high-school reading level. Short sentences. Plain words. No tricks.

This skill applies in two modes:

1. **Editing mode**: take existing prose and rewrite it cleanly. Most common use.
2. **Drafting mode**: write fresh content in this voice from the start.

## When to use this skill

- The user pastes a draft and asks for an edit, polish, tighten, or rewrite.
- The user says the writing sounds "AI," "consultant-y," "stiff," or "overwritten."
- The user is writing for a public audience and the draft was produced by another AI.
- A consultant deliverable (memo, summary, executive note) needs to land with a non-consulting audience.
- Slalom or client content for non-expert audiences, where readability matters as much as accuracy.

If the user wants academic, legal, or formal-register writing, do not apply this skill. The voice here is for everyday professional reading.

## The rules

### Punctuation

Remove em-dashes and en-dashes. Replace them. The em-dash is the single biggest AI tell in published prose right now.

How to replace:

- For an aside, use a comma or parentheses.
- For a strong break, use a period and start a new sentence.
- For a range, use the word "to" (use "10 to 15 days" rather than "10–15 days").
- For a bullet or list, use a period and a new bullet.

Before: "The questionnaire is short — about ten questions — and saves your progress."
After: "The questionnaire is short. It has about ten questions and saves your progress."

Before: "Most founders incorporate in Delaware—it is what investors expect."
After: "Most founders incorporate in Delaware. It is what investors expect."

Hyphens for compound words are fine. Keep "co-founder," "early-stage," "decision-driving."

### Sentence structure

Aim for sentences of 15 to 20 words. Vary length, but lean short. If a sentence runs past 25 words, break it.

Active voice over passive. Verbs over nominalizations.

Before: "The decision will be made by the steering committee."
After: "The steering committee decides."

Before: "We performed an evaluation of the process."
After: "We evaluated the process."

One idea per sentence. Two ideas, two sentences.

### Word choice

Plain words win. If a word would not appear in a normal conversation, swap it.

| Avoid | Use |
|---|---|
| Utilize | Use |
| Leverage | Use |
| Facilitate | Help, run, lead |
| Endeavor | Try |
| Commence | Start |
| Subsequent | Next |
| In order to | To |
| Due to the fact that | Because |
| At this point in time | Now |
| In the event that | If |
| A multitude of | Many |
| Approximately | About |

Cut filler. Words like "very," "really," "quite," "actually," "basically," and "literally" rarely add meaning. Strip them unless they earn their place.

### AI tells to remove

These patterns are recognizable AI fingerprints. Find them and rewrite.

**The "not X, but Y" comparative.**

This pattern shows up constantly in AI prose. It pretends to add nuance but usually just delays the point. Make a direct statement instead.

Before: "This isn't just about saving time. It's about saving energy."
After: "This saves time and energy."

Before: "It's not a tool. It's a system."
After: "It's a system."

If both halves are needed, use a plain "and." If only the second half matters, drop the first.

**The "This..." summary closer.**

AI loves to end paragraphs with a sentence that starts with "This" and restates the paragraph. The reader already read the paragraph. The summary closer is dead weight. Cut it.

Before:
> Most founders use 4-year vesting with a 1-year cliff. It protects the company from a co-founder who leaves early. This approach has become the default in early-stage tech.

After:
> Most founders use 4-year vesting with a 1-year cliff. It protects the company from a co-founder who leaves early.

If the closer adds new information, keep it but rewrite to lead with the information, not the word "This."

**Throat-clearing openers.**

Before: "It's worth noting that most companies file in Delaware."
After: "Most companies file in Delaware."

Other throat-clearing phrases to remove on sight:

- "It's important to remember that..."
- "It's worth pointing out..."
- "One thing to keep in mind..."
- "As we all know..."
- "In today's fast-paced world..."

**Hedging stacks.**

Pick one hedge. Stacking them weakens the sentence and signals AI.

Before: "It might possibly be a good idea to perhaps consider reviewing..."
After: "Consider reviewing..." or "Review..."

**Verb tics.**

Strike on sight. They are the most common AI vocabulary tells in 2025 and 2026.

- delve, delve into
- navigate, navigating
- harness, harnessing
- unlock
- empower
- robust
- seamless
- comprehensive
- cutting-edge
- transformative
- game-changer
- revolutionary

Ask: what specifically does this verb mean here? Then write that.

Before: "We harness AI to unlock seamless workflows."
After: "We use AI to make these workflows faster."

**Forced balance.**

AI tends to write in threes. Three bullets, three adjectives, three clauses. If you find yourself writing three for the sake of three, ask whether you actually have three things to say. Two is fine. One is often best.

**The motivational opener.**

Before: "Whether you're a first-time founder or a serial entrepreneur..."
After: Remove. Address the actual reader.

Before: "Let's dive in."
After: Remove.

**Closing flourishes.**

Strip these from anything that ships:

- "I hope this helps!"
- "Hopefully this gives you a sense of..."
- "Reach out if you have any questions."
- A summary paragraph that restates the whole document.

End where the content ends.

### Tone calibration

Professional and friendly means:

- Talk to the reader, not at them. Second person is fine when speaking to one person.
- Acknowledge the reader's situation if it helps the message land.
- Show, do not tell. "We tested 12 variants" beats "We did extensive testing."
- Allow contractions ("it's," "you'll"). Forbidding contractions makes prose stiff.
- Allow honest uncertainty. "I am not sure" is more credible than false confidence.

Avoid:

- Performative warmth ("So glad you asked!").
- Forced informality (no "yo," no exclamation points stacked, no emoji unless the user is using them).
- Sarcasm or irony in transactional content. They mistranslate.

### Reading level check

Aim for these rough targets:

- Average sentence length: 15 to 20 words.
- Long sentences: rare. Anything over 25 words gets a second look.
- Syllables per word: keep most words at one or two syllables. Specialist terms are fine when needed, but explain them on first use.
- Flesch Reading Ease: roughly 60 to 70 for general professional readers. Higher means simpler.

If a paragraph reads like a textbook, rewrite it.

## Editing process

When given a draft to humanize, do this in order:

1. **Read once for meaning.** Note what the draft is trying to say. The rewrite must preserve meaning.
2. **Cut the obvious.** Remove em-dashes, en-dashes, throat-clearers, summary closers, motivational openers, closing flourishes.
3. **Break long sentences.** Any sentence over 25 words gets cut into two.
4. **Replace jargon with plain words.** Use the table above.
5. **Strip AI verbs.** Replace with specific verbs that say what is actually happening.
6. **Reread aloud.** If a sentence feels stiff, rewrite it.
7. **Check the closer.** The piece should end on the last real point, not on a summary.
8. **Verify meaning is intact.** A clean rewrite that loses the point is worse than the original.

Optional: report a short summary of what you changed and why, so the user can spot edits they want to push back on.

## Examples

### Consultant memo to founder-friendly note

**Before:**
> In order to facilitate a robust and comprehensive transformation, we recommend leveraging a phased approach—starting with a discovery sprint—that will enable the organization to seamlessly navigate the complexities inherent in modern digital ecosystems. This approach is not merely a process improvement; it is a strategic imperative.

**After:**
> We recommend a phased approach. Start with a discovery sprint. It gives the team time to understand the problem before committing to a solution.

### AI-generated paragraph cleaned up

**Before:**
> It's worth noting that founder vesting—typically a four-year schedule with a one-year cliff—has become the de facto standard in early-stage tech. This isn't just about protecting the company; it's about ensuring alignment among co-founders. This approach has stood the test of time.

**After:**
> Founder vesting is typically a four-year schedule with a one-year cliff. It protects the company and keeps co-founders aligned.

### Email rewrite

**Before:**
> I wanted to circle back regarding the deliverable timeline. Given the complexity of the engagement, we've been navigating some challenges, but we're harnessing the team's expertise to drive forward. I hope this helps clarify things!

**After:**
> Quick update on the deliverable timeline. The engagement is more complex than expected, so we are running about a week behind. New target is May 15. Let me know if that works.

## How to use this skill alongside others

- For legal-adjacent or regulated copy, run this skill after the draft is written but before the legal-guardrails check. Voice cleanup happens before legal review so reviewers see the finished tone.
- For consulting deliverables produced under `consulting-management-consultant`, run this skill on any text destined for a non-consulting audience.
- For long-form content briefed under `strategy-seo-brief`, run this skill on the body, then re-check the SEO and GEO requirements since cuts can affect keyword placement.
- For AI-drafted content of any kind, this skill is the recommended last pass before publication.

## What this skill is not

- It is not a translation skill. Do not change the meaning. Cleanup never overrides accuracy.
- It is not a tone-flattener. The reader should still hear the writer's perspective. Strip the tells, keep the voice.
- It is not for academic, legal-document, or contract writing. Those have their own conventions.
- It is not a license to remove necessary nuance. If a sentence carries real qualification, keep the qualification. Strip the empty hedges, not the substantive ones.
## Source frameworks

- **Block, *Flawless Consulting*** — anti-jargon framing: "flawless" consulting is about authentic, plainspoken language between equals; the antidote to consultant-speak is presence not vocabulary. See [`../../references/book-flawless-consulting.md`](../../references/book-flawless-consulting.md).
- **Mabee, *The Consulting Discipline*** — direct, plainspoken style modeled in the book itself ("like using machines at the gym" rather than methodology jargon). See [`../../references/book-consulting-discipline-mabee.md`](../../references/book-consulting-discipline-mabee.md).

