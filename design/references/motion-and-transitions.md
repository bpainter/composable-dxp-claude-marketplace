---
type: reference
project: skills-library
scope: plugin
plugin: design
tags: [type/reference, plugin/design, scope/foundational, topic/design, topic/motion, topic/interaction]
status: active
---

# Motion and Transitions

The named, opinionated catalog for interaction motion — the layer that decides *whether* something should animate, *why*, *with what easing*, and *for how long*. The complement to [[interaction-patterns]] (the dynamic-design fundamentals) and [[design-taste]] (the anti-bland calibration).

This reference is the craft layer that separates "I added a fade transition" from "this interface feels alive without feeling loud." Most of what makes an interface feel crafted is unseen: 200ms instead of 400ms, `ease-out` instead of `ease`, `scale(0.97)` instead of `scale(0)`. None of it announces itself. The aggregate is what users feel.

Adapted with credit from:
- [emilkowalski/skill](https://github.com/emilkowalski/skill) — the Animation Decision Framework, the Sonner Principles, component-specific patterns. Emil is the author of [Sonner](https://sonner.emilkowal.ski/) (toasts) and [Vaul](https://vaul.emilkowal.ski/) (drawers) — both adopted by shadcn/ui.
- [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes) — layout-before-animation discipline, transition discipline across scenes, runtime contrast verification.

For framework-agnostic dynamic-design fundamentals (state matrices, focus, dismissal, touch targets, dark patterns), see [[interaction-patterns]].
For the dial system that contextualizes motion intensity, see [[design-taste]].

---

## The Animation Decision Framework

Before writing any animation code, answer these four questions in order. If you can't answer all four, you're decorating, not designing.

### 1. Should this animate at all?

The most-skipped question. Most "added a fade" decisions never run this filter.

| Frequency | Decision |
|---|---|
| 100+ times/day (keyboard shortcuts, command palette toggle) | **No animation. Ever.** |
| Tens of times/day (hover effects, list nav, type-ahead) | Remove or drastically reduce |
| Occasional (modals, drawers, toasts, route changes) | Standard animation |
| Rare/first-time (onboarding, celebrations, first-load reveal) | Can add delight |

**Never animate keyboard-initiated actions.** Cmd+K, slash commands, escape-to-close. These fire hundreds of times daily. Animation makes them feel slow even when they're identically fast.

> Raycast has no open/close animation. That's the optimal experience for something used hundreds of times a day.

### 2. What is the purpose?

Every animation has to answer "why does this animate?" Valid purposes:

- **Spatial consistency** — the toast enters and exits from the same direction so swipe-to-dismiss feels intuitive.
- **State indication** — the morphing feedback button shows the state changed.
- **Explanation** — a marketing animation that demonstrates how a feature works.
- **Feedback** — a button scales down on press, confirming the input registered.
- **Preventing jarring change** — elements appearing/disappearing without a transition feel broken.

If the answer is "it looks cool" and the user sees it often, **don't animate**.

### 3. What easing should it use?

A decision tree:

```
Is the element entering or exiting?
  Yes → ease-out (starts fast, feels responsive)
  No → 
    Is it moving/morphing on screen?
      Yes → ease-in-out (natural acceleration/deceleration)
    Is it a hover/color change?
      Yes → ease (default)
    Is it constant motion (marquee, progress bar)?
      Yes → linear
  Default → ease-out
```

**Critical: use custom easing curves.** Built-in CSS easings are too weak for premium UI.

```css
/* Strong ease-out for UI interactions */
--ease-out: cubic-bezier(0.23, 1, 0.32, 1);

/* Strong ease-in-out for on-screen movement */
--ease-in-out: cubic-bezier(0.77, 0, 0.175, 1);

/* iOS-like drawer curve (from Ionic) */
--ease-drawer: cubic-bezier(0.32, 0.72, 0, 1);
```

**Never use `ease-in` for UI animations.** It starts slow, which makes the interface feel sluggish at the exact moment the user is watching most closely. A dropdown with `ease-in` at 300ms feels slower than `ease-out` at the same 300ms.

For new curves, don't invent: pick stronger custom variants from [easing.dev](https://easing.dev/) or [easings.co](https://easings.co/).

### 4. How fast should it be?

| Element | Duration |
|---|---|
| Button press feedback | 100–160ms |
| Tooltips, small popovers | 125–200ms |
| Dropdowns, selects | 150–250ms |
| Modals, drawers | 200–500ms |
| Marketing/explanatory | Can be longer |

**Rule: UI animations stay under 300ms.** A 180ms dropdown feels more responsive than a 400ms one. A faster-spinning spinner makes the app feel like it loads faster, even when load time is identical.

**Asymmetric enter/exit.** Exits should be 60–70% of enter duration. The exit isn't where the user is making a decision; it's where the system is responding. Snap it.

```css
/* Press: slow and deliberate (user is deciding) */
.button:active .overlay {
  transition: clip-path 2s linear;
}

/* Release: snappy (system responds) */
.overlay {
  transition: clip-path 200ms ease-out;
}
```

---

## Component-specific patterns

### Buttons

**Press feedback is non-negotiable.** Every pressable element gets `transform: scale(0.97)` on `:active`.

```css
.button {
  transition: transform 160ms ease-out;
}

.button:active {
  transform: scale(0.97);
}
```

Subtle (0.95–0.98). Bigger feels cartoonish. The press confirms the UI heard the input.

### Popovers, dropdowns, hover cards

**Make them origin-aware.** Default `transform-origin: center` is wrong for almost every popover. They should scale from their trigger.

```css
/* Radix UI */
.popover {
  transform-origin: var(--radix-popover-content-transform-origin);
}

/* Base UI */
.popover {
  transform-origin: var(--transform-origin);
}
```

**Exception: modals.** Modals stay `transform-origin: center` because they aren't anchored to a trigger — they appear in the viewport.

### Tooltips

Delay before first appearance to prevent accidental activation. **Skip the delay on subsequent hovers** within a toolbar — once one is open, adjacent ones should appear instantly.

```css
.tooltip {
  transition:
    transform 125ms ease-out,
    opacity 125ms ease-out;
  transform-origin: var(--transform-origin);
}

.tooltip[data-starting-style],
.tooltip[data-ending-style] {
  opacity: 0;
  transform: scale(0.97);
}

.tooltip[data-instant] {
  transition-duration: 0ms;
}
```

The whole toolbar feels faster, even though only the first tooltip changed.

### Toasts (Sonner pattern)

Sonner is the canonical reference for toast craft. It's adopted by shadcn/ui as the official toast — when implementing toasts, don't roll your own. **Use Sonner.**

Key Sonner discipline:
- Pause toast timers when the tab is hidden.
- Fill gaps between stacked toasts with pseudo-elements so hover state holds across the stack.
- Capture pointer events during drag.
- Toast animations use **transitions, not keyframes** — toasts get added and removed rapidly; keyframes restart from zero on interruption, transitions retarget smoothly.

Sonner is at [sonner.emilkowal.ski](https://sonner.emilkowal.ski/). For implementation, the [shadcn Sonner docs](https://ui.shadcn.com/docs/components/radix/sonner) are authoritative.

### Drawers (Vaul pattern)

Vaul is the canonical drawer library. Same author, same craft. Adopted by shadcn/ui as the official drawer.

Key Vaul discipline:
- **`translateY(100%)` for hidden state** — percentage-based so it works regardless of drawer height.
- **iOS-like easing curve** — `cubic-bezier(0.32, 0.72, 0, 1)`.
- **Damping at boundaries** — when dragged past the natural top, allow movement with increasing friction. Never hard-stop.
- **Velocity-based dismissal** — don't require crossing a fixed threshold; calculate `Math.abs(distance) / time` and dismiss above ~0.11.
- **Pointer capture during drag** — drag continues even if pointer leaves bounds.
- **Multi-touch protection** — ignore additional touch points after drag begins.

Vaul is at [vaul.emilkowal.ski](https://vaul.emilkowal.ski/). Default for any sheet/drawer pattern.

### Modals and dialogs

- Backdrop fade in 200–300ms.
- Content scales from `scale(0.95)` plus opacity, never from `scale(0)` (see Bans below).
- Backdrop dismissal optional (Alert Dialog disables it for destructive confirms).
- Focus trap on open; focus return on close (see [[interaction-patterns]] §3.3).
- Escape always closes.
- Reduced-motion: opacity-only entrance.

### Tabs

Tab indicator slides between active states using `clip-path` or `layoutId` (Framer Motion shared layout). Color transition on active label uses the **clip-path duplicate trick**:

1. Render the tab list twice: one in default style, one styled as "active state."
2. Clip the active-state copy with `clip-path: inset(...)` so only the current tab is visible.
3. Animate the inset on tab change.

This produces a seamless color transition that timing individual color transitions on each tab can never match.

### Skeletons and shimmer

Skeletons reserve final layout space (no CLS) and fill with a shimmer gradient. Use for content loading 100ms–10s. Below 100ms, no loading state. Above 10s, switch to a progress bar with status text.

```css
.skeleton {
  background: linear-gradient(
    90deg,
    var(--bg) 0%,
    var(--bg-shimmer) 50%,
    var(--bg) 100%
  );
  background-size: 200% 100%;
  animation: shimmer 1.5s infinite;
}

@keyframes shimmer {
  0% { background-position: 200% 0; }
  100% { background-position: -200% 0; }
}
```

Match skeleton shape exactly to the final content. Generic skeletons are an AI tell.

### Stagger entries

When multiple items enter together, stagger them. **30–80ms between items.** Longer feels slow; shorter loses the cascade.

```css
.item:nth-child(1) { animation-delay: 0ms; }
.item:nth-child(2) { animation-delay: 50ms; }
.item:nth-child(3) { animation-delay: 100ms; }
.item:nth-child(4) { animation-delay: 150ms; }
```

In Framer Motion, use `staggerChildren: 0.05`. Stagger is decorative — never block interaction while it plays.

---

## The named bans (cite by name)

These are the AI-default motion patterns to actively reject. Add to the [[ai-tells-forbidden-patterns]] catalog.

### The `scale(0)` ban
**The tell:** Elements that animate from `scale(0)` to `scale(1)`. They appear from nowhere, like a magic trick.
**The override:** Start from `scale(0.95)` plus `opacity: 0`. Even a barely-visible initial scale makes the entrance feel natural.

```css
/* Banned */
.entering { transform: scale(0); }

/* Correct */
.entering {
  transform: scale(0.95);
  opacity: 0;
}
```

### The `ease-in` ban
**The tell:** UI animations using `ease-in`. Starts slow, finishes fast. Feels sluggish.
**The override:** `ease-out` for entrances and exits. Custom curves (`cubic-bezier(0.23, 1, 0.32, 1)`) for premium feel.
**Exception:** Exit animations *can* use `ease-in` to "swallow" the element back into its source. But default to ease-out.

### The `transition: all` ban
**The tell:** `transition: all 300ms;` on a component.
**The override:** Specify exact properties — `transition: transform 200ms ease-out, opacity 200ms ease-out;`. `all` triggers transitions on properties you didn't intend (color, background) and creates jank.

### The keyframe-on-rapid-trigger ban
**The tell:** `@keyframes` animation on toasts, list items, anything triggered repeatedly.
**The override:** CSS `transition` properties. Keyframes restart from zero on interruption; transitions retarget smoothly.

### The `transform-origin: center on popovers` ban
**The tell:** Default popover/dropdown that scales from the center of the popover bounds.
**The override:** Origin-aware popovers (`var(--radix-popover-content-transform-origin)`). Modals are exempt.

### The over-300ms-UI-animation ban
**The tell:** Modal entry at 600ms, dropdown at 500ms.
**The override:** UI under 300ms. Marketing/explanatory can be longer.

### The same-speed enter/exit ban
**The tell:** Modal enters in 300ms, exits in 300ms. Both feel sluggish.
**The override:** Exits 60–70% of enter duration.

### The Framer Motion `x`/`y` shorthand under load ban
**The tell:** `<motion.div animate={{ x: 100 }} />` in a context where the main thread gets busy (data loads, route changes, large lists).
**The override:** Use the full `transform` string for hardware acceleration:
```jsx
<motion.div animate={{ transform: "translateX(100px)" }} />
```
Framer Motion's shorthand uses `requestAnimationFrame` on the main thread — drops frames under load. The full transform string runs on the compositor.

### The animate-keyboard-action ban
**The tell:** Cmd+K opens a command palette with a 200ms slide-and-fade.
**The override:** Open instantly. No animation on keyboard-triggered actions.

---

## CSS transform mastery

### Percentages in `translate()`
**Use them.** `translateY(100%)` moves an element by its own height — works regardless of dimensions. This is how Sonner positions toasts and Vaul hides drawers.

```css
.drawer-hidden { transform: translateY(100%); }
.toast-enter { transform: translateY(-100%); }
```

Prefer percentages over hardcoded pixels. They adapt to content; they don't break when font size or content changes.

### `scale()` scales children
Unlike `width`/`height`, `scale()` proportionally scales children. When scaling a button on press, the icon and label scale with it. Feature, not bug.

### 3D transforms
`rotateX()`, `rotateY()` with `transform-style: preserve-3d` give real 3D depth in CSS. No JavaScript needed for orbits, coin flips, depth effects.

### `transform-origin`
Every transform has an anchor point. Default is center. Set it to match the trigger location for origin-aware effects (popovers from triggers, scaling from a corner, etc.).

---

## `clip-path` for animation

`clip-path` is one of the most powerful animation tools in CSS, far beyond shape clipping.

### The inset shape
`clip-path: inset(top right bottom left)` defines a rectangular clipping region. Each value eats into the element from that side.

### Tab color-transition trick
See "Tabs" above. Two stacked layouts, clip the active-styled one to the current tab. Animate the clip on tab change.

### Hold-to-delete pattern
Colored overlay starts at `clip-path: inset(0 100% 0 0)` (clipped to nothing). On `:active`, transition to `inset(0 0 0 0)` over 2s linear. On release, snap back at 200ms ease-out. Asymmetric timing — slow when deciding, fast when releasing.

### Image reveals on scroll
Start at `clip-path: inset(0 0 100% 0)` (hidden from bottom). Animate to `inset(0 0 0 0)` when entering the viewport. Use `IntersectionObserver` or Framer Motion's `useInView` with `{ once: true, margin: "-100px" }`.

### Comparison sliders
Overlay two images. Clip the top with `clip-path: inset(0 50% 0 0)`. Adjust the right inset based on drag position. No extra DOM, fully hardware-accelerated.

---

## Spring animations

Springs simulate real physics. They settle based on stiffness, damping, mass — no fixed duration. Better than duration-based for:

- Drag interactions with momentum.
- Elements that should feel "alive" (Apple's Dynamic Island).
- Gestures that can be interrupted mid-animation.
- Decorative mouse-tracking interactions.

### Configuration

**Apple's approach (recommended — easier to reason about):**
```js
{ type: "spring", duration: 0.5, bounce: 0.2 }
```

**Traditional physics (more control):**
```js
{ type: "spring", mass: 1, stiffness: 100, damping: 10 }
```

Keep bounce subtle (0.1–0.3). Avoid bounce in most UI. Use it for drag-to-dismiss and explicitly playful interactions.

### Why springs interrupt cleanly

Springs maintain velocity when interrupted. CSS animations and keyframes restart from zero. For gestures users might change mid-motion (drag-to-cancel, expand-then-escape), springs win.

### Spring-based mouse interactions

Direct mouse-position binding feels artificial because there's no inertia. Wrap with `useSpring` (Motion / Framer Motion):

```jsx
import { useSpring } from "framer-motion";

// Without spring: artificial, instant
const rotation = mouseX * 0.1;

// With spring: natural, has momentum
const springRotation = useSpring(mouseX * 0.1, {
  stiffness: 100,
  damping: 10,
});
```

This works because the animation is *decorative*. If the same axis drove a functional graph in a banking app, no animation would be better.

---

## Gesture and drag interactions

Default to Vaul (for drawers/sheets) and `react-use-gesture` for custom gestures. Roll your own only when those don't fit.

### Velocity-based dismissal

Don't require dragging past a fixed threshold. Compute velocity, dismiss on quick flicks:

```js
const elapsed = Date.now() - dragStartTime.current.getTime();
const velocity = Math.abs(swipeAmount) / elapsed;

if (Math.abs(swipeAmount) >= SWIPE_THRESHOLD || velocity > 0.11) {
  dismiss();
}
```

### Damping at boundaries

When the user drags past natural bounds (drawer up when already at top), apply damping. The further past, the less it moves. Real-world objects don't hit invisible walls.

### Pointer capture during drag

Once drag begins, capture all pointer events. Drag continues even if the pointer leaves the element. Otherwise the user has to keep their finger inside an invisible boundary.

### Multi-touch protection

Ignore additional touch points after drag starts. Otherwise switching fingers mid-drag jumps the element to the new position.

```js
function onPress() {
  if (isDragging) return;
  // Start drag...
}
```

### Friction over hard stops

Allow upward drag with increasing friction instead of preventing it entirely. Real-world behavior. Hard stops feel broken.

---

## Performance rules

### Only animate `transform` and `opacity`
These run on the GPU compositor. Skipping layout and paint = 60fps even under load.

**Banned:** animating `padding`, `margin`, `height`, `width`, `top`, `left`. These trigger layout, then paint, then composite. Stutter under load.

**Allowed:** `transform`, `opacity`, `filter` (sparingly — heavy in Safari), `clip-path`.

### CSS variable inheritance caveat
Changing a CSS variable on a parent recalculates styles for all children. In a drawer with many items, updating `--swipe-amount` on the container is expensive.

```js
// Bad: triggers recalc on all children
element.style.setProperty("--swipe-amount", `${distance}px`);

// Good: only this element
element.style.transform = `translateY(${distance}px)`;
```

### Framer Motion shorthand HW caveat
Framer Motion's `x`, `y`, `scale` props use `requestAnimationFrame` on the main thread. **Not hardware-accelerated.** Under load (data fetches, route transitions, big lists), they drop frames.

```jsx
// Convenient but drops frames under load
<motion.div animate={{ x: 100 }} />

// Hardware-accelerated, stays smooth
<motion.div animate={{ transform: "translateX(100px)" }} />
```

This caused frame drops on the Vercel dashboard's tab animation during page loads. Switching to CSS animations (off-main-thread) fixed it.

### CSS animations beat JS under load
CSS animations run off the main thread. When the browser is busy, Framer Motion drops frames; CSS keeps going. **Use CSS for predetermined animations; JS for dynamic, interruptible ones.**

### Web Animations API (WAAPI) for programmatic CSS
Hardware-accelerated, interruptible, scriptable. JS control with CSS performance — no library needed.

```js
element.animate(
  [{ clipPath: "inset(0 0 100% 0)" }, { clipPath: "inset(0 0 0 0)" }],
  {
    duration: 1000,
    fill: "forwards",
    easing: "cubic-bezier(0.77, 0, 0.175, 1)",
  },
);
```

### Blur is expensive
Keep `filter: blur()` under 20px. Heavier blur tanks Safari especially. Useful sparingly to mask imperfect transitions (see "Blur as transition mask" below).

---

## Accessibility

### `prefers-reduced-motion`

Reduced motion ≠ zero motion. Keep opacity and color transitions that aid comprehension. Remove movement and position animations.

```css
@media (prefers-reduced-motion: reduce) {
  .element {
    /* Keep opacity-based transitions */
    transition: opacity 200ms ease-out;
    /* Remove transform-based motion */
    transform: none !important;
  }
}
```

In Framer Motion, use `useReducedMotion()`:

```jsx
const reducedMotion = useReducedMotion();
const closedX = reducedMotion ? 0 : "-100%";
```

### Touch device hover gates

Hover states fire on tap on touch devices, causing stuck states.

```css
@media (hover: hover) and (pointer: fine) {
  .element:hover {
    transform: scale(1.05);
  }
}
```

This is the only correct pattern. Without it, mobile users tap a button and it stays "hovered" until something else takes the hover.

### Focus rings (always visible)

Per [[interaction-patterns]] §2: 3:1 contrast against the focus target, not the page background. `:focus-visible` only (not `:focus`) so mouse clicks don't trigger rings on buttons.

```css
:focus-visible {
  outline: 2px solid var(--color-focus);
  outline-offset: 2px;
}
```

---

## Layout before animation

Adapted from Hyperframes. Position every element where it should land at its **most visible moment** — the frame where it's fully entered, correctly placed, and not yet exiting. Build that as static CSS first. **No animation yet.**

### Why
If you position elements at their start state (offscreen, scaled to 0, opacity 0) and tween them to where you guess they should land, overlaps stay invisible until everything finishes. You're guessing the final layout. By building the end state first, layout problems are visible *before* motion is added.

### The process
1. Identify the hero frame for each scene/component — when most elements are simultaneously visible.
2. Write static CSS for that frame.
3. Add entrances with `gsap.from()` (or Framer Motion `initial`/`animate`) — animate FROM offscreen/invisible TO the CSS position. The CSS position is the ground truth; the tween is the journey.
4. Add exits with `gsap.to()` (or `exit` prop) — animate TO offscreen/invisible FROM the CSS position.

This applies to product UI, not just video composition: a list with staggered entry should have its end-state CSS rendered first, then the staggered `from` tweens added on top.

---

## Blur as transition mask

When a crossfade between two states feels off despite trying different easings and durations, add subtle `filter: blur(2px)` during the transition.

**Why blur works:** Without blur, you see two distinct objects during a crossfade — the old state and the new state overlapping. That looks unnatural. Blur bridges the visual gap so the eye perceives one smooth transformation, not two objects swapping.

```css
.button {
  transition: transform 160ms ease-out;
}

.button:active {
  transform: scale(0.97);
}

.button-content {
  transition:
    filter 200ms ease,
    opacity 200ms ease;
}

.button-content.transitioning {
  filter: blur(2px);
  opacity: 0.7;
}
```

Keep blur under 20px (perf). Don't reach for this first; reach for it when the crossfade *almost* works but feels weird.

---

## Cohesion matters

Animation values should match the personality of the component and the surface.

> Sonner's animation feels satisfying partly because the whole experience is cohesive. The easing and duration fit the vibe. It's slightly slower than typical UI animations and uses `ease` rather than `ease-out` to feel more elegant. The animation matches the toast design, which matches the page design, which matches the name — everything in harmony.

When choosing animation values:

| Surface vibe | Easing default | Duration default |
|---|---|---|
| Crisp / professional dashboard | strong `ease-out` | 150–200ms |
| Premium / brand-led marketing | `ease` (softer) | 250–400ms |
| Playful / consumer | spring with bounce 0.15–0.3 | duration 0.5s |
| Editorial / long-form | `ease-out`, longer scroll motion | 400–600ms scroll, 200ms UI |
| Cockpit / dense data | minimal motion | 100–150ms (or none) |

Match motion to the [[stylistic-vocabulary]] register from `design-taste`.

---

## Page and route transitions

For Next.js App Router with Framer Motion `AnimatePresence`:

- Route changes 200–400ms max.
- Direction-aware (forward = slide left, back = slide right) per the navigation hierarchy.
- Shared element transitions via `layoutId` for hero-to-detail patterns.
- Exit animations are critical — disabled exit makes layout feel broken.
- Reduced motion: opacity-only crossfade.

For server components, the boundary between server-rendered shell and client-animated content matters. `AnimatePresence` requires client components; structure the layout so the static frame is server-rendered and only the animated piece is client.

---

## Debugging animations

### Slow-motion testing
Play animations at reduced speed to spot issues invisible at full speed. Temporarily increase duration to 2–5×, or use Chrome DevTools Animations panel.

Look for:
- Do colors transition smoothly, or do you see two states overlapping mid-transition? (Add blur mask.)
- Does the easing feel right, or does it start/stop abruptly? (Try a custom curve.)
- Is `transform-origin` correct? (Especially for scale-from-trigger popovers.)
- Are coordinated properties (opacity, transform, color) in sync?

### Frame-by-frame
Step through animations frame-by-frame in Chrome DevTools (Animations panel). Reveals timing issues invisible at full speed.

### Real devices
For touch interactions (drawers, swipes), test on physical devices. Connect via USB, visit local dev by IP, use Safari remote devtools. Xcode Simulator works but real hardware reveals more.

### Review the next day
Animations look different with fresh eyes. Imperfections you missed during dev are visible the next day. Build it Tuesday, review Wednesday, ship Thursday.

---

## Review checklist

When reviewing UI motion code, check for these in a markdown table format (Before / After / Why):

| Issue | Fix |
|---|---|
| `transition: all` | Specify exact properties: `transition: transform 200ms ease-out` |
| `scale(0)` entry | Start from `scale(0.95)` with `opacity: 0` |
| `ease-in` on UI | Switch to `ease-out` or custom curve |
| `transform-origin: center` on popover | Set to trigger location or use Radix/Base UI CSS variable (modals exempt) |
| Animation on keyboard action | Remove animation entirely |
| Duration > 300ms on UI | Reduce to 150–250ms |
| Hover animation without media query | Add `@media (hover: hover) and (pointer: fine)` |
| Keyframes on rapidly-triggered element | Use CSS transitions for interruptibility |
| Framer Motion `x`/`y` props under load | Use `transform: "translateX()"` for hardware acceleration |
| Same enter/exit speed | Make exit 60–70% of enter |
| Elements all appear at once | Add stagger 30–80ms between items |
| Animating `width`/`height`/`top`/`left` | Use `transform` and/or `clip-path` |
| Direct mouse-to-rotation binding | Wrap in `useSpring` for inertia |
| Custom drawer/sheet implementation | Use Vaul |
| Custom toast implementation | Use Sonner |
| No reduced-motion alternative | Add `@media (prefers-reduced-motion: reduce)` block |
| Spinner without skeleton on >300ms loads | Switch to skeleton matching final layout |

---

## How this reference is used

- **Loaded by:** [[design-motion-interaction-designer]], [[design-product-designer]], [[design-web-designer]], [[design-screen]], [[design-handoff]], [[design-audit]].
- **Cited from front-end skills:** `software-engineering-frontend-developer`, `software-engineering-design-implementation`, `software-engineering-shadcn-component`. The motion principles here are the floor; the front-end skills implement them.
- **Pairs with:** [[interaction-patterns]] (state design, dismissal, focus, dark patterns — the framework-agnostic layer); [[design-taste]] (the dial system, MOTION_INTENSITY calibration).

When the user asks "is this animation right?" cite this file. When proposing motion, the framework (should/why/easing/how-fast) is the structure — don't skip it.

---

## Credits

Most of the animation-decision framework, component-specific patterns, the named bans, the CSS transform tactics, the spring guidance, the gesture rules, the performance caveats, the Sonner Principles, the debugging protocol, and the review checklist are adapted from [emilkowalski/skill](https://github.com/emilkowalski/skill) (also at [emilkowal.ski/skill](https://emilkowal.ski/skill)) under its public license. Emil Kowalski authored Sonner (toasts) and Vaul (drawers), both adopted by shadcn/ui — his interaction craft is the canonical reference layer for product UI motion.

The "layout before animation" discipline, transition-variety guidance across scenes, and runtime contrast-during-motion verification are adapted from [heygen-com/hyperframes](https://github.com/heygen-com/hyperframes). HeyGen's Hyperframes is HTML→video composition; its motion-craft principles port cleanly to product UI.

The dial system that contextualizes motion intensity ([[design-taste]] — MOTION_INTENSITY 1–10), the named-bans architecture, and the Slalom voice are ours.
