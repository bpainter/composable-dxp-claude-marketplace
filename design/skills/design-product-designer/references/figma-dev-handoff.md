---
type: reference
project: skills-library
scope: skill
plugin: design
parent: "[[product-designer]]"
tags: [type/reference, plugin/design, scope/skill, topic/design]
status: active
---
# Figma to Developer Handoff Best Practices

## Dev Mode Setup

Enable Figma Dev Mode to provide developers with:
- Inspect panel showing CSS, sizing, spacing measurements
- Code snippets for colors, shadows, and typography
- Component documentation and API reference
- Design token exports

## Component Documentation

For each component, document:
- **Props**: What variants, sizes, states exist
- **States**: Default, hover, active, disabled, loading, error
- **Accessibility**: Focus behavior, ARIA attributes, keyboard navigation
- **Responsive**: How component adapts across breakpoints

## Spacing & Layout Grid

- Define a consistent spacing scale (4px, 8px, 12px, 16px, 24px, 32px, etc.)
- Use auto-layout in Figma to enforce spacing rules
- Document breakpoints: mobile (320px), tablet (640px), desktop (1024px), wide (1280px)
- Explain how components reflow at each breakpoint

## Color & Tokens

- Create a semantic color system: primary, secondary, success, warning, error, neutral
- Map colors to use cases (buttons, text, backgrounds, borders)
- Document contrast ratios for accessibility (minimum 4.5:1 for text)
- Support light and dark themes with clear token naming

## Typography System

- Define type scale with font-size, line-height, letter-spacing
- Specify font families and weights (regular, semi-bold, bold)
- Map type styles to roles: display, heading, body, caption
- Include guidance on line length and readability

## Motion & Animation

- Document animation timing (fast: 150ms, normal: 250ms, slow: 400ms)
- Specify easing curves (ease-in, ease-out, ease-in-out)
- Explain when to use motion vs. static transitions
- Provide examples of microinteractions (hover, loading, validation)

## Responsive Breakpoints

```
Mobile:  320px - 639px
Tablet:  640px - 1023px
Desktop: 1024px - 1279px
Wide:    1280px+
```

Design with mobile-first approach: start small, enhance at larger breakpoints.

## Assets & Images

- Export all icons as SVG with transparent backgrounds
- Organize image assets by component or feature
- Provide retina versions for raster images (2x density)
- Use image optimization guidelines for web delivery

## Documentation Template

```
# Component Name

## Purpose
What job does this component do for users?

## Variants
- Size: small, medium, large
- Type: primary, secondary, tertiary
- State: default, hover, active, disabled

## Usage
Do use: [examples]
Don't use: [anti-patterns]

## Accessibility
- Keyboard navigation: [describe]
- Screen reader: [describe]
- Focus indicator: [visual spec]

## Props
- label: string
- size: enum
- disabled: boolean
- onClick: function

## Code Example
[Figma Dev Mode snippet]
```

## Version Control

- Use Figma branches for major changes
- Document breaking changes in library updates
- Communicate component deprecations 2+ sprints ahead
- Maintain a changelog of design system updates
