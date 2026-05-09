---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[composable-architect]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# Performance & Observability for Composable Platforms

## Web Core Vitals & Monitoring

### Three Key Metrics (INP replacing FID in 2024)

**Largest Contentful Paint (LCP)** - Loading speed
- Good: < 2.5 seconds
- Needs improvement: 2.5-4 seconds
- Poor: > 4 seconds

**Interaction to Next Paint (INP)** - Responsiveness
- Good: < 200ms
- Needs improvement: 200-500ms
- Poor: > 500ms

**Cumulative Layout Shift (CLS)** - Visual stability
- Good: < 0.1
- Needs improvement: 0.1-0.25
- Poor: > 0.25

### Monitoring with Vercel
```typescript
// app/layout.tsx
import { Analytics } from '@vercel/analytics/react'
import { SpeedInsights } from '@vercel/speed-insights/next'

export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        {children}
        <Analytics />
        <SpeedInsights />
      </body>
    </html>
  )
}
```

### Custom Core Web Vitals
```typescript
// lib/web-vitals.ts
import { getCLS, getFCP, getFID, getLCP, getTTFB } from 'web-vitals'

function sendToAnalytics(metric) {
  // Send to your analytics service
  fetch('/api/analytics', {
    method: 'POST',
    body: JSON.stringify(metric),
    keepalive: true,
  })
}

getCLS(sendToAnalytics)
getFCP(sendToAnalytics)
getFID(sendToAnalytics)
getLCP(sendToAnalytics)
getTTFB(sendToAnalytics)
```

## Content Delivery Optimization

### CDN Cache Strategy
```typescript
// middleware.ts
import { NextResponse } from 'next/server'

export function middleware(request) {
  const response = NextResponse.next()
  const pathname = request.nextUrl.pathname

  // Static assets: cache forever
  if (pathname.match(/\.(jpg|jpeg|png|gif|webp|svg|woff2|ttf)$/)) {
    response.headers.set('Cache-Control', 'public, max-age=31536000, immutable')
  }

  // HTML pages: short cache with ISR
  if (pathname.match(/\.html$|^\/[^.]*$/)) {
    response.headers.set('Cache-Control', 'public, s-maxage=60, stale-while-revalidate=3600')
  }

  // API responses: moderate cache
  if (pathname.startsWith('/api/')) {
    response.headers.set('Cache-Control', 'public, s-maxage=300, stale-while-revalidate=600')
  }

  // Dynamic content from CMS: no cache
  if (pathname.startsWith('/preview')) {
    response.headers.set('Cache-Control', 'no-cache, no-store, must-revalidate')
  }

  return response
}
```

### Image Optimization
```typescript
// next.config.js
module.exports = {
  images: {
    domains: ['images.ctfassets.net'], // Contentful domain
    deviceSizes: [640, 750, 828, 1080, 1200, 1920, 2048, 3840],
    imageSizes: [16, 32, 48, 64, 96, 128, 256, 384],
    formats: ['image/webp', 'image/avif'],
    minimumCacheTTL: 31536000,
  },
}
```

### Font Loading Optimization
```typescript
// app/layout.tsx
import { Rubik } from 'next/font/google'

const rubik = Rubik({
  subsets: ['latin'],
  display: 'swap',
  variable: '--font-rubik',
  preload: true,
})

export default function RootLayout({ children }) {
  return (
    <html className={rubik.variable}>
      <body>{children}</body>
    </html>
  )
}
```

## Database & API Performance

### Query Optimization
```typescript
// lib/contentful.ts
import { gql } from '@apollo/client'

// Bad: Fetching too many fields
const BAD_QUERY = gql`
  query {
    articleCollection {
      items {
        title
        slug
        body
        author { /* deeply nested */ }
        relatedArticles { /* nested collection */ }
        comments { /* large collection */ }
      }
    }
  }
`

// Good: Selective fields, flat relationships
const GOOD_QUERY = gql`
  query {
    articleCollection(limit: 10) {
      items {
        title
        slug
        featuredImageUrl
        authorId
        publishDate
      }
    }
  }
`

// Better: Fragment-based, reusable queries
const ARTICLE_BASIC_FRAGMENT = gql`
  fragment ArticleBasic on Article {
    title
    slug
    publishDate
  }
`

const ARTICLE_DETAIL_FRAGMENT = gql`
  fragment ArticleDetail on Article {
    ...ArticleBasic
    body
    featuredImage { url }
  }
`
```

### API Route Caching
```typescript
// pages/api/articles.ts
export default async function handler(req, res) {
  // Set cache headers
  res.setHeader('Cache-Control', 'public, s-maxage=600, stale-while-revalidate=3600')

  // Fetch from Contentful
  const articles = await fetchArticles()

  res.status(200).json(articles)
}
```

### Batch Requests
```typescript
// lib/contentful.ts
async function fetchArticlesWithAuthors(articleIds: string[]) {
  // Batch fetch: single query for articles + authors
  const { data } = await client.query({
    query: gql`
      query GetArticlesWithAuthors($ids: [String!]!) {
        articleCollection(where: { sys: { id_in: $ids } }) {
          items {
            id
            title
            author {
              id
              name
            }
          }
        }
      }
    `,
    variables: { ids: articleIds },
  })

  return data.articleCollection.items
}
```

## Edge Functions for Performance

### Redirect & Rewrite at the Edge
```typescript
// middleware.ts
import { NextResponse } from 'next/server'

export function middleware(request) {
  const { pathname, searchParams } = request.nextUrl

  // A/B test at edge: no network latency
  if (pathname === '/product') {
    const variant = request.cookies.get('ab-variant')?.value || 'A'

    if (variant === 'B') {
      return NextResponse.rewrite(new URL('/product-b', request.url))
    }
  }

  // Redirect old URLs instantly
  if (pathname.startsWith('/old-')) {
    const newPath = pathname.replace('/old-', '/')
    return NextResponse.redirect(new URL(newPath, request.url))
  }

  return NextResponse.next()
}
```

### Dynamic Content at the Edge
```typescript
// pages/api/edge-fetch.ts
export const config = {
  runtime: 'edge',
}

export default async function handler(request) {
  // This runs on Vercel Edge Network (no cold starts)
  const response = await fetch('https://api.contentful.com/...')
  return response
}
```

## Monitoring & Alerting

### Error Tracking
```typescript
// lib/sentry.ts
import * as Sentry from '@sentry/nextjs'

Sentry.init({
  dsn: process.env.NEXT_PUBLIC_SENTRY_DSN,
  tracesSampleRate: 0.1,
  environment: process.env.NODE_ENV,
})

export function captureException(error: Error, context?: Record<string, any>) {
  Sentry.captureException(error, { extra: context })
}
```

### Custom Metrics
```typescript
// lib/metrics.ts
export async function trackEvent(
  event: string,
  properties: Record<string, any>
) {
  await fetch('/api/events', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      event,
      timestamp: new Date().toISOString(),
      ...properties,
    }),
  })
}

// Usage
trackEvent('article_view', {
  articleId: 'abc123',
  authorId: 'user456',
  readTime: 5,
})
```

### Logging Strategy
```typescript
// lib/logger.ts
const isDev = process.env.NODE_ENV === 'development'

export const logger = {
  info: (message: string, data?: any) => {
    console.log(`[INFO] ${message}`, data)
  },
  error: (message: string, error: Error, context?: any) => {
    console.error(`[ERROR] ${message}`, error, context)
    captureException(error, context)
  },
  warn: (message: string, data?: any) => {
    console.warn(`[WARN] ${message}`, data)
  },
  debug: (message: string, data?: any) => {
    if (isDev) console.debug(`[DEBUG] ${message}`, data)
  },
}
```

## Performance Budgets

### Define Budgets
```json
{
  "bundles": [
    {
      "name": "main",
      "maxAssetSize": "150kb"
    },
    {
      "name": "article-page",
      "maxAssetSize": "100kb"
    }
  ],
  "metrics": [
    {
      "name": "LCP",
      "limit": "2.5s"
    },
    {
      "name": "FCP",
      "limit": "1.2s"
    },
    {
      "name": "CLS",
      "limit": "0.1"
    }
  ]
}
```

### Bundle Analysis
```typescript
// next.config.js
module.exports = {
  webpack: (config, { isServer }) => {
    if (!isServer) {
      config.plugins.push(
        new BundleAnalyzerPlugin({
          analyzerMode: 'disabled',
          generateStatsFile: true,
          openAnalyzer: false,
        })
      )
    }
    return config
  },
}
```

## Performance Checklist

**Content Delivery**
- [ ] CDN caching configured for all asset types
- [ ] Images optimized and served via CDN
- [ ] Fonts pre-loaded and optimized
- [ ] HTML compressed and minified

**Code Quality**
- [ ] Bundle size monitored and within budget
- [ ] Lazy loading for non-critical components
- [ ] Code splitting at route level
- [ ] Unused CSS and JavaScript removed

**API Optimization**
- [ ] GraphQL queries optimized (no over-fetching)
- [ ] Response times monitored (target < 200ms)
- [ ] Rate limiting and throttling in place
- [ ] Caching headers configured

**Monitoring**
- [ ] Core Web Vitals tracked
- [ ] Error tracking enabled
- [ ] Custom metrics logged
- [ ] Alerts configured for anomalies

**User Experience**
- [ ] LCP < 2.5 seconds
- [ ] INP < 200ms
- [ ] CLS < 0.1
- [ ] Time to Interactive < 3 seconds
