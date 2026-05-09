---
type: reference
project: skills-library
scope: plugin
plugin: design
tags: [type/reference, plugin/design, scope/foundational, topic/design]
status: active
---
# Interaction Patterns

How interfaces *behave* — states, motion, feedback, and the line between persuasive design and dark patterns. The dynamic counterpart to `visual-design-foundations.md`.

These principles cover what happens between clicks: when an element is focused, hovered, loading, succeeded, failed, dismissed. Get these right and a UI feels crafted. Get them wrong and even a beautiful screen feels janky.

Source synthesis: established interaction-design literature (Norman, Krug, Tognazzini), HIG and Material Design motion specs, and the curated practitioner posts at [@designmotionhq](https://www.instagram.com/designmotionhq/).

---

## 1. The four-state mental model

Every interactive element has at minimum four states. Designing only the default state is the most common interaction failure.

| State | What it tells the user | Visual cues |
|---|---|---|
| **Default** | "This exists" | Resting style |
| **Hover** | "This is interactive" | Subtle visual change (lift, color shift, cursor) |
| **Active / Pressed** | "Your input was received" | Quick depress, color deepen |
| **Focus** | "Keyboard is on this" | Visible ring, distinct from hover |

Add as needed: **disabled**, **loading**, **selected**, **error**, **success**.

If you can't immediately list the visual treatment for each state of a component you're designing, you haven't finished designing it.

---

## 2. Focus states

Focus is the most-undertreated state. Most teams skip it; the result is a product that's unusable for keyboard users and screen-reader users.

### 2.1 Focus rings have a contrast minimum

The focus indicator must have **3:1 contrast** against the focus target — not the page background. A subtle blue glow on a blue button fails accessibility silently.

### 2.2 Ring contrast 3:1, not "looks pretty"

Common mistake: a soft blue glow at low opacity. It looks tasteful but doesn't meet contrast requirements. Test with WCAG contrast tools, not your eyes.

### 2.3 Make focus *more* prominent than hover

Keyboard users see focus states more than mouse users see hover. If hover is more prominent, you've inverted the priority for the users who need it most.

### 2.4 Don't remove focus rings without a replacement

`outline: none` without an alternative is the single most common accessibility violation in production code. If you're styling the focus ring, ensure the replacement is at least as visible.

```css
/* ❌ accessibility nightmare */
:focus { outline: none; }

/* ✅ replacement with adequate contrast */
:focus-visible {
  outline: 2px solid var(--color-focus);
  outline-offset: 2px;
}
```

---

## 3. Focus-out and dismissal patterns

When a popover, dropdown, modal, or tooltip closes — *what triggers it*? This is interaction design at the seams.

### 3.1 Multi-trigger dismissal

Closing should support all four:

1. **Mouse leave** — for hover-triggered popovers
2. **Escape key** — universal "I'm done" signal; required for accessibility
3. **Focus out** (clicking elsewhere with keyboard) — programmatic dismiss
4. **Tap outside** — touch users dismissing by tapping the page

Implement all four, not just whichever was easiest to write.

### 3.2 Escape should always work

If a dialog, modal, or popover is open and the user presses Escape, it must close. This is a universal expectation.

### 3.3 Focus return on close

When a modal closes, focus should return to the element that opened it. Otherwise keyboard users lose their place in the page.

---

## 4. Direction matters: opens upward / opens downward

A dropdown, popover, or tooltip should open in the direction with more available space.

### 4.1 The collision check

```
if (dropdown_height < space_below):
    open_downward()
else:
    open_upward()
```

Modern positioning libraries (Floating UI, Popper) handle this automatically. Don't hard-code direction.

### 4.2 Visual continuity

Whichever direction it opens, the dropdown should feel anchored to its trigger:
- The edge nearest the trigger has matching alignment
- The arrow/caret points back to the trigger
- Animation origin matches the anchor edge (scale-from-bottom for upward, scale-from-top for downward)

---

## 5. Loading states

Loading is a system, not a default spinner. Pick one approach per context, consistently.

### 5.1 Match loading style to expected duration

| Duration | Right choice |
|---|---|
| 0–100ms | None — feels instant |
| 100–1000ms | Inline spinner on the triggering element |
| 1–10s | Skeleton screen (mimics final layout) |
| 10s+ | Progress bar (if duration is known) or progress + status text |
| Unknown long | Indeterminate bar + cancel option |

### 5.2 Skeleton screens > spinners for content

A skeleton screen showing the shape of the content reads as "this is loading" more clearly than a spinner. It also reduces perceived latency because the user starts forming a mental model of the content immediately.

### 5.3 Optimistic UI for high-confidence actions

When an action is very likely to succeed (`>99%`), show the result instantly. Reconcile failures.

```
Click "Like"
  → Heart fills immediately, count increments (optimistic)
  → Server request goes
  → On failure (rare): revert, show error toast
```

The 99% case feels instant. The 1% case is honest about its failure. Net win.

### 5.4 Don't double-loading

A spinner-on-button + skeleton screen + progress bar all firing at once for the same operation is loading-state cacophony. Pick one.

---

## 6. Toasts and notifications

### 6.1 Toasts are for transient feedback, not critical info

Use a toast for: "Message sent", "Payment received", "File uploaded".
Don't use a toast for: "Your account is at risk", "This action is irreversible". Critical info needs a modal or persistent banner.

### 6.2 Position consistency

Pick one corner (typically top-right or bottom-center) and use it for *all* toasts in the product. Mixing positions creates ambient anxiety — users don't know where to look for feedback.

### 6.3 Auto-dismiss timing

- **Success** — 3–4 seconds. Brief acknowledgment.
- **Info** — 4–6 seconds. Time to read.
- **Warning** — 6–8 seconds, or persistent with dismiss button.
- **Error** — persistent until dismissed. Errors deserve user attention.

### 6.4 Stack management

Multiple toasts stack vertically with consistent spacing. Older toasts appear below newer ones (or above, depending on origin corner). Limit visible stack to 3–5 — beyond that, queue or collapse.

---

## 7. Motion and easing

Motion isn't decoration. It's the language that ties cause to effect — "this happened because that happened."

### 7.1 Motion = action

Every animation should communicate something:
- **Direction** — where the element came from / went to
- **Causation** — what triggered this
- **State** — what the element is now

If an animation doesn't communicate one of those, it's noise.

### 7.2 Linear vs spring easing

```
Linear     ─────────  feels mechanical, robotic
Ease-out   ─────────╮ feels responsive, settling
Spring     ─────────~ feels natural, alive
```

- **Linear**: rare. Use for progress bars and other strict-ratio animations.
- **Ease-out**: most UI motion. Things "arrive" with a soft landing.
- **Spring**: high-fidelity micro-interactions. Buttons, drags, gestural elements. Use library physics (Framer Motion springs) — hand-tuning easing curves rarely matches a real spring's feel.

### 7.3 Duration scale

| Type | Duration |
|---|---|
| Hover transition | 100–150ms |
| Button press response | 80–120ms |
| Component appearance | 200–300ms |
| Page transition | 300–500ms |
| Hero / large motion | 400–800ms |

Anything over 500ms for normal UI feels slow. Anything under 100ms barely registers.

### 7.4 Hover scale and lift

A subtle hover effect on interactive elements signals affordance:

```css
.button {
  transition: transform 150ms ease-out, box-shadow 150ms ease-out;
}
.button:hover {
  transform: scale(1.02);  /* not 1.05 — subtle */
  box-shadow: var(--elevation-2);
}
```

Scale: 1.01–1.03 for buttons, 1.02–1.05 for cards. Bigger than that feels cartoonish.

### 7.5 Ripple feedback

A ripple from the click point gives spatial feedback — "you clicked HERE." Material Design popularized this; it works across paradigms. Subtle, not theatrical.

### 7.6 Reduced motion is a real preference

`prefers-reduced-motion: reduce` is set by users who get motion sickness or just dislike animation. Respect it:

```css
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

This isn't optional. It's accessibility.

---

## 8. Touch targets and Fitts' Law

### 8.1 Fitts' Law

> The time to acquire a target is a function of the distance to and size of the target.

Mathematically:
```
T = a + b × log₂(D / W + 1)
```

Where T = time to click, D = distance to target, W = width of target. The implications for design:

- **Bigger targets are faster to hit.** A 44×44pt button is faster than a 24×24pt button.
- **Closer targets are faster to hit.** Group related actions; don't scatter them.
- **Edge targets are infinitely large.** Items at screen edges (top bar, dock, edges) have effectively infinite width because the cursor can't overshoot. Use this — close buttons at corners, navigation at edges.

### 8.2 Minimum touch target sizes

- **iOS HIG**: 44×44 points
- **Material Design**: 48×48 dp
- **WCAG 2.2 (AAA)**: 44×44 CSS pixels

Below this, error rates climb sharply, especially for users with motor impairments. The button can be visually smaller — 32px tall is fine — but the *tap area* must be ≥44px.

### 8.3 Spacing between targets

Adjacent tappable elements need 8px+ between them. Two buttons touching is one accidental tap waiting to happen.

### 8.4 Place actions where the thumb already is

On mobile, the thumb naturally rests at the bottom-right (right-handed). Primary actions belong there. Critical destructive actions belong far from there.

---

## 9. Empty states are UX too

Empty isn't the absence of design — it's the design of absence.

### 9.1 The three-part empty state

1. **Visual** — small, on-brand illustration or icon
2. **Headline** — friendly acknowledgment of state ("Nothing here yet")
3. **Action** — clear next step ("Create your first project")

### 9.2 Voice matters

A defeated empty state ("No data found.") feels like the product is broken. A welcoming one ("Add your first item to get started") feels like the product is helpful.

### 9.3 Differentiate empty types

- **Brand-new user empty** ("Let's get you started")
- **Filtered-out empty** ("No matches for 'xyz'")
- **Cleared / completed empty** ("All caught up!")

Each has a different appropriate action. Don't show the same illustration for all three.

---

## 10. Mobile navigation patterns

### 10.1 Hamburger menus cost engagement

Mobile hamburger menus hide content. Research (Nielsen Norman) consistently shows desktop hamburgers reduce engagement ~50% vs visible navigation. On mobile, they're a necessary compromise but should only hide *secondary* navigation.

**Primary nav stays visible** — bottom tab bar on mobile, top nav on desktop. **Secondary nav** (settings, account, sign out, support) can hide behind a menu icon. Never hide things people use *frequently* behind a menu icon.

### 10.2 Bottom tab bar conventions

- 3–5 tabs maximum (4 is the sweet spot)
- Icons + labels (not icons alone)
- Active state is unambiguous (filled icon, color shift, or both)
- Each tab is a distinct destination, not an action

### 10.3 Don't hide nav on scroll, in most cases

The "hide nav on scroll down, show on scroll up" pattern saves a few pixels of screen real estate at the cost of orientation. Use only when the content height genuinely demands it (long-form reading, infinite feeds). For most apps, persistent nav wins.

---

## 11. Putting feeling into UI

### 11.1 "This button feels good"

When a button "feels good" it's almost always because:
- The press response is fast (< 120ms)
- The release has a tiny easing back to default
- The corner radius matches the rest of the system
- The hover state is subtle (1.02 scale, not 1.1)
- The active state actually depresses (slight scale-down or color-deepen)
- The color contrast meets WCAG and reads cleanly
- The label is centered with appropriate padding

It's not one thing. It's the accumulation of getting many small things right. **That's not an accident — it's design.**

### 11.2 Micro-interactions are the polish layer

After hierarchy, color, spacing, and typography are right, micro-interactions are what separate a "well-designed" product from a "delightful" product:

- The toggle that has a subtle bounce when you flip it
- The like button that has a tiny burst animation
- The input field whose label floats up when focused
- The drawer that springs in with momentum

Each one is small. The accumulation is the brand.

---

## 12. Dark patterns to avoid

Persuasive design and dark patterns differ on one axis: **does the user benefit, or does only the company?**

### 12.1 The taxonomy

| Pattern | What it does | Why it's a dark pattern |
|---|---|---|
| **Confirm shaming** | Decline button reads "No, I prefer paying full price" | Manipulates emotion to override preference |
| **Anchoring with a decoy** | Three-tier pricing where the middle tier is artificially inflated to make the right tier look cheap | Distorts comparison rather than informs |
| **Notification dot manipulation** | Red dots on icons that don't represent real new content | Trains users into reactive checking |
| **Roach motel** | Easy to subscribe, painful to cancel | Asymmetric friction punishing user intent |
| **Privacy zuckering** | Default sharing settings opt users in to data they wouldn't choose | Defaults exploit cognitive load |
| **Disguised ads** | Ads styled to look like content | Violates user trust |
| **Sneak into basket** | Pre-checked add-ons at checkout | User pays for things they didn't choose |

### 12.2 The fair-use line

Some persuasive techniques are legitimate:
- **Anchoring** is fine when the comparison is honest — showing the full price next to the discount.
- **Defaults** are fine when they reflect what 90% of users actually want.
- **Loss aversion framing** is fine when the loss is real ("Cancel and lose access to your data").

The line is whether the user, on reflection, would still want what the design pushed them toward. If yes, persuasion. If no, dark pattern.

### 12.3 The mirror test

Ask: "Would I describe this design to a user honestly, in plain words?"

> ❌ "We made the cancel button gray and small so people are less likely to cancel."

If you wouldn't say it to the user, it's a dark pattern.

### 12.4 Pricing dark patterns specifically

Watch for these on pricing pages:
- **Decoy pricing**: a useless third tier engineered to make the target tier look cheap
- **Artificial scarcity**: "Only 2 left at this price!" when supply is unlimited
- **Forced annual**: defaulting to annual billing without making monthly visible
- **Cancellation friction**: signup is one button; cancellation is six steps

Persuasion is fine. Manipulation isn't. The difference is whether you'd be embarrassed if a user understood your incentive.

---

## 13. Quick decision guide

When designing or reviewing an interaction, ask in this order:

1. **States**: Have I designed default, hover, active, focus, disabled, loading, error?
2. **Focus**: Is the focus ring visible, with 3:1 contrast, on every interactive element?
3. **Dismissal**: Can the user close this with mouse, keyboard, focus-out, and outside-tap?
4. **Loading**: Have I picked the right loading style for the expected duration?
5. **Motion**: Does this animation communicate something, or is it decoration?
6. **Touch targets**: Is every tappable element ≥44×44pt with ≥8px between adjacent targets?
7. **Empty states**: Do they exist? Do they have voice + action?
8. **Reduced motion**: Have I respected `prefers-reduced-motion`?
9. **Dark patterns**: Could I describe this design to the user without embarrassment?

If you fix only one thing on a struggling interaction, fix **state design**. Most "feels janky" comes from missing states, not bad ones.

---

## How this reference is used

Specific design skills lean on specific sections:

| Section | Most relevant skills |
|---|---|
| State mental model | `motion-interaction-designer`, `product-designer`, `design-screen` |
| Focus states | `accessibility-specialist`, `motion-interaction-designer` |
| Focus-out and dismissal | `motion-interaction-designer`, `accessibility-specialist` |
| Direction (opens up/down) | `motion-interaction-designer`, `product-designer` |
| Loading states | `motion-interaction-designer`, `product-designer` |
| Toasts | `motion-interaction-designer`, `product-designer` |
| Motion and easing | [[motion-interaction-designer]] |
| Touch targets / Fitts' Law | `accessibility-specialist`, `product-designer` |
| Empty states | `product-designer`, `motion-interaction-designer` |
| Mobile navigation | `product-designer`, `web-designer` |
| Dark patterns | `product-designer`; cross-reference behavioral-economics plugin |

Pair this reference with `visual-design-foundations.md` for the static side: hierarchy, color, space, depth.

---

## Cross-plugin notes

- **Dark patterns** overlap heavily with the `behavioral-economics` plugin's `choice-architect` skill. The line: this reference covers the *design execution* of dark patterns; `choice-architect` covers the underlying behavioral mechanisms (anchoring, loss aversion, defaults).
- **Accessibility** topics (focus rings, touch targets, reduced motion) are owned by the `accessibility-specialist` skill in this plugin and cross-reference WCAG 2.2.
- **Motion fidelity** is the deepest dive in `motion-interaction-designer` — this reference covers the principles; that skill covers Framer Motion / CSS implementation.

---

## Source

These principles synthesize:

- Don Norman, *The Design of Everyday Things* — affordances, mappings, feedback
- Steve Krug, *Don't Make Me Think* — clarity in interactive flows
- Bruce Tognazzini, *First Principles of Interaction Design* — Fitts' Law application
- iOS Human Interface Guidelines, Material Design Motion principles
- WCAG 2.2 Success Criteria (focus, target size, motion)
- Modern practitioner posts from [@designmotionhq](https://www.instagram.com/designmotionhq/) — curated examples of state design, motion, and dark pattern recognition
