---
name: brand-voice-tone
description: >
  Define and apply brand voice and tone — the verbal register the brand uses across surfaces. Build a 4-attribute voice framework with do/don't pairings, a tone register matrix that maps situations to tone shifts, and example sentences that prove the voice in its own writing. Cite the named filler-words bans (elevate, leverage, seamless, unleash, next-gen) and consultant-boilerplate patterns to reject. Use this skill any time brand voice is being defined, refreshed, or applied to specific copy.

# Project context
type: skill
project: skills-library
plugin: brand
aliases: [brand-voice-tone, brand-voice, voice, tone, brand-language]
tags: [type/skill, plugin/brand, topic/brand, topic/voice, topic/copy]
status: active
---

# Brand Voice and Tone

Voice is who the brand is. Tone is how the brand adjusts to context. Both are brand decisions; both belong in the guidelines. Most "we need a brand voice" requests get answered with vague adjectives ("Bold. Friendly. Trustworthy.") that don't constrain anything.

This skill defines voice as a *constraint*, not a label — and tone as a *register* the brand shifts within while remaining recognizably itself.

Pair with [[brand-strategist]] (positioning that informs voice), [[brand-identity-system]] (visual register that voice must match), [[brand-guidelines-composer]] (assembly of voice into the guidelines deliverable), and [[ai-tells-forbidden-patterns]] (filler-words bans).

## When to use this skill

- Defining brand voice for a new brand.
- Refreshing or evolving brand voice for an existing brand.
- Applying brand voice to specific copy (web, deck, document, email, ad).
- Auditing existing copy for voice drift.
- Briefing copywriters or content teams.

## What this skill is NOT

- Not the writing skill itself. Voice is the rules; copy is the work.
- Not the brand strategy skill (see [[brand-strategist]]).
- Not the visual-identity skill (see [[brand-identity-system]]).
- Not the consulting-humanize skill — that handles AI-tic cleanup. Voice is the broader register.

## The voice framework (4 attributes, with do/don't)

Pick **3 to 4 voice attributes** for the brand. Each attribute is paired with what it is *not* — the adjacent trait we're rejecting. This pairing is what gives the attribute teeth.

For each attribute:
- **What it means** — one paragraph.
- **What it is, not what it isn't** — the adjacent trait we're not.
- **In practice** — do / don't pairs.
- **Example sentence** that lands the attribute.

### Example: a single attribute

> **Direct, not blunt.**
> We respect the reader's time and lead with the answer. We don't bark orders or skip context the reader needs.
>
> *Do:* "Most founders incorporate in Delaware. Here is why."
> *Don't:* "You should incorporate in Delaware."
> *Don't:* "Among the various jurisdictional choices facing today's founder…"

The "not blunt" pairing prevents the attribute from sliding into curt instructional voice — which writers might default to if "direct" were the only descriptor.

### Common attribute pairings (don't copy; use as inspiration)

| Direct, not blunt | Respects time, leads with answer |
| Confident, not arrogant | Stakes a position; doesn't condescend |
| Warm, not folksy | Friendly; not cutesy or performative |
| Precise, not pedantic | Specific; not jargon-laden |
| Plain, not dumbed-down | Clear language; not insulting reader's intelligence |
| Curious, not credulous | Asks good questions; doesn't accept everything |
| Editorial, not academic | Engaging; not dry-citation-heavy |

Pick 3–4 that suit the brand. Don't use generic combinations like "Bold + Friendly + Modern + Trustworthy" — they constrain nothing.

## Tone register matrix

Voice is constant. Tone shifts. Map situations to tone shifts so writers know how to flex while staying recognizable.

| Situation | Tone shift | Example sentence |
|---|---|---|
| Announcing a product launch | Confident, energetic | "Composable DXP is shipping. Here's how to get on it." |
| Customer support apology | Direct, warm, accountable | "We dropped the ball on this. Here's what happened and what we're doing." |
| Marketing a download | Inviting, specific | "We made this for retail leaders thinking about commerce architecture. Grab it." |
| Crisis response | Sober, factual, clear | "On Tuesday, we identified an issue affecting [users]. Here's the impact and our response." |
| Internal team announcement | Direct, candid | "We're shifting priorities for Q2. Here's why and what changes." |
| Industry analysis / POV | Editorial, considered | "The composable wave isn't slowing — but the early adopters are running into issues no one warned them about." |
| Social hook | Specific, scroll-stopping | "Three patterns we keep seeing in failed composable DXP rollouts." |
| Sales follow-up | Warm, brief | "Here's the deck from yesterday. Two things to flag." |

Each tone shift must remain recognizably the brand voice. If the tone shift requires losing the voice attributes, the voice is too narrow.

## The do/don't pattern

The most useful single tool for voice rules is the do/don't pair. Specific examples beat abstract description.

```
Attribute: Direct, not blunt.

Do: "Email is missing. Add it to continue."
Don't: "Please fill in all required fields."

Do: "We've extended your trial to Friday. Anything else helpful?"
Don't: "We have processed your request and your trial has been extended."

Do: "Composable architecture isn't always the right answer. Here's when it isn't."
Don't: "There are situations in which composable architecture may not be optimal."
```

Aim for **5–8 do/don't pairs per voice attribute** in the brand guidelines.

## Filler-words bans (cite these)

The brand guidelines should explicitly ban consultant-cliché vocabulary. From [[ai-tells-forbidden-patterns]]:

**Banned for any Slalom-house brand voice:**
- elevate
- seamless
- unleash
- next-gen
- leverage
- synergy
- innovative
- best-in-class
- unlock
- empower
- revolutionize
- transform (used vaguely)
- cutting-edge
- world-class

**Replace with:** concrete verbs that name what actually happens.
- "elevate the customer experience" → "cut checkout abandonment by [N]%"
- "leverage AI" → "use [specific model] to [specific task]"
- "seamlessly integrate" → "integrates via [specific method]"

## AI-writing tics to reject

Beyond filler words, modern brand voice work has to actively reject AI-generated patterns:

| Tic | What it looks like | Fix |
|---|---|---|
| **Em-dash overuse** | Multiple em-dashes per paragraph for asides | Periods, commas, parentheses. Em-dash for genuine emphatic asides only. |
| **"Not X but Y"** | "It's not just an app — it's a movement" | Just say what it is. Direct claims beat comparative ones. |
| **Summary closer** | Every section ends with a recap sentence | End sections at the natural endpoint. Don't summarize what got said three sentences above. |
| **"Game-changing" / "revolutionary"** | Marketing hyperbole | Specific concrete impact. |
| **Three-item lists** | Everything organized as a comma-separated triple | Mix sentence structures. Sometimes use one. Sometimes use four. |
| **"In today's world..."** | Throat-clearing intro | Cut. Start with the substance. |
| **"As we navigate..."** | Vague journey metaphors | Specific verbs. |

For broader cleanup of AI-tic writing, route to `consulting-humanize` (consulting plugin).

## Voice authoring workflow

When asked to define brand voice:

1. **Read the brand strategy** from [[brand-strategist]]. Voice has to match positioning.
2. **Identify the audience** — voice for a CIO-audience B2B brand differs from voice for a consumer DTC brand.
3. **Pick 3–4 attributes with their not-X pairings.** Generate 8–12 candidates, group, narrow.
4. **Write 5–8 do/don't pairs per attribute.** Specific examples; not abstract.
5. **Build the tone register matrix.** Map 6–10 situations to tone shifts with example sentences.
6. **Compile the filler-words ban list** specific to this brand (inherits from the standard list above).
7. **Write voice in its own voice.** The voice page in the guidelines should *prove* the voice — its writing demonstrates the attributes.
8. **Test on real copy.** Apply to a current piece (a recent email, blog post, web hero). Does the new voice change anything? If not, the voice is too vague.

## Voice document structure

For a deliverable voice document (chapter in brand guidelines):

```
# Brand voice

## Who we are when we write
[One paragraph that proves the voice in its own writing.]

## Voice attributes (3–4)

### Attribute 1: {name}, not {adjacent trait}
- What it means
- Do / don't pairs (5–8)
- Example sentence

### Attribute 2: …
[Same structure]

## Tone register (situation → tone shifts)
[Matrix]

## What we don't write

### Banned filler words
[List with replacements]

### Banned AI-writing tics
[Em-dash overuse, "not X but Y," summary closers, etc.]

### Banned consultant-boilerplate patterns
[Specific to brand]

## Authority
- Who owns brand voice? Who approves edits?
- How to flag voice drift?
```

## Common voice failure modes

| Failure | Symptom | Fix |
|---|---|---|
| **Vague adjectives** | "Bold. Friendly. Trustworthy. Modern." | Pair each with not-X; add do/don't pairs |
| **Voice without examples** | Document describes voice but doesn't write in it | Voice page proves voice in its own writing |
| **Tone register missing** | Voice is defined but writers don't know how to flex | Add the situation→tone matrix |
| **Filler words still present** | "Elevate," "leverage," "seamless" appear in marketing copy | Add explicit ban list |
| **AI-tics in copy** | Em-dash overuse, "not X but Y" patterns | Add tic ban list; route serious offenders to `consulting-humanize` |
| **Voice that contradicts visual identity** | Premium-restrained visual + casual-playful voice | Realign voice with [[brand-identity-system]] register |
| **Voice attributes that drift in different directions** | "Bold + Calm" or "Direct + Diplomatic" — fighting attributes | Pick attributes that compose, not contradict |
| **Authority unclear** | Writers don't know who approves voice questions | Document approval owner |

## Hand-offs

- **[[brand-strategist]]** — for positioning that informs voice attributes.
- **[[brand-identity-system]]** — for visual register matching.
- **[[brand-guidelines-composer]]** — for compiling voice into the guidelines document.
- **[[brand-naming]]** — for ensuring names match the voice register.
- **`consulting-humanize`** (consulting plugin) — for AI-tic cleanup on specific copy.
- **`marketing-copywriter`** (marketing plugin) — for ongoing copy that applies the voice.
- **`design-document`**, **`design-presentation`** — when voice is being applied to specific deliverables.

## Constraints

- Don't define voice with adjectives alone. Pair with not-X and do/don't pairs.
- Don't ship a voice without examples in the voice's own writing.
- Don't create voice attributes that contradict each other.
- Don't ignore the filler-words ban list — even one "leverage" undermines the voice.
- Don't write voice in academic or corporate-distant style. Voice should feel like the brand's actual register.
- For Slalom Composable DXP voice: default register is **direct, not blunt + confident, not arrogant + curious, not credulous + practical, not preachy.** Adjust per client engagement.
