---
type: reference
project: skills-library
scope: skill
plugin: design
parent: "[[web-designer]]"
tags: [type/reference, plugin/design, scope/skill, topic/design]
status: active
---
# Landing Page Architecture & Responsive Design

## Landing Page Anatomy

A high-converting landing page is engineered, not random. Each section has a specific job in moving visitors closer to conversion.

### Section Types & Conversion Roles

| Section | Purpose | Typical Content | Placement |
|---------|---------|-----------------|-----------|
| **Hero** | Grab attention, set value prop | Headline + subhead + CTA + visual | Above the fold |
| **Problem/Value** | Establish relevance | Problem statement or key benefit + supporting text | Right after hero |
| **Features** | Build credibility, show how | Feature grid, icons, descriptions | Mid-page |
| **Social Proof** | Reduce buyer anxiety | Testimonials, logos, stats, case studies | Mid-page |
| **Pricing** | Show options, nudge conversion | Pricing tiers, feature comparison, FAQ | Mid-to-lower page |
| **CTA/Close** | Final conversion push | Bold call-to-action, urgency signal, guarantee | Above fold + footer |
| **FAQ** | Handle objections | Common questions, concise answers | Before final CTA |
| **Footer** | Navigation, trust | Links, trust signals, fine print | Bottom |

---

## Hero Section: Patterns & Variations

The hero is your first impression. It must answer: What is this? Why should I care? What do I do next?

### Hero Pattern 1: Centered (Clean, Elegant)

```
┌─────────────────────────────────────┐
│                                     │
│         HEADLINE (Centered)         │
│                                     │
│     Supporting subheadline text.    │
│       Why should I care about       │
│             this product?           │
│                                     │
│     ┌──────────────────────────┐   │
│     │  PRIMARY CTA BUTTON      │   │
│     └──────────────────────────┘   │
│                                     │
│         [Optional: Logo wall]       │
│                                     │
└─────────────────────────────────────┘
```

**When to use**: SaaS, finance, healthcare, B2B. Conveys clarity, authority, trustworthiness.

**Tailwind**:
```jsx
<div className="min-h-screen bg-white flex items-center justify-center">
  <div className="text-center max-w-2xl px-6">
    <h1 className="text-5xl md:text-6xl font-bold text-gray-900 mb-6">
      Headline Here
    </h1>
    <p className="text-xl text-gray-600 mb-8">
      Supporting subheadline explaining value.
    </p>
    <button className="bg-blue-600 text-white px-8 py-4 rounded-lg font-semibold hover:bg-blue-700">
      Get Started
    </button>
  </div>
</div>
```

### Hero Pattern 2: Split (Content + Visual)

```
┌──────────────────┬──────────────────┐
│                  │                  │
│   HEADLINE       │    Product       │
│                  │    Screenshot    │
│   Subheadline    │    or Hero       │
│                  │    Image         │
│   [CTA Button]   │                  │
│                  │                  │
└──────────────────┴──────────────────┘
```

**When to use**: Product showcases, design tools, e-commerce. Shows product in action.

**Tailwind**:
```jsx
<div className="grid grid-cols-1 md:grid-cols-2 gap-12 items-center min-h-screen">
  <div>
    <h1 className="text-5xl font-bold text-gray-900 mb-4">Headline</h1>
    <p className="text-lg text-gray-600 mb-8">Subheadline</p>
    <button className="bg-blue-600 text-white px-8 py-4 rounded-lg font-semibold">
      CTA
    </button>
  </div>
  <div className="relative">
    <img src="hero-image.png" alt="Product" className="w-full rounded-lg shadow-lg" />
  </div>
</div>
```

### Hero Pattern 3: Image Background (Dramatic, High-Impact)

```
┌─────────────────────────────────────┐
│   [Full-width background image]     │
│                                     │
│        HEADLINE (light text)        │
│     Subheadline (light text)        │
│                                     │
│  [HIGH-CONTRAST CTA BUTTON]         │
│                                     │
└─────────────────────────────────────┘
```

**When to use**: Luxury, hospitality, fashion, lifestyle. Emotional, aspirational.

**Tailwind**:
```jsx
<div
  className="min-h-screen bg-cover bg-center relative"
  style={{ backgroundImage: `url('bg.jpg')` }}
>
  <div className="absolute inset-0 bg-black/40"></div>
  <div className="relative z-10 flex items-center justify-center h-screen text-white text-center">
    <div className="max-w-2xl px-6">
      <h1 className="text-6xl font-bold mb-6">Headline</h1>
      <p className="text-xl mb-8">Subheadline</p>
      <button className="bg-white text-black px-8 py-4 font-semibold rounded-lg hover:bg-gray-200">
        CTA
      </button>
    </div>
  </div>
</div>
```

### Hero Pattern 4: Video Background (Modern, Engaging)

Like image background but with video element instead. Auto-plays, muted, loops. Great for SaaS product demonstrations.

### Hero Pattern 5: Animated/Interactive (Premium Feel)

- Parallax scrolling (background moves slower than foreground)
- Lottie animations (lightweight JSON animations)
- Gradient animations, floating elements
- Must be performance-optimized (don't block page load)

---

## Above-the-Fold Checklist

What must be visible before scrolling?

- [ ] **Headline**: Clear, benefit-driven (not feature-driven). 8–12 words ideal.
- [ ] **Value Proposition**: Why should I care? What problem does this solve?
- [ ] **Primary CTA**: Obvious, high-contrast button (Start Free Trial, Get Started, Demo).
- [ ] **Visual Signal**: Image, icon, video, or product screenshot showing what this is.
- [ ] **Social Proof Signal**: Logo, testimonial snippet, stat, or "Trusted by 500+ companies."
- [ ] **Unique Value**: What makes this different? Why not competitor X?

**Headline Formula**:
- Benefit-driven: "Save 10 hours/week with automated data entry"
- Problem-aware: "Stop losing candidates to slow hiring processes"
- Question-based: "What if onboarding took days, not weeks?"

---

## Feature Section Layouts

### Layout 1: Alternating Image/Text (Classic)

```
┌─────────────────────────────────────┐
│ [Image]  │  Feature 1 heading       │
│          │  Feature 1 description   │
├─────────────────────────────────────┤
│  Feature 2 heading  │  [Image]      │
│  Feature 2 desc     │               │
├─────────────────────────────────────┤
│ [Image]  │  Feature 3 heading       │
│          │  Feature 3 description   │
└─────────────────────────────────────┘
```

**Tailwind**:
```jsx
<div className="space-y-16">
  {/* Feature 1: Image Left */}
  <div className="grid grid-cols-1 md:grid-cols-2 gap-8 items-center">
    <img src="feature1.png" alt="Feature 1" className="rounded-lg" />
    <div>
      <h3 className="text-3xl font-bold mb-4">Feature 1</h3>
      <p className="text-gray-600">Description...</p>
    </div>
  </div>

  {/* Feature 2: Image Right */}
  <div className="grid grid-cols-1 md:grid-cols-2 gap-8 items-center">
    <div className="order-2 md:order-1">
      <h3 className="text-3xl font-bold mb-4">Feature 2</h3>
      <p className="text-gray-600">Description...</p>
    </div>
    <img src="feature2.png" alt="Feature 2" className="order-1 md:order-2 rounded-lg" />
  </div>
</div>
```

### Layout 2: Icon Grid (Scannable)

```
┌─────────────────────────────────────┐
│  Icon 1    Icon 2    Icon 3         │
│  Heading   Heading   Heading        │
│  Text      Text      Text           │
│                                     │
│  Icon 4    Icon 5    Icon 6         │
│  Heading   Heading   Heading        │
│  Text      Text      Text           │
└─────────────────────────────────────┘
```

**Tailwind**:
```jsx
<div className="grid grid-cols-1 md:grid-cols-3 gap-8">
  {features.map((feature) => (
    <div key={feature.id} className="text-center">
      <div className="mb-4 text-4xl">{feature.icon}</div>
      <h3 className="text-xl font-bold mb-2">{feature.heading}</h3>
      <p className="text-gray-600">{feature.description}</p>
    </div>
  ))}
</div>
```

### Layout 3: Tabbed Features (Compact)

```
┌─────────────────────────────────────┐
│ Tab 1  | Tab 2  | Tab 3  | Tab 4    │
├─────────────────────────────────────┤
│ [Image]  │ Feature heading         │
│          │ Feature description     │
│          │ [CTA Link]              │
└─────────────────────────────────────┘
```

**When to use**: Many features (4+ tiers), limited vertical space.

### Layout 4: Comparison Table (Transparent Differentiation)

For SaaS: Show how your features compare to competitors or across pricing tiers.

```
┌──────────┬──────────┬──────────────┐
│ Feature  │ Lite     │ Pro          │
├──────────┼──────────┼──────────────┤
│ Users    │ 5        │ Unlimited ✓  │
│ API      │ ✗        │ ✓            │
│ Support  │ Email    │ 24/7 Phone ✓ │
└──────────┴──────────┴──────────────┘
```

---

## Social Proof: Placement Strategy

**Types of Social Proof**:
1. **Testimonials**: Customer quote + name + company + photo
2. **Logos**: "Trusted by [Company] [Company] [Company]"
3. **Stats**: "10,000+ companies use us" or "99.9% uptime"
4. **Case Studies**: Deep-dive with metrics (increase efficiency by X%)
5. **Reviews**: Star rating + platform (G2, Trustpilot)

**Where to Place**:
- Right after hero: Logo wall or one star testimonial
- Mid-page: Testimonials carousel or grid (3–4 visible)
- Before CTA: 1–2 power testimonials from recognizable companies
- Social proof near CTA reduces friction by 20–30%

**Testimonial Format**:
```
"[Specific quote about results or experience]"

— Name, Title
   Company Name
   [Optional: Company logo or profile photo]
```

**Avoid**: Generic praise ("Great product!"). Require specificity: "Reduced onboarding time from 2 weeks to 3 days."

---

## Pricing Page Psychology

### Anchor Pricing (Set Expectations)

Display your highest-price option first (briefly) to anchor expectations upward. When visitors see the mid-tier later, it seems more reasonable.

### Decoy Effect (Nudge Toward Middle Tier)

Offer 3 tiers; make the middle tier the "obvious" choice:

```
┌──────────────┬──────────────┬──────────────┐
│  STARTER     │  POPULAR ★   │  ENTERPRISE  │
│              │              │              │
│ $29/mo       │ $99/mo       │ Custom       │
│              │ (BEST VALUE) │              │
│ 5 projects   │ 50 projects  │ Unlimited    │
│ Email        │ Email + chat │ Dedicated    │
│              │ ✓ Highlighted│ support      │
│ [Choose]     │ [CHOOSE]     │ [Contact]    │
└──────────────┴──────────────┴──────────────┘
```

**Highlight the middle tier**: Bold border, background color, "POPULAR" badge, tick icon. Make it obviously the best deal.

### Feature Comparison Table (Transparency)

Show what each tier includes. Use checkmarks ✓ for yes, dashes – for no.

```
| Feature          | Starter | Popular | Enterprise |
|------------------|---------|---------|------------|
| Users            | 5       | 25      | Unlimited  |
| API Access       | –       | ✓       | ✓          |
| Custom Branding  | –       | ✓       | ✓          |
| Dedicated Support| –       | –       | ✓          |
```

### FAQ Placement & Objection Handling

Place FAQ immediately below pricing, in an accordion format:

**Common Questions**:
- "Can I change plans later?" → Reassure: Yes, anytime.
- "Is there a setup fee?" → Clear: No hidden fees.
- "Do you offer discounts for annual?" → Specific: 20% off annual.
- "What if I need more than [limit]?" → Path: Contact sales or upgrade.

---

## CTA Hierarchy & Placement

**Primary CTA**: High contrast, above fold, clear action. "Start Free Trial" or "Get Started."

**Secondary CTA**: Less prominent, mid-page. "Learn More" or "See Demo."

**Tertiary CTA**: Text links, footer. "Documentation" or "Contact Us."

**Button Styling**:
- Primary: Bright accent color, white text, padding 12px 24px, font-semibold
- Secondary: Outline or gray background, smaller padding
- Tertiary: Text link, no background

**Placement**:
- Hero: Primary CTA (above fold)
- Feature sections: Subtle secondary CTA
- Before FAQ: Primary CTA (re-engage before objections)
- After testimonials: Primary CTA (momentum from proof)
- Footer: Secondary + tertiary CTAs

---

## Section Spacing & Rhythm

Consistent vertical spacing creates harmony and guides the eye.

**Typical Spacing Values** (in px):
- Between major sections: 80–200px (depending on screen size)
- Between smaller elements: 24–48px
- Inside cards/boxes: 16–32px padding

**Responsive Spacing with Tailwind**:
```
Spacing scale: my-6 (1.5rem = 24px), my-12 (3rem = 48px), my-20 (5rem = 80px)

On mobile: <section className="my-12"> (24px spacing)
On desktop: <section className="my-20"> (80px spacing)

Use: my-12 md:my-20 for responsive spacing
```

**Breathing Room Rule**: The space around a section should match or exceed the space inside elements. Crowded = rushed. Spacious = premium.

---

## shadcndesign Pro Blocks: Landing Page Composition

shadcndesign.com offers pre-built Tailwind + React blocks. Composition strategy:

### Block Categories for Landing Pages

**Hero Sections**:
- `hero-01` through `hero-06` — Multiple patterns (centered, split, image bg, video)
- Pick based on your product type (see earlier Hero Patterns)

**Feature Sections**:
- `features-01` through `features-09` — Icon grids, alternating layouts, tabbed
- `description-list-*` — For structured feature lists

**Social Proof**:
- `testimonials-01` through `testimonials-10` — Carousel, grid, quote-based
- `logo-list-*` — "Trusted by" logo walls

**Pricing**:
- `pricing-01` through `pricing-06` — 3-tier, comparison tables, calculator
- Use `pricing-03` for decoy-effect (highlighted middle tier)

**CTA & Close**:
- `cta-*` blocks — Final call-to-action sections
- `faq-*` — Accordion FAQ patterns

**Footer**:
- Standard footer patterns for navigation, legal, contact

### Composition Workflow

1. **Select Hero Block**: Match to your brand and product type
2. **Add 1–2 Feature Blocks**: Show value, keep it tight
3. **Add Social Proof Block**: Testimonials or logos
4. **Add Pricing Block**: If applicable
5. **Add FAQ Block**: Handle objections
6. **Add CTA Block**: Final push
7. **Adjust Spacing**: Ensure consistent rhythm between blocks
8. **Dark Mode**: Test and adjust colors for dark mode

**Example Full-Page Structure**:
```jsx
<Hero block="hero-03" />           {/* Split image/text */}
<Features block="features-02" />   {/* Icon grid */}
<Testimonials block="testimonials-04" /> {/* 3-column carousel */}
<Pricing block="pricing-03" />     {/* 3 tiers with highlight */}
<FAQ block="faq-01" />             {/* Accordion */}
<CTA block="cta-01" />             {/* Final CTA */}
<Footer block="footer-01" />
```

---

## Responsive Design Strategy

### Mobile-First Methodology

Start designing for mobile (smallest screen), then add CSS for larger screens.

**Tailwind Breakpoints**:
- `(default)` — 0px and up (mobile)
- `sm:` — 640px and up
- `md:` — 768px and up
- `lg:` — 1024px and up
- `xl:` — 1280px and up
- `2xl:` — 1536px and up

**Example**:
```jsx
{/* Mobile-first: starts at 1 column */}
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3">
  {/* 1 column on mobile, 2 on tablet, 3 on desktop */}
</div>
```

### What Changes at Each Breakpoint?

| Element | Mobile | Tablet | Desktop |
|---------|--------|--------|---------|
| Navigation | Hamburger menu | Horizontal nav | Horizontal nav |
| Grid columns | 1 | 2 | 3–4 |
| Font size | 14–16px | 16–18px | 16–20px |
| Padding | 16px | 20px | 32px |
| Images | 100% width, reduced size | 80% width | Full width |
| Hero height | 100vh | 100vh | 100vh |
| Spacing | Tight (8–12px gaps) | Medium (16px) | Generous (24px+) |

### Container Queries (Component-Level Responsiveness)

Instead of viewport breakpoints, respond to container width:

```css
@container (min-width: 400px) {
  .card { grid-template-columns: 1fr 1fr; }
}
```

Advantage: Reusable components that adapt to their parent, not just viewport.

### Touch Target Sizing (Mobile)

- Minimum: 44×44px (Apple HIG)
- Recommended: 48×48px
- Spacing between targets: 8px minimum

**Tailwind**:
```jsx
<button className="h-12 w-12 md:h-10 md:w-10"> {/* 48px mobile, 40px desktop */}
  Click
</button>
```

### Responsive Images

**Next.js Image Component** (best practice):
```jsx
<Image
  src="hero.jpg"
  alt="Hero"
  width={1200}
  height={600}
  sizes="(max-width: 640px) 100vw, (max-width: 1024px) 90vw, 80vw"
  priority // For above-the-fold images
/>
```

**HTML srcset** (alternative):
```html
<img
  src="hero-medium.jpg"
  srcset="hero-small.jpg 640w, hero-medium.jpg 1024w, hero-large.jpg 1200w"
  sizes="(max-width: 640px) 100vw, (max-width: 1024px) 90vw, 80vw"
  alt="Hero"
/>
```

### Common Responsive Patterns

1. **Stack** (vertical on mobile, horizontal on desktop):
   ```jsx
   <div className="flex flex-col md:flex-row gap-8">
     <div>Left</div>
     <div>Right</div>
   </div>
   ```

2. **Shift** (reorder on different screens):
   ```jsx
   <div className="order-2 md:order-1">Content</div>
   <div className="order-1 md:order-2">Content</div>
   ```

3. **Reflow** (columns reduce):
   ```jsx
   <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3">
   ```

4. **Reveal/Hide** (show different content at different breakpoints):
   ```jsx
   <div className="hidden md:block">Desktop only</div>
   <div className="md:hidden">Mobile only</div>
   ```

5. **Progressive Disclosure** (accordions, tabs on mobile):
   ```jsx
   <div className="md:grid md:grid-cols-2">
     {/* Tabs on mobile, side-by-side on desktop */}
   </div>
   ```

---

## Page Performance & Design

Performance affects user experience and SEO. Design decisions that matter:

### Image Optimization

- **Format**: Use WebP for modern browsers, JPEG fallback for old browsers
- **Size**: Compress images, resize for device. Don't serve 2000px image to mobile.
- **Lazy Loading**: Load images below the fold only when they come into view
  ```jsx
  <img loading="lazy" src="..." /> {/* Browser-native lazy loading */}
  ```

### Cumulative Layout Shift (CLS)

Avoid layout shifts that frustrate users. Causes:
- Images without defined dimensions
- Ads/embeds without reserved space
- Font loading causing text size change

**Prevent CLS**:
- Always define image dimensions (width/height or aspect-ratio)
- Use `font-display: swap` for web fonts (show fallback immediately)
- Reserve space for ads/lazy-loaded content

### Lazy Loading Below the Fold

```jsx
{/* Priority for hero images */}
<Image src="hero.jpg" priority />

{/* Lazy load for images below fold */}
<Image src="feature.jpg" loading="lazy" />
```

### Code Splitting & Progressive Enhancement

Load heavy components only when needed:
```jsx
const Pricing = dynamic(() => import('./Pricing'), { ssr: false });
```

---

## Responsive Design Checklist

- [ ] Mobile-first: Designed for smallest screen first
- [ ] Touch targets: All interactive elements 44×44px minimum
- [ ] Breakpoints: Tested at sm/md/lg/xl/2xl
- [ ] Images: Responsive, lazy-loaded, properly sized
- [ ] Typography: Scales smoothly across breakpoints
- [ ] Spacing: Adjusts for mobile density
- [ ] Navigation: Hamburger on mobile, horizontal on desktop
- [ ] Forms: Single-column on mobile, readable inputs
- [ ] Horizontal scroll: Avoided on all breakpoints
- [ ] CLS: No layout shifts, images have dimensions
- [ ] Performance: Lighthouse score 90+

---

## Summary: Landing Page Checklist

**Hero Section**:
- [ ] Headline answers: What is this?
- [ ] Subheadline answers: Why do I care?
- [ ] Primary CTA is obvious and high-contrast
- [ ] Visual (image/video) shows the product in context
- [ ] All essential content above the fold

**Content Sections**:
- [ ] Features shown with icons/images, not just text
- [ ] One primary conversion goal per page (avoid choice overload)
- [ ] Social proof (logos/testimonials) visible without scrolling far
- [ ] FAQ handles top objections
- [ ] Spacing is consistent and generous

**Conversion Elements**:
- [ ] CTA hierarchy: primary > secondary > tertiary
- [ ] Primary CTA appears 2–3 times on page
- [ ] Button text action-oriented ("Get Started", not "Submit")
- [ ] No surprise costs or friction (trust signals visible)

**Responsive**:
- [ ] Mobile-first structure
- [ ] Touch-friendly buttons and spacing
- [ ] Images responsive and properly sized
- [ ] No horizontal scrolling
- [ ] Tested at all breakpoints

**Performance**:
- [ ] Lazy-loaded images below fold
- [ ] No CLS (layout shifts)
- [ ] Lighthouse mobile score 85+
