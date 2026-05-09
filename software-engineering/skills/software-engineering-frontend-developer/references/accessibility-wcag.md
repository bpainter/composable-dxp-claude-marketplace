---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[frontend-developer]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# Accessibility & WCAG 2.1 (2025)

## WCAG 2.1 Levels

- **Level A**: Basic accessibility
- **Level AA**: Recommended minimum (legal requirement in many places)
- **Level AAA**: Enhanced accessibility (nice-to-have)

## Key Accessibility Principles

### 1. Perceivable
Users can see/hear content.

**Checklist:**
- Images have alt text (descriptive, not "image of X")
- Videos have captions and transcripts
- Color is not the only way to convey information
- Text has sufficient contrast (4.5:1 for normal text)

```html
<!-- Good alt text -->
<img src="chart.png" alt="Revenue growth 2024: Q1 $5M, Q2 $7M, Q3 $8M" />

<!-- Bad alt text -->
<img src="chart.png" alt="chart" />
```

### 2. Operable
Users can navigate and interact.

**Checklist:**
- All functionality available via keyboard
- No keyboard traps (can tab out of elements)
- Focus is visible (don't remove outline without replacement)
- Users have time to read/interact (no auto-playing content)

```css
/* Good: Visible focus indicator */
button:focus {
  outline: 3px solid #4A90E2;
  outline-offset: 2px;
}

/* Bad: No visible focus */
button:focus {
  outline: none;
}
```

### 3. Understandable
Content is clear and predictable.

**Checklist:**
- Language is clear and simple
- Page has logical structure (headings, landmarks)
- Navigation is consistent
- Form errors are explained

```html
<!-- Good: Semantic structure -->
<header>Logo and nav</header>
<main>
  <article>
    <h1>Title</h1>
    <p>Content</p>
  </article>
</main>
<footer>Footer info</footer>

<!-- Bad: No structure -->
<div id="content">Everything in divs</div>
```

### 4. Robust
Works with assistive technologies.

**Checklist:**
- Valid HTML
- ARIA roles/attributes correct
- Works with screen readers
- Sufficient color contrast

## Screen Reader Testing

Test with:
- NVDA (Windows, free)
- JAWS (Windows, paid, most common)
- VoiceOver (Mac, built-in)
- TalkBack (Android, built-in)

Key: Test with real screen reader users.

## Keyboard Navigation

All interactive elements must be keyboard accessible:

```html
<!-- Good: Interactive element, naturally focusable -->
<button>Click me</button>
<a href="/page">Link</a>
<input type="text" />

<!-- Bad: Div needs tabindex -->
<div onclick="doSomething()">Not keyboard accessible</div>

<!-- Fix: Make it a button -->
<button onclick="doSomething()">Now accessible</button>
```

## ARIA (Accessible Rich Internet Applications)

ARIA adds semantics when HTML can't.

```html
<!-- Good: Use semantic HTML first -->
<button>Submit</button>

<!-- If must use div, add ARIA -->
<div role="button" tabindex="0">Submit</div>

<!-- Add ARIA attributes for complex widgets -->
<div role="slider" aria-label="Volume" aria-valuemin="0" aria-valuemax="100" aria-valuenow="50"></div>
```

## Automated Testing

```bash
# axe accessibility checker
npm install @axe-core/react

# Lighthouse built-in accessibility audit
lighthouse https://example.com --only-categories=accessibility
```

But automated tools catch only ~30% of issues. Manual testing is essential.

## 2025 Requirements

- **EAA (European Accessibility Act)**: One of strictest a11y laws globally, effective 2026
- **Legal liability**: Companies face lawsuits for inaccessible sites
- **Business case**: Accessibility = better UX for everyone
