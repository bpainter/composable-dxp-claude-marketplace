---
type: reference
project: skills-library
scope: skill
plugin: design
parent: "[[motion-interaction-designer]]"
tags: [type/reference, plugin/design, scope/skill, topic/design]
status: active
---
# Framer Motion Patterns & Implementation

Master reference for Framer Motion patterns, spring physics, orchestration, and Next.js integration. Copy-paste ready, production-tested patterns.

---

## 1. Framer Motion Fundamentals

### Motion Components
Every HTML element gets a `motion.` prefix in Framer Motion. The API is identical to React, with added animation props.

```tsx
import { motion } from 'framer-motion';

// Basic motion component
<motion.div
  initial={{ opacity: 0 }}
  animate={{ opacity: 1 }}
  transition={{ duration: 0.5 }}
>
  Hello
</motion.div>
```

### Core Props: initial, animate, exit

- **initial**: Starting state (or `false` to skip initial animation)
- **animate**: Target state when component mounts or deps change
- **exit**: Target state when component unmounts (requires `AnimatePresence`)
- **transition**: Duration, delay, easing, spring config

```tsx
<motion.button
  initial={{ scale: 1 }}
  whileHover={{ scale: 1.05 }}
  whileTap={{ scale: 0.95 }}
  transition={{ type: 'spring', stiffness: 400, damping: 17 }}
>
  Click me
</motion.button>
```

### AnimatePresence: The Critical Exit Animation Hook

Without `AnimatePresence`, exit animations don't work—unmounting is instant. Always wrap conditionally-rendered motion components.

```tsx
import { AnimatePresence, motion } from 'framer-motion';

export function Modal({ isOpen, onClose }) {
  return (
    <AnimatePresence mode="wait">
      {isOpen && (
        <motion.div
          key="modal"
          initial={{ opacity: 0, scale: 0.95 }}
          animate={{ opacity: 1, scale: 1 }}
          exit={{ opacity: 0, scale: 0.95 }}
          transition={{ duration: 0.2 }}
          onClick={onClose}
        >
          Modal content
        </motion.div>
      )}
    </AnimatePresence>
  );
}
```

### Variants: Reusable Animation Templates

Variants are named animation definitions. They're DRY, composable, and make orchestration possible.

```tsx
const containerVariants = {
  hidden: { opacity: 0 },
  show: {
    opacity: 1,
    transition: {
      staggerChildren: 0.1,
      delayChildren: 0.3,
    },
  },
};

const itemVariants = {
  hidden: { opacity: 0, y: 20 },
  show: { opacity: 1, y: 0 },
};

export function CardList({ cards }) {
  return (
    <motion.div
      variants={containerVariants}
      initial="hidden"
      animate="show"
    >
      {cards.map((card) => (
        <motion.div key={card.id} variants={itemVariants}>
          {card.title}
        </motion.div>
      ))}
    </motion.div>
  );
}
```

---

## 2. Spring Physics: The Soul of Modern UI Animation

Springs are more natural than duration-based animations. Damping and stiffness control the feel.

### Spring Config Presets

```tsx
// Snappy: tight, responsive (buttons, toggles)
{ type: 'spring', stiffness: 300, damping: 30, mass: 1 }

// Gentle: smooth, ease-into-motion (modals, panels)
{ type: 'spring', stiffness: 150, damping: 20, mass: 1 }

// Bouncy: playful, overshoot (celebration, cards)
{ type: 'spring', stiffness: 400, damping: 10, mass: 1 }

// Slow & deliberate: emphasis (hero reveals, page transitions)
{ type: 'spring', stiffness: 100, damping: 20, mass: 1 }
```

### Understanding Stiffness & Damping

- **Stiffness** (100–500): Higher = faster spring returns. 150 is gentle, 300 is snappy.
- **Damping** (10–50): Controls oscillation. Higher = less bounce. 20 is balanced.
- **Mass** (0.5–2): Inertia. Usually leave at 1.

```tsx
// Loose & bouncy (mass 2)
<motion.div
  animate={{ x: 100 }}
  transition={{ type: 'spring', stiffness: 300, damping: 10, mass: 2 }}
/>

// Tight & responsive
<motion.div
  animate={{ x: 100 }}
  transition={{ type: 'spring', stiffness: 400, damping: 30 }}
/>
```

---

## 3. Duration-Based vs Spring-Based

### Use Duration When:
- Choreographed sequences (multiple animations in sync)
- Page transitions (consistent, predictable timing)
- Complex easing curves (cubic-bezier control)

### Use Springs When:
- Interactive feedback (button press, drag, hover)
- Natural motion (scrolling, inertia, momentum)
- Responsive feel (feels alive, not robotic)

```tsx
// Duration: choreographed entrance
const listVariants = {
  hidden: { opacity: 0 },
  show: {
    opacity: 1,
    transition: {
      duration: 0.3,
      staggerChildren: 0.05,
      delayChildren: 0.1,
    },
  },
};

// Spring: interactive feedback
const buttonVariants = {
  rest: { scale: 1 },
  hover: { scale: 1.05 },
  tap: { scale: 0.95 },
};

<motion.button
  variants={buttonVariants}
  initial="rest"
  whileHover="hover"
  whileTap="tap"
  transition={{ type: 'spring', stiffness: 400, damping: 17 }}
/>
```

---

## 4. Easing Reference: Cubic-Bezier Mapped to Use Cases

Framer Motion accepts `transition={{ ease: "easeInOut" }}` or custom cubic-bezier.

```tsx
// Presets
easeIn: [0.42, 0, 1, 1]        // Slow start, fast end (entrances)
easeOut: [0, 0, 0.58, 1]       // Fast start, slow end (exits)
easeInOut: [0.42, 0, 0.58, 1]  // Smooth both ends
linear: [0, 0, 1, 1]           // No easing (avoid for UI)

// Custom
circ: [0.6, 0.04, 0.98, 0.335]  // Circular ease
back: [0.68, -0.55, 0.265, 1.55] // Overshoot (bouncy)
expo: [0.7, 0, 0.84, 0]         // Exponential (dramatic)

// Usage
<motion.div
  animate={{ x: 100 }}
  transition={{ duration: 0.4, ease: 'easeOut' }}
/>
```

**Rule of thumb:**
- Entrance: `easeOut` (fast start, lands smoothly)
- Exit: `easeIn` (quick and clean)
- Interactive: spring or `easeInOut`

---

## 5. Variant Orchestration: Parent-Child Synchronization

Parent containers orchestrate child animations via `staggerChildren` and `delayChildren`.

```tsx
const containerVariants = {
  hidden: { opacity: 0 },
  visible: {
    opacity: 1,
    transition: {
      staggerChildren: 0.1,      // Delay between children: 0, 0.1s, 0.2s, etc.
      delayChildren: 0.2,        // All children wait 0.2s before starting
      when: "beforeChildren",    // Parent animates before children (default)
    },
  },
};

const itemVariants = {
  hidden: { opacity: 0, y: 20 },
  visible: { opacity: 1, y: 0, transition: { duration: 0.3 } },
};

// Result: parent fades in, then children stagger
<motion.ul variants={containerVariants} initial="hidden" animate="visible">
  {items.map((item, i) => (
    <motion.li key={i} variants={itemVariants}>
      {item}
    </motion.li>
  ))}
</motion.ul>
```

### Orchestration Order: `when` Prop

```tsx
when: "beforeChildren"  // Parent animates first, then children
when: "afterChildren"   // Children finish, then parent animates
// (Default is beforeChildren)
```

---

## 6. AnimatePresence Modes & Exit Animations

### Mode: "wait" vs "sync"

```tsx
// mode="wait": Wait for exit before next entry (sequential)
<AnimatePresence mode="wait">
  {page === 1 && <Page1 />}
  {page === 2 && <Page2 />}
</AnimatePresence>

// mode="sync": All animations happen simultaneously (default, can overlap)
<AnimatePresence>
  {items.map((item) => <Item key={item.id} />)}
</AnimatePresence>
```

### Exit Animations with Direction Props

Use custom `direction` prop to animate based on which direction the user navigated.

```tsx
interface PageProps {
  direction: 1 | -1;
}

const pageVariants = {
  enter: (direction: number) => ({
    x: direction > 0 ? 1000 : -1000,
    opacity: 0,
  }),
  center: {
    zIndex: 1,
    x: 0,
    opacity: 1,
  },
  exit: (direction: number) => ({
    zIndex: 0,
    x: direction < 0 ? 1000 : -1000,
    opacity: 0,
  }),
};

export function PageTransition({ direction, children }: PageProps) {
  return (
    <AnimatePresence mode="wait" custom={direction}>
      <motion.div
        key="page"
        variants={pageVariants}
        custom={direction}
        initial="enter"
        animate="center"
        exit="exit"
        transition={{ type: 'spring', stiffness: 300, damping: 30 }}
      >
        {children}
      </motion.div>
    </AnimatePresence>
  );
}
```

---

## 7. Layout Animations: Shared Element Transitions

`layoutId` enables "shared layout" animations—one element transitions to another's position.

```tsx
import { motion, AnimateSharedLayout } from 'framer-motion';

export function Tabs() {
  const [activeTab, setActiveTab] = useState(0);

  return (
    <AnimateSharedLayout>
      <div>
        {['Tab 1', 'Tab 2', 'Tab 3'].map((tab, i) => (
          <button
            key={i}
            onClick={() => setActiveTab(i)}
            style={{ position: 'relative' }}
          >
            {tab}
            {i === activeTab && (
              <motion.div
                layoutId="underline"
                style={{
                  position: 'absolute',
                  bottom: -2,
                  left: 0,
                  right: 0,
                  height: 2,
                  backgroundColor: 'blue',
                }}
                transition={{ type: 'spring', stiffness: 300, damping: 30 }}
              />
            )}
          </button>
        ))}
      </div>
    </AnimateSharedLayout>
  );
}
```

### Auto Layout: Animate Height Changes

```tsx
<motion.div layout transition={{ type: 'spring', stiffness: 300, damping: 30 }}>
  <p>Content that grows/shrinks</p>
  {showMore && <p>Additional content</p>}
</motion.div>
```

---

## 8. Gesture Animations

### whileHover, whileTap, whileDrag, whileInView

```tsx
// Hover + Tap
<motion.button
  whileHover={{ scale: 1.05, boxShadow: '0 10px 20px rgba(0,0,0,0.1)' }}
  whileTap={{ scale: 0.95 }}
  transition={{ type: 'spring', stiffness: 400, damping: 17 }}
>
  Hover & tap me
</motion.button>

// Drag constraints
<motion.div
  drag
  dragConstraints={{ left: -100, right: 100, top: -50, bottom: 50 }}
  whileDrag={{ boxShadow: '0 20px 40px rgba(0,0,0,0.2)' }}
>
  Drag me
</motion.div>

// Scroll-triggered animation
<motion.div
  whileInView={{ opacity: 1, y: 0 }}
  initial={{ opacity: 0, y: 50 }}
  viewport={{ once: true, amount: 0.3 }}
>
  Appears when 30% visible
</motion.div>
```

---

## 9. Scroll Utilities: useScroll, useTransform, useMotionValueEvent

### useScroll: Track Scroll Progress

```tsx
import { useScroll, useTransform, motion } from 'framer-motion';
import { useRef } from 'react';

export function ScrollProgress() {
  const ref = useRef(null);
  const { scrollYProgress } = useScroll({
    target: ref,
    offset: ['start end', 'end start'], // [when box enters, when box leaves]
  });

  const scale = useTransform(scrollYProgress, [0, 1], [0.8, 1]);
  const opacity = useTransform(scrollYProgress, [0, 0.5, 1], [0, 1, 0.5]);

  return (
    <motion.div
      ref={ref}
      style={{ scale, opacity }}
      className="box"
    />
  );
}
```

### useTransform: Map One Value to Another

```tsx
// Map scroll progress to rotation, position, color
const { scrollY } = useScroll();

const rotate = useTransform(scrollY, [0, 1000], [0, 360]);
const y = useTransform(scrollY, [0, 1000], [0, 100]);
const opacity = useTransform(scrollY, [0, 500, 1000], [0, 1, 0]);

<motion.div style={{ rotate, y, opacity }} />
```

### useMotionValueEvent: React to Motion Changes

```tsx
import { useMotionValue, useMotionValueEvent } from 'framer-motion';

export function TrackMouseMove() {
  const x = useMotionValue(0);
  const y = useMotionValue(0);

  useMotionValueEvent(x, 'change', (latest) => {
    console.log('X changed to:', latest);
  });

  return (
    <motion.div
      onMouseMove={(e) => {
        x.set(e.clientX);
        y.set(e.clientY);
      }}
    />
  );
}
```

---

## 10. Next.js App Router Integration

### Client Component Boundaries

In Next.js App Router, animations must be client-side. Use `'use client'` at the top of files using Framer Motion.

```tsx
'use client';

import { motion } from 'framer-motion';

export function AnimatedComponent() {
  return (
    <motion.div
      initial={{ opacity: 0 }}
      animate={{ opacity: 1 }}
    >
      This must be a client component
    </motion.div>
  );
}
```

### Page Transitions with `template.tsx`

`template.tsx` is like `layout.tsx` but creates a new instance for each route. Perfect for page transitions.

```tsx
// app/template.tsx
'use client';

import { motion } from 'framer-motion';

export default function Template({ children }: { children: React.ReactNode }) {
  return (
    <motion.div
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      exit={{ opacity: 0, y: -20 }}
      transition={{ duration: 0.3 }}
    >
      {children}
    </motion.div>
  );
}
```

### AnimatePresence in App Router

Must wrap in a client component at the layout or template level.

```tsx
// app/layout.tsx (cannot use AnimatePresence directly)
import { MotionLayout } from './motion-layout';

export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <html>
      <body>
        <MotionLayout>{children}</MotionLayout>
      </body>
    </html>
  );
}

// app/motion-layout.tsx
'use client';

import { AnimatePresence } from 'framer-motion';
import { usePathname } from 'next/navigation';

export function MotionLayout({ children }: { children: React.ReactNode }) {
  const pathname = usePathname();

  return (
    <AnimatePresence mode="wait" initial={false}>
      <div key={pathname}>{children}</div>
    </AnimatePresence>
  );
}
```

---

## 11. Reusable Animation Components

Build wrapper components for common patterns.

### FadeIn Wrapper

```tsx
'use client';

import { motion } from 'framer-motion';
import { ReactNode } from 'react';

interface FadeInProps {
  children: ReactNode;
  delay?: number;
  duration?: number;
}

export function FadeIn({
  children,
  delay = 0,
  duration = 0.5,
}: FadeInProps) {
  return (
    <motion.div
      initial={{ opacity: 0 }}
      animate={{ opacity: 1 }}
      transition={{ duration, delay }}
    >
      {children}
    </motion.div>
  );
}
```

### SlideUp Wrapper

```tsx
'use client';

import { motion } from 'framer-motion';
import { ReactNode } from 'react';

interface SlideUpProps {
  children: ReactNode;
  delay?: number;
  distance?: number;
}

export function SlideUp({
  children,
  delay = 0,
  distance = 20,
}: SlideUpProps) {
  return (
    <motion.div
      initial={{ opacity: 0, y: distance }}
      animate={{ opacity: 1, y: 0 }}
      transition={{
        type: 'spring',
        stiffness: 300,
        damping: 30,
        delay,
      }}
    >
      {children}
    </motion.div>
  );
}
```

### StaggerContainer

```tsx
'use client';

import { motion } from 'framer-motion';
import { ReactNode } from 'react';

interface StaggerContainerProps {
  children: ReactNode;
  staggerDelay?: number;
  delayChildren?: number;
}

export function StaggerContainer({
  children,
  staggerDelay = 0.1,
  delayChildren = 0,
}: StaggerContainerProps) {
  return (
    <motion.div
      variants={{
        hidden: { opacity: 0 },
        visible: {
          opacity: 1,
          transition: {
            staggerChildren: staggerDelay,
            delayChildren,
          },
        },
      }}
      initial="hidden"
      animate="visible"
    >
      {children}
    </motion.div>
  );
}
```

### PageTransition Wrapper

```tsx
'use client';

import { motion } from 'framer-motion';
import { ReactNode } from 'react';

export function PageTransition({ children }: { children: ReactNode }) {
  return (
    <motion.div
      initial={{ opacity: 0, y: 10 }}
      animate={{ opacity: 1, y: 0 }}
      exit={{ opacity: 0, y: -10 }}
      transition={{ duration: 0.3, type: 'spring', stiffness: 300, damping: 30 }}
    >
      {children}
    </motion.div>
  );
}
```

---

## 12. Performance: GPU-Composited Properties Only

Only animate these properties for 60fps:
- `transform` (translate, scale, rotate, skew)
- `opacity`

Avoid animating:
- `width`, `height`, `top`, `left`, `right`, `bottom` (causes layout thrashing)
- `backgroundColor`, `color` (causes repaints, unless absolutely necessary)
- `padding`, `margin` (layout thrashing)

```tsx
// GOOD: 60fps
<motion.div
  animate={{ x: 100, opacity: 0.5 }}
  transition={{ duration: 0.3 }}
/>

// BAD: choppy, repaints
<motion.div
  animate={{ width: 200, height: 100 }}
  transition={{ duration: 0.3 }}
/>

// EXCEPTION: Use layout animations for height changes (Framer Motion optimizes these)
<motion.div layout transition={{ type: 'spring' }}>
  {showMore && <p>Extra content</p>}
</motion.div>
```

### will-change CSS Hint

For complex animations, hint to the browser:

```tsx
<motion.div
  style={{ willChange: 'transform, opacity' }}
  animate={{ x: 100, opacity: 0.5 }}
/>
```

---

## 13. Animation Design Tokens

Define a standard scale for consistency across the app.

```tsx
// animation-tokens.ts
export const durations = {
  instant: 0.1,  // 100ms — instant feedback
  fast: 0.2,     // 200ms — quick interactions
  normal: 0.3,   // 300ms — default transitions
  slow: 0.5,     // 500ms — emphasis, hero animations
  slower: 0.8,   // 800ms — elaborate choreography
};

export const springs = {
  snappy: { type: 'spring' as const, stiffness: 300, damping: 30 },
  gentle: { type: 'spring' as const, stiffness: 150, damping: 20 },
  bouncy: { type: 'spring' as const, stiffness: 400, damping: 10 },
  deliberate: { type: 'spring' as const, stiffness: 100, damping: 20 },
};

export const easings = {
  in: [0.42, 0, 1, 1],
  out: [0, 0, 0.58, 1],
  inOut: [0.42, 0, 0.58, 1],
  circ: [0.6, 0.04, 0.98, 0.335],
  back: [0.68, -0.55, 0.265, 1.55],
};

export const distances = {
  xs: 8,
  sm: 16,
  md: 24,
  lg: 48,
  xl: 96,
};

// Usage
<motion.button
  whileHover={{ scale: 1.05 }}
  transition={springs.snappy}
/>
```

---

## 14. Common Pitfalls & Solutions

| Pitfall | Solution |
|---------|----------|
| Exit animations don't work | Wrap in `<AnimatePresence>` |
| Spring feels floaty/slow | Increase stiffness (300+) or use duration-based |
| Animations cause layout shift | Only animate `transform`, `opacity` |
| Janky on mobile | Reduce spring damping, use shorter durations |
| Animations persist between pages | Use `key` prop to force remount |
| Children don't stagger | Use `staggerChildren` in parent variants |

---

## 15. Testing Animations

```tsx
// Disable animations in tests
jest.mock('framer-motion', () => ({
  ...jest.requireActual('framer-motion'),
  motion: {
    div: ({ children, ...props }: any) => (
      <div {...props}>{children}</div>
    ),
  },
  AnimatePresence: ({ children }: any) => children,
}));
```

Use Chrome DevTools Animation panel to slow down and inspect (Ctrl+Shift+P → "Animations").
