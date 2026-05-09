---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[frontend-developer]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# Core Web Vitals Optimization (2025)

## The Three Metrics

### LCP (Largest Contentful Paint)
The render time of the largest visible element. Target: <2.5s

**Optimize by:**
- Prioritize critical resources (CSS, above-fold images)
- Reduce server response time
- Minimize CSS blocking rendering
- Lazy load below-the-fold images
- Use CDN for fast delivery

```html
<!-- Add fetchpriority for critical images -->
<img src="hero.jpg" fetchpriority="high" />

<!-- Preload critical fonts -->
<link rel="preload" href="font.woff2" as="font" type="font/woff2" crossorigin />
```

### CLS (Cumulative Layout Shift)
Unexpected layout changes during load. Target: <0.1

**Prevent by:**
- Always set image/video dimensions (width/height)
- Avoid late-loading CSS that changes layout
- Use size-container queries for flexible layouts
- Be careful with dynamic content insertion

```html
<!-- Good: Dimensions set -->
<img src="photo.jpg" width="640" height="480" />

<!-- Bad: No dimensions, will cause layout shift -->
<img src="photo.jpg" />
```

### FID (First Input Delay) / INP (Interaction to Next Paint)
Time from user interaction to browser responding. Target: <100ms (FID) or <200ms (INP)

**Optimize by:**
- Break long tasks (JS >50ms blocks main thread)
- Use requestIdleCallback for non-urgent work
- Defer non-critical JavaScript
- Consider web workers for heavy computation

```javascript
// Bad: Blocking main thread
function heavyComputation() {
  // 500ms of work
}

// Good: Break into chunks
function heavyComputationChunked() {
  if (moreWork) {
    requestIdleCallback(heavyComputationChunked);
  }
}
```

## Performance Budget

Set targets for each metric:

```json
{
  "bundles": [
    {
      "name": "main",
      "maxSize": "150kb"
    }
  ],
  "metrics": [
    {
      "name": "LCP",
      "maxValue": 2500
    },
    {
      "name": "CLS",
      "maxValue": 0.1
    },
    {
      "name": "INP",
      "maxValue": 200
    }
  ]
}
```

Enforce in CI/CD. Fail builds that exceed budgets.

## Image Optimization

### Format Selection
- **WebP**: Modern, 25-35% smaller than JPEG (use with fallback)
- **AVIF**: Newer, even better compression (use with fallback)
- **JPEG**: Universal, good for photos
- **PNG**: Lossless, good for graphics
- **SVG**: Vector, infinitely scalable

### Responsive Images
```html
<picture>
  <source srcset="hero-640w.webp" media="(max-width: 640px)" type="image/webp" />
  <source srcset="hero-640w.jpg" media="(max-width: 640px)" />
  <source srcset="hero-1280w.webp" media="(max-width: 1280px)" type="image/webp" />
  <source srcset="hero-1280w.jpg" media="(max-width: 1280px)" />
  <img src="hero-1280w.jpg" alt="Hero image" />
</picture>
```

### Lazy Loading
```html
<img src="below-fold.jpg" loading="lazy" />
```

## 2025 Best Practices

- **Use web.dev tools**: Chrome DevTools, Lighthouse, PageSpeed Insights
- **Monitor in production**: Real User Monitoring (RUM) with services like Vercel Analytics
- **Set realistic targets**: Desktop vs. mobile may have different budgets
- **Test on real devices**: Synthetic metrics ≠ real user experience
- **Continuous monitoring**: Core Web Vitals are always moving; monitor continuously
