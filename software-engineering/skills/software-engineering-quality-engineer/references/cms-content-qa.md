---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[quality-engineer]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# CMS Content QA & Validation (2025)

## Content Model Testing

Contentful content models are schema. Validate structure, constraints, and relationships.

### Field Validation Rules

```
Title field:
├── Type: Short text
├── Required: Yes
├── Min length: 5 characters
├── Max length: 100 characters
└── Regex: ^[A-Za-z0-9\s\-\.!?]+$ (no special chars)

Body field:
├── Type: Rich text
├── Required: Yes
├── Allowed marks: Bold, italic, underline
└── Allowed blocks: Paragraph, heading, list
```

### Test Cases

```
1. Title too short (<5 chars) → Should show error
2. Title too long (>100 chars) → Should show error
3. Title with invalid characters → Should show error
4. Empty required field → Should block publish
5. Leaving optional field empty → Should allow publish
```

## Content Integrity Testing

### Reference Validation
```
Article model:
├── author: Reference to Author model
├── category: Reference to Category model
└── relatedArticles: Multiple references

Test:
1. Create article without author → Should error or default
2. Delete referenced author → Frontend should handle gracefully
3. Broken reference → Content should not render
```

### Localization Testing

```
For each field in each locale:
├── Is content translated? (or inherited from default locale?)
├── Does translated content render correctly?
├── Are special characters/diacritics preserved?
└── Is SEO slug unique per locale?

Test cases:
1. English: "Hello" → French: "Bonjour" → German: "Hallo"
2. Missing French translation → Should fallback to English
3. Character encoding: Chinese, Arabic, emojis → Should display correctly
```

## Content Publishing Workflow QA

```
Draft → Review → Scheduled Publish → Published → Archived

Test:
1. Can editors save as draft?
2. Can reviewers see drafts?
3. Can editors schedule publish to future date?
4. Does scheduled publish trigger at correct time?
5. Is archived content hidden from frontend?
6. Can archived content be restored?
```

## Content Rendering Testing

### Template Rendering
Test content renders correctly in all contexts.

```
Blog Article in:
├── Blog listing (summary view)
├── Blog detail page (full view)
├── Related articles sidebar (excerpt)
├── Search results (snippet)
└── Social sharing (meta tags)

Test each context for:
- All required fields present
- Optional fields gracefully hidden if missing
- Rich text renders as HTML correctly
- Images display with correct alt text
- Links work (internal and external)
```

### Content Preview
Test preview functionality in Contentful.

```
1. Editor creates content
2. Clicks "preview"
3. Sees draft content on frontend
4. Makes edits
5. Preview updates in real-time (or on refresh)
```

## Content Gap Testing

For multilingual, multi-regional content:

```
Checklist:
├── [ ] All locales have required fields?
├── [ ] All regions have content?
├── [ ] No orphaned references?
├── [ ] All images have alt text?
├── [ ] All links are valid?
└── [ ] SEO fields (title, description) complete per locale?
```

## Automated Content Validation

```javascript
// Example: Content integrity check
async function validateContent(contentItem) {
  const errors = [];

  if (!contentItem.title) errors.push('Missing title');
  if (!contentItem.slug) errors.push('Missing slug');
  if (contentItem.slug.match(/[^a-z0-9\-]/)) {
    errors.push('Slug contains invalid characters');
  }

  // Check references exist
  if (contentItem.authorId) {
    const author = await contentful.getEntry(contentItem.authorId);
    if (!author) errors.push('Referenced author not found');
  }

  return { valid: errors.length === 0, errors };
}
```

Run in CI/CD before publishing.

## Editorial QA Sign-Off

Before launch, editorial team validates:

```
Content Checklist:
- [ ] All required content present
- [ ] Tone/voice consistent
- [ ] No typos or grammatical errors
- [ ] Links accurate and functional
- [ ] Images match content
- [ ] Metadata (title, description) optimized for SEO
- [ ] Localization complete and accurate
- [ ] Legal/compliance review complete
```
