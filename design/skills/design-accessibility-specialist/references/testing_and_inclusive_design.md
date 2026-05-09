---
type: reference
project: skills-library
scope: skill
plugin: design
parent: "[[accessibility-specialist]]"
tags: [type/reference, plugin/design, scope/skill, topic/design]
status: active
---
# Accessibility Testing and Inclusive Design Patterns

## Automated Testing Tools and What They Catch

### axe-core
**What it is**: Automated accessibility testing engine powering many tools; checks ~80 WCAG rules.
**What it catches**: ~30-40% of accessibility violations (good for catching obvious issues early)
**What it misses**: Visual contrast calculations (slightly), reading order interpretation, user experience flows, content clarity

#### Setup in Jest/Vitest
```js
import { axe, toHaveNoViolations } from 'jest-axe';

expect.extend(toHaveNoViolations);

test('form has no accessibility violations', async () => {
  const { container } = render(<LoginForm />);
  const results = await axe(container);
  expect(results).toHaveNoViolations();
});
```

#### Setup in Playwright E2E Tests
```js
import { injectAxe, checkA11y } from 'axe-playwright';

test('homepage is accessible', async ({ page }) => {
  await page.goto('http://localhost:3000');
  await injectAxe(page);
  await checkA11y(page); // Throws if violations found
});

// Exclude known issues or third-party elements
await checkA11y(page, '#main', {
  rules: {
    'color-contrast': { enabled: false }, // Known issue in design system
  },
});
```

#### Common axe Rules
- `color-contrast` — Text/background contrast ratio
- `image-alt` — Images have alt text
- `label` — Form fields have labels
- `aria-required-attr` — ARIA attributes are valid
- `button-name` — Buttons have accessible name
- `landmark-main` — Page has main landmark
- `heading-order` — Heading hierarchy doesn't skip levels
- `valid-aria-role` — ARIA roles are valid
- `link-name` — Links have accessible name

#### Custom Rules Example
```js
// Create custom rule: all buttons must have aria-label or text content
const customRules = [
  {
    id: 'button-accessible-name',
    evaluate: (node) => {
      const ariaLabel = node.getAttribute('aria-label');
      const text = node.textContent?.trim();
      return !!(ariaLabel || text);
    },
    impact: 'critical',
  },
];

const results = await axe(container, { rules: customRules });
```

---

### Lighthouse
**What it is**: Google's web quality auditing tool (bundled in Chrome DevTools, CLI available)
**What it catches**: Performance, SEO, accessibility (a11y subset), PWA
**What it misses**: Many manual accessibility checks; good for general health

#### Lighthouse A11y Scoring
- Checks ~50 automated rules
- Returns score 0-100
- Common failures: contrast, missing alt text, missing form labels, missing heading structure
- Score is decent baseline, not comprehensive

#### Lighthouse CI Integration
```js
// lighthouse-config.json
{
  "ci": {
    "preset": "lighthouse:recommended",
    "numberOfRuns": 3,
    "assert": {
      "preset": "lighthouse:recommended",
      "assertions": {
        "categories:accessibility": ["error", { "minScore": 0.95 }]
      }
    }
  }
}

// In CI/CD: lhci autorun
```

---

### eslint-plugin-jsx-a11y
**What it is**: ESLint plugin catching JSX accessibility violations at code review time
**What it catches**: ~60 rules for JSX patterns (missing labels, invalid ARIA, etc.)
**What it misses**: Runtime behavior, actual keyboard navigation, screen reader announcements

#### Setup
```js
// .eslintrc.js
{
  "extends": ["plugin:jsx-a11y/recommended"],
  "rules": {
    "jsx-a11y/alt-text": "error",
    "jsx-a11y/label-has-associated-control": "error",
    "jsx-a11y/aria-unsupported-elements": "error",
    "jsx-a11y/aria-props": "error",
    "jsx-a11y/aria-proptypes": "error",
    "jsx-a11y/aria-role": "error",
    "jsx-a11y/click-events-have-key-events": "warn", // Can be noisy
    "jsx-a11y/no-autofocus": "warn",
  }
}
```

#### Common Rules
- `alt-text` — All images must have alt attribute
- `label-has-associated-control` — Form inputs have labels
- `click-events-have-key-events` — onClick should also handle keyboard
- `no-interactive-element-to-noninteractive-role` — Don't use buttons as text containers
- `anchor-is-valid` — Links have valid href or role
- `aria-props` — aria-* attributes are valid
- `aria-role` — role attribute values are valid
- `aria-unsupported-elements` — Don't add ARIA to elements that don't support it

---

### @storybook/addon-a11y
**What it is**: Storybook addon integrating axe-core; visual playground for accessibility testing
**What it catches**: Same as axe-core, but in design context; easy to test component states
**What it misses**: Interactions, keyboard navigation (without Storybook interactions addon)

#### Setup
```js
// .storybook/main.js
module.exports = {
  addons: ['@storybook/addon-a11y'],
};

// In story
export const ButtonPrimary = () => <Button>Click me</Button>;
ButtonPrimary.parameters = {
  a11y: {
    config: {
      rules: [
        {
          id: 'color-contrast',
          enabled: false, // Skip for this story
        },
      ],
    },
  },
};
```

#### Workflow
1. Run Storybook: `npm run storybook`
2. Click "Accessibility" tab in Storybook UI
3. View axe violations for current component + state
4. Test all variants (default, hover, focused, disabled, etc.)
5. Fix violations before merging

---

### Pa11y CLI
**What it is**: Command-line accessibility testing tool (wrapper around axe and other engines)
**What it catches**: Similar to axe; good for batch scanning multiple pages
**What it misses**: Interactive behavior; slower than axe in isolation

#### Usage
```bash
# Test single page
pa11y http://localhost:3000

# Test multiple pages (reporter)
pa11y http://localhost:3000/page1 http://localhost:3000/page2

# Output as JSON
pa11y --reporter json http://localhost:3000 > results.json

# Threshold: fail if violations >= critical count
pa11y --threshold 1 http://localhost:3000
```

---

### Manual Testing Checklist

Automation catches ~30-40%; manual testing finds the rest. Use this checklist per page/component:

#### Keyboard Navigation
- [ ] Can I Tab to all interactive elements (buttons, links, inputs)?
- [ ] Is Tab order logical (left-to-right, top-to-bottom)?
- [ ] Can I use Shift+Tab to go backward?
- [ ] Can I activate buttons and links with Enter or Space?
- [ ] Can I dismiss overlays with Escape?
- [ ] Are focus indicators visible at all times?
- [ ] Can I navigate dropdown/menu/combobox with arrow keys?
- [ ] Is there a skip-to-main-content link?

#### Screen Reader (VoiceOver on Mac, NVDA on Windows)
- [ ] Are all images announced with meaningful alt text?
- [ ] Are form fields announced with labels?
- [ ] Are error messages announced immediately?
- [ ] Is page title descriptive?
- [ ] Are headings in logical order (h1 → h2 → h3)?
- [ ] Are landmarks present (main, nav, aside, footer)?
- [ ] Are list items announced as lists?
- [ ] Are table headers properly announced?
- [ ] Are interactive components announced with role and state (e.g., "button, collapsed")?
- [ ] Do dynamic content updates (toasts, notifications) get announced?
- [ ] Can I navigate by headings, landmarks, links?

#### Zoom and Text Sizing
- [ ] Does page layout hold at 200% zoom?
- [ ] Can I increase font size 2× without content hiding or breaking?
- [ ] No horizontal scrolling at 320px viewport?

#### Color and Contrast
- [ ] Text contrast is 4.5:1 for normal text?
- [ ] UI component contrast is 3:1?
- [ ] Focus indicators are visible (2px+, 3:1 contrast)?
- [ ] Color isn't used alone to convey info (red for required)?
- [ ] Does page work in high contrast mode (Windows)?

#### Motion and Animation
- [ ] Do animations respect `prefers-reduced-motion`?
- [ ] Are there no flashing elements (3+ flashes per second)?
- [ ] Animated GIFs/videos have controls?

#### Mobile Screen Reader (VoiceOver on iOS, TalkBack on Android)
- [ ] Can I navigate by touch?
- [ ] Are touch targets at least 44×44 pixels?
- [ ] Are there adequate spacing between targets?
- [ ] Does screen reader announce correctly in portrait and landscape?
- [ ] Can I use gestures (rotor, swipe, etc.)?

---

## CI/CD Integration Pattern

### Pre-commit: ESLint
```bash
# In pre-commit hook or husky
eslint --ext .js,.jsx,.ts,.tsx src/

# Catches JSX a11y issues before code review
```

### Build Time: Storybook A11y Addon
```bash
# In CI: run Storybook and test all stories for a11y
npm run storybook:ci
# Built-in a11y addon reports violations

# Or use @chromatic-com/storybook for cloud-based testing
```

### E2E Tests: Playwright + axe-core
```js
// tests/accessibility.spec.ts
import { test, expect } from '@playwright/test';
import { injectAxe, checkA11y } from 'axe-playwright';

const CRITICAL_PAGES = [
  '/',
  '/login',
  '/products',
  '/checkout',
];

CRITICAL_PAGES.forEach((page) => {
  test(`${page} has no accessibility violations`, async ({ browser }) => {
    const context = await browser.newContext();
    const page = await context.newPage();
    await page.goto(`http://localhost:3000${page}`);
    await injectAxe(page);
    await checkA11y(page);
  });
});
```

### Threshold Strategy
```js
// Don't block on every minor violation; set meaningful thresholds
const axeOptions = {
  rules: {
    'critical': { enabled: true },   // Always fail
    'serious': { enabled: true },     // Always fail
    'moderate': { enabled: false },   // Warn, don't block
    'minor': { enabled: false },      // Track, don't block
  },
};

// OR: fail if count >= X
const results = await axe(container);
const violations = results.violations.filter(v => v.impact === 'critical' || v.impact === 'serious');
if (violations.length > 0) throw new Error('Critical a11y violations found');
```

---

## Remediation Prioritization Framework

### Severity Levels (WCAG Impact)

#### Critical (Fix First)
- Blocks access entirely for some users
- Examples:
  - No keyboard access to buttons/links
  - Missing form labels
  - No alt text on functional images (buttons, linked images)
  - Broken focus management in dialogs
  - Non-functional screen reader (invalid HTML, duplicate IDs)
- Timeline: 1-2 sprints

#### Serious (Fix Soon)
- Significantly degrades experience
- Examples:
  - Poor contrast (fails WCAG AA)
  - Missing heading structure
  - Confusing link text ("click here" vs "Download PDF")
  - Missing live region announcements
  - Unclear error messages
- Timeline: 2-4 sprints

#### Moderate (Fix When Possible)
- Hinders but doesn't block
- Examples:
  - Verbose or redundant ARIA
  - Imprecise alt text
  - Skip link missing (navigation keyboard access works, just not fast)
  - Focus trap missing from modal (but can use Escape or Tab out)
- Timeline: 4-8 sprints

#### Minor (Nice-to-Have)
- Cosmetic or best-practice improvements
- Examples:
  - ARIA improvements for better screen reader experience
  - Enhanced focus indicator design
  - Improved color palette for colorblind friendliness
  - Adding aria-roledescription for custom widgets
- Timeline: Backlog or next release

### Prioritization Checklist
1. **User Impact**: How many users can't use this? (Screen reader users, keyboard users, color-blind, etc.)
2. **Ease of Fix**: Can this be fixed in 1 hour vs 1 week?
3. **Legal/Compliance**: Is there contractual a11y requirement (WCAG AA)?
4. **Scope**: Single component vs sitewide issue?
5. **Dependencies**: Does this block other fixes?

### Example Remediation Plan
```
Sprint 1-2 (Critical)
- [ ] Audit all form fields for labels
- [ ] Implement keyboard navigation for custom dropdown
- [ ] Add alt text to functional images
- [ ] Fix modal focus trap

Sprint 3-4 (Serious)
- [ ] Improve contrast issues (text, focus indicators)
- [ ] Review heading hierarchy; add missing landmarks
- [ ] Audit link text; fix "click here" links
- [ ] Test with screen reader; fix announcements

Sprint 5-8 (Moderate + Minor)
- [ ] Refactor ARIA for clarity
- [ ] Enhance focus indicator styling
- [ ] Test on mobile screen readers
```

---

## Inclusive Design Principles

### Design for Extremes, Benefit the Middle
The idea: solve for edge cases (blind, deaf, motor disabilities) and the general population benefits.

Examples:
- **Captions for videos** → Helps Deaf users AND users in noisy environments AND non-native speakers
- **Keyboard navigation** → Helps motor disability AND power users AND users in car/on phone
- **Large touch targets** → Helps motor disability AND elderly users AND kids AND users in high vibration (bus)
- **Plain language** → Helps cognitive disability AND non-native speakers AND busy users skimming
- **Color + text/icon** → Helps color-blind AND print/screenshot users AND low-vision magnification users

### Three Types of Disability (Permanent, Temporary, Situational)
Design for permanent; benefit all:

| Capability | Permanent | Temporary | Situational |
|---|---|---|---|
| **Visual** | Blindness | Eye surgery recovery | Bright sunlight, visor down |
| **Motor** | Paralysis | Broken arm | Holding baby, on escalator |
| **Hearing** | Deafness | Ear infection | Loud venue, sleeping baby |
| **Cognitive** | Learning disability | Concussion | Stressed, multitasking, jet lag |

Same solution helps all: captions, keyboard nav, large text, plain language, motor-adaptive input methods.

---

## Color Accessibility in Design

### Contrast Ratios (WCAG 2.2 AA)
- **Normal text** (≤18pt): 4.5:1 ratio
- **Large text** (≥18pt or 14pt bold): 3:1 ratio
- **UI components** (borders, buttons, icons): 3:1 ratio
- **Focus indicators**: 3:1 against adjacent color (NOT white-on-white)

### Tools for Checking Contrast
- WebAIM Contrast Checker: https://webaim.org/resources/contrastchecker/
- Chrome DevTools: Inspect element, click color swatch
- Figma: Color contrast plugin
- Accessible Colors: https://accessible-colors.com/

### Color-Blind Friendly Palettes
~8% of males, ~0.5% of females have color blindness. Types:
- **Red-Green** (most common): red/green indistinguishable
- **Blue-Yellow**: blue/yellow hard to distinguish
- **Complete** (rare): complete color blindness

#### Design for Color-Blind Safety
1. Never use red + green as only differentiators
2. Include text or icon in addition to color (e.g., "Error" label + red background + ✕ icon)
3. Use accessible palette: blues, oranges, grays work well
4. Test with Sim Daltonism (macOS) or Color Oracle (Windows/Mac)

#### Accessible Color Palette Example
```css
/* Use OKLCH color space (perceptually uniform) */
:root {
  /* Primary: Blue (safe for all color blindness types) */
  --primary-50: oklch(97% 0.02 268);
  --primary-500: oklch(62% 0.29 268);
  --primary-900: oklch(28% 0.24 268);

  /* Secondary: Orange (distinct from blue even for red-green blind) */
  --secondary-50: oklch(98% 0.02 75);
  --secondary-500: oklch(72% 0.21 75);
  --secondary-900: oklch(28% 0.19 75);

  /* Success: Green (pair with checkmark icon) */
  --success-500: oklch(71% 0.22 142);

  /* Error: Red (pair with X icon) */
  --error-500: oklch(64% 0.25 25);

  /* Warning: Yellow (pair with ! icon) */
  --warning-500: oklch(80% 0.18 106);

  /* Grayscale: for backgrounds */
  --gray-50: oklch(97% 0.005 264);
  --gray-900: oklch(22% 0.008 264);
}
```

Test with Sim Daltonism or Coblis to verify each color is distinguishable.

---

## Touch Target Sizing (WCAG 2.2 AA)

### Minimum: 44×44 CSS Pixels
- Applies to every interactive element (button, link, checkbox, menu item, etc.)
- Exception: inline links in text (but should still have spacing)
- CSS pixels, not screen pixels (account for device pixel ratio)

```css
/* Ensure 44×44px minimum */
button, a, input[type="checkbox"], input[type="radio"] {
  min-width: 44px;
  min-height: 44px;
}

/* Spacing between targets */
button + button {
  margin-left: 8px; /* Space so they're not adjacent */
}
```

### Spacing Between Targets
- Avoid targets touching or overlapping
- Minimum 8px spacing between interactive elements
- Helps motor disability users (tremor, low dexterity) tap targets accurately

---

## Focus State Design

### Best Practices
1. **Always visible**: Never hide focus indicator
2. **Consistent**: Same style across all components
3. **Contrasted**: 3:1 contrast against adjacent color
4. **Substantial**: 2px minimum width or outline
5. **On-brand**: Feels intentional, not an afterthought

### Implementation in Tailwind CSS
```css
/* Default focus style */
button:focus-visible {
  outline: 2px solid #0066cc;
  outline-offset: 2px;
}

/* Or use ring utility */
button {
  @apply focus-visible:ring-2 focus-visible:ring-blue-500 focus-visible:ring-offset-2;
}

/* Dark mode */
@media (prefers-color-scheme: dark) {
  button:focus-visible {
    outline-color: #66b3ff; /* Lighter blue for dark bg)
  }
}

/* High contrast mode */
@media (forced-colors: active) {
  button:focus-visible {
    outline: 2px solid CanvasText;
  }
}
```

### Design System Checklist
- [ ] Focus style visible in light mode
- [ ] Focus style visible in dark mode
- [ ] Focus style visible in high contrast mode
- [ ] Focus indicator 2px+ width
- [ ] Focus indicator 3:1 contrast against adjacent color
- [ ] Applied to all interactive elements (button, link, input, select, etc.)
- [ ] Consistent across all components
- [ ] Works with `:focus-visible` (not just `:focus`)

---

## Error State Design

### Anatomy of Accessible Error Message
1. **Identify the field**: "Email field:"
2. **Describe the problem**: "invalid format"
3. **Suggest a fix**: "Use format: name@example.com"

```jsx
<div className="mb-4">
  <label htmlFor="email">Email address</label>
  <input
    id="email"
    type="email"
    aria-invalid="true"
    aria-describedby="email-error"
    className="border-2 border-red-500"
  />
  <div id="email-error" role="alert" className="text-red-600 text-sm mt-1">
    Invalid email. Use format: name@example.com
  </div>
</div>
```

### Best Practices
- Use color + icon + text (never color alone)
- Place error message near field (aria-describedby links them)
- Announce errors immediately (role="alert" triggers aria-live="assertive")
- Use clear, concise language
- Suggest how to fix
- Don't blame the user ("You made a mistake" → "Invalid format")

---

## Cognitive Accessibility

### Plain Language
- Short sentences (15-20 words max)
- Common words (avoid jargon)
- Active voice (subject-verb-object)
- List format for multiple items
- One idea per sentence

Good: "You can't submit the form yet. Please fill in the email field."
Bad: "Form submission has been disabled pending completion of required input fields."

### Predictable Navigation
- Consistent menu placement (same position on every page)
- Consistent terminology (always "Settings," never "Preferences" or "Options")
- Clear section headings
- Breadcrumb for location context

### Input Assistance
- Required field clearly marked: `<span aria-label="required">*</span>` or plain text "required"
- Helpful error messages: "Your password must be 8+ characters" (not "Invalid password")
- Inline suggestions: `<datalist>` for autocomplete
- Progressive disclosure: hide advanced options until needed

### Sufficient Time
- No auto-expiring content (or ask before expiring)
- Extend sessions on request
- Pause auto-playing content by default
- Allow users to adjust timeouts

### Consistency and Help
- Glossary for domain-specific terms
- FAQ section
- Contact support link in consistent location
- Consistent help icon / help label

---

## shadcn/ui Accessibility Profile

### Components with Good Built-In Accessibility
(Built on Radix UI primitives)

- **Button** — Semantic `<button>` with proper focus
- **Dialog** — Focus trap, role="dialog", ARIA modal
- **Select** — Combobox pattern with roving tabindex
- **Tabs** — role="tablist/tab", arrow key navigation, roving tabindex
- **Accordion** — role="button", aria-expanded, proper grouping
- **Popover** — Focus trap, Escape to dismiss
- **Checkbox/Radio** — Proper ARIA states, keyboard support
- **Label** — Proper htmlFor association
- **Form helpers** (useForm) — Error messages, validation feedback
- **Tooltip** — aria-describedby, proper triggers
- **Alert** — role="alert", aria-live announcement
- **Toast** — Live region announcements
- **Navigation Menu** — Menu patterns with keyboard support

### Components Needing Extra Attention
- **DataTable** — Custom implementation; test heading hierarchy, sorting announcement, row selection
- **Combobox** — If heavily customized; verify multiselect and async loading announce correctly
- **Calendar** — Verify keyboard navigation and date announcement
- **Command** — If used as autocomplete; test live region for suggestions

### Common shadcn/ui Gaps
1. **Icon buttons** — Missing aria-label; add manually: `<Button variant="ghost" size="icon" aria-label="Close menu">×</Button>`
2. **Custom styling breaking focus** — Adding `outline: none` without replacement
3. **Toast announcements** — Verify aria-live region works; axe-core won't catch slow announcements
4. **DataTable reading order** — Column headers may not be properly announced
5. **Disable without disabled prop** — Using CSS display:none; use disabled attribute instead

### Testing shadcn/ui Components
```jsx
// In Storybook, test all states
export const ButtonStory = () => (
  <>
    <Button>Default</Button>
    <Button disabled>Disabled</Button>
    <Button variant="outline">Outline</Button>
  </>
);

// In tests, check keyboard and screen reader
test('button is accessible', async () => {
  const { container } = render(<Button>Click me</Button>);
  const results = await axe(container);
  expect(results).toHaveNoViolations();

  // Manual keyboard test
  const button = container.querySelector('button');
  button.focus();
  expect(document.activeElement).toBe(button);
  button.dispatchEvent(new KeyboardEvent('keydown', { key: 'Enter' }));
  // Verify click fired
});
```

---

## Accessibility Audit Template

Use this checklist to audit any page or component:

```
Project: ___________________
Date: _____________________
Target: WCAG 2.2 AA

FOUNDATIONAL (Critical)
[ ] Semantic HTML (button, link, form, etc.; not divs)
[ ] Valid HTML (no duplicate IDs, proper nesting)
[ ] Landmarks present (main, nav, aside, footer)
[ ] Heading hierarchy (h1, then h2+, no skipping)
[ ] Images have alt text (descriptive, not "image")
[ ] Form fields have labels (explicit htmlFor)
[ ] Keyboard navigation works (Tab, arrows, Enter, Escape)

WCAG CRITICAL (Block Release)
[ ] No keyboard trap
[ ] Focus indicator visible (2px+)
[ ] Contrast meets 4.5:1 (text) / 3:1 (UI)
[ ] Functional images have alt text
[ ] Form errors identified and described
[ ] Live regions announce dynamic content
[ ] Modal has focus trap and aria-modal

WCAG AA (Target)
[ ] All criteria from "Critical" above
[ ] Heading and label descriptions (not vague)
[ ] Link purpose clear (not "click here")
[ ] No reliance on color alone
[ ] Text spacing customizable (don't break)
[ ] 200% zoom doesn't break layout
[ ] Content doesn't automatically refresh/timeout
[ ] Audio/video has captions or transcript
[ ] Motion respects prefers-reduced-motion
[ ] Target size ≥44×44px

NICE-TO-HAVE (AAA)
[ ] Higher contrast 7:1 (text) / 4.5:1 (UI)
[ ] Sign language video (for video content)
[ ] Extended captions (describes sounds)
[ ] Adjustable line height, letter spacing, word spacing

SCREEN READER TEST (Manual)
[ ] Open VoiceOver (Mac: Cmd+F5) or NVDA (Windows: Ctrl+Alt+N)
[ ] Navigate by heading (H key in NVDA, rotor in VO)
[ ] Check heading hierarchy makes sense
[ ] Navigate by link (L key)
[ ] Check link text is meaningful
[ ] Navigate form fields (F key)
[ ] Tab through form; check labels announced
[ ] Test error messages announced

KEYBOARD TEST (No Mouse)
[ ] Tab through all interactive elements in order
[ ] Shift+Tab goes backward
[ ] Arrow keys navigate composite widgets (menu, tabs)
[ ] Enter/Space activates buttons
[ ] Escape dismisses overlays
[ ] Skip-to-main-content link present and works
```

---

## Common Violations by Component Type

### Forms
- [ ] Missing labels or labels not associated (no htmlFor)
- [ ] No indication of required fields
- [ ] Error messages not linked to fields (no aria-describedby)
- [ ] Errors announced; form doesn't auto-submit without warning
- [ ] Validation happens on blur (not keystroke interruption)

### Navigation
- [ ] No skip-to-main link
- [ ] Menu items keyboard navigable (arrow keys, roving tabindex)
- [ ] Current page marked (aria-current="page")
- [ ] Landmark regions present (nav, main)

### Data Tables
- [ ] Header row properly marked (`<thead>`, `<th>`, scope="col")
- [ ] Row headers marked if present (scope="row")
- [ ] Table has caption or is introduced by heading
- [ ] Sorting announced via live region or title update
- [ ] Complex tables have associated description

### Images and Media
- [ ] All images have alt text
- [ ] Decorative images have alt=""
- [ ] Complex images (charts) have long description
- [ ] Videos have captions and transcript
- [ ] Auto-play disabled

### Buttons and Links
- [ ] Button text or aria-label describes action
- [ ] Link text describes destination (not "click here")
- [ ] Icon buttons have aria-label
- [ ] Buttons/links keyboard accessible (Tab, Enter/Space)
- [ ] Focus indicator visible

