---
type: reference
project: skills-library
scope: skill
plugin: marketing
parent: "[[seo-geo-strategist]]"
tags: [type/reference, plugin/marketing, scope/skill, topic/marketing]
status: active
---
# SEO Audit Framework

## Technical SEO Audit

### Core Web Vitals Assessment
| Metric | Threshold | Tool | Fix |
|--------|-----------|------|-----|
| Largest Contentful Paint (LCP) | <2.5s | PageSpeed Insights | Image optimization, lazy loading, server response time |
| First Input Delay (FID) | <100ms | PageSpeed Insights | Reduce JavaScript execution, defer non-critical JS |
| Cumulative Layout Shift (CLS) | <0.1 | PageSpeed Insights | Define image/video dimensions, avoid font swaps, fixed ads |

### Site Architecture
- [ ] Is there a clear URL hierarchy (example.com/category/topic/page)?
- [ ] Are URLs descriptive and keyword-relevant?
- [ ] Is site depth shallow? (Ideally <3 clicks from homepage)
- [ ] Are there orphan pages (no internal links pointing to them)?
- [ ] Is there a clear internal linking strategy?

### Crawlability & Indexation
- [ ] Can Google crawl all important pages? (Check GSC Coverage tab)
- [ ] Is robots.txt blocking important content?
- [ ] Are there unnecessary noindex tags?
- [ ] Is XML sitemap submitted and current?
- [ ] Are there crawl errors in GSC? (Fix 4xx and 5xx errors)

### Mobile & Responsive Design
- [ ] Is site fully responsive and tested on mobile?
- [ ] Are buttons and CTAs touch-friendly (48px minimum)?
- [ ] Is text readable on mobile (font size, line height)?
- [ ] Are forms mobile-optimized?

### HTTPS & Security
- [ ] Is entire site HTTPS (no mixed content)?
- [ ] Is SSL certificate valid and not expired?
- [ ] Are there no security warnings in browsers?

### Site Speed
- [ ] Is Time to First Byte (TTFB) <600ms?
- [ ] Are images optimized (WebP format, appropriate sizes)?
- [ ] Is JavaScript minified and deferred?
- [ ] Are third-party scripts (ads, analytics) deferred?
- [ ] Is server geolocation appropriate for audience?

---

## On-Page SEO Audit

### Title Tags & Meta Descriptions
| Element | Best Practice | Example |
|---------|----------------|---------|
| Title Tag | 50-60 characters, keyword-first, brand-last | "Best Yoga Mats for Beginners 2025 \| YogaPro" |
| Meta Description | 150-160 characters, compelling, call-to-action | "Find the perfect yoga mat for beginners. Non-slip, eco-friendly, under $50. Free shipping on orders over $75." |

- [ ] Does every page have a unique title tag?
- [ ] Does every page have a unique meta description?
- [ ] Are keywords naturally incorporated (not stuffed)?
- [ ] Do descriptions compel clicks (includes benefit or CTA)?

### Heading Structure
- [ ] Is there exactly one H1 per page?
- [ ] Do H1, H2, H3 form a logical hierarchy?
- [ ] Are headers descriptive and keyword-relevant?
- [ ] Do headers make sense as outline/table of contents?

### Content Quality
- [ ] Is content >1500 words for competitive terms? (2500-3500 for very competitive)
- [ ] Is content original and unique (not duplicate)?
- [ ] Does content answer the search query comprehensively?
- [ ] Is content well-structured with headers, bullets, and whitespace?
- [ ] Does content include relevant images and media?
- [ ] Is language clear, accessible, jargon-free?
- [ ] Does content demonstrate expertise (E-E-A-T)?

### Keyword Usage
- [ ] Is primary keyword in title, H1, and first 100 words?
- [ ] Are related keywords and semantic variations used naturally?
- [ ] Is keyword density 0.5-2.5% (not over-optimized)?
- [ ] Are LSI keywords (semantically related terms) included?

### Internal Linking
- [ ] Are internal links contextual (within content, not just navigation)?
- [ ] Do anchor texts describe the linked page?
- [ ] Are high-value pages linked to from multiple sources internally?
- [ ] Are low-value pages (thank you pages, etc.) linked to sparingly?

### Images & Media
- [ ] Are all images optimized for web (compressed, WebP)?
- [ ] Do all images have descriptive alt text?
- [ ] Are images responsive (different sizes for mobile/desktop)?
- [ ] Are images related to content (not decorative filler)?
- [ ] Is there a video if appropriate for topic?

### Schema Markup (Structured Data)
- [ ] Is there appropriate schema for page type (Article, Product, LocalBusiness)?
- [ ] Is JSON-LD markup valid (test with Schema.org testing tool)?
- [ ] Are rich snippets appearing in search results?

---

## Content Gap Analysis

### Keyword Research Audit
1. **Primary Keywords**: What are you ranking for? (GSC)
2. **Missing Keywords**: What are competitors ranking for? (Ahrefs, SEMrush)
3. **Keyword Difficulty**: Are you targeting feasible keywords (not impossible 100-difficulty terms)?
4. **Search Volume**: Are you targeting enough search volume (not ultra-niche)?
5. **Commercial Intent**: Are keywords aligned to business goals?

### Competitive Landscape
- [ ] Who are top 3 competitors in your space?
- [ ] What keywords do they rank for that you don't?
- [ ] What content do they have that's missing from your site?
- [ ] What's their backlink profile and quality?
- [ ] What's your relative domain authority vs. competitors?

### Topic Clustering Analysis
- [ ] Do you have clear topic pillars (broad topics)?
- [ ] Do you have supporting cluster content (detailed subtopics)?
- [ ] Are pillar and cluster pages internally linked?
- [ ] Is there topical overlap or cannibalization?

### Content Age & Freshness
- [ ] How old is your oldest content?
- [ ] When was each page last updated?
- [ ] Are there outdated stats or references?
- [ ] Should any content be refreshed or consolidated?

---

## Backlink Profile Audit

### Link Quality
- [ ] What's your Domain Authority (DA) vs. competitors?
- [ ] How many referring domains link to you?
- [ ] What % of backlinks are from high-authority (DA>50) domains?
- [ ] Are backlinks relevant (from related industries/topics)?
- [ ] Are there suspicious/spam links (PBNs, footer links, unrelated links)?

### Link Distribution
- [ ] Which pages have the most backlinks?
- [ ] Are all backlinks going to homepage, or spread across site?
- [ ] Are there internal pages that should have more links?

### Opportunities
- [ ] What topics are competitors linked to that you're not?
- [ ] Are there broken links on authority sites you could replace?
- [ ] Are there industry directories or resource pages you're missing?

---

## GSC (Google Search Console) Analysis

### Performance Metrics
- **Impressions**: How many times does your site appear in search results?
- **Clicks**: How many clicks did you get from search?
- **CTR**: Click-through rate (clicks/impressions). Benchmark: 2-5% depending on query type.
- **Avg. Position**: Average ranking position. Lower is better.

### Performance Trends
- [ ] Are impressions growing (expanding keyword coverage)?
- [ ] Are clicks growing (improving CTR or more impressions)?
- [ ] Are there keywords where position is improving (trending up)?
- [ ] Are there keywords/pages in decline (need attention)?

### Issues to Fix
- [ ] **Crawl Errors**: Fix server errors (5xx), client errors (4xx).
- [ ] **Mobile Usability**: Fix mobile rendering issues.
- [ ] **Core Web Vitals**: Identify pages failing LCP, FID, CLS.
- [ ] **Coverage**: Ensure important pages are indexed (check exclusions).
- [ ] **Indexed Pages**: Check if actual indexed count matches expected.

### Opportunities
- [ ] **Queries with low CTR**: Improve title/description (A/B test).
- [ ] **Queries with low position**: Improve content, add backlinks, refresh.
- [ ] **Queries declining**: Investigate; competitor moved ahead? Content quality declined?

---

## Audit Report Structure

### Executive Summary
- Top 3 strengths
- Top 3 opportunities
- Quick wins (implementable in <1 week)
- Long-term roadmap (implementable in 3-6 months)

### Technical SEO
- Core Web Vitals status
- Crawlability and indexation issues
- Mobile-friendliness assessment
- Site speed analysis

### On-Page Analysis
- Title and meta description quality
- Content depth and quality
- Internal linking structure
- Schema markup audit

### Content Gaps
- Keyword analysis (current vs. opportunity)
- Topic clustering review
- Content freshness assessment
- Competitive content gaps

### Link Profile
- Domain Authority and benchmark vs. competitors
- Backlink quality and distribution
- Link building opportunities

### GSC Insights
- Traffic trends
- Keyword performance (impressions, CTR, position)
- Issues and quick fixes

### Recommendations
- Priority fixes (quick wins)
- Content strategy (topic clusters, new content)
- Technical improvements (Core Web Vitals, crawlability)
- Link building strategy
- Timeline and resource estimates

---

## Audit Tools

| Tool | Purpose |
|------|---------|
| **Google Search Console** | Monitor search performance, indexation, crawl errors |
| **Google PageSpeed Insights** | Test Core Web Vitals and page speed |
| **Ahrefs** | Backlink analysis, keyword research, competitor analysis |
| **SEMrush** | Keyword research, rank tracking, technical audit |
| **Screaming Frog** | Site crawl, find technical issues, XML sitemap review |
| **Schema.org Testing Tool** | Validate structured data (JSON-LD) |
| **Mobile-Friendly Test** | Test mobile usability |
| **GTmetrix** | Detailed page speed analysis |

---

## Common Issues & Quick Fixes

| Issue | Cause | Fix |
|-------|-------|-----|
| Low CTR | Weak title/description | Rewrite with keyword and benefit |
| Crawl errors | Broken links, server errors | Fix links, investigate 5xx errors in GSC |
| Slow LCP | Large image, slow server | Optimize image, improve TTFB |
| Layout shift (CLS) | Images/ads without fixed dimensions | Define width/height, avoid late-loaded ads |
| Not ranking | Thin content, no backlinks | Expand content (2500+ words), build links |
| Dropped rankings | Outdated content, competitor surpassed | Refresh content, build more links |
| Mixed HTTP/HTTPS | Partial HTTPS setup | Force HTTPS, redirect all HTTP to HTTPS |
| Duplicate content | Multiple URLs with same content | Use canonical tag, redirect duplicates |
