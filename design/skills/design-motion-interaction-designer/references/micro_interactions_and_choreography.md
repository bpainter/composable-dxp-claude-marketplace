---
type: reference
project: skills-library
scope: skill
plugin: design
parent: "[[motion-interaction-designer]]"
tags: [type/reference, plugin/design, scope/skill, topic/design]
status: active
---
# Micro-Interactions & Choreography Playbook

Deep reference for designing and implementing micro-interactions, loading choreography, scroll patterns, and reduced-motion accessibility. Production-ready specifications and code.

---

## 1. Micro-Interaction Anatomy

Every micro-interaction has four parts:

1. **Trigger**: What initiates it (hover, click, focus, data load, error)
2. **Rules**: What happens (constraints, conditions, exceptions)
3. **Feedback**: What the user sees/feels (visual change, sound, haptic)
4. **Loops/Modes**: What repeats (hover stays, click reverts, etc.)

### Example: Button Press

```
Trigger:  User clicks button
Rules:    Only if button is not disabled; if loading, show spinner instead
Feedback: Scale down 0.98 (10ms), then animate to normal color over 200ms
Loops:    Revert to hover state after, ready for next click
```

---

## 2. Button Interactions

### Hover State

```tsx
'use client';

import { motion } from 'framer-motion';

export function HoverButton() {
  return (
    <motion.button
      className="px-4 py-2 rounded bg-blue-500 text-white"
      whileHover={{
        scale: 1.02,
        boxShadow: '0 10px 20px rgba(0, 0, 0, 0.15)',
      }}
      whileTap={{ scale: 0.98 }}
      transition={{ type: 'spring', stiffness: 400, damping: 17 }}
    >
      Hover me
    </motion.button>
  );
}
```

**Spec:**
- Hover: scale 1.02, shadow lift (0 10px 20px)
- Tap/press: scale 0.98 instantly
- Duration: spring (snappy)
- Return: automatic on unhover

### Loading State

```tsx
'use client';

import { motion } from 'framer-motion';
import { useState } from 'react';

export function LoadingButton() {
  const [isLoading, setIsLoading] = useState(false);

  const handleClick = async () => {
    setIsLoading(true);
    await new Promise((resolve) => setTimeout(resolve, 2000));
    setIsLoading(false);
  };

  return (
    <motion.button
      onClick={handleClick}
      disabled={isLoading}
      className="relative px-4 py-2 rounded bg-blue-500 text-white"
      animate={{ opacity: isLoading ? 0.6 : 1 }}
      transition={{ duration: 0.2 }}
    >
      <motion.span
        animate={{ opacity: isLoading ? 0 : 1 }}
        transition={{ duration: 0.1 }}
      >
        Submit
      </motion.span>

      {isLoading && (
        <motion.span
          className="absolute inset-0 flex items-center justify-center"
          initial={{ opacity: 0 }}
          animate={{ opacity: 1 }}
        >
          <motion.div
            className="w-4 h-4 border-2 border-transparent border-t-white rounded-full"
            animate={{ rotate: 360 }}
            transition={{ duration: 1, repeat: Infinity, ease: 'linear' }}
          />
        </motion.span>
      )}
    </motion.button>
  );
}
```

**Spec:**
- Click → fade out text, spin loader in
- Duration: 100-200ms for text fade
- Loader: continuous 1s rotation
- Disabled: opacity 0.6, pointer-events disabled
- On complete: reverse animation

### Success State

```tsx
'use client';

import { motion, AnimatePresence } from 'framer-motion';
import { useState } from 'react';

export function SuccessButton() {
  const [state, setState] = useState<'idle' | 'loading' | 'success'>('idle');

  const handleClick = async () => {
    setState('loading');
    await new Promise((resolve) => setTimeout(resolve, 1500));
    setState('success');
    setTimeout(() => setState('idle'), 2000);
  };

  return (
    <motion.button
      onClick={handleClick}
      disabled={state !== 'idle'}
      className="relative px-4 py-2 rounded bg-green-500 text-white overflow-hidden"
      animate={{
        backgroundColor:
          state === 'success' ? '#10b981' : state === 'loading' ? '#3b82f6' : '#10b981',
      }}
      transition={{ duration: 0.3 }}
    >
      <AnimatePresence mode="wait">
        {state === 'idle' && (
          <motion.span
            key="text"
            initial={{ opacity: 0, y: 10 }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: -10 }}
          >
            Save
          </motion.span>
        )}

        {state === 'loading' && (
          <motion.div
            key="loader"
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            exit={{ opacity: 0 }}
          >
            <motion.div
              className="w-4 h-4 border-2 border-white border-t-transparent rounded-full inline-block"
              animate={{ rotate: 360 }}
              transition={{ duration: 1, repeat: Infinity, ease: 'linear' }}
            />
          </motion.div>
        )}

        {state === 'success' && (
          <motion.span
            key="checkmark"
            initial={{ scale: 0, rotate: -180 }}
            animate={{ scale: 1, rotate: 0 }}
            exit={{ scale: 0 }}
            transition={{ type: 'spring', stiffness: 400, damping: 15 }}
          >
            ✓
          </motion.span>
        )}
      </AnimatePresence>
    </motion.button>
  );
}
```

---

## 3. Form Interactions

### Input Focus with Label Float

```tsx
'use client';

import { motion } from 'framer-motion';
import { useState } from 'react';

export function FloatingLabelInput() {
  const [isFocused, setIsFocused] = useState(false);
  const [hasValue, setHasValue] = useState(false);

  return (
    <div className="relative">
      <motion.label
        animate={{
          y: isFocused || hasValue ? -24 : 0,
          scale: isFocused || hasValue ? 0.85 : 1,
          color: isFocused ? '#3b82f6' : '#6b7280',
        }}
        transition={{ type: 'spring', stiffness: 300, damping: 30 }}
        className="absolute left-0 top-0 cursor-text origin-left"
      >
        Email
      </motion.label>

      <motion.input
        type="email"
        onFocus={() => setIsFocused(true)}
        onBlur={() => setIsFocused(false)}
        onChange={(e) => setHasValue(!!e.target.value)}
        className="w-full px-0 py-2 border-b-2 border-gray-300 focus:outline-none focus:border-blue-500"
        animate={{
          borderColor: isFocused ? '#3b82f6' : '#d1d5db',
        }}
        transition={{ duration: 0.2 }}
      />
    </div>
  );
}
```

**Spec:**
- Focus: label floats up, scale 0.85, color blue
- Blur: label returns if empty, stays if value present
- Border: gray → blue on focus, 200ms
- Duration: spring (snappy)

### Validation with Shake & Check

```tsx
'use client';

import { motion } from 'framer-motion';
import { useState } from 'react';

interface ValidationInputProps {
  onValidate?: (email: string) => boolean;
}

export function ValidationInput({ onValidate }: ValidationInputProps) {
  const [value, setValue] = useState('');
  const [state, setState] = useState<'idle' | 'invalid' | 'valid'>('idle');

  const handleChange = (e: string) => {
    setValue(e);
    setState('idle');
  };

  const handleBlur = () => {
    if (!value) return;
    const isValid = onValidate ? onValidate(value) : /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value);
    setState(isValid ? 'valid' : 'invalid');
  };

  const shakeVariants = {
    invalid: {
      x: [0, -5, 5, -5, 5, 0],
      transition: { duration: 0.4 },
    },
  };

  return (
    <div className="relative">
      <motion.input
        type="email"
        value={value}
        onChange={(e) => handleChange(e.target.value)}
        onBlur={handleBlur}
        className="w-full px-3 py-2 border-2 rounded"
        animate={state === 'invalid' ? shakeVariants.invalid : {}}
        style={{
          borderColor:
            state === 'invalid' ? '#ef4444' : state === 'valid' ? '#10b981' : '#d1d5db',
        }}
        transition={{ duration: 0.2 }}
      />

      <motion.div
        initial={{ opacity: 0, scale: 0 }}
        animate={{
          opacity: state === 'valid' ? 1 : 0,
          scale: state === 'valid' ? 1 : 0,
        }}
        transition={{ type: 'spring', stiffness: 400, damping: 15 }}
        className="absolute right-3 top-2.5 text-green-500 text-xl"
      >
        ✓
      </motion.div>
    </div>
  );
}
```

**Spec:**
- Invalid: shake (±5px, 400ms), border red
- Valid: green checkmark, spring scale (snappy)
- Duration: shake 400ms, checkmark 300ms

### Form Submission Flow

```tsx
'use client';

import { motion, AnimatePresence } from 'framer-motion';
import { useState } from 'react';

export function FormFlow() {
  const [submitted, setSubmitted] = useState(false);
  const [loading, setLoading] = useState(false);

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    setLoading(true);

    // Simulate API call
    await new Promise((resolve) => setTimeout(resolve, 1500));

    setLoading(false);
    setSubmitted(true);
    setTimeout(() => setSubmitted(false), 3000);
  };

  return (
    <form onSubmit={handleSubmit} className="space-y-4">
      <AnimatePresence mode="wait">
        {!submitted && !loading && (
          <motion.div
            key="form"
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            exit={{ opacity: 0 }}
          >
            <input type="text" placeholder="Name" className="w-full px-3 py-2 border rounded" />
            <input type="email" placeholder="Email" className="w-full px-3 py-2 border rounded" />
            <button
              type="submit"
              className="w-full px-3 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
            >
              Submit
            </button>
          </motion.div>
        )}

        {loading && (
          <motion.div
            key="loading"
            initial={{ opacity: 0, y: 20 }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: -20 }}
            className="text-center py-6"
          >
            <motion.div
              className="w-8 h-8 border-2 border-blue-500 border-t-transparent rounded-full mx-auto"
              animate={{ rotate: 360 }}
              transition={{ duration: 1, repeat: Infinity, ease: 'linear' }}
            />
            <p className="mt-2 text-gray-600">Submitting...</p>
          </motion.div>
        )}

        {submitted && !loading && (
          <motion.div
            key="success"
            initial={{ opacity: 0, scale: 0.8 }}
            animate={{ opacity: 1, scale: 1 }}
            exit={{ opacity: 0, scale: 0.8 }}
            className="text-center py-6 bg-green-50 rounded border border-green-200"
          >
            <motion.span
              className="text-4xl"
              animate={{ scale: [1, 1.2, 1] }}
              transition={{ duration: 0.6 }}
            >
              ✓
            </motion.span>
            <p className="mt-2 text-green-700">Success!</p>
          </motion.div>
        )}
      </AnimatePresence>
    </form>
  );
}
```

---

## 4. Toggle & Switch Animations

### Spring-Based Toggle

```tsx
'use client';

import { motion } from 'framer-motion';
import { useState } from 'react';

export function AnimatedToggle() {
  const [isOn, setIsOn] = useState(false);

  return (
    <motion.button
      onClick={() => setIsOn(!isOn)}
      className={`w-14 h-8 rounded-full flex items-center p-1 transition-colors ${
        isOn ? 'bg-green-500' : 'bg-gray-300'
      }`}
      animate={{ backgroundColor: isOn ? '#10b981' : '#d1d5db' }}
      transition={{ duration: 0.3 }}
    >
      <motion.div
        className="w-6 h-6 rounded-full bg-white"
        animate={{ x: isOn ? 24 : 0 }}
        transition={{ type: 'spring', stiffness: 300, damping: 30 }}
      />
    </motion.button>
  );
}
```

**Spec:**
- Thumb: spring animation, x: 0 → 24px
- Background: color transition 300ms
- Duration: spring (snappy) for thumb, 300ms for color
- Reversed on click

### Icon-Morphing Toggle (SVG)

```tsx
'use client';

import { motion } from 'framer-motion';
import { useState } from 'react';

export function IconMorphToggle() {
  const [isEnabled, setIsEnabled] = useState(false);

  return (
    <motion.button
      onClick={() => setIsEnabled(!isEnabled)}
      className="p-2 rounded hover:bg-gray-100"
    >
      <motion.svg
        width="24"
        height="24"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        animate={{ scale: isEnabled ? 1.1 : 1 }}
        transition={{ type: 'spring', stiffness: 400, damping: 17 }}
      >
        <motion.path
          d={isEnabled ? 'M5 12l5 5l9-9' : 'M12 5v14M5 12h14'}
          strokeWidth="2"
          strokeLinecap="round"
          initial={false}
          animate={{ d: isEnabled ? 'M5 12l5 5l9-9' : 'M12 5v14M5 12h14' }}
          transition={{ duration: 0.3 }}
        />
      </motion.svg>
    </motion.button>
  );
}
```

---

## 5. Accordion & Disclosure

### Height-Based Expand/Collapse

```tsx
'use client';

import { motion, AnimatePresence } from 'framer-motion';
import { useState } from 'react';

export function Accordion() {
  const [openId, setOpenId] = useState<string | null>(null);

  const items = [
    { id: '1', title: 'What is this?', content: 'This is a collapsible accordion item.' },
    { id: '2', title: 'How does it work?', content: 'It uses Framer Motion for animations.' },
    { id: '3', title: 'Can I use it?', content: 'Yes, copy and customize!' },
  ];

  return (
    <div className="space-y-2">
      {items.map((item) => (
        <div key={item.id} className="border rounded">
          <motion.button
            onClick={() => setOpenId(openId === item.id ? null : item.id)}
            className="w-full px-4 py-3 flex justify-between items-center font-semibold hover:bg-gray-50"
          >
            <span>{item.title}</span>
            <motion.span
              animate={{ rotate: openId === item.id ? 180 : 0 }}
              transition={{ duration: 0.2 }}
            >
              ▼
            </motion.span>
          </motion.button>

          <AnimatePresence initial={false}>
            {openId === item.id && (
              <motion.div
                initial={{ height: 0, opacity: 0 }}
                animate={{ height: 'auto', opacity: 1 }}
                exit={{ height: 0, opacity: 0 }}
                transition={{ type: 'spring', stiffness: 300, damping: 30 }}
                className="overflow-hidden"
              >
                <div className="px-4 py-3 bg-gray-50 text-sm">{item.content}</div>
              </motion.div>
            )}
          </AnimatePresence>
        </div>
      ))}
    </div>
  );
}
```

**Spec:**
- Open: height 0 → auto, opacity 0 → 1, spring animation
- Chevron: rotate 0 → 180°, 200ms
- Duration: spring (snappy) for height, 200ms for chevron
- AnimatePresence mode="initial" to animate on first render

---

## 6. Toast & Notification Animations

### Slide-In Toast

```tsx
'use client';

import { motion, AnimatePresence } from 'framer-motion';
import { useEffect, useState } from 'react';

interface Toast {
  id: string;
  message: string;
  type: 'success' | 'error' | 'info';
}

export function ToastContainer() {
  const [toasts, setToasts] = useState<Toast[]>([]);

  const addToast = (message: string, type: 'success' | 'error' | 'info' = 'info') => {
    const id = Math.random().toString(36).substr(2, 9);
    const toast = { id, message, type };
    setToasts((prev) => [...prev, toast]);

    // Auto-remove after 4 seconds
    setTimeout(() => {
      setToasts((prev) => prev.filter((t) => t.id !== id));
    }, 4000);
  };

  const bgColor = {
    success: 'bg-green-500',
    error: 'bg-red-500',
    info: 'bg-blue-500',
  };

  return (
    <>
      <button
        onClick={() => addToast('Success!', 'success')}
        className="px-3 py-1 bg-green-500 text-white rounded text-sm mr-2"
      >
        Success
      </button>

      <div className="fixed bottom-4 right-4 space-y-2 z-50">
        <AnimatePresence>
          {toasts.map((toast) => (
            <motion.div
              key={toast.id}
              initial={{ x: 400, opacity: 0 }}
              animate={{ x: 0, opacity: 1 }}
              exit={{ x: 400, opacity: 0 }}
              transition={{ type: 'spring', stiffness: 300, damping: 30 }}
              className={`px-4 py-3 rounded text-white shadow-lg ${bgColor[toast.type]}`}
            >
              {toast.message}
            </motion.div>
          ))}
        </AnimatePresence>
      </div>
    </>
  );
}
```

**Spec:**
- Enter: slide from right (x: 400 → 0), fade in, spring (snappy)
- Exit: slide right, fade out
- Auto-dismiss: 4000ms
- Stack: 8px gap via space-y-2

### Swipe-to-Dismiss

Add drag to the toast:

```tsx
<motion.div
  key={toast.id}
  initial={{ x: 400, opacity: 0 }}
  animate={{ x: 0, opacity: 1 }}
  exit={{ x: 400, opacity: 0 }}
  transition={{ type: 'spring', stiffness: 300, damping: 30 }}
  drag="x"
  dragConstraints={{ left: 0, right: 100 }}
  onDragEnd={(event, info) => {
    if (info.offset.x > 50) {
      setToasts((prev) => prev.filter((t) => t.id !== toast.id));
    }
  }}
  className={`px-4 py-3 rounded text-white shadow-lg cursor-grab ${bgColor[toast.type]}`}
>
  {toast.message}
</motion.div>
```

---

## 7. Modal & Dialog Animations

### Backdrop Fade + Content Scale

```tsx
'use client';

import { motion, AnimatePresence } from 'framer-motion';
import { useState } from 'react';

export function Modal() {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <>
      <button
        onClick={() => setIsOpen(true)}
        className="px-4 py-2 bg-blue-500 text-white rounded"
      >
        Open Modal
      </button>

      <AnimatePresence>
        {isOpen && (
          <>
            {/* Backdrop */}
            <motion.div
              initial={{ opacity: 0 }}
              animate={{ opacity: 1 }}
              exit={{ opacity: 0 }}
              transition={{ duration: 0.2 }}
              onClick={() => setIsOpen(false)}
              className="fixed inset-0 bg-black/50 z-40"
            />

            {/* Content */}
            <motion.div
              initial={{ opacity: 0, scale: 0.95, y: 20 }}
              animate={{ opacity: 1, scale: 1, y: 0 }}
              exit={{ opacity: 0, scale: 0.95, y: 20 }}
              transition={{ type: 'spring', stiffness: 300, damping: 30 }}
              className="fixed top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 bg-white rounded-lg p-6 shadow-xl z-50 w-96"
            >
              <h2 className="text-xl font-bold mb-4">Modal Title</h2>
              <p className="text-gray-600 mb-6">Modal content goes here.</p>
              <button
                onClick={() => setIsOpen(false)}
                className="px-4 py-2 bg-blue-500 text-white rounded"
              >
                Close
              </button>
            </motion.div>
          </>
        )}
      </AnimatePresence>
    </>
  );
}
```

**Spec:**
- Backdrop: fade (0 → 1), 200ms, dark overlay
- Content: scale 0.95 → 1 + fade + slide up, spring
- Exit: reversed immediately on close
- Backdrop dismissible via click

---

## 8. Skeleton Screens & Loading Choreography

### Shimmer Gradient

```tsx
'use client';

import { motion } from 'framer-motion';

const shimmerVariants = {
  animate: {
    backgroundPosition: ['200% 0', '-200% 0'],
  },
};

export function SkeletonLoader() {
  return (
    <div className="space-y-4">
      {/* Avatar skeleton */}
      <motion.div
        className="w-12 h-12 rounded-full bg-gradient-to-r from-gray-200 via-gray-100 to-gray-200 bg-[length:200%_100%]"
        variants={shimmerVariants}
        animate="animate"
        transition={{ duration: 1.5, repeat: Infinity }}
      />

      {/* Text skeleton lines */}
      {[1, 2, 3].map((i) => (
        <motion.div
          key={i}
          className="h-4 bg-gradient-to-r from-gray-200 via-gray-100 to-gray-200 bg-[length:200%_100%] rounded"
          style={{ width: i === 3 ? '70%' : '100%' }}
          variants={shimmerVariants}
          animate="animate"
          transition={{ duration: 1.5, repeat: Infinity, delay: i * 0.1 }}
        />
      ))}
    </div>
  );
}
```

### Progressive Content Reveal

```tsx
'use client';

import { motion, AnimatePresence } from 'framer-motion';
import { useState, useEffect } from 'react';

interface ContentItem {
  id: string;
  title: string;
  description: string;
}

export function ProgressiveLoader({ items }: { items: ContentItem[] }) {
  const [visibleCount, setVisibleCount] = useState(0);

  useEffect(() => {
    if (visibleCount < items.length) {
      const timeout = setTimeout(() => {
        setVisibleCount((prev) => prev + 1);
      }, 100); // 100ms stagger

      return () => clearTimeout(timeout);
    }
  }, [visibleCount, items.length]);

  return (
    <div className="space-y-4">
      <AnimatePresence mode="popLayout">
        {items.slice(0, visibleCount).map((item, idx) => (
          <motion.div
            key={item.id}
            initial={{ opacity: 0, y: 20 }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: -20 }}
            transition={{ type: 'spring', stiffness: 300, damping: 30 }}
            className="p-4 border rounded bg-white"
          >
            <h3 className="font-semibold">{item.title}</h3>
            <p className="text-gray-600">{item.description}</p>
          </motion.div>
        ))}
      </AnimatePresence>

      {visibleCount < items.length && (
        <motion.div
          className="text-center py-4"
          animate={{ opacity: [0.6, 1, 0.6] }}
          transition={{ duration: 1.5, repeat: Infinity }}
        >
          Loading...
        </motion.div>
      )}
    </div>
  );
}
```

---

## 9. Scroll-Triggered Animations

### Fade In on Scroll

```tsx
'use client';

import { motion } from 'framer-motion';

export function FadeOnScroll({ children }: { children: React.ReactNode }) {
  return (
    <motion.div
      initial={{ opacity: 0, y: 50 }}
      whileInView={{ opacity: 1, y: 0 }}
      viewport={{ once: true, amount: 0.2 }}
      transition={{ duration: 0.6, type: 'spring', stiffness: 100, damping: 20 }}
    >
      {children}
    </motion.div>
  );
}
```

### Scroll-Linked Progress Bar

```tsx
'use client';

import { useScroll, useTransform, motion } from 'framer-motion';

export function ScrollProgress() {
  const { scrollYProgress } = useScroll();
  const width = useTransform(scrollYProgress, [0, 1], ['0%', '100%']);

  return (
    <motion.div
      style={{ width }}
      className="fixed top-0 left-0 h-1 bg-blue-500 z-50"
    />
  );
}
```

---

## 10. Reduced Motion Accessibility

### Detecting prefers-reduced-motion

```tsx
'use client';

import { motion } from 'framer-motion';
import { useEffect, useState } from 'react';

export function useReducedMotion() {
  const [shouldReduce, setShouldReduce] = useState(false);

  useEffect(() => {
    // Check on mount
    const mediaQuery = window.matchMedia('(prefers-reduced-motion: reduce)');
    setShouldReduce(mediaQuery.matches);

    // Listen for changes
    const handleChange = (e: MediaQueryListEvent) => setShouldReduce(e.matches);
    mediaQuery.addEventListener('change', handleChange);

    return () => mediaQuery.removeEventListener('change', handleChange);
  }, []);

  return shouldReduce;
}

export function AnimatedButton() {
  const shouldReduce = useReducedMotion();

  return (
    <motion.button
      whileHover={shouldReduce ? {} : { scale: 1.05 }}
      whileTap={shouldReduce ? {} : { scale: 0.95 }}
      transition={
        shouldReduce ? { duration: 0 } : { type: 'spring', stiffness: 400, damping: 17 }
      }
      className="px-4 py-2 bg-blue-500 text-white rounded"
    >
      Click me
    </motion.button>
  );
}
```

### Alternative: Simplify Rather Than Remove

Sometimes simplifying motion is better than removing it entirely:

```tsx
const getTransition = (shouldReduce: boolean) => {
  if (shouldReduce) {
    // Instant, no spring, no easing—just opacity
    return { duration: 0.1, type: 'tween' };
  }
  return { type: 'spring', stiffness: 300, damping: 30 };
};

export function FadeInComponent() {
  const shouldReduce = useReducedMotion();

  return (
    <motion.div
      initial={{ opacity: 0 }}
      animate={{ opacity: 1 }}
      transition={getTransition(shouldReduce)}
    >
      Content
    </motion.div>
  );
}
```

---

## 11. Common Mistakes & Fixes

| Mistake | Fix |
|---------|-----|
| Animations too slow (>400ms) | Reduce duration. Use 200-300ms for interactions. |
| Animations too bouncy | Reduce spring damping (aim for 20-30). |
| Inconsistent easing | Use design tokens. All buttons use snappy, all modals use gentle. |
| Animating `width`, `height`, `position` | Use `layout` prop or `useTransform`. Only animate `transform`, `opacity`. |
| Ignoring `prefers-reduced-motion` | Always check. Remove/simplify springs, parallax, bounces. Keep opacity fades. |
| Missing `AnimatePresence` | Exit animations don't work without it. Always wrap conditionals. |
| Janky mobile animations | Reduce damping, lower duration. Test on real device. |

---

## 12. Performance Checklist

- Use GPU-composited properties only (transform, opacity)
- Don't animate layout properties (width, height, top, left)
- Use `will-change` CSS for complex animations
- Test on mobile/low-end devices (Chrome DevTools throttling)
- Profile with Chrome DevTools Animation panel
- Check FPS with DevTools Performance tab
- Limit simultaneous animations (avoid 50+ concurrent)
- Use `layout` animations for height changes (Framer optimizes these)

---

## 13. Animation Token System

```tsx
// animation-tokens.ts
export const micro = {
  buttonHover: {
    scale: 1.02,
    shadow: '0 10px 20px rgba(0,0,0,0.15)',
  },
  buttonActive: {
    scale: 0.98,
  },
  inputFocus: {
    borderColor: '#3b82f6',
    labelScale: 0.85,
    labelY: -24,
  },
  validationShake: {
    x: [0, -5, 5, -5, 5, 0],
    duration: 0.4,
  },
  toastSlide: {
    x: 400,
  },
};

export const choreography = {
  skeleton: {
    duration: 1.5,
    repeat: Infinity,
  },
  progressiveReveal: {
    stagger: 0.1,
    duration: 0.3,
  },
};
```

Use these tokens across the codebase for consistency and easy theme updates.
