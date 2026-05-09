---
description: Define brand voice and tone — 3-4 attributes with not-X pairings, do/don't pairs, tone register matrix, filler-words ban list

# Project context
type: command
project: skills-library
plugin: brand
tags: [type/command, plugin/brand]
status: active
---

Define a brand voice and tone system that constrains writing rather than describing it.

Steps:

1. **Read the brand strategy** from [[brand-strategist]]. Voice has to match positioning. If strategy doesn't exist, route there first.

2. **Identify the audience.** Voice for a CIO-audience B2B brand differs from voice for a consumer DTC brand. Be specific.

3. **Pick 3–4 voice attributes** via [[brand-voice-tone]] framework:
   - Each attribute paired with what it is NOT (the adjacent trait we reject).
   - Generate 8–12 candidates first; group; narrow to 3–4 that compose (not contradict).

4. **For each attribute, write 5–8 do/don't pairs** with specific example sentences. Abstract description is useless; concrete examples constrain.

5. **Build the tone register matrix:**
   - 6–10 situations (announcing a launch, customer support apology, marketing a download, crisis response, sales follow-up, social hook, internal announcement, industry POV, etc.).
   - Per situation: tone shift + example sentence.
   - Voice stays constant; tone flexes within the voice.

6. **Compile the filler-words ban list** specific to this brand (inherit from the standard list in [[ai-tells-forbidden-patterns]]):
   - Banned: elevate, leverage, seamless, unleash, next-gen, synergy, innovative, best-in-class, unlock, empower, revolutionize, transform (used vaguely), cutting-edge, world-class.
   - Replacements: concrete verbs that name what actually happens.

7. **Compile the AI-writing-tics ban list:**
   - Em-dash overuse for routine asides.
   - "Not X but Y" comparative dramatic framing.
   - Summary closers ending every section.
   - "In today's world..." throat-clearing.
   - Three-item lists as a tic.

8. **Write voice in its own voice.** The voice page in the deliverable should *prove* the voice — its writing demonstrates the attributes. This is the test: if the voice page reads in voice, the voice works.

9. **Test on real copy.** Apply the new voice to a current piece (a recent email, blog post, web hero). Does it change anything? If not, the voice is too vague.

10. **Hand off** to [[brand-guidelines-composer]] (Verbal identity chapter) and to `marketing-copywriter` for ongoing application.

Output format:

```
# Brand voice: {Brand}

## Who we are when we write
[One paragraph that proves the voice in its own writing.]

## Voice attributes (3–4)

### {Attribute 1}, not {adjacent trait}
- What it means: [one paragraph]
- Do / don't pairs (5–8):
  - Do: "..." | Don't: "..."
  - ...
- Example sentence: "..."

### {Attribute 2}, not {adjacent trait}
[Same structure]

## Tone register (situation → tone shift)
| Situation | Tone shift | Example sentence |
| --- | --- | --- |
| Product launch | confident, energetic | "..." |
| Customer support apology | direct, warm, accountable | "..." |
| ...

## Banned filler words
| Word | Replacement strategy |
| elevate | concrete impact verb |
| leverage | "use" plus specifics |
| seamless | "integrates via..." or specifics |
| ...

## Banned AI-writing tics
- Em-dash overuse → periods, commas, parentheses
- "Not X but Y" comparative framing → direct claims
- Summary closers → end at natural endpoint
- ...

## Authority
- Brand voice owner: {role / person}
- How to flag voice drift: {process}
```
