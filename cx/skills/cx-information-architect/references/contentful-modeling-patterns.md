---
type: reference
project: skills-library
scope: skill
plugin: cx
parent: "[[information-architect]]"
tags: [type/reference, plugin/cx, scope/skill, topic/customer-experience]
status: active
---
# Contentful Content Modeling Patterns & Best Practices

## Content Type Basics

### Field Types & When to Use Each

**Text Fields**
- Short Text: Headlines, labels, product names (max ~255 chars)
- Long Text: Descriptions, abstracts, alt text
- Rich Text: Body content with formatting, links, embedded assets

**Structured Fields**
- Integer/Decimal: Prices, quantities, ratings
- Boolean: Feature flags, availability status
- Enum (Dropdown): Fixed categories (status, type, priority)
- JSON Object: Complex, structured data (seo metadata, feature specs)

**Content Relationships**
- Reference (single): One-to-one relationships (article → featured image)
- Reference (multi): One-to-many or many-to-many (article → tags, authors)
- Rich Text with Inline References: Linking content within body text

**Media**
- Asset Link: Images, videos, documents
- Multiple Assets: Gallery, media collections

### Field Visibility & Control

Make fields editor-friendly:
- **Required**: Use sparingly (only for critical data)
- **Default Values**: Pre-fill common values to reduce author burden
- **Help Text**: Explain purpose and expected content format
- **Validation**: Regex, min/max length, enum restrictions
- **Appearance**: Dropdown, multi-select, rich text, JSON editor

## Reusable Component Patterns

### Hero Component
```
Content Type: Hero (reusable component)
Fields:
- heading (Short Text, required)
- description (Rich Text, optional)
- cta_text (Short Text)
- cta_url (Short Text)
- background_image (Asset Link)
- background_color (Text, optional - hex color)
Usage: Landing pages, article headers, campaign pages
```

### Rich Text with Embedded Assets
```
Content Type: Article (topic)
Fields:
- title (Short Text)
- body (Rich Text with embedded assets/hyperlinks)
  - Supports images, videos, code blocks inline
  - Links to other articles, products, authors
- featured_image (Asset Link)
- tags (Reference multi → Tag type)
```

### Product with Variants
```
Content Type: Product
Fields:
- name (Short Text)
- description (Rich Text)
- variants (Reference multi → ProductVariant)

Content Type: ProductVariant
Fields:
- product_reference (Reference → Product)
- sku (Short Text)
- size (Enum: XS, S, M, L, XL)
- color (Enum: Red, Blue, Green)
- price (Decimal)
- stock (Integer)
- images (Multiple Assets)
```

### FAQ Component
```
Content Type: FAQ
Fields:
- question (Short Text)
- answer (Rich Text)
- category (Enum or Reference → FAQCategory)
- related_articles (Reference multi → Article)

Content Type: FAQCategory
Fields:
- name (Short Text)
- description (Short Text)
- sort_order (Integer)
```

## Topics vs. Assemblies

### Topics (Core Content)
Content that stands alone and is reusable:
- Article, Product, Person, Event, Blog Post
- Contains primary information (what, who, when, why)
- Can be queried, filtered, and sorted independently
- No layout information

### Assemblies (Presentation/Layout)
Container content that composes Topics:
- Landing Page, Homepage, Product Detail Page
- Contains layout and arrangement decisions
- Primarily assembled from Topic references
- Can include layout-specific fields (hero background, sidebar)

### Example: News Article vs. Homepage

**Topic: Article**
```
- headline
- body_text
- featured_image
- author (Reference)
- publish_date
- tags (References)
```

**Assembly: Homepage (uses Topics)**
```
- featured_article (Reference → Article)
- recent_articles (Reference multi → Article, limit 5)
- hero_section (embedded Hero component)
- sidebar_content (free content)
```

## Relationship Patterns

### One-to-One
- Article has one featured image
- Product has one primary category

```
Article → featured_image (Reference, single)
```

### One-to-Many
- Blog has many articles
- Product has many reviews

```
Blog → articles (Reference multi, linked from Article type)
// OR
Article → blog (Reference single back-link)
```

### Many-to-Many
- Articles have multiple authors
- Products have multiple categories
- Articles have multiple tags

```
Article → authors (Reference multi → Author)
Article → tags (Reference multi → Tag)
```

### Circular References (use with care)
- Author has written articles (Article → Author)
- Author profile shows recent articles (Author → Articles)

Solution: Use back-links or query both directions in your API.

## Validation Strategies

### Field-Level Validation
```json
{
  "title": {
    "type": "Text",
    "required": true,
    "validations": {
      "size": { "max": 100 }
    }
  },
  "slug": {
    "type": "Text",
    "validations": {
      "regexp": { "pattern": "^[a-z0-9-]+$" }
    }
  },
  "price": {
    "type": "Decimal",
    "validations": {
      "range": { "min": 0 }
    }
  },
  "status": {
    "type": "Text",
    "validations": {
      "in": ["draft", "published", "archived"]
    }
  }
}
```

### Conditional Visibility
Use JSON fields to show/hide fields based on other content:
- If product type is "digital", hide inventory field
- If article status is "draft", hide publish_date

### Linked Content Validation
- Required reference (article must have author)
- Reference must be of specific type (only Article content type in landing page)
- Limit reference count (max 10 related items)

## Scalability Patterns

### Flat Hierarchy (Preferred)
```
Product → Variant (references, not nested)
Article → Comments (flat collection, not nested in article)
```
Benefits: Easier to query, update, delete individual items; scales to 100,000s entries.

### Limited Nesting (Use Sparingly)
```
ArticleBody (rich text) → inline embedded assets
```
Benefits: Keeps content together; no nesting beyond 1-2 levels.

### Avoid Deep Nesting
```
❌ Don't do this:
Article → Chapter → Section → Subsection → Paragraph
```
Better: Store everything flat, use references for relationships.

## Localization Patterns

### Separate Content Type per Language
```
Content Type: ArticleEN
Content Type: ArticleES
Content Type: ArticleDE
```
Simpler but requires duplication.

### Shared Type with Translatable Fields
```
Content Type: Article
Fields:
- slug (not translatable)
- title (translatable)
- body (translatable)
- featured_image (not translatable)
- publish_date (not translatable)
```
Better: Single content type, Contentful handles translations.

### Language-Aware Queries
```
query {
  article(locale: "es-MX") {
    title
    body
  }
}
```

## SEO & Metadata Patterns

### Embedded SEO Object
```
Content Type: Article
Fields:
- seo_metadata (JSON Object)
  - meta_title (string)
  - meta_description (string)
  - keywords (string, comma-separated)
  - og_image (reference)
  - canonical_url (string)
```

### Open Graph & Twitter Cards
```json
{
  "og_title": "Article Title",
  "og_description": "Preview text",
  "og_image": "url_to_image",
  "og_url": "https://example.com/article",
  "twitter_card": "summary_large_image",
  "twitter_handle": "@author"
}
```

## Content Governance Examples

### Editorial Workflow
```
Content Status (Enum):
- draft (editor can edit)
- review (requires approval)
- published (live, locked)
- archived (hidden)

Related Fields:
- created_by (Author reference, auto-filled)
- last_edited_by (Author reference, auto-updated)
- publish_date (Date, future-scheduled if needed)
- expires_at (Date, optional content expiration)
```

### Quality Assurance Checklist
- All required fields filled
- Image assets have alt text
- External links are valid
- SEO metadata complete (title, description)
- Related content linked (at least X related items)
