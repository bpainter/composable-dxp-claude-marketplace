---
description: Extract a brand's color, typography, spacing, and elevation system into design-system tokens — semantic naming, HSL channel format, ready for Tailwind/shadcn or Figma variables

# Project context
type: command
project: skills-library
plugin: brand
tags: [type/command, plugin/brand]
status: active
---

Extract a brand's identity system into design-system tokens — the format that flows into Tailwind config, shadcn theme, and Figma variables.

Steps:

1. **Confirm the source identity exists** from [[brand-identity-system]]. If not, route there first. Tokenizing without an identity is just naming colors.

2. **Tokenize colors:**
   - Per color: semantic name (`accent`, not `blue-500`), HEX, RGB, HSL, **HSL channel format** (`220 90% 56%` for shadcn/Tailwind v4 wrapping).
   - Group: primary, secondary, neutrals, functional, surface, text.
   - Contrast pairings documented per token.
   - Dark-mode values documented separately.

3. **Tokenize typography:**
   - Type scale as numbered tokens (`type-display`, `type-h1`, `type-h2`, `type-body`, `type-caption`, `type-mono`).
   - Per token: font-family, weight, size, line-height, letter-spacing.
   - Fallback stacks documented for digital surfaces.

4. **Tokenize spacing:**
   - Base unit (4 or 8).
   - Scale: `space-xs` (4), `space-sm` (8), `space-md` (16), `space-lg` (24), `space-xl` (32), `space-2xl` (48), `space-3xl` (64).

5. **Tokenize border-radius:**
   - Scale: `radius-sm`, `radius-md`, `radius-lg` (per [[visual-design-foundations]] §4.1).

6. **Tokenize elevation/shadow:**
   - Scale: `elevation-0` through `elevation-4`.
   - Per token: shadow stack values.

7. **Tokenize motion (if defined):**
   - Duration scale: `duration-fast`, `duration-default`, `duration-slow`.
   - Easing tokens: `ease-out`, `ease-in-out`, `ease-drawer` (per [[motion-and-transitions]] custom curves).

8. **Output token specification** in:
   - **CSS custom properties** format for direct shadcn integration.
   - **Tailwind config** snippet for `tailwind.config.ts`.
   - **Figma Variables** format if relevant.

9. **Hand off:**
   - To `software-engineering-tailwind-tokens` for Tailwind implementation.
   - To `software-engineering-shadcn-component` for shadcn theme customization.
   - To Figma plugin for design-tool wiring.

Output format:

```
# Token spec: {Brand}

## Colors

### Primary (semantic)
| Token | HEX | HSL channel | Contrast pairings |
| --color-primary | #4F2C8E | 263 53% 36% | --color-primary-foreground (4.6:1 AA) |
| --color-primary-foreground | #FAFAFA | 0 0% 98% | |
| ...

### Functional
| Token | HEX | HSL channel | Use |
| --color-success | ... | ... | success states |
| --color-warning | ... | ... | warning states |
| --color-error | ... | ... | error states |
| --color-info | ... | ... | info states |

### Dark-mode overrides
[Same tokens with dark-mode values]

## Typography
| Token | Family | Weight | Size | Line-height |
| --type-display | Geist | 700 | 96px | 1.0 |
| --type-h1 | Geist | 600 | 44px | 1.1 |
| ...

## Spacing
| Token | Value |
| --space-xs | 4px |
| --space-sm | 8px |
| ...

## Radius
| Token | Value |
| --radius-sm | 4px |
| --radius-md | 8px |
| --radius-lg | 16px |

## Elevation
| Token | Shadow stack |
| --elevation-1 | 0 1px 2px rgba(0,0,0,0.05) |
| --elevation-2 | 0 4px 8px rgba(0,0,0,0.08) |
| ...

## Motion
| Token | Value |
| --duration-fast | 150ms |
| --duration-default | 250ms |
| --duration-slow | 400ms |
| --ease-out | cubic-bezier(0.23, 1, 0.32, 1) |
| --ease-in-out | cubic-bezier(0.77, 0, 0.175, 1) |
| --ease-drawer | cubic-bezier(0.32, 0.72, 0, 1) |

## Implementation files

### globals.css (shadcn-compatible)
[CSS custom properties with light- and dark-mode blocks]

### tailwind.config.ts
[Snippet wiring tokens into Tailwind theme]

### Figma Variables
[Variable groups + values]
```
