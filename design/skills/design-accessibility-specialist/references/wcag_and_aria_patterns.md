---
type: reference
project: skills-library
scope: skill
plugin: design
parent: "[[accessibility-specialist]]"
tags: [type/reference, plugin/design, scope/skill, topic/design]
status: active
---
# WCAG 2.2 and ARIA Patterns Reference

## WCAG 2.2 at a Glance: Every AA Criterion

### Perceivable: Content Must Be Perceivable to All Senses

#### 1.1 Text Alternatives
- **1.1.1 Non-text Content (Level A)**: All images, buttons, icons have text alternatives
  - Decorative images: `alt=""`
  - Informative images: `alt="descriptive text of content/purpose"`
  - Complex images (charts, diagrams): `alt="brief description"` + visible long description nearby or via link
  - Functional images (buttons, links): `alt="action the image represents"`

#### 1.2 Time-based Media
- **1.2.1 Audio-only and Video-only (Prerecorded, Level A)**: Transcripts provided
- **1.2.2 Captions (Prerecorded, Level A)**: Videos have closed captions
- **1.2.3 Audio Description or Media Alternative (Prerecorded, Level A)**: Video has narration or transcript describing visual content
- **1.2.5 Audio Description (Prerecorded, Level AA)**: Videos with dialogue have narrated descriptions of important visual information

#### 1.3 Adaptable
- **1.3.1 Info and Relationships (Level A)**: Semantic HTML conveys meaning; don't use layout elements for structure
  - Headings, landmarks, lists, labels, table headers
  - Relationships: labels associated with form fields, related form fields grouped
  - Common violation: using `<div>` styled as button instead of `<button>`
  - Common violation: skipping heading levels (h1 → h3, skipping h2)

- **1.3.2 Meaningful Sequence (Level A)**: Reading order matches visual order
  - Avoid tabindex > 0 (positive values break natural order)
  - DOM order determines reading order; CSS doesn't change it
  - Common violation: floated elements read out of visual order

- **1.3.4 Orientation (Level AA)**: Content not restricted to portrait or landscape
  - Don't lock to one orientation; allow users to rotate device
  - Use CSS media queries: `@media (orientation: portrait) { ... }`

- **1.3.5 Identify Input Purpose (Level AA)**: Form fields have autocomplete attributes
  - `autocomplete="given-name"`, `"family-name"`, `"email"`, `"tel"`, `"address-line1"`, etc.
  - Helps users with cognitive disabilities and autofill
  - HTML spec defines full list of valid values

- **1.3.6 Identify Purpose (Level AAA)**: Icons, symbols, and interactive components have text labels or accessible names
  - Icon buttons need aria-label: `<button aria-label="Close menu">×</button>`
  - Tooltip or nearby text can provide label (aria-describedby)

#### 1.4 Distinguishable
- **1.4.1 Use of Color (Level A)**: Color not used alone to convey information
  - Pair color with text, icon, pattern, or number
  - Don't say "required fields are red"; use asterisk or explicit text label

- **1.4.3 Contrast (Minimum, Level AA)**: Text and background contrast
  - Normal text: 4.5:1 contrast ratio (or 18pt+ text: 3:1)
  - UI components (borders, focus indicators): 3:1 contrast ratio
  - Use WebAIM contrast checker or built-in browser tools
  - Include focus indicators in contrast requirement

- **1.4.5 Images of Text (Level AA)**: Don't use images to display text (except logos, branding)
  - Text in images can't be resized, recolored, or read by screen readers
  - Use actual text with CSS styling instead

- **1.4.10 Reflow (Level AA)**: Content doesn't require horizontal scrolling at 320px width or 200% zoom
  - Use responsive design; mobile-friendly
  - Single-column layouts below 320px viewport

- **1.4.11 Non-text Contrast (Level AA)**: Graphical elements, buttons, icons have 3:1 contrast
  - Applies to any visual information needed to identify the element
  - Focus indicators: 2px solid, 3:1 contrast against adjacent color

- **1.4.12 Text Spacing (Level AA)**: Users can override spacing without content loss
  - Don't break if users set: line-height 1.5×, letter-spacing 0.12em, word-spacing 0.16em, paragraph spacing 2×

- **1.4.13 Content on Hover or Focus (Level AA)**: Content triggered on hover/focus is dismissable, hoverable, and persistent
  - Escape key dismisses tooltips
  - Hover/focus dismissal: stay until user moves focus or presses Escape
  - Don't hide content that was visible before

---

### Operable: Functionality Must Be Operable via Keyboard

#### 2.1 Keyboard Accessible
- **2.1.1 Keyboard (Level A)**: All functionality available via keyboard
  - Tab/Shift+Tab to navigate widgets
  - Enter/Space to activate buttons/links
  - Arrow keys for composite widgets (tabs, menus, selects)
  - Escape to dismiss overlays
  - No keyboard trap (unless intentional like dialog focus trap with documented escape)

- **2.1.2 No Keyboard Trap (Level A)**: Users can tab away from every element
  - Exception: modal dialogs intentionally trap focus (but Escape exits)
  - Exception: fullscreen games/applications with known workaround

- **2.1.4 Character Key Shortcuts (Level A)**: Keyboard shortcuts using single keys (not Ctrl+key) don't interfere
  - Document shortcuts or disable them for text input fields
  - Example: single `s` key shouldn't activate search unless form is focused

#### 2.2 Enough Time
- **2.2.1 Timing Adjustable (Level A)**: Don't auto-expire content before 3 seconds without warning
  - Sessions: allow users to extend timeout
  - Auto-playing content: can be paused or hidden
  - Real-time games: exempt (can't stop time)

#### 2.3 Seizures and Physical Reactions
- **2.3.1 Three Flashes or Below Threshold (Level A)**: Avoid flashing 3+ times per second
  - Includes flashing elements in focus or unfocused regions
  - GIFs, videos, animations—especially important for photosensitive epilepsy

#### 2.4 Navigable
- **2.4.1 Bypass Blocks (Level A)**: Skip navigation link to main content
  - "Skip to main content" link visible on focus or always visible
  - Allows keyboard users to bypass header, navigation, etc.

- **2.4.2 Page Titled (Level A)**: Page has descriptive `<title>`
  - Title should identify page and site
  - Update title on SPAs when route changes (use `next/head` or metadata API)

- **2.4.3 Focus Order (Level A)**: Tab order is logical and meaningful
  - Use natural DOM order (tabindex 0 or default)
  - Don't use positive tabindex (tabindex > 0) — breaks natural order
  - Use -1 for elements navigated programmatically but not via Tab

- **2.4.4 Link Purpose (From Context, Level A)**: Link text or accessible name describes its destination
  - Good: `<a href="/about">Learn about us</a>` or `<a href="details.pdf" aria-label="Download 2024 annual report">Download</a>`
  - Bad: `<a href="/page">Click here</a>` or `<a href="/page">→</a>`
  - Context can come from surrounding paragraph, aria-label, aria-labelledby, title

- **2.4.5 Multiple Ways (Level AA)**: Content is findable through multiple navigation methods
  - Search, site map, table of contents, or navigation menu
  - Allows different user preferences to find content

- **2.4.6 Headings and Labels (Level AA)**: Headings and form labels are descriptive
  - Don't use vague headings like "Info" or "Links"
  - Form labels clearly describe the field: `<label htmlFor="email">Email address</label>`

- **2.4.7 Focus Visible (Level AA)**: Keyboard focus indicator is visible
  - CSS: `:focus-visible` (not just `:focus`) or outline style
  - Minimum 2px width, 3:1 contrast against adjacent color
  - Don't use `outline: none` without replacement (e.g., `box-shadow` or underline)

- **2.4.8 Focus Visible (Enhanced, Level AAA)**: Focus indicator is at least 2px wide/tall and has 3:1 contrast

#### 2.5 Input Modalities (New WCAG 2.2)
- **2.5.1 Pointer Gestures (Level A)**: Gestures (swipe, pinch, long-press) have keyboard alternatives
  - Touch target minimum 44×44 CSS pixels
  - Consider voice control and speech-to-text compatibility

- **2.5.7 Dragging Movements (Level AA, New in 2.2)**: Any drag-and-drop operation has non-drag equivalent
  - Example: drag to reorder AND arrow keys to reorder
  - Helps users with motor disabilities

---

### Understandable: Content and Navigation Must Be Understandable

#### 3.1 Readable
- **3.1.1 Language of Page (Level A)**: Page language specified in HTML
  - `<html lang="en">` or `<html lang="en-US">`
  - Helps screen readers pronounce correctly

- **3.1.2 Language of Parts (Level AA)**: Text in different languages marked
  - `<span lang="es">Hola mundo</span>` within English page
  - Screen readers switch pronunciation and voice

#### 3.2 Predictable
- **3.2.1 On Focus (Level A)**: Focus change doesn't cause unexpected context switch
  - Focusing an input shouldn't submit the form
  - Focusing a dropdown shouldn't open it (user must choose/activate)
  - Exception: explicitly labeled behavior (aria-label states outcome)

- **3.2.2 On Input (Level A)**: Value change doesn't cause unexpected context switch
  - Changing input value shouldn't submit form (user must click Submit)
  - Selecting option in dropdown shouldn't navigate away
  - Document if behavior deviates

- **3.2.3 Consistent Navigation (Level AA)**: Navigation mechanisms repeat in same order
  - Header menu order consistent across pages
  - Sidebar position and order consistent
  - Helps users with cognitive disabilities predict flow

- **3.2.4 Consistent Identification (Level AA)**: Components with same function are identified consistently
  - "Search" button always says "Search," not "Find" or "Look up"
  - Icons always have the same meaning
  - Helps users learn patterns

#### 3.3 Input Assistance
- **3.3.1 Error Identification (Level A)**: Error messages identify the field and the problem
  - Show which input has the error: "Email field: invalid format"
  - Don't just show "Error" or generic message
  - Use `aria-describedby` to link error message to input

- **3.3.2 Labels or Instructions (Level A)**: Form fields have visible labels or instructions
  - Label must be associated: `<label htmlFor="email">Email</label> <input id="email" />`
  - Placeholder doesn't count as label (disappears on focus)
  - Required fields marked: asterisk + text "required" or aria-required="true"

- **3.3.3 Error Suggestion (Level AA)**: Error messages suggest solutions
  - "Invalid email. Use format: name@example.com"
  - "Password too short. Must be 8+ characters."

- **3.3.4 Error Prevention (Legal, Financial, Data Deletion, Level AA)**: Reversible or confirmed for high-stakes actions
  - Delete account: confirmation page
  - Financial transactions: review before submit
  - Data entry: allow user to check/confirm
  - Submissions can be reversible (like draft save)

#### 3.3.5 Help (Level AAA)**: Help text available
- Tooltips, examples, step-by-step guidance
- Links to documentation or support

---

### Robust: Content Must Be Compatible with Current and Future Technologies

#### 4.1 Compatible
- **4.1.1 Parsing (Level A)**: Valid HTML (no duplicate IDs, proper nesting, closed tags)
  - Use HTML validator: validator.w3.org
  - Invalid HTML breaks assistive tech parsing

- **4.1.2 Name, Role, Value (Level A)**: Custom components expose name, role, and state
  - Name: accessible label (aria-label, aria-labelledby, label, button text)
  - Role: ARIA role (button, link, tab, combobox, etc.)
  - Value: current state (aria-checked, aria-expanded, aria-selected)
  - Example: `<button aria-pressed="true" aria-label="Toggle mute">🔇</button>`
  - Helps screen readers announce components correctly

- **4.1.3 Status Messages (Level AA, New in 2.2)**: Status messages announced to screen readers
  - Live regions: `aria-live="polite"` or `aria-live="assertive"` (for alerts)
  - Example: "3 items added to cart" announced when item added
  - No need for aria-label if message text is clear

---

## New in WCAG 2.2

These are the high-impact criteria added in WCAG 2.2 (released September 2023):

1. **2.4.11 Focus Not Obscured (Minimum, Level AA)**: Focus indicator isn't completely hidden
   - Focus indicator must be at least partially visible
   - Dialogs, tooltips, dropdowns shouldn't completely hide focus

2. **2.5.7 Dragging Movements (Level A)**: Drag-and-drop has keyboard alternative
   - Provide button, arrow keys, or other non-drag method
   - E.g., reorder list via drag AND arrow keys

3. **2.5.8 Target Size (Minimum, Level AA)**: Touch targets (buttons, links) minimum 44×44 CSS pixels
   - Applies to every target user can interact with
   - Exception: inline links in paragraph (but should still have adequate spacing)

4. **3.2.6 Consistent Help (Level A)**: Help mechanisms in same location across site
   - "Contact us," "FAQ," "Support" in consistent place
   - Helps users with cognitive disabilities

5. **3.3.7 Accessible Authentication (Level AA)**: Authentication doesn't require recognition memory
   - Avoid: "Remember the 3rd word from your security question"
   - OK: "Enter your password," "Scan your biometric"
   - OK: CAPTCHA alternatives (password + security key, SMS code, etc.)
   - People with cognitive disabilities struggle with memory-based auth

6. **3.3.8 Redundant Entry (Level A)**: Don't ask for same information twice in same session
   - If user entered email on step 1, prefill on step 2
   - Exception: security-sensitive (password entry)

---

## ARIA Fundamentals

### The 5 Rules of ARIA
1. **Always use native HTML first.** `<button>` beats `<div role="button">`
2. **Don't use ARIA to fix bad HTML.** Bad: `<h1 role="button">Click me</h1>`. Good: `<button>Click me</button>`
3. **ARIA roles, states, properties don't change keyboard behavior.** Add keyboard listeners yourself
4. **Always manage focus for interactive ARIA widgets.** `role="dialog"` needs focus trap; `role="tab"` needs arrow key handling
5. **All interactive ARIA widgets must be keyboard operable.** If you can click it, you must be able to Tab to it and activate via keyboard

### ARIA Roles

#### Landmark Roles (Provide Document Structure)
```html
<header role="banner">Top of page</header>
<nav role="navigation">Links</nav>
<main role="main">Primary content</main>
<aside role="complementary">Sidebar, ads</aside>
<footer role="contentinfo">Copyright, links</footer>
<section role="region" aria-label="Related articles">...</section>
```
Modern HTML: use semantic elements (`<header>`, `<nav>`, `<main>`, `<aside>`, `<footer>`) instead of role attribute. Roles are implicit.

#### Widget Roles (Represent Interactive Controls)
- `role="button"` — Only if you can't use `<button>`; then add `tabindex="0"`, click handler, keyboard listener
- `role="link"` — Only if you can't use `<a>`; then add `tabindex="0"`, click handler, Enter/Space listener
- `role="tab"` — Part of `tablist`; parent manages focus via roving tabindex
- `role="tabpanel"` — Content panel controlled by tab
- `role="tablist"` — Container of tabs
- `role="menuitem"` — Part of `menu` or `menubar`
- `role="menu"` or `role="menubar"` — Container
- `role="combobox"` — Input + listbox + button (e.g., autocomplete dropdown)
- `role="listbox"` — List of selectable options
- `role="option"` — Individual option in listbox/combobox
- `role="dialog"` or `role="alertdialog"` — Modal; needs focus trap and aria-modal
- `role="alertdialog"` — Dialog announcing alert (role="alert" + dialog behavior)
- `role="tooltip"` — Appears on hover/focus; aria-describedby links it to trigger
- `role="tree"`, `role="treeitem"` — Hierarchical list (folders, navigation)
- `role="toolbar"` — Group of tool buttons; uses roving tabindex
- `role="slider"` — Input for range (volume, brightness); arrow keys adjust

#### Document Structure Roles
- `role="article"` — Self-contained content (blog post, news article, comment)
- `role="section"` — Thematic grouping; use with aria-label or aria-labelledby
- `role="search"` — Search region; usually wraps search form

#### Live Region Roles
- `role="alert"` — Urgent message; auto-announces (aria-live="assertive")
- `role="log"` — Chat, status updates; announces new content (aria-live="polite")
- `role="marquee"` — Scrolling ticker; aria-live="off" (non-essential)
- `role="status"` — Status message (polite announcement)
- `role="timer"` — Countdown or stopwatch

### ARIA States and Properties

#### States (Change During Interaction)
- `aria-checked="true|false|mixed"` — Checkbox/radio/toggle state
- `aria-expanded="true|false"` — Accordion, menu, collapsible content
- `aria-selected="true|false"` — Tab, option in listbox, tree item
- `aria-pressed="true|false"` — Toggle button state
- `aria-invalid="true|false"` — Form input validation error
- `aria-current="page|step|location|date|time"` — Marks current page in breadcrumb/nav
- `aria-disabled="true|false"` — When disabled attribute isn't appropriate (custom widget)

#### Properties (Static or Describe Relationship)
- `aria-label="description"` — Accessible name (short text)
- `aria-labelledby="id1 id2"` — Link to element(s) providing label
- `aria-describedby="id1 id2"` — Link to element(s) providing description
- `aria-placeholder="text"` — Hint inside input (different from placeholder attribute)
- `aria-required="true|false"` — Form field is required
- `aria-hidden="true|false"` — Hide from accessibility tree (decorative icons, duplicate text)
- `aria-live="polite|assertive|off"` — Announce dynamic content changes
- `aria-atomic="true|false"` — Announce whole region or just changed node
- `aria-relevant="additions removals text all"` — What changes trigger announcement
- `aria-controls="id"` — This control manages another element (button controls panel)
- `aria-owns="id"` — This element owns another (menu button owns popup menu)
- `aria-flowto="id"` — Reading order follows this element (not recommended; use DOM order)
- `aria-modal="true"` — Overlay blocks interaction with page
- `aria-haspopup="true|menu|listbox|dialog|grid|tree"` — Button opens popup
- `aria-level="n"` — Hierarchy level (tree item, heading)
- `aria-roledescription="description"` — Custom role name (use sparingly; overrides standard role announcement)
- `aria-rowcount="n"` — Total rows in virtualized table
- `aria-colcount="n"` — Total columns in virtualized table

---

## Common ARIA Patterns with Code Examples

### Alert / Status Message
```jsx
// Assertive (urgent): "Payment failed" or "Form submitted"
<div role="alert" aria-live="assertive" aria-atomic="true">
  Payment failed. Please check your card details.
</div>

// Polite (less urgent): "Item added to cart"
<div role="status" aria-live="polite" aria-atomic="true">
  3 items added to cart
</div>

// In React, update state to trigger announcement
const [message, setMessage] = useState('');
useEffect(() => {
  setMessage('Item added'); // Component re-renders, announcement fires
}, [itemAdded]);
```

### Dialog / Modal
```jsx
<div
  role="dialog"
  aria-modal="true"
  aria-labelledby="dialog-title"
  aria-describedby="dialog-desc"
>
  <h2 id="dialog-title">Confirm deletion</h2>
  <p id="dialog-desc">This action cannot be undone.</p>
  <button>Cancel</button>
  <button>Delete</button>
</div>

// Focus trap: when opened, focus first button; when closed, restore focus to trigger
// Example: use focus-trap-react or @radix-ui/react-dialog (shadcn/ui Dialog)
```

### Tabs
```jsx
<div role="tablist">
  <button
    role="tab"
    aria-selected="true"
    aria-controls="panel-1"
    id="tab-1"
    tabIndex="0"
  >
    Tab 1
  </button>
  <button
    role="tab"
    aria-selected="false"
    aria-controls="panel-2"
    id="tab-2"
    tabIndex="-1"  // Roving tabindex: only active tab is focusable
  >
    Tab 2
  </button>
</div>

<div id="panel-1" role="tabpanel" aria-labelledby="tab-1">
  Content 1
</div>
<div id="panel-2" role="tabpanel" aria-labelledby="tab-2" hidden>
  Content 2
</div>

// Keyboard handler: arrow keys move focus between tabs
// Only focused tab is tabindex="0"; others are tabindex="-1"
```

### Combobox (Autocomplete Dropdown)
```jsx
<div role="combobox" aria-expanded="true" aria-haspopup="listbox" aria-owns="listbox-id">
  <input
    aria-controls="listbox-id"
    aria-autocomplete="list"
    type="text"
    placeholder="Search users"
  />
</div>

<ul id="listbox-id" role="listbox">
  <li role="option" aria-selected="true">John Doe</li>
  <li role="option" aria-selected="false">Jane Smith</li>
</ul>

// Keyboard: arrow keys navigate options, Enter selects, Escape closes
```

### Menu (Button + Popup Menu)
```jsx
<button aria-haspopup="menu" aria-expanded="false" id="menu-btn">
  Options
</button>

<ul role="menu" aria-labelledby="menu-btn" hidden>
  <li role="menuitem">Edit</li>
  <li role="menuitem">Delete</li>
  <li role="menuitem" tabIndex="-1">Archive</li>
</ul>

// Keyboard: Tab into button, Space/Enter opens menu, arrows navigate, Enter activates, Escape closes
// Only one menuitem is tabindex="0"
```

### Accordion
```jsx
<div>
  <h3>
    <button
      aria-expanded="false"
      aria-controls="accordion-panel-1"
      id="accordion-btn-1"
    >
      Section 1
    </button>
  </h3>
  <div id="accordion-panel-1" hidden>
    Content 1
  </div>
</div>

// Keyboard: Tab through buttons, Space/Enter toggles expanded state
```

### Tooltip
```jsx
<button aria-describedby="tooltip-id">
  Help icon
</button>

<div id="tooltip-id" role="tooltip" hidden>
  This is additional context about the button.
</div>

// Show on hover or focus; Escape dismisses
// Don't use title attribute; use role="tooltip" + aria-describedby
```

### Breadcrumb
```jsx
<nav aria-label="Breadcrumb">
  <ol>
    <li><a href="/">Home</a></li>
    <li><a href="/products">Products</a></li>
    <li aria-current="page">Shoes</li>
  </ol>
</nav>

// Last item (current page) is not a link, uses aria-current="page"
// Navigation announces "Breadcrumb" via aria-label
```

### Tree View (Hierarchical List)
```jsx
<ul role="tree">
  <li role="treeitem" aria-expanded="false" aria-level="1">
    <button aria-controls="subtree-1">
      Folder 1
    </button>
    <ul id="subtree-1" role="group" hidden>
      <li role="treeitem" aria-level="2">File 1.txt</li>
    </ul>
  </li>
</ul>

// Keyboard: arrows expand/collapse, arrows navigate, Enter activates
```

---

## Keyboard Navigation Patterns

### Basic Patterns for All Components

| Interaction | Keys |
|---|---|
| Navigate between widgets | Tab, Shift+Tab |
| Activate button/link | Enter, Space |
| Navigate within menu/list | Arrow Up/Down, arrow Left/Right (for expand/collapse) |
| Close overlay | Escape |
| First/Last in list | Home, End |

### Tab Order Best Practices
1. Use natural DOM order (don't override with tabindex)
2. Use `tabindex="0"` only when needed (e.g., divs that should be focusable)
3. Use `tabindex="-1"` for elements managed programmatically (hidden menu items)
4. Never use positive tabindex (e.g., `tabindex="1"`, `tabindex="2"`); breaks natural order

### Roving Tabindex Pattern
Used for composite widgets (menu, tabs, toolbar) where only one child is focusable via Tab:

```jsx
// Only active item is tabindex="0"; others are "-1"
// When user arrows to next item, move tabindex="0" to new item
function updateRovingTabindex(newFocusItem) {
  items.forEach(item => item.tabIndex = -1);
  newFocusItem.tabIndex = 0;
  newFocusItem.focus();
}

// Arrow keys trigger updateRovingTabindex
```

### Focus Trap (Modals, Dialogs)
```jsx
// All focusable elements within modal: keep focus inside
// If Tab from last focusable element, move to first
// Escape key closes modal and restores focus to trigger button

const lastFocusable = modal.querySelector('[data-last-focusable]');
if (event.key === 'Tab' && !event.shiftKey && document.activeElement === lastFocusable) {
  event.preventDefault();
  firstFocusable.focus();
}
```

### Skip Navigation Link
```html
<!-- Visible on focus, hidden off focus; allows keyboard users to skip to main content -->
<a href="#main" class="sr-only focus:not-sr-only">Skip to main content</a>

<header>...</header>
<nav>...</nav>

<main id="main">
  Primary content...
</main>

/* CSS: sr-only = screen reader only */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  overflow: hidden;
}

.sr-only:focus {
  position: static;
  width: auto;
  height: auto;
  overflow: visible;
}
```

---

## Screen Reader Behavior Differences

| Feature | VoiceOver (Mac/iOS) | NVDA (Windows) | JAWS (Windows) |
|---|---|---|---|
| **Live regions** | Announces immediately | Slight delay | May require focus |
| **aria-atomic** | Entire region read | Entire region read | Entire region read |
| **aria-live="assertive"** | Interrupts | Interrupts | May queue |
| **Skip links** | Works via rotor or direct link | Works | Works |
| **Form validation** | Announces aria-invalid, error text | Announces aria-invalid, error text | Announces aria-invalid |
| **Heading navigation** | Rotor shows all headings | Keyboard: H key | Keyboard: H key |
| **Link announcements** | "Link, visit Apple" | "Link, visit Apple" | "Link, visit Apple" |
| **Decorative images** | `alt=""` ignored | `alt=""` ignored | `alt=""` ignored |
| **aria-label** | Replaces content | Replaces content | Replaces content |

### Key VoiceOver Quirks
- Announces `<button>` as "button, label"
- May announce aria-live regions late or require explicit focus
- Rotor (VO + U) shows headings, links, images, landmarks

### Key NVDA Quirks
- Browse mode vs focus mode (browse = reading, focus = form interaction)
- May not announce aria-live regions immediately
- Form mode needed to interact with inputs

### Key JAWS Quirks
- Announcement queue; may skip assertive live regions if rapid updates
- Virtual cursor (reading) vs focus mode (interaction)
- Custom roving tabindex handling can be unpredictable

---

## Image Alt Text Decision Tree

```
Is the image decorative (visual enhancement, borders, repeated)?
├─ Yes → alt="" (empty alt attribute)
│
└─ No, is it informative (photo, icon conveying meaning)?
   ├─ Yes, is it simple (one main idea)?
   │  ├─ Yes → alt="concise description of content/purpose"
   │  │         Example: alt="Apple logo"
   │  │
   │  └─ No, is it complex (chart, diagram, map)?
   │     ├─ Yes → alt="brief description" + visible long description nearby
   │     │         Example: alt="Sales by region" + table below chart
   │     │
   │     └─ No → alt="description of what the image shows"
   │
   └─ No, is it a button or interactive?
      └─ Yes → alt="action the button represents"
              Example: alt="Search" for search button icon
```

### Examples
- Decorative divider: `<img src="divider.svg" alt="" />`
- Photo in article: `<img src="sunset.jpg" alt="Orange sunset over ocean at Cannon Beach" />`
- Complex chart: `<img src="revenue-chart.svg" alt="2024 Revenue by Quarter" /> <table>...detailed breakdown...</table>`
- Icon button: `<button aria-label="Share"><IconShare /></button>` OR `<img src="share.svg" alt="Share" />`
- Functional image (logo): `<a href="/"><img src="logo.svg" alt="Company name" /></a>`

---

## Form Accessibility Essentials

### Label Association
```jsx
// Explicit association (best)
<label htmlFor="email">Email address</label>
<input id="email" type="email" />

// Implicit (acceptable, but less reliable)
<label>
  Email address
  <input type="email" />
</label>

// aria-label (when visible label not possible)
<input aria-label="Email address" type="email" />

// aria-labelledby (label in non-standard location)
<h3 id="email-label">Contact information</h3>
<input aria-labelledby="email-label" type="email" />
```

### Required Field Indication
```jsx
// Text indicator
<label htmlFor="email">
  Email address <span aria-label="required">*</span>
</label>
<input id="email" required aria-required="true" />

// aria-required alone (if visual indicator present)
<input aria-required="true" />

// Don't rely on color alone; pair with text/icon
```

### Error Messaging
```jsx
<label htmlFor="email">Email address</label>
<input id="email" aria-invalid="true" aria-describedby="email-error" />
<div id="email-error" role="alert">
  Invalid email. Use format: name@example.com
</div>

// aria-describedby links input to error message
// role="alert" announces error immediately
// aria-invalid="true" announces field state
```

### Grouped Related Fields
```jsx
<fieldset>
  <legend>Contact method</legend>
  <label>
    <input type="radio" name="contact" value="email" />
    Email
  </label>
  <label>
    <input type="radio" name="contact" value="phone" />
    Phone
  </label>
</fieldset>

// fieldset groups related inputs
// legend provides group name
```

---

## Common Violations and Fixes

| Violation | Why It's Wrong | Fix |
|---|---|---|
| `<div onclick="">` | No semantics, no keyboard | Use `<button>` |
| `<a href="javascript:void(0)">` | Not a real link | Use `<button>` for actions |
| `<h1 style="font-size: 12px">` | Heading skipped, text too small | Use correct `<h2>`/`<h3>` |
| `<img src="icon.svg">` (no alt) | Image purpose hidden | Add `alt="descriptive text"` |
| `<label>Email</label> <input>` (no htmlFor) | Label not associated | Add `htmlFor="input-id"` |
| Color as only indicator (red for required) | Colorblind users miss it | Add text or icon: "required *" |
| `outline: none` without replacement | Focus invisible | Add `box-shadow` or `border` |
| `tabindex="5"` | Breaks natural tab order | Remove, use CSS or DOM order |
| No skip link | Keyboard users stuck in header | Add "Skip to main content" link |
| Auto-playing video with sound | Unexpected audio startles users | Provide play/pause controls, mute by default |
| Form submit on blur | Unexpected behavior | Only submit on explicit button click |
| Placeholder only (no label) | Placeholder disappears on focus | Add visible `<label>` |
| Unannounced dynamic content | Screen readers miss updates | Add `aria-live="polite"` |

