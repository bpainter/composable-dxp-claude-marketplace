---
type: reference
project: skills-library
scope: plugin
plugin: design
tags: [type/reference, plugin/design, scope/foundational, topic/design, topic/iconography, topic/tool]
status: active
---

# Tool: Flaticon

The sourcing protocol for [Flaticon](https://www.flaticon.com). Loaded by [[design-iconography]] when the project needs illustrative or decorative icons beyond what UI libraries (Lucide, Heroicons, Phosphor, Tabler) cover.

Flaticon is the default for **non-UI iconography** in decks, documents, social assets, and brand applications. It's a paid/freemium icon marketplace — millions of icons in dozens of styles. The breadth is the value; the discipline is staying consistent within one style across an asset family.

## Slalom practice override (why we use Flaticon over the official set)

Slalom ships an official illustrated icon set ("Icon Set 1," 130+ icons in the Creative Library + embedded in the PowerPoint template). For Bermon's practice and our customer-facing work, we **override** that set:

> **The official Slalom illustrated icons lean kindergarten/childish in places.** We replace them with refined, professional icons sourced from Flaticon.

**Defaults for our practice:**

- **Primary style:** outlined icons in **black or Slalom brand colors** (`#0C62FB` Slalom Blue, `#002FAF` Slalom Dark Blue).
- **Secondary style:** colored icons using **fill, outline, or lineal-color** styles — chosen to match the asset family.
- **Style discipline:** pick ONE Flaticon style family per deliverable. Hold it across every icon.

**Override applies to:**

- Customer-facing decks (proposals, whitepaper supporting decks, executive briefs).
- Whitepapers, POVs, articles, playbooks.
- Solution briefs, marketing assets.
- Any asset where we want to read as premium-restrained.

**Override does NOT apply to:**

- Internal Slalom comms or templates that explicitly call for the illustrated set.
- ERG-branded materials (use the ERG Style Guide).
- Slalom.com (use the Lucide-equivalent icon system per the web design system).

See [[../../../50_Knowledge/Frameworks/Slalom_Brand/Iconography]] for the full picture.

## When Flaticon is the right tool

- Decks needing illustrative or sectorial icons (industry-specific, decorative).
- Documents needing callout / sidebar icons in a brand style.
- Social assets needing visual style beyond UI-icon flatness.
- Brand applications where a custom icon set isn't budgeted but a curated style is needed.

## When Flaticon is NOT the right tool

- Product UI — use Lucide, Heroicons, Phosphor, or Tabler. UI consistency matters more than visual variety.
- Brand identity (logos, marks) — commission custom.
- Anywhere the license model conflicts with the deliverable.

## License model (read carefully)

Flaticon icons fall into three buckets:

### Free icons
- Free to use **with attribution** ("Icons by [author] from Flaticon").
- Attribution must appear in the deliverable or in proximity to the icon.
- For deliverables where attribution is impractical (logos, decks distributed outside Slalom), use the premium license.

### Premium icons (Flaticon Premium subscription)
- No attribution required.
- Allowed in commercial projects.
- The Slalom default if we have a Flaticon Premium subscription.

### Pro / paid individual icons
- Some icons require individual purchase.
- Use case dependent.

**Always verify license per icon.** Flaticon's UI shows the license model on each icon's detail page. Don't assume.

## Sourcing protocol

When a deliverable needs Flaticon icons:

1. **Confirm license access.**
   - Does Slalom (or the project) have Flaticon Premium?
   - If yes, proceed with premium icons.
   - If no, restrict to free icons with attribution discipline, or use a free alternative library (Lucide / Phosphor) and cut decorative breadth.

2. **Pick a style family.**
   - Flaticon offers icons in many styles: flat, outline, filled, isometric, gradient, hand-drawn, etc.
   - **Pick ONE style.** Hold it across the deliverable.
   - When searching, use the style filter to constrain results.

3. **Search and select.**
   - Search for the concept (e.g., "data pipeline").
   - Filter by style.
   - Filter by license type (free / premium) if relevant.
   - Pick icons that match the chosen stroke weight, fill style, and visual register.

4. **Download in correct format.**
   - **SVG preferred** — scales cleanly, can be color-edited.
   - PNG at 512×512 minimum if SVG unavailable.
   - Avoid raster JPGs (no transparency).

5. **Customize for the brief.**
   - **Color:** edit the SVG to match brand tokens. Most Flaticon icons are single-color or two-color — easily themeable.
   - **Stroke weight:** if pulling from outline icons, verify weight matches your icon-system spec (1.5px or 2px). Adjust if needed.
   - **Sizing:** export to the size scale defined in [[design-iconography]].

6. **Track attribution if free-license.**
   - Maintain a `flaticon-attributions.md` per project listing every icon used, the author, and the URL.
   - Place attribution in the deliverable as required.

## Style family discipline

Flaticon's styles vary widely. For Slalom practice work, use these Flaticon style names directly when filtering:

| Flaticon style | When (Slalom practice) |
|---|---|
| **Outline** (1.5–2px stroke, single color) | **Default for our customer-facing decks, whitepapers, POVs, articles.** Color = black or Slalom Blue. |
| **Lineal Color** | Secondary default — outlined icons with restrained color fills. Use Slalom palette colors only. |
| **Linear color** | Same as Lineal Color (Flaticon naming variation). |
| **Fill / Solid** | Use sparingly for emphasis or when outline reads too thin at small sizes. Single color from the Slalom palette. |
| **Filled outline** | Hybrid — outline + selective fill. Works for editorial registers. |
| **Flat** | Multi-color flat avoids; reads as consumer SaaS. **Not our default.** |
| **Gradient** | Avoid; the Slalom brand is flat. **Banned for our practice work.** |
| **Isometric** | Niche use only — process/architecture explanatory diagrams. **Not for general icons.** |
| **Hand-drawn** | Avoid — reads kindergarten, which is the very style we're overriding. **Banned.** |
| **Glyph** (mono filled) | Dense data displays only. Rare. |

**Pick one Flaticon style family per deliverable. Don't mix.**

A deck with Flaticon outline icons next to Flaticon lineal-color icons reads as inconsistent. The whole point of the override is style discipline.

## Customization workflow

When pulling Flaticon SVG into a deliverable:

```
1. Download SVG.
2. Open in editor (Figma, Illustrator, or text-edit the SVG directly).
3. Replace fill/stroke with brand color tokens or currentColor.
4. Verify stroke weight matches icon-system spec.
5. Export to PNG/SVG at the deliverable's size scale.
6. (If multi-icon deliverable) batch-process all icons together to verify consistency.
```

For batch consistency, use Figma's "Replace selection with..." or Illustrator's symbols feature to swap icons en masse and verify alignment.

## Attribution template (when using free-license icons)

In the deliverable's footer or back-matter:

```
Icons by {Author Name} from Flaticon.
{URL of icon page}
```

For decks: small caption on the closing slide or back cover.
For documents: in the colophon or footnotes.
For social: link in caption or first comment.

## What to avoid

- **The "many icons many styles" pattern** — using 5 icons each in a different Flaticon style. Reads as random.
- **Multi-color flat for B2B** — bright multi-color flat icons read as consumer / SaaS-startup. Wrong register for premium consulting.
- **Free icons without attribution** — license violation; expensive to fix at scale.
- **Premium icons without verifying license** — some require special licenses for embedded redistribution.
- **Flaticon icons in product UI** — use Lucide / Phosphor / Tabler instead for consistency with shadcn.
- **Flaticon icons in brand identity** — commission custom for marks, logos, brand-system iconography.

## When to escalate to custom commission

If the project needs a coherent icon set that no Flaticon style perfectly matches — and the deliverable will be reused, redistributed, or trademark-protected — escalate to custom commission. Brief via [[brand-identity-system]] (brand plugin).

Custom commission is right for:
- Brand identity work (icon set as part of visual identity).
- Marquee deliverables (a flagship whitepaper that defines the brand for years).
- Industries where the visual language doesn't exist in stock libraries.

Custom commission is wrong for:
- One-off decks or documents.
- Internal-use deliverables.
- Anything where the timeline is < 2 weeks.

## Cross-references

- **Skill:** [[design-iconography]] (the workflow that uses this tool).
- **Surface:** all surface skills can cite Flaticon for non-UI icons.
- **Brand:** [[brand-identity-system]] (brand plugin) for custom commission briefs.
- **License:** verify per icon at flaticon.com; for organization-level subscription, check with the team.
