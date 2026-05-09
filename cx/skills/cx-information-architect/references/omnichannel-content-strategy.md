---
type: reference
project: skills-library
scope: skill
plugin: cx
parent: "[[information-architect]]"
tags: [type/reference, plugin/cx, scope/skill, topic/customer-experience]
status: active
---
# Omnichannel Content Strategy & Delivery

## Core Principles

### Single Source of Truth
- Author content once in the CMS
- Multiple channels consume the same data (web, mobile, email, API, search)
- No duplication or channel-specific silos
- Content updates propagate everywhere automatically

### Channel-Agnostic Modeling
Structure content independent of where it will be displayed:
- Don't model "Blog Post Page" (layout-specific)
- Model "Blog Post" (content-focused) that can appear on:
  - Blog archive page
  - Homepage featured section
  - Email newsletter
  - Mobile app
  - Search results
  - Voice assistant

### Flexible Composition
- Core content (topics) remains stable
- Different channels arrange and present content differently (assemblies)
- Same product description works in catalog, email, and mobile app

## Channel Distribution Map

### Web (Desktop & Responsive)
- Full article with rich text formatting
- Images at multiple resolutions
- Interactive elements (comments, related content, search)
- SEO-optimized metadata

### Mobile App
- Optimized for small screens
- Images at mobile resolution
- Simplified navigation
- Fast loading time

### Email
- Plain text or light HTML
- Images inline (mobile-optimized)
- CTA buttons prominent
- Content truncated to 3-4 paragraphs

### Search Engines
- Title and meta description (50-160 chars)
- Keywords and topics
- Schema.org structured data
- Canonical URL

### Voice/Smart Speakers
- Condensed summary (spoken format)
- Q&A format content
- Clear CTAs
- Conversational language

## Content Model for Omnichannel

### Core Content Type: Article
```
Fields:
- title (Short Text, 50 chars ideal for search)
- slug (URL-friendly identifier)
- summary (Short Text, 120 chars for email/search)
- body (Rich Text with embedded assets)
- featured_image (Asset)
- author (Reference)
- publish_date
- category (Enum)
- tags (References)
- seo_metadata (JSON)
```

### SEO Metadata (Structure for Search)
```json
{
  "meta_title": "Engaging article title (50-60 chars)",
  "meta_description": "Compelling preview of article (150-160 chars)",
  "keywords": "topic, keywords, phrases",
  "og_image": "asset_reference",
  "schema_type": "Article",
  "canonical_url": "https://example.com/article"
}
```

### Email-Specific Variant
```
Fields:
- email_subject (50-60 chars, optimized for inbox)
- email_preview_text (brief preview text)
- email_cta_text (call-to-action wording)
- email_plain_text (fallback for email clients)
- email_send_date (scheduled)
```
Better approach: Use same Article content; transform in email template.

## Transformation & Presentation Layer

### Don't Model for Channels; Transform Content

Bad approach: Separate models for web, mobile, email
```
❌ ArticleWeb
❌ ArticleMobile
❌ ArticleEmail
```

Better approach: One model; transform at presentation layer
```
Article (single model)
  ↓
Web Template → Rendered HTML
  ↓
Mobile Template → JSON API response
  ↓
Email Template → HTML email
  ↓
Search Template → JSON-LD schema
```

### Transformation Examples

**Web to Mobile**
```
- Full body rich text
- Featured image (desktop)
→ Limit to 3 paragraphs on mobile
→ Mobile-optimized image size
```

**Web to Email**
```
- Full article with rich formatting
→ Simplified HTML
→ Plain text version
→ Reduce to 2-3 key paragraphs + CTA
```

**Web to Search**
```
- Full article data
→ Extract title, summary, published date
→ Generate schema.org JSON-LD
→ Format for search engine crawlers
```

## Asset Management for Multi-Channel

### Image Strategy
Store one high-quality master image; Contentful API transforms:
- Desktop: 1200px width, 75% quality
- Mobile: 640px width, 70% quality
- Thumbnail: 300px width, 80% quality
- Email: 600px width (email client limitation), 80% quality

### Responsive Images
```
<img
  src="image.jpg?w=1200&q=75"
  srcset="image.jpg?w=640&q=75 640w,
          image.jpg?w=1200&q=75 1200w"
  alt="Descriptive alt text"
/>
```

### Video & Rich Media
- Store video URL (YouTube, Vimeo)
- Generate thumbnail for email
- Embed conditionally (desktop/mobile, not email)
- Provide transcript for accessibility

## Localization for Omnichannel

### Translatable vs. Not Translatable
```
Article:
✓ Translatable:
  - title
  - body
  - summary
  - tags (language-specific tags)

✗ Not Translatable:
  - slug (canonical identifier)
  - created_date
  - author (person reference)
  - featured_image (same for all languages)
```

### Language-Specific Channels
```
English: en-US, en-GB
Spanish: es-MX, es-ES
French: fr-FR, fr-CA
```
Same content; different language/regional variants.

### Fallback Strategy
If content isn't translated:
1. Use default language version
2. Show translator notice to user
3. Prioritize content translation (high-traffic pages first)

## Performance Optimization

### Efficient Content Queries
```graphql
# Bad: Overly nested queries
query {
  articles {
    author {
      profile {
        social {
          twitter
        }
      }
    }
  }
}

# Good: Flat, selective queries
query {
  articles {
    id
    title
    authorId
  }
}
```

### Caching Strategy
- Cache immutable content (published articles)
- Invalidate on publish
- Cache-busting headers for CDN
- Stale-while-revalidate for mobile apps

### API Efficiency
- Return only needed fields (GraphQL fragments)
- Pagination for large collections
- Filtering and searching server-side
- Webhook updates instead of polling

## Governance Across Channels

### Content Quality Standards
```
Before Publishing:
- Title: 50-60 characters
- Summary: 120-150 characters
- Body: Rich text with images
- Featured image: Hi-res (2000px+)
- Author: Required
- SEO metadata: Complete
- Proofread and spell-check
```

### Approval Workflow
```
Draft → Review (editor checks)
      → Feedback (if needed)
      → Approved (ready to publish)
      → Published (all channels)
      → Monitoring (performance tracking)
```

### Multi-Channel Checklists
- Web: responsive, accessible, SEO-optimized
- Mobile: fast-loading, touch-friendly, optimized images
- Email: HTML + plain text, working links, unsubscribe footer
- Search: schema markup, meta tags, canonical URL
- Voice: Q&A format, natural language, concise

## Example: Article Delivery Across Channels

### Content Entry
```json
{
  "id": "article-123",
  "title": "10 Ways to Improve Your Workflow",
  "slug": "improve-workflow",
  "summary": "Learn 10 practical strategies to streamline your work...",
  "body": "[Rich text with embedded images and links]",
  "featured_image": "workflow-tips-hero.jpg",
  "author": "Sarah Chen",
  "publish_date": "2025-04-01",
  "category": "Productivity",
  "tags": ["workflow", "productivity", "tips"],
  "seo_metadata": {
    "meta_title": "10 Ways to Improve Your Workflow | Blog",
    "meta_description": "Discover 10 practical strategies...",
    "og_image": "workflow-tips-hero.jpg"
  }
}
```

### Web Rendering
- Full title, body, featured image
- Rich text formatting preserved
- Related articles recommended
- Author bio linked
- SEO meta tags in HTML head

### Mobile App
- Optimized title and summary
- Featured image mobile-sized
- Body truncated to first 3 paragraphs
- "Read More" link to full article
- Lightweight JSON payload

### Email Newsletter
- Subject line from email_subject field
- Featured image (600px)
- Summary + first paragraph
- "Read Full Article" CTA
- Unsubscribe footer

### Search Engine
```json-ld
{
  "@context": "https://schema.org",
  "@type": "BlogPosting",
  "headline": "10 Ways to Improve Your Workflow",
  "description": "Learn 10 practical strategies to streamline...",
  "image": "https://example.com/workflow-tips-hero.jpg",
  "datePublished": "2025-04-01",
  "author": "Sarah Chen"
}
```

### Voice Assistant
```
Title: Ten ways to improve your workflow
Summary: Learn practical strategies to streamline your work.
Q: How can I improve my workflow?
A: We have ten ways. First, organize your workspace...
```
