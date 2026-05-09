---
type: reference
project: skills-library
scope: plugin
plugin: design
tags: [type/reference, plugin/design, scope/foundational, topic/design]
status: active
---
# Visual Design Foundations

Layout, hierarchy, color, space, and depth principles that apply across every design skill in this plugin. Read this once per session when working on visual design tasks; reference specific sections when reviewing or critiquing.

These are the *what-readers-see-first* principles — the foundational moves that make a UI feel either crafted or amateur, before anyone clicks anything.

Source synthesis: established design literature (Tufte, Lupton, Norman, Bringhurst) plus the curated practitioner posts at [@designmotionhq](https://www.instagram.com/designmotionhq/).

---

## 1. Hierarchy and emphasis

A user's eyes follow size, weight, and color before they read words. If everything is emphasized, nothing is.

### 1.1 The rule of one

One screen → one primary action → one piece of content the user is supposed to look at first.

If the user can't tell where to look in 3 seconds, the hierarchy is broken.

### 1.2 Size doubles when intent doubles

When an element matters more, it shouldn't be 10% bigger — it should be **2× bigger**, sometimes more. Subtle hierarchy is invisible hierarchy.

```
Headline:  48px / weight 700
Subhead:   24px / weight 500
Body:      16px / weight 400
Caption:   12px / weight 400
```

The ratio between levels matters more than the exact numbers. Aim for at least 1.5× between adjacent levels.

### 1.3 The 1.618 (Golden Ratio)

When in doubt about a proportion — column widths, image aspect, padding ratios — try 1:1.618. It works because it's approximately the ratio that maximizes the difference between adjacent levels while staying harmonious.

Practical: a card that's 320px wide → ~518px tall. A two-column layout → 38% / 62%.

This isn't sacred. But it's a useful starting point when nothing else suggests a ratio.

### 1.4 Visual weight beats word count

A small bright button outperforms a large gray one. Don't fight visual weight with explanation.

---

## 2. Spacing and proximity

### 2.1 Proximity = relationship

The single most underused tool in interface design. **Close = related. Far = separate.**

```
[label]                    [label]
[input]                    [input]                  ← related
                                                    ← gap defines relationship
[label]                    [label]
[input]                    [input]                  ← unrelated, larger gap
```

If two things are related, they should be 8–16px apart. If they're not, they should be 32px+ apart. The gap *is* the design.

### 2.2 Three types of white space

- **Micro** — padding inside elements (buttons, inputs, cards). The breathing room between content and container. Typical: 8–24px.
- **Macro** — margins between sections. The vertical rhythm of a page. Typical: 48–96px.
- **Active** — intentional emptiness to draw focus. The space that makes one element pop. Often appears as 2–4× the macro spacing on either side of a hero element.

White space is content. Treat it as a positive element, not "leftover space."

### 2.3 Element spacing fixes 80% of "this looks wrong"

When a screen looks "off" but you can't say why, **measure the gaps**. Most design problems are spacing problems:

- Items in a list that should feel grouped → reduce gap
- Sections that should feel distinct → increase gap
- Comments / threaded content → consistent 16–18px gap

### 2.4 Intentionally large gaps

A larger-than-expected gap creates emphasis. The space *around* an element is part of how it reads. A button surrounded by 80px of nothing feels more important than the same button packed in.

---

## 3. Color

### 3.1 Adjacent hues and the 60° rule

On a color wheel, hues that are 60° apart feel naturally harmonious. They're related but not identical — close enough to feel like a system, far enough to feel intentional.

For a primary + secondary palette, sample two hues 60° apart on the wheel. For a triadic accent, go 120°. For complementary tension, go 180° (use sparingly — it's loud).

### 3.2 Simultaneous contrast

**The same color reads differently against different backgrounds.** A "neutral gray" on white looks dark; the same gray on black looks light. The same orange button on a light background and on a dark background appears to be two different oranges.

Practical implications:
- Don't color-pick a value once and assume it works in both light and dark mode. Test in context.
- Component tokens should reference *semantic intent* (`--color-button-primary`) not hex values, because the same intent needs different values per context.
- Brand colors usually require adjustment for light vs dark mode. Pure brand color often becomes either too dim (in dark mode) or too saturated (in light mode).

### 3.3 Contrast for readability

WCAG minimums:
- **4.5:1** for body text
- **3:1** for large text (18pt+) and UI components / states
- **3:1** for focus rings against the focus target

If text fails these ratios, it's not stylish — it's unreadable for a meaningful percentage of users. Fix the color, don't make the user squint.

### 3.4 The psychology of saturation

- High saturation reads as energetic, urgent, attention-grabbing → use sparingly, on CTAs and alerts
- Low saturation reads as calm, professional, neutral → use for body and chrome
- Desaturating disabled states is more readable than just adding opacity

---

## 4. Shape and corners

### 4.1 The psychology of border-radius

- **0px (sharp corners)**: serious, technical, brutal, editorial
- **4–8px**: practical, neutral, the default
- **12–20px**: friendly, approachable, modern app
- **24–40px+**: soft, playful, casual
- **Pill (full radius)**: action-oriented, often CTAs

Pick a radius scale once and stick to it. Mixing 4px, 12px, and 20px on the same screen looks like an accident.

### 4.2 Border-radius nesting

When a child element sits inside a parent with rounded corners, the child's radius should equal `parent_radius − padding`:

```
outer: 32px radius
padding: 14px
inner: 32 − 14 = 18px radius   ← matches the visual edge
```

If the inner radius equals the outer, the inner shape looks like it's bursting out. If it's significantly less, the inner shape looks miscalculated. The math makes the rounded edges visually parallel.

### 4.3 The "every element rounded" rule

Pick a side. Either every element on the screen has rounded corners (consistent rounded look) or every element is sharp. Mixing — a sharp card with rounded buttons — almost always looks unintentional.

---

## 5. Depth and elevation

### 5.1 Backdrop blur as elevation

A premium-feeling modal blurs what's behind it (`backdrop-filter: blur(20px)`). The blur creates real depth — the modal feels like it's floating above the canvas, not pasted on top.

Depth via blur:
```css
.modal-backdrop {
  backdrop-filter: blur(20px);
  background: rgba(0, 0, 0, 0.4);
}
```

### 5.2 Shadows tell the user something is interactive

A flat element with no shadow reads as background. A subtle drop shadow reads as foreground / interactive. Use this to signal what the user can click.

Shadow scale (consistent z-axis):
```
elevation-0: none                                          (background)
elevation-1: 0 1px 2px rgba(0,0,0,0.05)                    (cards)
elevation-2: 0 4px 8px rgba(0,0,0,0.08)                    (raised cards)
elevation-3: 0 8px 24px rgba(0,0,0,0.12)                   (modals, popovers)
elevation-4: 0 16px 48px rgba(0,0,0,0.16)                  (overlays)
```

### 5.3 Glow as state, not decoration

A subtle colored glow (`box-shadow` with a brand-color rgba) reads as "this is active / focused / important." Use it intentionally — for active selections, focused inputs, hover states on key CTAs. Don't sprinkle.

---

## 6. Typography

### 6.1 Pair a serif with a sans, not two sans

If you want type variety, pair across categories (serif + sans, mono + sans). Pairing two similar sans-serifs almost always looks like a mistake unless you're an expert.

### 6.2 Font sizes scale

Don't pick arbitrary sizes (15px, 17px, 19px). Use a scale: 12, 14, 16, 18, 24, 32, 48, 64. Or a modular scale (1.25× or 1.333× ratios). Predictable scales make hierarchy visible.

### 6.3 Line length and line height

- **Body text line length**: 50–75 characters per line for readability. Wider = harder to read.
- **Line height**: 1.4–1.6× font size for body. Tighter for display, looser for small print.

### 6.4 Don't over-hierarchy

A page with H1, H2, H3, H4, H5, H6 is rare. Most products need 2–3 heading levels plus body. More than that and the user can't maintain the mental model.

---

## 7. The system, not the screen

### 7.1 The difference is a system

The biggest visual upgrade you can make to a product isn't a redesign — it's a *system*. Tokens for color, spacing, typography, radius, elevation. Every screen pulls from the same set of values.

A product with a system has consistent spacing rhythm, consistent corner radii, consistent shadow stack. A product without a system feels like 47 different designers built it.

### 7.2 Token tiers

```
1. Primitive tokens     (--gray-100, --gray-200, ..., --blue-500)
   Raw values. Brand-agnostic.

2. Semantic tokens      (--color-text-primary, --color-surface-default)
   Intent-based. References primitives. This is what components use.

3. Component tokens     (--button-primary-bg, --card-elevation)
   Component-specific. References semantic tokens.
```

Components should never hard-code primitive tokens. Always go through semantic.

### 7.3 The audit test

A useful design audit: open three different screens. Are they using the same spacing? Same radii? Same type scale? Same shadow stack? If no, you have a system problem, not a screen problem.

---

## 8. Empty and loading states

### 8.1 One tiny illustration > a wall of text

Empty states are an opportunity, not an inconvenience. A small illustration + a one-line headline + a clear next action beats an apologetic paragraph.

```
[icon: empty box]
"Nothing here yet"
"Your projects will appear here once you create one."
[ Create your first project ]
```

The illustration sets tone. The headline acknowledges state. The button gives direction.

### 8.2 Loading is a system

Pick *one* loading style per context, and stick to it. Don't mix:
- Spinners on buttons
- Skeleton screens for content
- Progress bars for known-duration operations
- Indeterminate bars for unknown-duration operations

**1 right choice per context.** The wrong move is using all of them across one product.

### 8.3 Optimistic UI

For actions where the success rate is >99%, show the result *before* the server confirms. The user sees instant feedback; you reconcile failures rarely.

```
User clicks "Like"
   → UI updates immediately to "Liked" (optimistic)
   → Request goes to server
   → If success: nothing more (UI already correct)
   → If failure: revert UI, show error (rare)
```

The user perceives the app as fast. Confirmation latency disappears.

---

## 9. Quick decision guide

When designing or reviewing a screen, ask in this order:

1. **Hierarchy**: Can I tell what's most important in 3 seconds?
2. **Proximity**: Are related things close, unrelated things far?
3. **Spacing**: Is the rhythm consistent across sections?
4. **Color**: Does the palette feel intentional? Are contrasts WCAG-passing?
5. **Type**: Is there a clear size scale? Does it pair well?
6. **Corners and elevation**: Is the radius scale consistent? Do shadows match the elevation system?
7. **System**: Are tokens being used? Is this consistent with adjacent screens?
8. **States**: Does empty exist? Loading? Error?

If you fix only one thing on a struggling screen, fix **proximity and spacing**. It's the highest-leverage move 80% of the time.

---

## How this reference is used

Specific design skills lean on specific sections:

| Section | Most relevant skills |
|---|---|
| Hierarchy and emphasis | `product-designer`, `web-designer`, `design-screen` |
| Spacing and proximity | All design skills |
| Color | `product-designer`, `web-designer`, `accessibility-specialist` |
| Shape and corners | `product-designer`, `design-screen` |
| Depth and elevation | `product-designer`, `web-designer` |
| Typography | All design skills |
| The system | `design-audit`, `design-process` |
| Empty and loading states | `product-designer`, paired with `interaction-patterns.md` |

Pair this reference with `interaction-patterns.md` for the dynamic side: states, motion, feedback, dark patterns.

---

## Source

These principles synthesize:

- Established design literature: Edward Tufte (data visualization), Ellen Lupton (typography), Don Norman (design of everyday things), Robert Bringhurst (typography)
- WCAG 2.2 accessibility specifications
- Modern practitioner posts from [@designmotionhq](https://www.instagram.com/designmotionhq/) (DesignMotion) — curated visual examples and short-form principle illustrations
- Material Design and Human Interface Guidelines (system thinking)
