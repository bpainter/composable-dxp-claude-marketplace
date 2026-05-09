---
type: reference
project: skills-library
scope: skill
plugin: design
parent: "[[product-designer]]"
tags: [type/reference, plugin/design, scope/skill, topic/design]
status: active
---
# Building Scalable Component Systems in Figma

## Atomic Design Structure

Organize components from smallest to largest:

### Atoms (Base Elements)
- Button, Input, Badge, Icon, Divider
- Single, focused responsibility
- Reused across many patterns

### Molecules (Simple Components)
- Input with label and error message
- Button with icon and text
- Card with image, title, description

### Organisms (Complex Components)
- Form with multiple inputs and validation
- Header with navigation and search
- Table with sorting, filtering, pagination

### Templates & Layouts
- Page layouts with component placeholders
- Grid systems and responsive containers
- Common page structures (marketing, product, admin)

## Figma File Organization

```
Design System /
├── Atoms /
│   ├── Button
│   ├── Input
│   ├── Badge
│   └── Icon
├── Molecules /
│   ├── Form Field
│   ├── Card
│   └── Chip
├── Organisms /
│   ├── Form
│   ├── Header
│   └── Table
├── Tokens /
│   ├── Color
│   ├── Typography
│   └── Spacing
└── Documentation /
    ├── Guidelines
    └── Examples
```

## Component Variants Pattern

Use Figma's variant system to show all component states:

### Required Variants
- **Size**: small, medium, large (or xs, sm, md, lg, xl)
- **State**: default, hover, active, disabled, loading, error
- **Color/Type**: primary, secondary, danger, success
- **Content Length**: short, long (for text variants)

### Variant Naming Convention
```
Component/Size/Type/State

Button/Large/Primary/Default
Button/Large/Primary/Hover
Button/Large/Primary/Disabled
Input/Medium/Normal/Focus
Card/Medium/Default
```

### Implementation Tips
- Use nested variants for complex components
- Create a "preview" frame showing all variants
- Use auto-layout to maintain spacing relationships
- Link variants to interactive prototypes

## Tailwind CSS Integration

Map design tokens to Tailwind classes:

### Colors
```
Token: primary-600
Tailwind: bg-blue-600, text-blue-600
Light Mode: #2563eb
Dark Mode: #1d4ed8
```

### Spacing
```
Token: space-4
Tailwind: p-4, m-4, gap-4
Value: 16px
```

### Typography
```
Token: body-medium
Tailwind: text-base
Font-size: 16px
Line-height: 1.5
```

### Sizing
```
Token: button-height-md
Tailwind: h-10
Value: 40px
```

## Responsive Design Patterns

### Mobile-First Approach
1. Design base experience for mobile (320px+)
2. Enhance for tablet (640px+)
3. Optimize for desktop (1024px+)

### Component Responsiveness
- **Button**: Always full-width on mobile, auto-width on desktop
- **Input**: Full-width on mobile, fixed/flexible width on desktop
- **Card Grid**: 1 column mobile, 2 columns tablet, 3+ columns desktop
- **Typography**: Smaller sizes on mobile, scale up on desktop

### Breakpoint Coverage
- Mobile: 320px, 375px, 425px (portrait phones)
- Tablet: 640px, 768px (landscape phones, tablets)
- Desktop: 1024px, 1280px, 1440px (desktop monitors)

## Accessibility Checklist

### Visual
- [ ] Color contrast minimum 4.5:1 for text
- [ ] Focus indicator visible on all interactive elements
- [ ] Icons have text labels or ARIA descriptions
- [ ] Motion doesn't auto-play or flash excessively

### Interactive
- [ ] Keyboard navigation works without a mouse
- [ ] Tab order follows logical flow
- [ ] Form inputs have associated labels
- [ ] Error messages are clear and actionable

### Semantic
- [ ] Heading hierarchy is correct (h1 > h2 > h3, no skipping)
- [ ] Buttons and links are semantically distinct
- [ ] Lists use proper list structure
- [ ] Images have descriptive alt text

## Design Token System

### Token Categories
1. **Base Tokens**: Raw values (colors, sizes)
2. **Semantic Tokens**: Contextual aliases (primary, error, success)
3. **Component Tokens**: Component-specific defaults

### Token Naming
```
{category}-{property}-{variant}

color-background-primary
color-text-secondary
size-padding-medium
size-border-radius-lg
```

### Documentation Format
```yaml
Token: color-primary-600
Value: #2563eb
Usage: Primary buttons, links, active states
Light Mode: #2563eb
Dark Mode: #1d4ed8
```

## Version Management

### Major Changes
- New component architecture
- Breaking changes to existing components
- Requires all files to update

### Minor Changes
- New variants or sizes
- Non-breaking additions
- Backward compatible

### Patch Changes
- Bug fixes
- Documentation updates
- Internal naming changes

### Communication
- Announce changes 2+ weeks ahead
- Provide migration guide for breaking changes
- Deprecate old components before removal
- Celebrate when teams adopt new patterns
