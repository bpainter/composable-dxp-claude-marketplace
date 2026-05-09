---
type: reference
project: skills-library
scope: skill
plugin: design
parent: "[[web-designer]]"
tags: [type/reference, plugin/design, scope/skill, topic/design]
status: active
---
# Visual Design Foundations for Web

## Color Theory for Web

### Color Models & Perception

**HSL (Hue, Saturation, Lightness)**
- Hue: 0–360° (red, yellow, green, cyan, blue, magenta)
- Saturation: 0–100% (0% = gray, 100% = pure color)
- Lightness: 0–100% (0% = black, 100% = white, 50% = pure color)
- Practical use: Hue stays constant while adjusting saturation and lightness for tints/shades

**OKLCH (Perceptual Color Space)**
- L: Lightness (0–100, perceptually uniform)
- C: Chroma (saturation in human perception)
- H: Hue (0–360°)
- Advantage: Colors maintain consistent brightness and saturation across adjustments
- Use case: Modern design systems, generating accessible color scales, dark mode color conversion

### Color Harmony Rules

**Complementary**: Opposite on the color wheel (e.g., blue + orange). Creates high contrast, vibrant, energetic. Use sparingly for emphasis.

**Analogous**: Adjacent colors on the wheel (e.g., blue, blue-green, green). Harmonious, calm, natural. Safe for primary + secondary palettes.

**Triadic**: Three evenly spaced colors (e.g., red, yellow, blue). Balanced vibrancy. Use one dominant, two for accent.

**Split-Complementary**: Primary color + two colors adjacent to its complement. High contrast, less jarring than complementary.

**Monochromatic**: Single hue with varying saturation/lightness. Cohesive, elegant, professional. Add semantic colors (green for success) for function.

### The 60-30-10 Rule

Apply to web palettes for visual harmony:

- **60%**: Primary/dominant color. Usually neutral (white, off-white, dark gray) or brand primary.
- **30%**: Secondary color. Supporting shade, often a lighter or darker variant of primary.
- **10%**: Accent color. Draws attention to CTAs, warnings, success states, key information.

**Example for B2B SaaS**:
- 60%: Off-white (#F9FAFB) background + charcoal (#1F2937) text
- 30%: Light gray (#F3F4F6) surfaces, borders (#E5E7EB)
- 10%: Teal (#14B8A6) for CTAs and interactive elements

### Color Psychology by Industry

| Industry | Primary Colors | Psychology | Use Case |
|----------|---------------|-----------|----------|
| Finance | Blue, Dark Gray | Trust, stability, security | Banks, insurance, wealth management |
| Health | Green, Blue | Calm, healing, growth | Healthcare, wellness, mental health |
| Tech | Purple, Dark Blue, Black | Innovation, intelligence, forward-thinking | SaaS, AI, developer tools |
| E-commerce | Orange, Red, Black | Energy, urgency, confidence | Retail, flash sales, CTAs |
| Education | Blue, Orange, Green | Trust, creativity, growth | Learning platforms, courses |
| Legal | Dark Blue, Gray, White | Authority, professionalism, stability | Law firms, compliance |
| Real Estate | Warm earth tones, White | Trust, home, comfort | Property, home services |
| Creative/Agency | Bold, multi-color | Innovation, creativity, personality | Design, marketing, media |

**Don't follow blindly**: If everyone in your category uses blue, consider differentiation. Just acknowledge the psychology and make an intentional choice.

### Palette Generation Workflow

**Step 1: Start with Brand Primary**
- If brand color exists, use it
- Otherwise, pick one based on industry psychology + differentiation
- Example: For fintech targeting young professionals, consider teal or purple (not default blue)

**Step 2: Generate Extended Palette**
- Use tools: Coolors.co, Adobe Color, Huemint, or Tailwind palette builder
- Generate 5–7 tints/shades of primary (lighter for backgrounds, darker for text)
- Generate secondary (complementary or analogous), then tints/shades

**Step 3: Map to Semantic Colors**
- Success: Green (e.g., #10B981)
- Warning: Amber/Yellow (e.g., #F59E0B)
- Error: Red (e.g., #EF4444)
- Info: Blue (e.g., #3B82F6)
- These should work across light and dark modes

**Step 4: Tailwind Configuration**
```javascript
// tailwind.config.js
module.exports = {
  theme: {
    colors: {
      primary: {
        50: '#f0f9ff',
        100: '#e0f2fe',
        200: '#bae6fd',
        300: '#7dd3fc',
        400: '#38bdf8',
        500: '#0ea5e9', // Primary brand
        600: '#0284c7',
        700: '#0369a1',
        800: '#075985',
        900: '#0c3d66',
      },
      secondary: {
        // Similar structure for secondary
      },
      accent: '#14b8a6', // 10% emphasis color
      neutral: {
        // Grays for backgrounds, borders, text
      },
    },
  },
};
```

### Accessible Color Combinations

**WCAG Guidelines**:
- Level AA: Contrast ratio of 4.5:1 for normal text, 3:1 for large text
- Level AAA: Contrast ratio of 7:1 for normal text, 4.5:1 for large text

**Tools**: WebAIM Contrast Checker, Contrast Ratio Calculator, Polished.js

**Common Mistakes**:
- Relying on color alone for meaning (always pair with icon, text, or pattern)
- Low contrast on hover states
- Insufficient contrast for disabled/secondary states

**Best Practice**: Test text color on all background colors. Most common pairing issues:
- Dark text on light background ✓ (high contrast)
- Light text on dark background ✓ (high contrast)
- Light text on light background ✗ (avoid)
- Dark text on dark background ✗ (avoid)

### Dark Mode Color Science

Dark mode is NOT just inverting colors. Proper dark mode:

1. **Reduce Luminance**: Colors appear brighter in dark mode. Darken your brand color.
   - Light mode primary: #0EA5E9 (sky-500)
   - Dark mode primary: #38BDEF (sky-400) — lighter shade, same hue

2. **Increase Saturation**: Low-saturation colors become muddy. Boost saturation in dark mode.
   - Light mode secondary: #E5E7EB (gray-200)
   - Dark mode secondary: #4B5563 (gray-700) — darker and more saturated

3. **Elevate Surfaces**: Use lighter shades on top layers to create depth.
   - Background: #0F172A (slate-950)
   - Cards/raised surfaces: #1E293B (slate-900)
   - Inputs/borders: #334155 (slate-700)

4. **Extend Neutral Range**: Dark mode needs 11-13 shades for proper contrast.
   - Light mode: 9 shades (50, 100, 200, ..., 900)
   - Dark mode: Often extend with 950, 975 for deepest backgrounds

**Example Tailwind Extension**:
```javascript
// Add dark mode colors
colors: {
  slate: {
    950: '#0f172a',
    975: '#020617', // Extra dark for deep backgrounds
  }
}
```

---

## Typography Fundamentals

### Type Scale Ratios & Math

A type scale creates proportion and hierarchy. Build it with a mathematical ratio.

**Common Ratios**:
- **Major Third (1.25)**: Conservative, professional. 16px → 20px → 25px → 31px
- **Perfect Fourth (1.33)**: Balanced. 16px → 21px → 28px → 37px
- **Golden Ratio (1.618)**: Elegant, often in print. 16px → 26px → 42px

**Building a Scale (Major Third, base 16px)**:

| Level | Size | Use |
|-------|------|-----|
| xs | 13px (16 × 0.81) | Labels, captions, fine print |
| sm | 14px (16 × 0.875) | Secondary copy, hints, helper text |
| base | 16px | Body text, default |
| md | 18px (16 × 1.125) | Emphasis text, intro paragraphs |
| lg | 20px (16 × 1.25) | Subheadings (h6) |
| xl | 25px (16 × 1.56) | Subheadings (h5) |
| 2xl | 31px (16 × 1.95) | Subheadings (h4) |
| 3xl | 39px (16 × 2.44) | Subheadings (h3) |
| 4xl | 49px (16 × 3.06) | Subheadings (h2) |
| 5xl | 61px (16 × 3.82) | Main heading (h1) |

### Line Height, Measure, and Readability

**Line Height**:
- Body text: 1.5–1.6 (more space for comfortable reading)
- Headings: 1.2–1.3 (tighter, more compact)
- Captions/fine text: 1.4–1.5

**Measure** (line length):
- Ideal: 45–75 characters
- Too narrow: Breaks reading flow
- Too wide: Eyes struggle to return to the next line
- Example at 16px: ~65ch (characters) = ~600px width

**Letter Spacing**:
- Body: 0 (default)
- Headings: Tighten slightly (−0.02em to −0.05em)
- All caps: Increase (0.05em to 0.1em)
- Small text: Slight increase (0.02em) for clarity

### Font Pairing Strategies

**Contrast Principle**: Pair fonts with clear visual differences. Don't pair two similar sans-serif fonts.

**Common Winning Pairs**:

1. **Serif Heading + Sans Body** (Classic, trustworthy)
   - Heading: Georgia, Crimson Text, Playfair Display
   - Body: Segoe UI, Open Sans, Inter

2. **Geometric Sans + Humanist Sans** (Modern, friendly)
   - Heading: Poppins, Futura, Geometric Sans
   - Body: Inter, Nunito, Roboto

3. **Display Font + Classic Sans** (Personality + legibility)
   - Heading: Space Grotesk, Montserrat, Bebas Neue
   - Body: Inter, Lato, Source Sans Pro

4. **Mono + Modern Sans** (Tech-forward)
   - Code/accent: JetBrains Mono, Fira Code, IBM Plex Mono
   - Body: Helvetica Neue, Work Sans, Outfit

### Top 15 Google Font Pairings for Web

| Pair | Heading | Body | Best For |
|------|---------|------|----------|
| 1 | Playfair Display | Inter | Luxury, editorial, B2C |
| 2 | Poppins | Roboto | Modern SaaS, tech, friendly |
| 3 | Space Grotesk | Inter | AI/ML, tech, cutting-edge |
| 4 | Crimson Text | Open Sans | News, publishers, credible |
| 5 | Montserrat | Lato | Minimal, agency, clean |
| 6 | Merriweather | Open Sans | Academic, healthcare, warm |
| 7 | IBM Plex Serif | IBM Plex Sans | Enterprise, professional, B2B |
| 8 | DM Sans | Manrope | Indie, design-forward, cool |
| 9 | Bebas Neue | Work Sans | Bold, energetic, retail |
| 10 | Syne | Outfit | Creative, startup, playful |
| 11 | Abril Fatface | Raleway | Premium, aspirational |
| 12 | Inconsolata | Fira Sans | Developer tools, code-heavy |
| 13 | Quicksand | Nunito | Education, young audience |
| 14 | Cormorant Garamond | Source Sans Pro | Finance, legal, premium |
| 15 | Righteous | Ubuntu | Energetic, youthful, bold |

### Variable Fonts & Responsive Sizing

**Variable Fonts Advantage**:
- Single file, multiple weights (100–900)
- Lower file size than multiple font files
- Smooth interpolation between weights
- Examples: Inter, Poppins, JetBrains Mono

**Responsive Typography with CSS clamp()**:
- `font-size: clamp(min, preferred, max)`
- Scales smoothly between breakpoints without media queries
- Example: `clamp(16px, 4vw, 32px)` — min 16px, max 32px, scales with viewport width

**Formula for Fluid Type**:
```
font-size: clamp(MIN_SIZE, (MIN_SIZE + (MAX_SIZE - MIN_SIZE)) * ((100vw - MIN_VIEWPORT) / (MAX_VIEWPORT - MIN_VIEWPORT)), MAX_SIZE)
```

**Practical Example** (scale from 16px on mobile to 24px on desktop):
```css
font-size: clamp(1rem, 1rem + 1vw, 1.5rem); /* 16px to 24px */
```

---

## Visual Hierarchy: 7 Tools

1. **Size**: Larger = more important. Headlines > subheadings > body. Simple and effective.
2. **Weight**: Bold = emphasis. Use regular for body, 600+ for headings.
3. **Color**: Bright/saturated colors attract attention. Use accent color for CTAs, not body text.
4. **Contrast**: High contrast (dark on light) = more prominent. Low contrast = secondary.
5. **Spacing**: More whitespace around an element = more important. Crowded = secondary.
6. **Alignment**: Centered = more prominent than left-aligned. Symmetry feels important.
7. **Typography**: Font choice signals importance. Distinctive heading font > default body font.

**Apply Hierarchy**: CTA button should be large (size) + bold (weight) + accent color (color) + more whitespace (spacing). Disclaimer text should be small + regular weight + low contrast + no extra space.

---

## Gestalt Principles Applied to Web

**Proximity**: Elements grouped closely together are perceived as related. Use whitespace to separate unrelated groups.

**Similarity**: Elements that look alike (same color, shape, size) are perceived as related. Use consistent styling for related content.

**Continuity**: The eye follows smooth lines and curves. Align elements along invisible grid lines.

**Closure**: The eye completes incomplete shapes. You don't need fully drawn boxes; suggestion is enough.

**Figure-Ground**: Objects stand out from their background. Make buttons "pop" with color or shadow.

---

## Whitespace: Macro vs. Micro

**Macro Whitespace** (section to section):
- Gap between major sections (hero → features → pricing → CTA)
- Typical: 80px to 200px depending on screen size
- Creates breathing room, prevents visual fatigue

**Micro Whitespace** (element to element):
- Padding inside buttons, space between text and images, list item spacing
- Typical: 8px to 32px
- Creates visual group coherence

**The Golden Rule**: More whitespace is almost always better than cramped. If in doubt, add space.

---

## UI Style Taxonomy

**15 Common UI Styles & When to Use**:

1. **Minimalism**: Extreme simplicity, lots of whitespace, essential elements only. Use for: SaaS dashboards, productivity apps, healthcare, finance (conveys clarity, trust).

2. **Glassmorphism**: Frosted glass effect with blurred backgrounds, semi-transparent surfaces. Use for: Modern apps, ambient UX, design-forward products (feels premium, contemporary).

3. **Neobrutalism**: Bold, raw, intentional. Heavy borders, oversized typography, limited color. Use for: Creative agencies, indie products, design tools (rebellious, confident).

4. **Skeuomorphism**: UI mimics real-world objects (leather, wood, leather). Use for: Not recommended for modern web, but works for playful/nostalgic products.

5. **Material Design**: Shadow depth, bold colors, animation. Google's system. Use for: Android apps, Google ecosystem, accessible design (functional, familiar).

6. **Flat Design**: No depth, solid colors, 2D shapes. Use for: Web apps, social media, user-friendly interfaces (clean, fast).

7. **Neumorphism**: Soft shadows, subtle, 3D but subdued. Use for: Personal projects, design experiments (trendy, but less accessible).

8. **Retro/Vintage**: 80s/90s colors, pixelated fonts, nostalgic vibes. Use for: Gaming, indie brands, nostalgia-driven products (personality, fun).

9. **Organic**: Curved shapes, nature-inspired, warm tones. Use for: Wellness, health, eco-friendly brands (human, natural, calming).

10. **Dark Luxury**: Dark backgrounds, gold accents, elegant typography. Use for: Premium products, luxury goods, high-end services (sophisticated, exclusive).

11. **Tech/Cyberpunk**: Neon colors, dark background, grid patterns, futuristic fonts. Use for: AI, developer tools, gaming, tech startups (cutting-edge, intense).

12. **Editorial**: Magazine-like, rich typography, dramatic images. Use for: Content platforms, publishers, luxury brands (authoritative, engaging).

13. **Playful/Rounded**: Rounded buttons, soft colors, friendly vibes. Use for: Consumer apps, kids' products, friendly brands (approachable, fun).

14. **Corporate Clean**: Safe, professional, navy/gray/white. Use for: Enterprise, B2B, financial services (trust, professional).

15. **Handcrafted**: Hand-drawn elements, imperfect shapes, personal feel. Use for: Design agencies, freelancers, artisan products (authentic, personal).

**Key Principle**: Pick one style and apply consistently. Mixing styles creates visual confusion.

---

## Tailwind CSS for Visual Design

### Extending Tailwind for Custom Palettes

```javascript
module.exports = {
  theme: {
    colors: {
      primary: {
        50: '#f0f9ff', // lightest
        500: '#0ea5e9', // base
        950: '#0c2d4a', // darkest
      },
      secondary: {
        // Similar scale
      },
    },
    spacing: {
      0: '0',
      4: '1rem',  // 16px
      8: '2rem',  // 32px
      12: '3rem', // 48px
      16: '4rem', // 64px
      24: '6rem', // 96px
    },
  },
};
```

### Using Tailwind for Hierarchy

- **Headlines**: `text-5xl font-bold text-gray-900`
- **Body**: `text-base font-normal text-gray-700`
- **Secondary**: `text-sm font-normal text-gray-500`
- **CTA**: `bg-primary-500 text-white font-semibold px-6 py-3 rounded-lg hover:bg-primary-600`

---

## Summary: Design Decision Checklist

- [ ] Is color choice based on industry psychology + brand differentiation?
- [ ] Does palette pass WCAG contrast ratios (4.5:1 for normal text)?
- [ ] Do fonts pair with clear contrast (serif + sans, geometric + humanist)?
- [ ] Is type scale built on a mathematical ratio (1.25, 1.33, or 1.618)?
- [ ] Does visual hierarchy use 3+ of the 7 tools?
- [ ] Is there sufficient whitespace (macro + micro)?
- [ ] Is the UI style consistent across the page?
- [ ] Does dark mode adjust luminance and saturation, not just invert?
- [ ] Are colors accessible for color-blind users (not color-only meaning)?
