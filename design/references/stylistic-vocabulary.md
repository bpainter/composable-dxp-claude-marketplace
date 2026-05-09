---
type: reference
project: skills-library
scope: plugin
plugin: design
tags: [type/reference, plugin/design, scope/foundational, topic/design, topic/taste]
status: active
---

# Stylistic Vocabulary

Named visual languages, what each one *lands*, what each one *risks*, and when to reach for it. This is the vocabulary the `design-taste` skill draws from when answering "what should this feel like?" — and the constraint set that prevents the work from slipping into the AI-default center.

A style is not a mood board. It's a coherent set of decisions across type, color, layout, density, motion, and detail. Pick one as the dominant register, optionally borrow tactics from a second, but commit. Mixing four styles half-heartedly is how output ends up feeling generic.

For each style: **what it lands**, **what it risks**, **typography signal**, **color signature**, **layout posture**, **canonical examples**, **consulting-deck fit**.

---

## Editorial

A style that treats every page like a magazine spread. Owes to print typography, long-form journalism, and Nordic design publications. Reads as authoritative, considered, slow.

- **Lands:** trust, depth, credibility, slowness as a virtue.
- **Risks:** if the writing is thin, the form mocks the content.
- **Type:** quality serif (Tiempos, GT Sectra, Söhne Sans for body). Generous leading. Drop caps where appropriate. Tight tracking on headlines.
- **Color:** restrained. Black, off-white, one accent (often a desaturated rust, navy, or olive).
- **Layout:** asymmetric grid. Pull-quotes. Marginalia. Captions break the grid intentionally. Generous whitespace.
- **Examples:** Stripe Press, Wallpaper magazine layouts, *Works That Work*, the *NYT Magazine* digital essays.
- **Consulting fit:** **strong** for whitepapers, POVs, thought-leadership essays, executive briefings. Weak for sales decks (too slow), weak for marketing carousels (wrong rhythm).

## Swiss-modern (International Typographic Style)

The mid-century Swiss design movement — grid-driven, sans-serif, asymmetric, ruthlessly hierarchical. Reads as precise, neutral, expert.

- **Lands:** clarity, expertise, calm authority, "this team knows what they're doing."
- **Risks:** can feel cold or corporate-blank if the brief calls for warmth.
- **Type:** Helvetica Neue, Akzidenz-Grotesk, Söhne, Neue Haas Grotesk, Inter (Swiss is the one place Inter lands). Tight tracking. Heavy use of weight contrast.
- **Color:** primary-dominant white space, single bold accent (red/black/yellow are canonical). Often functional rather than decorative.
- **Layout:** strict grid (12-column or 6-column). Asymmetric within the grid. Type set at angles or rotated. Heavy reliance on negative space.
- **Examples:** Massimo Vignelli, Müller-Brockmann, the original NY Subway map, IBM brand work, Linear's marketing site, Vercel's product pages.
- **Consulting fit:** **strong** for technical decks, architecture diagrams, methodology documents, B2B SaaS proposals. The Slalom default register for technical work.

## Brutalist

Unapologetic, raw, "I refuse to please you." Owes to architectural Brutalism — exposed concrete, functional honesty. Reads as confident, anti-corporate, often anti-clean.

- **Lands:** rebellion, confidence, "we don't need your approval," anti-establishment.
- **Risks:** wrong for any audience that values polish or institutional trust. Easy to do badly (just-ugly is not Brutalist).
- **Type:** monospace prominent (JetBrains Mono, Iosevka, IBM Plex Mono). Or plain Helvetica/Arial used aggressively, no decoration. Sometimes raw system fonts.
- **Color:** stark. Black on white, or white on black. Maybe one chemical-bright accent (lime, electric cyan, traffic red).
- **Layout:** raw HTML aesthetic. Tables and lists left as tables and lists. Borders as architectural element. Sometimes deliberate visual chaos.
- **Examples:** Bloomberg terminal, Craigslist, Hacker News, Gwern.net, Marketing on Bloomberg, Are.na, brutalist-websites.com.
- **Consulting fit:** **niche.** Works for tech-native audiences (engineering leaders, developer-tools clients). Wrong for finance, healthcare, retail executive audiences.

## Minimal-soft

The dominant SaaS aesthetic of 2020–2025: rounded corners, subtle gradients, generous whitespace, soft shadows, friendly geometric sans-serifs. Reads as approachable, modern, calm.

- **Lands:** approachability, modernity, friendliness, "we're easy to work with."
- **Risks:** **this is the AI default.** Without strong signature elements, output ends up looking like every Y Combinator company. The 3-column-card-row ban exists for this style.
- **Type:** Geist, Inter, Plus Jakarta Sans, Outfit, Cabinet Grotesk. Medium weights. Generous line-height.
- **Color:** soft neutrals (warm grays, cream, off-white) with a single saturated accent. Subtle gradients permitted in hero areas only.
- **Layout:** 8pt grid. Cards with 12–16px radius. Soft shadows. Symmetric or near-symmetric. Pill-shaped CTAs.
- **Examples:** Linear (early), Notion, Stripe (post-2020), Vercel (early), Framer.
- **Consulting fit:** **medium.** Default for SaaS proposals. Easy to default to when the brief doesn't earn it. **Apply only when the brief calls for "approachable modern" specifically.**

## Neo-skeu / textured-realism

A reaction against flat minimalism. Embraces texture, depth, gradients, and physical metaphor — without going full skeuomorphic-2010. Reads as crafted, premium, sometimes nostalgic.

- **Lands:** craft, depth, premium feel, "this was made by someone who cares."
- **Risks:** crosses into kitsch fast. Requires craft execution to land.
- **Type:** quality sans (Söhne, Migra, GT Walsheim, Domaine Sans). Variable axes used intentionally.
- **Color:** rich neutrals with subtle gradients, real shadow play, sometimes paper or canvas textures.
- **Layout:** layered surfaces with real depth — backdrop blur, inner-shadow refraction edges, multi-stop gradients, glass panels. Liquid Glass refraction (1px inner border, subtle inner shadow).
- **Examples:** Apple Vision Pro marketing, macOS Big Sur+ system design, Arc browser, Things 3, Linear's recent (2025+) work.
- **Consulting fit:** **strong** for premium-positioning work, executive briefings where the medium signals investment, brand identity systems.

## Anti-design / low-fi

A deliberate rejection of "good design." Owes to zine culture, early-web aesthetics, indie publishing. Reads as authentic, hand-made, anti-polish.

- **Lands:** authenticity, personality, "a real person made this."
- **Risks:** wrong for institutional trust contexts. Can read as amateur if not committed.
- **Type:** typewriter fonts, system defaults, hand-drawn elements, deliberate font mixing.
- **Color:** photocopy black-and-white, risograph palette (fluorescent pink, pacific blue, mustard), or web-safe defaults (default link blue).
- **Layout:** intentionally rough. Hand-cut imagery. Asymmetric in an organic way. Rules deliberately broken.
- **Examples:** Are.na, *The Creative Independent*, Stripe Press's print work (specific essays), riso-printed zines.
- **Consulting fit:** **rarely.** Internal team rituals, retro decks, off-sites, internal "we shipped this" celebrations.

## Editorial-tech

Hybrid of editorial and Swiss-modern. Treats the technical content like long-form journalism. Reads as both expert and considered.

- **Lands:** authority on technical topics, "this team thinks deeply."
- **Risks:** blurs into Swiss if you don't lean editorial enough; blurs into vanilla if you don't lean technical enough.
- **Type:** sans-serif for body (Söhne, Inter, IBM Plex Sans), monospace for code/data (JetBrains Mono, Berkeley Mono), serif for pull-quotes (Tiempos, Charter).
- **Color:** editorial restraint with a single technical accent (electric blue, terminal green, or rust).
- **Layout:** asymmetric grid with sidenotes/marginalia for technical asides. Code blocks as design elements. Diagrams treated as figures with captions.
- **Examples:** Stripe documentation, Linear's changelog/blog, Increment magazine, *A List Apart*, Anthropic's engineering blog.
- **Consulting fit:** **strong** for technical whitepapers, architecture POVs, engineering-audience POVs, developer-relations content. **Slalom Composable DXP house style for technical thought leadership.**

## Dense-data / cockpit

Pilot's-cockpit information density. Owes to financial terminals, scientific instruments, ops-room displays. Reads as expert, data-rich, "for people who know what they're doing."

- **Lands:** expert-tool credibility, data-density-as-trust signal.
- **Risks:** unreadable for executives. Wrong for marketing surfaces.
- **Type:** mono prominent. Tabular numerals. Tight type. Heavy use of weight contrast in small sizes.
- **Color:** dark mode default. Functional color (red/green/yellow as alerts, not decoration). Single brand accent. Monospace numbers in tabular layout.
- **Layout:** dense grid. No card containers — `border-t` and `divide-y` only. Tiny paddings (p-2, p-3). Multiple data streams visible simultaneously.
- **Examples:** Bloomberg Terminal, Datadog, Grafana, trading dashboards, Linear's command palette, Raycast.
- **Consulting fit:** **niche.** Internal ops tools, data-platform demos, financial-services prototypes.

## Premium-restrained

Quiet luxury for B2B. Restrained palette, generous whitespace, careful typography, subtle motion, no shouting. Reads as established, confident, expensive.

- **Lands:** institutional trust, "we charge a premium because we earn it."
- **Risks:** can feel cold or distant if the brief needs warmth. Easy to do half-heartedly and end up just-bland.
- **Type:** quality sans (Söhne, GT Walsheim, Neue Haas Grotesk) or quality serif (Tiempos, Domaine, GT Sectra). Single weight family used with care.
- **Color:** deep neutrals with one quiet accent. Often a single deep brand color used sparingly. No gradients on primary surfaces.
- **Layout:** generous whitespace, asymmetric grid, hero left-aligned not centered. Small type for body, large type for moments.
- **Examples:** Stripe (current), Linear (recent), Apple's marketing pages (when restrained), Berkshire Hathaway's letter design, Hermès digital, Loro Piana digital.
- **Consulting fit:** **strong** default for executive-audience proposals, premium client work, partner-firm marketing. **Slalom Composable DXP house style for client-facing decks.**

## Maximalist / vibrant

Saturated color, layered illustration, heavy motion, dense visual content. Reads as energetic, brand-confident, fun.

- **Lands:** energy, brand confidence, "this team has personality."
- **Risks:** wrong for trust-building contexts. Easy to overwhelm signal.
- **Type:** display fonts as brand statement. Variable fonts used expressively. Multiple weights and styles per surface.
- **Color:** saturated palette of 4–6 colors. Color used as primary brand signal. Gradients and color-blocking heavily used.
- **Layout:** layered, overlapping, scrolling. Illustration as architecture. Bold geometric shapes. Color blocking.
- **Examples:** Mailchimp, Slack (early), Klarna, Glossier, brand-led DTC e-commerce.
- **Consulting fit:** **rarely.** Internal-rallying surfaces, off-site decks, employer-brand work, consumer-DTC client work.

---

## Picking a style for a brief

Don't pick a style first. Answer the intent-first questions (`design-taste`), produce the four required outputs (Domain, Color world, Signature, Defaults to reject), *then* the style register usually selects itself.

Default registers for Slalom Composable DXP work:

| Surface | Default register | Alternates |
|---|---|---|
| Technical whitepaper / POV | Editorial-tech | Editorial, Swiss-modern |
| Executive proposal deck | Premium-restrained | Swiss-modern, Editorial |
| Architecture deck | Swiss-modern | Editorial-tech, Dense-data |
| Solution brief (8.5×11) | Editorial-tech | Premium-restrained |
| LinkedIn carousel | Premium-restrained or Editorial-tech | Minimal-soft only if brief calls for it |
| Instagram (rare for DXP) | Minimal-soft | Maximalist if consumer-brand context |
| Internal off-site / retro | Anti-design or Maximalist | Brutalist for engineering audiences |
| Product UI mockup | Swiss-modern + minimal-soft hybrid | Dense-data for ops/data tools |

---

## Mixing styles (rare, careful)

You can borrow tactics across styles, but commit to one as dominant. Example pairings that work:

- **Swiss-modern + Dense-data** — technical proposals where data tables are first-class citizens.
- **Editorial + Editorial-tech** — long-form thought leadership with deep technical chapters.
- **Premium-restrained + Neo-skeu** — when the brief calls for "expensive" and the surface (hero, cover, key moment) earns extra craft.

Pairings that fail:

- **Minimal-soft + Brutalist** — fights itself.
- **Editorial + Maximalist** — incompatible registers.
- **Swiss-modern + Anti-design** — Swiss-modern *is* the establishment Anti-design rejects.

---

## How this reference is used

- **Loaded by:** [[design-taste]], [[design-presentation]], [[design-document]], [[design-social-asset]], [[design-screen]], [[brand-strategist]], [[brand-designer]] (during split: `brand-identity-system`).
- **Pairs with:** [[visual-design-foundations]] (the *what readers see first* layer), [[interaction-patterns]] (the dynamic layer), [[ai-tells-forbidden-patterns]] (the explicit bans).

When the user asks "what style should this be?", point them at this file. When the user gives a brief, use this catalog to select a register and then commit to it across all decisions in `design-taste`'s before-each-artifact metadata block.
