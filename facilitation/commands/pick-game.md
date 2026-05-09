---
description: Pick the right facilitation game(s) from the Gamestorming catalog for a specific moment in a session — input is what you need to accomplish + time + people; output is 1–3 named games with delivery notes.

# Project context
type: command
project: skills-library
plugin: facilitation
tags: [type/command, plugin/facilitation]
status: active
---

Use the `facilitation-gamestorming-coach` skill to pick the right game(s) from the catalog of 123 games at [`gamestorming-catalog.md`](../skills/facilitation-gamestorming-coach/references/gamestorming-catalog.md).

This is a fast-path command for "what game should I run?" — for a single moment, not a full session design. For full session design, use `/design-session`.

Steps:

1. **Get the inputs.** Ask if not provided:
   - **What outcome** does this block need to produce? (Decision, alignment, generative ideas, surfaced perspectives, named owners, prioritized list, etc.)
   - **How long** do you have for this block? (10 min, 30 min, 60 min, 90 min)
   - **How many people**? (And in what mix — same team, cross-functional, with/without leadership, virtual/in-person)
   - **Where in the session** are you? (Opening, exploring, closing — Gamestorming's three movements)

2. **Map outcome to category.** From the catalog's category index:
   - Generate / surface / warm up → Games for opening or fresh thinking
   - Sort / make sense / develop → Games for design or problem-solving
   - Prioritize / decide / commit → Games for decision-making or closing
   - Reset stuck energy / break a pattern → Games for fresh thinking, Bodystorming, 3-12-3 Brainstorm

3. **Filter by time and group size.** Many games have explicit time bounds; respect them. A 90-min World Café won't fit in a 30-min slot. A 4-person Five-Fingered Consensus is overkill for 12 people; switch to Dot Voting.

4. **Recommend with rationale.** Give the user **one primary recommendation** with a one-line rationale, and **one or two alternatives** with brief tradeoffs. Don't dump the catalog. Pattern: "Run [Game X] because [specific reason for this room and outcome]. If [condition], use [Game Y] instead."

5. **Show the delivery notes.** For the recommended game, surface the digest from the catalog inline — Object of play, Players, Duration, How to play (numbered steps), Strategy. The user shouldn't have to navigate to the catalog file.

6. **Anticipate the failure mode.** Most games have a typical way they fall flat. Name it specifically:
   - Dot Voting → strategic voting, no real prioritization. Mitigation: vote against criteria, not preferences.
   - Pre-Mortem → polite "what could go wrong" instead of real risk surfacing. Mitigation: assign devil's advocate roles.
   - Cover Story → analysis mode kills the game. Mitigation: enforce past tense and ban "reality checks."
   - Empathy Map → filled from stereotypes instead of evidence. Mitigation: require interview-tagged stickies.

7. **Sequence option.** If the user is picking multiple games for sequential blocks, recommend the chain. The Core Games (7Ps Framework, Affinity Map, Bodystorming, Card Sort, Dot Voting, Empathy Map, Forced Ranking, Post-Up, Storyboard, WhoDo) chain naturally; non-core games may need bridging blocks.

8. **Source link.** Always include the gamestorming.com URL for the recommended game so the user can grab the canvas/template if needed.

Don't recommend more than 3 games per call. The discipline is *picking*, not browsing.
