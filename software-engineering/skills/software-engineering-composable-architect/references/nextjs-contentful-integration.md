---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[composable-architect]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# Next.js & Contentful Integration Patterns

## Fetching Content from Contentful

### Contentful GraphQL Setup
```typescript
// lib/contentful.ts
const client = new ApolloClient({
  ssrMode: typeof window === 'undefined',
  link: new HttpLink({
    uri: `https://graphql.contentful.com/content/v1/spaces/${SPACE_ID}`,
    credentials: 'include',
    headers: {
      Authorization: `Bearer ${ACCESS_TOKEN}`,
    },
  }),
  cache: new InMemoryCache(),
})

export default client
```

### Static Generation with ISR
```typescript
// pages/articles/[slug].tsx
import { GetStaticProps, GetStaticPaths } from 'next'
import { ApolloClient } from '@apollo/client'

export const getStaticPaths: GetStaticPaths = async () => {
  const { data } = await client.query({
    query: GET_ALL_ARTICLE_SLUGS,
  })

  return {
    paths: data.articleCollection.items.map((article) => ({
      params: { slug: article.slug },
    })),
    fallback: 'blocking', // SSR for new articles
  }
}

export const getStaticProps: GetStaticProps = async ({ params }) => {
  const { data } = await client.query({
    query: GET_ARTICLE_BY_SLUG,
    variables: { slug: params?.slug },
  })

  return {
    props: { article: data.articleCollection.items[0] },
    revalidate: 3600, // ISR: revalidate every hour
  }
}

export default function Article({ article }) {
  return <ArticleDetail {...article} />
}
```

### Server-Side Rendering for Personalization
```typescript
// pages/articles/[slug].tsx
export const getServerSideProps: GetServerSideProps = async ({
  params,
  req,
}) => {
  // Get user segment from session/cookie
  const userSegment = req.cookies.segment || 'default'

  const { data } = await client.query({
    query: GET_ARTICLE_WITH_RECOMMENDATIONS,
    variables: {
      slug: params?.slug,
      segment: userSegment,
    },
  })

  return {
    props: {
      article: data.articleCollection.items[0],
      recommendations: data.recommendedArticles,
    },
  }
}
```

## Content Streaming with Suspense

```typescript
// app/articles/[slug]/page.tsx
import { Suspense } from 'react'

async function ArticleContent({ slug }) {
  const article = await fetchArticle(slug)
  return <ArticleBody {...article} />
}

async function RelatedArticles({ slug }) {
  const related = await fetchRelatedArticles(slug)
  return <ArticleGrid articles={related} />
}

export default function ArticlePage({ params }) {
  return (
    <article>
      <Suspense fallback={<ArticleSkeleton />}>
        <ArticleContent slug={params.slug} />
      </Suspense>

      <section>
        <h2>Related Articles</h2>
        <Suspense fallback={<GridSkeleton count={3} />}>
          <RelatedArticles slug={params.slug} />
        </Suspense>
      </section>
    </article>
  )
}
```

## Webhook-Triggered Rebuilds

### Contentful Webhook Configuration
```json
{
  "name": "Vercel Rebuild",
  "url": "https://api.vercel.com/v1/integrations/deploy/...",
  "triggers": ["Entry.publish", "Entry.unpublish", "Entry.delete"],
  "headers": {
    "Authorization": "Bearer YOUR_VERCEL_TOKEN"
  }
}
```

### API Route for Webhook Handling
```typescript
// pages/api/webhooks/contentful.ts
import { NextApiRequest, NextApiResponse } from 'next'

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  if (req.method !== 'POST') {
    return res.status(405).json({ error: 'Method not allowed' })
  }

  // Verify webhook authenticity
  const signature = req.headers['x-contentful-content-management-webhook-signature']
  if (!verifySignature(req.body, signature)) {
    return res.status(401).json({ error: 'Unauthorized' })
  }

  const { sys } = req.body
  const contentType = sys.contentType?.sys?.id

  // Trigger targeted revalidation
  try {
    if (contentType === 'article') {
      const slug = req.body.fields.slug['en-US']
      await res.revalidate(`/articles/${slug}`)
    } else if (contentType === 'homepage') {
      await res.revalidate('/')
    }

    return res.json({ revalidated: true })
  } catch (err) {
    return res.status(500).json({ error: 'Revalidation failed' })
  }
}
```

## API Routes for Custom Logic

### Contentful + Third-Party Integration
```typescript
// pages/api/search.ts
import { NextApiRequest, NextApiResponse } from 'next'
import { searchIndex } from 'algoliasearch'

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  const { q } = req.query

  try {
    // Search Algolia
    const results = await searchIndex.search(q)

    // Enrich with Contentful metadata
    const enriched = await Promise.all(
      results.hits.map(async (hit) => {
        const fullContent = await fetchFromContentful(hit.id)
        return { ...hit, ...fullContent }
      })
    )

    return res.json({ results: enriched })
  } catch (error) {
    return res.status(500).json({ error: 'Search failed' })
  }
}
```

## Middleware for Personalization

```typescript
// middleware.ts
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

export function middleware(request: NextRequest) {
  // Get user segment from cookie or Braze
  const segment = request.cookies.get('segment')?.value || 'default'

  // Clone request and add custom header
  const requestHeaders = new Headers(request.headers)
  requestHeaders.set('x-user-segment', segment)

  return NextResponse.next({
    request: {
      headers: requestHeaders,
    },
  })
}

export const config = {
  matcher: ['/articles/:path*', '/products/:path*'],
}
```

## Image Optimization from Contentful

```typescript
// components/ArticleImage.tsx
import Image from 'next/image'

interface ArticleImageProps {
  src: string
  alt: string
  width: number
  height: number
}

export default function ArticleImage({
  src,
  alt,
  width,
  height,
}: ArticleImageProps) {
  // Use Contentful Image API for transformations
  const optimizedUrl = `${src}?w=1200&h=630&fit=fill&fm=jpg&q=75`

  return (
    <Image
      src={optimizedUrl}
      alt={alt}
      width={width}
      height={height}
      responsive
      priority={false}
    />
  )
}
```

## Draft Preview with Contentful

```typescript
// pages/api/preview.ts
import { NextApiRequest, NextApiResponse } from 'next'

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  // Verify preview token
  if (req.query.token !== process.env.CONTENTFUL_PREVIEW_TOKEN) {
    return res.status(401).json({ error: 'Invalid token' })
  }

  // Enable preview mode
  res.setPreviewData({})

  // Redirect to preview URL
  const slug = req.query.slug
  res.redirect(`/articles/${slug}`)
}
```

```typescript
// pages/articles/[slug].tsx
import { GetStaticProps } from 'next'

export const getStaticProps: GetStaticProps = async ({
  params,
  preview,
}) => {
  // Use preview API if in preview mode
  const accessToken = preview
    ? process.env.CONTENTFUL_PREVIEW_TOKEN
    : process.env.CONTENTFUL_ACCESS_TOKEN

  const { data } = await client.query({
    query: GET_ARTICLE,
    variables: { slug: params?.slug },
    context: {
      headers: {
        Authorization: `Bearer ${accessToken}`,
      },
    },
  })

  return {
    props: { article: data.articleCollection.items[0], preview },
    revalidate: preview ? 0 : 3600, // No caching in preview
  }
}

export default function Article({ article, preview }) {
  return (
    <>
      {preview && <PreviewBanner onExit={() => router.push('/api/exit-preview')} />}
      <ArticleDetail {...article} />
    </>
  )
}
```

## Caching Strategy

### ISR Cache Invalidation
```typescript
// pages/api/revalidate.ts
export default async function handler(req, res) {
  // Verify webhook signature
  if (!verifySignature(req)) {
    return res.status(401).json({ error: 'Invalid signature' })
  }

  try {
    // Revalidate static page
    await res.revalidate('/articles/[slug]')

    // Revalidate parent page
    await res.revalidate('/articles')

    // Revalidate homepage if featured
    const article = req.body.fields
    if (article.featured) {
      await res.revalidate('/')
    }

    return res.json({ revalidated: true })
  } catch (err) {
    return res.status(500).json({ error: 'Revalidation failed' })
  }
}
```

### Cache Headers
```typescript
// middleware.ts
import { NextResponse } from 'next/server'

export function middleware(request) {
  const response = NextResponse.next()

  // Cache static assets
  if (request.nextUrl.pathname.match(/\.(jpg|jpeg|png|gif|svg|woff2)$/)) {
    response.headers.set('Cache-Control', 'public, max-age=31536000, immutable')
  }

  // Cache API responses
  if (request.nextUrl.pathname.startsWith('/api/')) {
    response.headers.set('Cache-Control', 'public, max-age=60, s-maxage=600')
  }

  return response
}
```

## Error Handling & Fallbacks

```typescript
// pages/articles/[slug].tsx
async function fetchArticle(slug) {
  try {
    const response = await fetch(
      `https://graphql.contentful.com/content/v1/spaces/${SPACE_ID}`,
      {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          Authorization: `Bearer ${ACCESS_TOKEN}`,
        },
        body: JSON.stringify({ query: GET_ARTICLE, variables: { slug } }),
      }
    )

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`)
    }

    return response.json()
  } catch (error) {
    console.error('Failed to fetch article:', error)

    // Return cached version if available
    return getCachedArticle(slug) || null
  }
}

export const getStaticProps: GetStaticProps = async ({ params }) => {
  const article = await fetchArticle(params?.slug)

  if (!article) {
    return { notFound: true, revalidate: 60 }
  }

  return {
    props: { article },
    revalidate: 3600,
  }
}
```
