---
type: reference
project: skills-library
scope: plugin
plugin: design
tags: [type/reference, plugin/design, scope/foundational, topic/design, topic/quality]
status: active
---

# Pre-Delivery Checklist

The named, priority-ordered checks every design artifact passes before it leaves the studio. Run this before sharing with the user, the client, or engineering. Failing any CRITICAL item blocks delivery; HIGH items block delivery unless explicitly waived; MEDIUM and LOW items get fixed before signoff but don't block initial review.

This file is the post-production filter. The pre-production filter is [[design-taste]] (intent-first protocol, four required outputs, the four checks). The runtime filter is [[design-process]] (the nine rules and five phases). This is the *gate before showing*.

For each check: **the rule**, **the test**, **what failure looks like**.

Adapted from the priority-categorized rule database in [nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) (MIT) — extended to non-product-UI surfaces (decks, documents, social) and reframed for consulting deliverables.

---

## Priority 1: Accessibility (CRITICAL)

Failure here means people can't use the artifact. These are non-negotiable.

- **Color contrast** — Body text ≥ 4.5:1 against its background. Large text (18pt+ or 14pt bold+) ≥ 3:1. UI controls and graphical elements ≥ 3:1. *Test:* run the actual hex values through a contrast checker. *Fail looks like:* light gray placeholder text on white inputs; brand color CTAs that fail against light surfaces.
- **Focus rings present and visible** — Every interactive element has a visible `:focus-visible` state with ≥ 3:1 contrast against the focus target. *Test:* tab through the artifact. *Fail looks like:* `outline: none` with no replacement.
- **Alt text on meaningful imagery** — Decorative images can have `alt=""`. Meaningful images describe their purpose, not their pixels. Charts have a text summary or accessible data table. *Test:* would the artifact still convey its message if all images were stripped? *Fail looks like:* "image of a chart" or no alt at all.
- **Keyboard navigation** — Every interactive flow can be completed without a mouse. Tab order matches visual order. Modal traps return focus on close. *Test:* unplug the mouse. *Fail looks like:* drag-only interactions with no keyboard alternative; modals that don't close on Escape.
- **Heading hierarchy is sequential** — H1 → H2 → H3 without skipping levels. Decks: titles sized consistently per layer. Documents: heading levels semantic, not visual. *Test:* outline view. *Fail looks like:* H1 → H4 jumps; multiple H1s on one screen.
- **Color is never the only signal** — Status, validity, importance must be communicated by icon, text, or weight in addition to color. *Test:* desaturate the artifact. Does meaning survive? *Fail looks like:* red/green error states with no icon; "click the blue items" instructions.
- **Reduced-motion respected** — Animations honor `prefers-reduced-motion`. *Test:* enable system-level reduced motion. *Fail looks like:* parallax scroll that ignores the preference.
- **Dynamic type / system text scaling** — On mobile, content reflows when system text size scales. *Test:* set system text to largest. *Fail looks like:* truncation, overlap, or content cut off.
- **Screen reader labels** — Icon-only buttons have `aria-label` or equivalent. Form inputs have proper `<label for>` association. *Test:* turn on VoiceOver / NVDA / TalkBack. *Fail looks like:* "button" announced with no context.
- **Skip link / escape route** — Long-form artifacts have a skip-to-main-content link or equivalent. Modals and multi-step flows have a clear cancel/back affordance. *Fail looks like:* a 5-step wizard with no way back.

---

## Priority 2: Touch and Interaction (CRITICAL — for mobile and tap-driven surfaces)

- **Touch target size ≥ 44×44pt** — Apple HIG / WCAG 2.2 AAA minimum. Material Design 48×48dp also acceptable. Visual element can be smaller; tap area cannot. *Fail looks like:* 24px close icons in mobile modals; tightly packed icon rows.
- **Touch spacing ≥ 8px** — Adjacent tappable elements don't share an edge. *Fail looks like:* icon button rows with zero spacing.
- **Tap feedback within 100–150ms** — Visible response on touch (ripple, opacity, scale, or color shift). *Fail looks like:* taps with no acknowledgment until the result loads.
- **Hover-only interactions banned on touch surfaces** — No info that's only visible on hover. *Fail looks like:* tooltips that never trigger on mobile.
- **Loading buttons are disabled during async** — A button that fires a request becomes disabled and shows a spinner or progress until the response. *Fail looks like:* double-submission via repeated taps.
- **Pressed state doesn't shift layout** — `:active` uses transform/opacity, not bounds-changing margin or padding. *Fail looks like:* surrounding content jumping when a button is pressed.
- **Standard gestures preserved** — Swipe-back on iOS, pinch-zoom, system back gestures all work. *Fail looks like:* a custom horizontal scroll that conflicts with iOS edge-swipe.
- **Safe-area aware** — Content respects notch, Dynamic Island, gesture bar, and screen edges on mobile. *Fail looks like:* CTAs hidden under the gesture bar.

---

## Priority 3: Performance (HIGH)

For digital surfaces (web, app, prototype). Skip for static documents and decks except the bundle/asset items.

- **Image dimensions declared** — Width/height or aspect-ratio set on every image to prevent CLS. *Fail looks like:* layout reflowing as images load.
- **Modern image formats** — WebP or AVIF for raster, SVG for vector. Lazy-load below-the-fold images with `loading="lazy"`. *Fail looks like:* hero image as a 4MB JPEG.
- **Font loading discipline** — `font-display: swap` for primary fonts. Preload only the critical weights. *Fail looks like:* invisible text for 2 seconds while a 200KB display font loads.
- **CLS < 0.1** — Cumulative layout shift below the Core Web Vitals threshold. *Fail looks like:* navigation jumping when ads or async content load.
- **Hardware-accelerated animations only** — Animate `transform` and `opacity`. Never animate `top`, `left`, `width`, or `height`. *Fail looks like:* janky modal entry animating `top`.
- **Bundle / asset size reasonable** — JavaScript bundles split by route. Documents and decks check final file size (decks > 50MB raise a flag). *Fail looks like:* a 200MB pptx with embedded uncompressed video.

---

## Priority 4: Style consistency (HIGH)

The signature checks. Most "feels generic" failures live here.

- **Style register committed** — One dominant register from [[stylistic-vocabulary]] selected and applied across all decisions. *Fail looks like:* Swiss-modern type with maximalist color and minimal-soft layout in the same artifact.
- **Token discipline** — All colors are semantic tokens, not raw hex. All spacing is a multiple of the base unit (4 or 8). All type uses the defined scale. *Fail looks like:* `#3B82F6` and `#3b83f6` and `#4080F0` all appearing in the same file.
- **Icon set unified** — One icon family (Lucide, Heroicons, Phosphor, custom) used throughout. Stroke widths consistent. Filled vs. outline disciplined per hierarchy level. *Fail looks like:* Lucide outline icons next to Material filled icons.
- **No emojis as structural icons** — Emojis are font-dependent and inconsistent across platforms. Use SVG icons for navigation, settings, and system controls. *Fail looks like:* 🚀 in a button label.
- **Brand assets correct** — Logos use official files. Clearspace and minimum size respected. No unauthorized recoloring or proportional changes. *Fail looks like:* a stretched, pixelated, or recolored client logo.
- **Dark mode parity (where applicable)** — Light and dark modes both designed and tested. Not just inverted. *Fail looks like:* text contrast that works in light mode and fails in dark.

---

## Priority 5: Layout and responsive (HIGH)

For digital surfaces. Decks check the equivalent slide-layout consistency; documents check page-grid consistency.

- **Mobile-first or mobile-aware** — Every digital design has an explicit mobile state, not an after-thought. Asymmetric desktop layouts collapse to single-column on mobile. *Fail looks like:* horizontal scroll on a 375px viewport.
- **Breakpoint consistency** — Standard breakpoints used (e.g., 375 / 768 / 1024 / 1440). *Fail looks like:* a `@media (min-width: 1097px)` arbitrary number.
- **Container width disciplined** — `max-w-7xl` or similar bound on text-heavy sections. *Fail looks like:* full-bleed paragraphs at 2000px width.
- **Z-index scale** — Defined scale (e.g., 0 / 10 / 20 / 40 / 100 / 1000). No arbitrary `z-9999`. *Fail looks like:* tooltips appearing under modals.
- **Spacing rhythm** — Consistent spacing scale across the artifact. Section spacing > component spacing > micro spacing. *Fail looks like:* gaps that vary unpredictably across screens.
- **No horizontal scroll** — Mobile content fits viewport width. *Fail looks like:* "scroll right to see more →" indicators on phone widths.

---

## Priority 6: Typography and color (MEDIUM)

- **Body text ≥ 16px on mobile** — Smaller triggers iOS auto-zoom on input focus. *Fail looks like:* 14px form labels that zoom the page on tap.
- **Line length 50–75 characters** — Mobile 35–60. Desktop 60–75. *Fail looks like:* full-page-width body text on a 1600px container.
- **Line height 1.4–1.6× for body** — Tighter for display, looser for small print. *Fail looks like:* body text at line-height 1.0.
- **Modular type scale** — No arbitrary 15px/17px/19px. Use a scale (12, 14, 16, 18, 24, 32, 48) or modular ratio. *Fail looks like:* sizes scattered across an artifact with no discernible system.
- **Semantic color tokens** — Components reference `--color-text-primary`, not `--gray-900`. *Fail looks like:* `bg-blue-600` hardcoded in a component when the brand color changes.
- **Tabular numerals for data** — Tables, prices, timers use `font-variant-numeric: tabular-nums` or a mono. *Fail looks like:* a price column where digits don't align.
- **Whitespace as content** — Generous margins around hero elements. Gaps between sections > gaps within sections. *Fail looks like:* every section the same distance from the next.

---

## Priority 7: Animation and motion (MEDIUM)

- **Motion communicates** — Every animation expresses cause-effect, direction, or state. Never decoration. *Fail looks like:* a fade-in on the hero that conveys nothing.
- **Duration discipline** — Micro-interactions 150–300ms. Component transitions ≤ 400ms. Page transitions ≤ 500ms. *Fail looks like:* a 1.2s modal entry animation that delays user input.
- **Easing intentional** — Ease-out for entering. Ease-in for exiting. Spring/physics for natural feel. Linear only for progress bars and strict-ratio motion. *Fail looks like:* default `transition: all 0.5s linear` on every state change.
- **Exit faster than enter** — Exit animations 60–70% of enter duration. Feels responsive. *Fail looks like:* a modal that takes longer to close than to open.
- **Stagger on lists** — List items mount with 30–50ms staggers. *Fail looks like:* 20 items appearing simultaneously.
- **Animations interruptible** — User input cancels in-progress animations immediately. *Fail looks like:* a slide that ignores taps until it finishes.
- **No layout-shifting motion** — Motion uses transform/opacity, not properties that reflow.
- **Reduced-motion respected** — Animations either disabled or shortened to <50ms when `prefers-reduced-motion` is set.

For deeper motion-craft principles, see [[motion-and-transitions]] (TBD — Session 1.5 enhancement).

---

## Priority 8: Forms and feedback (MEDIUM)

- **Visible labels above inputs** — Not placeholder-only. *Fail looks like:* a form where the placeholder disappears as the user types and they forget what the field was for.
- **Errors below the related field** — Inline, with the field. Not summarized at the top of the form. *Fail looks like:* "Please correct the highlighted fields" with no inline messages.
- **Helper text persistent for complex inputs** — Below the field, not in the placeholder.
- **Required fields marked** — Asterisk or text label. *Fail looks like:* ambiguity about which fields are mandatory.
- **Submit shows loading then success/error** — The button transitions through states. *Fail looks like:* a click that does nothing visible for 4 seconds.
- **Error messages name cause and recovery** — "Email is missing" not "Please fill all required fields." *Fail looks like:* "An error occurred."
- **Inline validation on blur** — Not on every keystroke. *Fail looks like:* "Email is invalid" appearing while the user types `b@`.
- **Confirmation before destructive actions** — Delete, cancel-subscription, irreversible-publish all confirm. *Fail looks like:* a one-click delete.
- **Undo for bulk or destructive actions** — Toast with undo for 5+ seconds. *Fail looks like:* "all 47 items deleted" with no recovery path.
- **Auto-save for long forms** — Drafts persisted; user doesn't lose work on accidental dismissal. *Fail looks like:* an essay-length form that resets when the tab refreshes.

---

## Priority 9: Navigation patterns (HIGH for product UI)

- **Bottom-nav max 5 items** — More than 5 is overflow. *Fail looks like:* 7 tabs cramped at the bottom.
- **Active state visible on current location** — Color, weight, icon-fill, or indicator distinguishes "you are here." *Fail looks like:* nav items that look identical regardless of route.
- **Back behavior predictable** — Browser back / iOS back-swipe / Android back all work. Scroll position and form state restored. *Fail looks like:* back-button that resets the page to top with empty filters.
- **Deep links work** — Every key destination has a stable URL or route. *Fail looks like:* "you must navigate from the homepage to see this view."
- **Modal vs. navigation distinction** — Modals are for transient tasks, not primary navigation. *Fail looks like:* a "settings" modal that's actually a multi-screen flow.
- **Persistent core nav** — Nav doesn't disappear on deep pages. *Fail looks like:* a sub-page where the user can't get home except via browser back.
- **Destructive actions separated from nav** — Logout, delete-account, etc. visually and spatially separate. *Fail looks like:* "log out" next to "home" in the same nav bar.

---

## Priority 10: Charts and data (LOW for non-data work, HIGH for data-heavy)

- **Chart type matches data type** — Trend → line. Comparison → bar. Proportion → bar (rarely pie). Distribution → histogram. *Fail looks like:* pie charts with 12 slices.
- **Color is supplemented** — Pattern, texture, label, or shape backs up color encoding. *Fail looks like:* a red-vs-green chart that fails for color-blind users.
- **Legend present and near** — Position adjacent to the chart, not below the fold.
- **Tooltips on interaction** — Exact values surface on hover (web) or tap (mobile).
- **Empty data state explicit** — "No data yet" with guidance, not a blank axis frame.
- **Loading state has skeleton** — Don't show an empty axis while data loads.
- **Tabular numerals on axes** — Numbers align across rows.
- **Direct labeling for small datasets** — Label values on the chart itself when there are <10 data points.
- **Locale-aware formatting** — Numbers, dates, currencies formatted per user locale.
- **Export option for data-heavy charts** — CSV or image export available.

---

## Surface-specific extras

### Presentations (16:9)

- **One claim per slide.** Body content under 30 words.
- **Title-case or sentence-case picked once and held** across all slide titles.
- **Speaker notes populated** for any slide that's not self-explanatory.
- **Outline view passes** — slide titles alone tell the story.
- **No slide reads as a document.** Slide-as-document goes in a paired document.
- **Cover and closing slides earn attention** — neither is template default.

### Documents (8.5×11)

- **Margins consistent** across pages. Top/bottom/inside/outside per the grid.
- **Running header/footer disciplined** — page numbers, doc title, version, date present and correct.
- **Cover earns the document** — no full-bleed Unsplash defaults.
- **TOC accurate** — page numbers match.
- **Pull quotes used sparingly** — one per spread, max.
- **Captions on every figure** — figure number, title, source where applicable.
- **Citation style consistent** — one style (APA, Chicago, footnote) used throughout.

### Social assets (LinkedIn / Instagram)

- **Aspect ratios native** — LinkedIn 1.91:1 (1200×627) for link previews, 1:1 for posts; Instagram 1:1, 4:5, 9:16 for stories. No 1080×1080 cropped from a 1200×627.
- **Type readable on mobile** — body type ≥ 18px equivalent at the rendered size.
- **Brand wordmark or signature present** — discoverable without being intrusive.
- **Carousel cohesive** — first card hooks, last card commits, middle cards earn the click.
- **Single-claim test** — if you removed every word and kept only the typography and color, would the asset still suggest a topic?

### Brand identity application

- **Logo files in correct format** — SVG primary, PNG fallback. Light and dark variants exist.
- **Clearspace respected** — minimum margin around the mark per guidelines.
- **Tagline lockup correct** — if a tagline lockup exists, used per guidelines (not retyped).
- **Typography licensed for use** — display and text fonts licensed for the surface (web, app, video, embedded).
- **Color palette applied with hierarchy** — primary owns the moments; secondary supports; functional handles state.

---

## Final pre-show pass

Run this before any output leaves the studio. Five questions, in order. If any fails, iterate.

1. **Did the four checks pass?** (`design-taste`'s swap, squint, signature, token tests.)
2. **Are all CRITICAL items above clear?**
3. **Are all HIGH items either clear or explicitly waived with a noted reason?**
4. **Is the artifact metadata complete?** (Title, status, version, date, owner, linked story per `design-process` Rule 9.)
5. **Could you describe what makes this artifact specific to its brief in one sentence?** If not, it's still defaulted somewhere.

If the artifact passes all five, ship it. If it doesn't, fix what's broken and run the pass again — don't waive on aesthetic grounds.

---

## How this reference is used

- **Loaded by:** [[design-taste]], [[design-process]], all execution skills (`design-screen`, `design-presentation`, `design-document`, `design-social-asset`, `design-product-designer`, `design-web-designer`).
- **Pairs with:** [[ai-tells-forbidden-patterns]] (the bans), [[stylistic-vocabulary]] (the registers), [[visual-design-foundations]] (static fundamentals), [[interaction-patterns]] (dynamic fundamentals).
- **Cited in audit:** `design-audit` skill runs this checklist against the artifact and produces the named-failure report.
