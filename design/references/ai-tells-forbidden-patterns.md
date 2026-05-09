---
type: reference
project: skills-library
scope: plugin
plugin: design
tags: [type/reference, plugin/design, scope/foundational, topic/design, topic/taste]
status: active
---

# AI Tells — Forbidden Patterns

The named, surface-extended catalog of AI default patterns that make output feel generic. Cite these by name in critique. The bans are non-negotiable defaults — override only with explicit user direction.

This file extends the bans named in [[design-taste]] across surfaces: web/product UI, presentations (16:9), documents (8.5×11), social (LinkedIn / Instagram), and brand identity application.

For each ban: **the tell**, **why it shows up**, **the override**.

---

## Color tells

### The LILA ban (purple/blue AI gradient)
- **Tell:** Purple-to-blue gradient hero. Indigo glow on buttons. Saturated accent at 70%+ saturation. Often paired with dark background for "premium tech."
- **Why it shows up:** ChatGPT and DALL-E branding made this the AI-default. Stable Diffusion gradients reinforced it. Models generate it because it dominates training data.
- **Override:** Desaturated neutrals (Zinc, Slate, Stone) as base. Single high-contrast accent at <60% saturation. If a gradient must exist, use a tonal one (within the same hue family) or an off-axis pairing the audience wouldn't predict (rust + slate, olive + cream, terracotta + ink).

### The pure-black ban
- **Tell:** `#000000` used as primary or text color.
- **Why it shows up:** It's the lazy default. Models produce `#000` because that's what the prompt says when the prompt says "black."
- **Override:** Off-Black (`#0A0A0A`), Zinc-950 (`#09090B`), Charcoal (`#1A1A1A`). Pure black is for OLED-only contexts and ink-heavy print.

### The neon-glow ban
- **Tell:** `box-shadow` with a saturated colored shadow. "Glowing" buttons. Drop shadows in brand colors.
- **Why it shows up:** Default tutorial pattern. Looks "modern" without effort.
- **Override:** Tinted shadow (shadow color = background hue with tint). 1px inner border for refraction edge. Subtle elevation, not theatrical glow.

### The rainbow-gradient text ban
- **Tell:** Headlines with text-fill gradients running across multiple colors.
- **Why it shows up:** Looks "designed." Easy to apply.
- **Override:** Solid color, possibly with subtle character-by-character variance for moments. Save gradient text for one moment per surface, max — a callout or a CTA, not a section header.

### The two-warring-primaries ban
- **Tell:** Two primary colors of equal saturation fighting for attention. No clear hierarchy.
- **Why it shows up:** Models pull from training data with split-brand systems.
- **Override:** One accent earns its place. Other "primary" colors become functional (success, warning, error) at calibrated saturation levels.

---

## Typography tells

### The Inter ban (for premium briefs)
- **Tell:** Inter as the default sans-serif on a brief that calls for premium, creative, editorial, or distinctive.
- **Why it shows up:** Inter is free, ubiquitous, technically excellent. Models generate it as the safe sans-serif.
- **Override:** Geist, Outfit, Cabinet Grotesk, Satoshi, Söhne, Neue Haas Grotesk, Migra, GT Walsheim. Pair with a quality serif (Tiempos, GT Sectra, Domaine, Charter) for editorial register.
- **Exception:** Inter is fine — even excellent — for pure utility UI, dashboards, and Swiss-modern technical work.

### The serif-on-dashboard ban
- **Tell:** A serif font used for navigation labels, table headers, or button labels in a dense data interface.
- **Why it shows up:** Models confuse "premium" with "serif."
- **Override:** Sans-serif for technical/dense UI. Reserve serif for editorial register, hero moments, or brand identity touches.

### The oversized-H1 ban
- **Tell:** Hero headlines at 80px+ on desktop with no proportional structure to support them. The H1 *screams*.
- **Why it shows up:** Looks "bold." Easy to dial up.
- **Override:** Hierarchy comes from weight, color, surrounding negative space, and tracking — not just size. A 48px headline with the right tracking and color contrast lands harder than a 96px headline yelling.

### The arbitrary type-scale ban
- **Tell:** Sizes like 15px, 17px, 19px scattered across components. No discernible scale.
- **Why it shows up:** Fine-tuning by eye in the design tool, never extracted to a system.
- **Override:** Modular scale (12, 14, 16, 18, 24, 32, 48, 64) or modular ratio (1.25× or 1.333×). Document it as a token. Use it.

### The seven-heading-levels ban
- **Tell:** A document or page with H1, H2, H3, H4, H5, H6 all in use.
- **Why it shows up:** Default markdown rendering. Semantic structure dragged into visual structure.
- **Override:** 2–3 visible heading levels plus body. Use weight and color for sub-hierarchy below that.

---

## Layout tells

### The centered-hero ban (variance > 4)
- **Tell:** Centered headline + centered subheadline + centered CTA + centered hero image. The vertical-stack-of-centered-things pattern.
- **Why it shows up:** Easiest layout to generate. Safe. Symmetric. Forgettable.
- **Override:** Split-screen (50/50 left content / right asset, or 38/62 golden ratio). Asymmetric whitespace (`padding-left: 20vw`). Left-aligned content with right-aligned asset that bleeds off-edge.

### The 3-column card-row ban
- **Tell:** Three equal-width cards in a horizontal row, each with icon + headline + 2-line description. The "feature row."
- **Why it shows up:** Bootstrap default. Tailwind UI default. Models trained on every SaaS marketing page.
- **Override:** 2-column zig-zag (alternating left-right). Asymmetric grid (2fr 1fr 1fr). Bento layout. Horizontal scroll. Vertical accordion.

### The card-overuse ban (density > 7)
- **Tell:** Every piece of content wrapped in a card with 12px radius and a soft shadow. Cards within cards.
- **Why it shows up:** Tutorial defaults. Material Design's card-everywhere influence.
- **Override:** Above density 7, cards earn their place only when elevation communicates real hierarchy. Otherwise: `border-t`, `divide-y`, or pure negative space for grouping. Data metrics breathe.

### The broken-grid ban
- **Tell:** Asymmetric layouts that don't gracefully collapse on mobile. Horizontal scrolling on phone widths. Content cut off.
- **Why it shows up:** Models generate desktop-first compositions; mobile collapse is an afterthought.
- **Override:** Any layout above DESIGN_VARIANCE 4 must have an explicit mobile-collapse story (single-column, `w-full`, `px-4`, no horizontal scroll). Test at 375px.

### The mobile-h-screen bug
- **Tell:** `h-screen` (100vh) used for full-height hero on mobile, causing content jump when iOS Safari URL bar appears/disappears.
- **Why it shows up:** Tutorial default before `dvh` was widely supported.
- **Override:** `min-h-[100dvh]` for full-height sections on mobile. Test on real iOS Safari.

---

## Content and data tells

### The Jane Doe effect
- **Tell:** Placeholder names like "John Doe," "Sarah Chan," "Jack Su," "Jane Smith." Stock-photo-grade names that signal "I didn't think about this."
- **Why it shows up:** Models default to common training-data names.
- **Override:** Realistic, contextually appropriate names. For Slalom client work: use real names with permission, or label clearly as `{Client Name}`. For internal demos: use names that fit the deck context (e.g., a retail deck uses retail-CMO names, not generic ones).

### The Acme/Nexus/SmartFlow ban
- **Tell:** Company names like "Acme Corp," "Nexus," "SmartFlow," "Cloudify," "Streamline AI."
- **Why it shows up:** Tutorial-grade fake brand names that scream "AI-generated."
- **Override:** Real client names (with permission), or brief-specific fictional names with backstory ("Birchwood Retail," "Kestrel Industries" — names that suggest a believable company). For pure placeholders, label as `{Company Name}` rather than inventing a fake.

### The fake-numbers ban
- **Tell:** `99.99% uptime`, `50% increase`, `+25%`, `1234567`, `(555) 555-5555`.
- **Why it shows up:** Round numbers feel "designed." Models default to clean fractions.
- **Override:** Organic, slightly messy data. `47.2%` not `50%`. `+1 (312) 847-1928` not `(555) 555-5555`. `$3.4M ARR` not `$1M revenue`. Real-looking, not pretend-real.

### The filler-words ban
- **Tell:** "Elevate," "seamless," "unleash," "next-gen," "leverage," "synergy," "innovative," "best-in-class," "unlock," "empower," "revolutionize," "transform" (used vaguely), "cutting-edge," "world-class."
- **Why it shows up:** AI training data is saturated with marketing-speak. Models default to safe corporate vocabulary.
- **Override:** Concrete verbs that name what actually happens. Not "elevate the customer experience" — "cut checkout abandonment by [N]%." Not "leverage AI" — "use [specific model] to [specific task]."

### The em-dash overuse
- **Tell:** Multiple em-dashes per paragraph, used for parenthetical asides where commas or periods would work.
- **Why it shows up:** GPT-4 and Claude over-rely on em-dashes structurally.
- **Override:** Periods, commas, parentheses. Em-dashes for genuine emphatic asides only — not as a default rhythm.

### The "not X but Y" cliché
- **Tell:** Sentences structured as "It's not X — it's Y." Comparative dramatic framing.
- **Why it shows up:** Common in AI output. Reads as overwritten.
- **Override:** Just say what it is. Direct claims beat comparative ones.

### The "summary closer" tic
- **Tell:** Every section ends with a recap sentence that says what the section just said.
- **Why it shows up:** AI training reinforced this as polite structure.
- **Override:** End sections at the natural endpoint. Don't summarize what just got said three sentences above.

---

## Stock and placeholder tells

### The Unsplash defaults ban
- **Tell:** Hands-on-keyboard, back-of-someone's-laptop, abstract-tech-blob, generic-team-meeting, sunset-over-mountains-as-metaphor.
- **Why it shows up:** Free stock photo defaults. Models reference what's free and ubiquitous.
- **Override:** Commission via higgsfield (see [[tool-higgsfield]]). Use `https://picsum.photos/seed/{specific-brief-string}/W/H` for placeholders. Use brand-shot photography. Use abstract typographic compositions instead of stock photos.

### The egg-avatar ban
- **Tell:** Default SVG silhouette user icons. Lucide `<User />` icon as avatar placeholder.
- **Why it shows up:** Default placeholder pattern.
- **Override:** Specific photo placeholders (commissioned or licensed). Initials in a circle with brand-colored background. Geometric pattern unique to that user (deterministic from name hash).

### The shadcn-default ban
- **Tell:** shadcn/ui in its out-of-the-box state — default radius (`0.5rem`), default neutrals (Slate or Zinc 50/100), default shadows.
- **Why it shows up:** shadcn is the dev default. Models generate it without customization.
- **Override:** Customize the radii (sharper for technical, softer for approachable), custom neutrals (warm or cool depending on intent), custom elevation scale. The theme should announce the brief, not announce shadcn.

---

## Surface-specific tells: presentations (16:9)

### The "agenda → context → solution → benefits → next steps" ban
- **Tell:** Standard McKinsey-template deck structure. Agenda slide. Three-column "what we'll cover." Pillar slides. Closing "questions?" slide.
- **Why it shows up:** It's the expected consultancy structure. Models generate it as the safe shape.
- **Override:** Open with a claim, not an agenda. Structure by argument, not by template. Closing slide says something — a specific question, a commitment, a number — not "questions?".

### The five-pillar ban
- **Tell:** A slide with five identical icon-headline-blurb columns presenting the five things you do.
- **Why it shows up:** It's how every consultancy positions itself. Identical to every competitor.
- **Override:** Show the differentiation, not the symmetry. Often: pick the *one* pillar that matters most and structure the slide around it. Or: zigzag layout that contrasts pillars rather than equalizing them.

### The headshot-grid ban
- **Tell:** A "meet the team" slide with N headshots in a grid, all same size, all same crop.
- **Why it shows up:** Default template. "Show the team" instinct.
- **Override:** One headshot at a time, with a sentence the person said. Or: drop the grid, list names with one-line credentials. Or: skip the team page entirely if the meeting is small enough that names are already known.

### The closing-Q-and-A ban
- **Tell:** Final slide reads "Questions?" or "Thank you."
- **Why it shows up:** Default pattern.
- **Override:** Final slide says something specific. A commitment ("We'll share the architecture diagram by Tuesday"). A question we want answered ("What would have to be true for you to short-list us?"). A number the audience should remember ("$3.4M, 6 months, two SKUs").

### The dense-text-slide ban
- **Tell:** A slide with a paragraph of body text in 14pt or smaller. Eight bullets.
- **Why it shows up:** Slide-as-document instinct.
- **Override:** One claim per slide. The supporting detail goes in speaker notes or a paired document. If the slide has more than 30 words, split it.

---

## Surface-specific tells: documents (8.5×11)

### The "executive summary that summarizes the document" ban
- **Tell:** A page-one exec summary that lists everything covered in the document.
- **Why it shows up:** Template default.
- **Override:** The exec summary makes a claim and earns the rest of the document. "Composable DXP cuts time-to-launch by 40% if you stage the rollout this way" — and the rest defends that claim.

### The "our approach" five-pillar page ban
- **Tell:** Page in the document with five identical pillar boxes describing the methodology.
- **Why it shows up:** Methodology theatre. Same problem as the five-pillar deck.
- **Override:** Methodology shows up as concrete steps with concrete artifacts. Skip the pillar page. Or: show the methodology by example (one real engagement's path).

### The full-bleed-stock-image-cover ban
- **Tell:** Document cover with a full-bleed Unsplash photo, big serif headline, small subhead, logo bottom-right.
- **Why it shows up:** Marketing whitepaper template.
- **Override:** Typographic cover (no image), or commissioned image, or a data-driven cover (the chart that proves the document's claim).

### The "about us" boilerplate
- **Tell:** A page in the document with the firm's history, employee count, geographic footprint.
- **Why it shows up:** Template inheritance.
- **Override:** Skip it (the audience already knows or can look it up) or replace it with one specific signal — the engagement that's most relevant to this audience, not the firm-overview boilerplate.

### The "pillar boxes with check-marks" ban
- **Tell:** A page-grid of capabilities with green checkmarks on each.
- **Why it shows up:** Sales-collateral default.
- **Override:** Strike the checkmark grid. If you must list capabilities, prose them. Or: show the capabilities as a comparison-with-implications table.

---

## Surface-specific tells: social (LinkedIn / Instagram)

### The teal-quote-card ban
- **Tell:** Quote in white text on a teal or navy box. Headshot in corner. Logo bottom.
- **Why it shows up:** Default LinkedIn carousel template.
- **Override:** Quote as type-driven moment (no box). Photography-led. Or: drop the quote-card format entirely if the quote isn't memorable.

### The "10 things you didn't know" ban
- **Tell:** Listicle carousels with arbitrary numbered items. Generic insights ("Tip 1: ask better questions").
- **Why it shows up:** Engagement-bait template.
- **Override:** A genuine list (4–7 items) where each item is specific. Or: drop the list structure. A single-claim post often outperforms a list-bait carousel.

### The "lessons learned" ban
- **Tell:** Carousel with "5 lessons from [my project / my failure / my experience]" + generic platitudes.
- **Why it shows up:** LinkedIn template. Engagement-engineered.
- **Override:** Specific, surprising lessons that name names and dates. Or: drop the format.

### The personal-photo-of-author-pointing ban
- **Tell:** Photo of the author pointing at the camera or at imagined text. Looking-thoughtful pose.
- **Why it shows up:** Stock-photo posing default.
- **Override:** Real-context photo (at a desk, at a whiteboard, at a client site). Or: no photo of author at all.

### The motivational-quote-graphic ban
- **Tell:** A quote from Steve Jobs / Marie Curie / a generic CEO on a branded background.
- **Why it shows up:** Easy content. Easy template.
- **Override:** Don't post motivational quotes. If the quote is genuinely earned (you said it, in a real context), say it as a post.

---

## Surface-specific tells: brand identity application

### The "geometric circle logo" ban
- **Tell:** A circle, a swoosh, a leaf, or a stylized abstract shape that could belong to any tech company. Often paired with a single-syllable brand name in a geometric sans.
- **Why it shows up:** AI-image-gen default. Trademark-safe-by-blandness.
- **Override:** Brand mark grounded in the brand's actual story. Or: a wordmark only (no symbol). Or: a custom letterform.

### The five-color-brand-palette ban
- **Tell:** A brand palette with five equally weighted colors. Each "color" has equal status.
- **Why it shows up:** Template thinking. "A real brand has many colors."
- **Override:** Two-color foundational palette (one ink, one accent) plus functional colors and neutrals. The brand owns one color, prominently.

### The "personality" trait grid ban
- **Tell:** Brand guideline page with four "personality traits" each illustrated with a stock-photo treatment ("Bold. Friendly. Trustworthy. Modern.").
- **Why it shows up:** Brand-deck template.
- **Override:** Voice attributes with specific do/don't pairings, not adjectives in boxes. The voice page proves the voice in its own writing.

---

## How this reference is used

- **Loaded by:** [[design-taste]], [[design-presentation]], [[design-document]], [[design-social-asset]], [[design-screen]], [[design-audit]].
- **Cited in critique:** When reviewing output, cite the specific ban by name. "This is the centered-hero ban above variance 4 — propose split-screen alternative." Naming the failure mode prevents handwaving.
- **Override authority:** The user can explicitly override any ban. "I want a centered hero; the brief is corporate-conservative" — fine, override stated. The bans are *defaults*, not absolutes. But the override has to be stated.

For the dial system that contextualizes these bans, see [[design-taste]].
For the stylistic registers each ban serves, see [[stylistic-vocabulary]].
For the post-production checks, see [[pre-delivery-checklist]].
