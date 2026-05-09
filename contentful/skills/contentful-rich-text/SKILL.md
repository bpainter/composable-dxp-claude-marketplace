---
name: contentful-rich-text
description: Render Contentful Rich Text into React with embedded entries and assets, custom marks, design-system mapping, and the gotchas that show up when authors paste from Word or Google Docs. Use this skill any time a Rich Text field is being rendered, when "the article body looks weird" comes up, when embedded entries / assets need handling, when sanitization is in question, or when the design system needs to control rich text typography. Trigger whenever Rich Text rendering decisions are being made.

# Project context
type: skill
project: skills-library
plugin: contentful
aliases: [contentful-rich-text]
tags: [type/skill, plugin/contentful, topic/contentful, topic/rich-text]
status: active
---

# Contentful Rich Text

Rich Text in Contentful is a **structured AST**, not HTML. That's the right design — it's portable, sanitizable, and renderable into anything. But it means every site needs an explicit renderer that maps AST nodes to design-system components. Skip that step and you ship a stylesheet-less wall of text.

This skill owns the renderer layer. Pair with `contentful-content-model` (when to use Rich Text vs. Markdown vs. references), `contentful-react-wrapper` (the wrapper passes Rich Text to the renderer), `contentful-graphql` (querying Rich Text and embeds), and `contentful-delivery-optimization` (image-heavy bodies).

## When to use this skill

- Wiring the Rich Text renderer for a new project.
- "The article body looks unstyled" or "weird spacing in the body."
- Embedding entries (a `CallToAction`) inside a Rich Text body.
- Embedding assets (images, video) inside Rich Text.
- Paste-from-Word or Google Docs is producing surprising output.
- Adding a custom mark or block (a callout, a footnote, a pull-quote).

## Core posture

- **Render through the design system.** Every node type maps to a typography or block component. No raw `<h1>` / `<p>` / `<ul>` in the final output.
- **Embedded entries render via the same wrappers as standalone entries.** If the front end has a `CallToActionFromContentful`, that's what an embedded CTA inside an article body uses. Same code path; same revalidation tags.
- **Sanitize is not the renderer's job.** Rich Text is structured; you don't need to strip arbitrary HTML. You do need to validate node types against an allowlist if you accept user-generated content.
- **Author intent matters.** If authors keep producing the same shape that's not native (an "important callout" they fake with bold + emoji), build it as an embedded entry type.

## The Rich Text data shape

A Rich Text value is a tree:

```json
{
  "nodeType": "document",
  "data": {},
  "content": [
    {
      "nodeType": "heading-2",
      "data": {},
      "content": [{ "nodeType": "text", "value": "Section title", "marks": [], "data": {} }]
    },
    {
      "nodeType": "paragraph",
      "data": {},
      "content": [
        { "nodeType": "text", "value": "Some ", "marks": [], "data": {} },
        { "nodeType": "text", "value": "bold", "marks": [{ "type": "bold" }], "data": {} },
        { "nodeType": "text", "value": " text.", "marks": [], "data": {} }
      ]
    },
    {
      "nodeType": "embedded-entry-block",
      "data": { "target": { "sys": { "type": "Link", "linkType": "Entry", "id": "abc123" } } },
      "content": []
    }
  ]
}
```

Block-level node types (defaults):

`document, paragraph, heading-1, heading-2, heading-3, heading-4, heading-5, heading-6, ordered-list, unordered-list, list-item, hr, blockquote, embedded-entry-block, embedded-asset-block, embedded-resource-block, table, table-row, table-cell, table-header-cell`

Inline node types:

`hyperlink, entry-hyperlink, asset-hyperlink, resource-hyperlink, embedded-entry-inline, embedded-resource-inline`

Marks (text decorations):

`bold, italic, underline, code, superscript, subscript, strikethrough`

You can disable any of these in the field's validation. Author intent quality goes up when the editor doesn't have an option that the design system can't honor.

## The renderer

`@contentful/rich-text-react-renderer` is the canonical React renderer. It takes the document and a render-options config that maps node types to components.

```bash
pnpm add @contentful/rich-text-react-renderer @contentful/rich-text-types
```

```tsx
// components/rich-text.tsx
import {
  documentToReactComponents,
  Options,
} from "@contentful/rich-text-react-renderer";
import { BLOCKS, INLINES, MARKS } from "@contentful/rich-text-types";
import { Heading2, Heading3, Body, Eyebrow, List, ListItem } from "@/components/typography";
import { CallToActionFromContentful } from "@/components/blocks/call-to-action-from-contentful";
import { ImageBlock } from "@/components/blocks/image-block";

const options: Options = {
  renderMark: {
    [MARKS.BOLD]: (text) => <strong className="font-semibold">{text}</strong>,
    [MARKS.ITALIC]: (text) => <em>{text}</em>,
    [MARKS.CODE]: (text) => <code className="rounded bg-muted px-1 py-0.5 text-sm">{text}</code>,
  },
  renderNode: {
    [BLOCKS.PARAGRAPH]: (_, children) => <Body className="mb-4">{children}</Body>,
    [BLOCKS.HEADING_2]: (_, children) => <Heading2 className="mt-8 mb-4">{children}</Heading2>,
    [BLOCKS.HEADING_3]: (_, children) => <Heading3 className="mt-6 mb-3">{children}</Heading3>,
    [BLOCKS.UL_LIST]: (_, children) => <List variant="bullet">{children}</List>,
    [BLOCKS.OL_LIST]: (_, children) => <List variant="number">{children}</List>,
    [BLOCKS.LIST_ITEM]: (_, children) => <ListItem>{children}</ListItem>,
    [BLOCKS.QUOTE]: (_, children) => (
      <blockquote className="border-l-4 border-primary pl-4 my-6 italic text-muted-foreground">
        {children}
      </blockquote>
    ),
    [INLINES.HYPERLINK]: (node, children) => (
      <a href={node.data.uri} className="underline underline-offset-4">
        {children}
      </a>
    ),
    [BLOCKS.EMBEDDED_ENTRY]: (node) => {
      const id = node.data.target.sys.id;
      const typename = node.data.target.__typename;
      switch (typename) {
        case "CallToAction":
          return <CallToActionFromContentful id={id} />;
        // add cases per allowed embedded type
        default:
          return null;
      }
    },
    [BLOCKS.EMBEDDED_ASSET]: (node) => {
      const asset = node.data.target;
      return (
        <ImageBlock
          src={asset.url}
          alt={asset.description ?? ""}
          width={asset.width}
          height={asset.height}
        />
      );
    },
  },
};

export function RichText({ document }: { document: any }) {
  if (!document) return null;
  return <>{documentToReactComponents(document, options)}</>;
}
```

Notes:

- The renderer doesn't sanitize — it ignores HTML; the AST has no escape hatch by design.
- `node.data.target` for embedded entries/assets carries the full referenced object **only if you queried for it**. With GraphQL, you ask for the embedded references in the query (see "Querying Rich Text with embeds" below).
- Use the design system's typography components — don't render `<h2>` directly. Spacing, type ramp, line-height, and color all need to come from the system.

## Querying Rich Text with embeds

The Rich Text body alone is just the AST. To resolve embedded entries and assets, the GraphQL query has to ask for them:

```graphql
query Article($slug: String!, $preview: Boolean) {
  articleCollection(where: { slug: $slug }, preview: $preview, limit: 1) {
    items {
      title
      body {
        json
        links {
          entries {
            block {
              sys { id }
              __typename
              ...on CallToAction { ...CallToActionFields }
            }
            inline {
              sys { id }
              __typename
              ...on GlossaryTerm { ...GlossaryTermFields }
            }
          }
          assets {
            block {
              sys { id }
              url
              title
              description
              width
              height
              contentType
            }
          }
        }
      }
    }
  }
}
```

The renderer needs the resolved entries and assets to render them. The shape coming back from GraphQL is `{ json, links }`; you pass `json` to the renderer and use `links` to look up the targets:

```tsx
function buildEntryMap(entries: any[]) {
  return new Map(entries.map((e) => [e.sys.id, e]));
}

const entryMap = buildEntryMap([
  ...(article.body.links.entries.block ?? []),
  ...(article.body.links.entries.inline ?? []),
]);
const assetMap = new Map(
  (article.body.links.assets.block ?? []).map((a: any) => [a.sys.id, a]),
);

const options: Options = {
  // ...
  renderNode: {
    [BLOCKS.EMBEDDED_ENTRY]: (node) => {
      const target = entryMap.get(node.data.target.sys.id);
      if (!target) return null;
      switch (target.__typename) {
        case "CallToAction": return <CallToAction {...target} />;
        // ...
      }
    },
    [BLOCKS.EMBEDDED_ASSET]: (node) => {
      const asset = assetMap.get(node.data.target.sys.id);
      if (!asset) return null;
      return <ImageBlock {...asset} />;
    },
  },
};
```

The `Maps` close over the renderer's options on each render. For frequently-rendered Rich Text (a body field on every article view), pull the option building into a function called per render rather than building options once globally.

## Validating allowed nodes and embeds

In the field validation:

- **Allowed node types** — restrict block-level nodes (`heading-1`, `embedded-resource-block`, etc.) authors can use. Match the renderer.
- **Allowed embedded entry types** — `linkContentType: ["callToAction", "calloutBlock"]`. The renderer should switch only on those types.
- **Allowed embedded asset MIME types** — `linkMimetypeGroup: ["image", "video"]`.
- **Hyperlink types allowed** — `hyperlink`, `entry-hyperlink`, `asset-hyperlink`, `resource-hyperlink`.

Set these at the model level. A renderer that handles an unexpected type should return `null` (silently skip) rather than fail; better to drop one widget than break the article.

## Custom marks and blocks

Two ways to extend:

### 1. Use embedded entries (preferred)

A "callout" block becomes a `CalloutBlock` content type with fields like `tone` (info | warning | success), `body`, `icon`. Authors embed it in Rich Text. The renderer routes on `__typename`. The model is reusable, the design-system component is testable in Storybook.

### 2. Use a custom App Framework field editor

For inline marks the AST doesn't support natively (a "footnote ref" mark), build a custom Rich Text field control with the App Framework. See `contentful-app-framework`. Higher cost; reach for it only when embedded entries don't fit.

## Sanitization and user-generated content

If the Rich Text field accepts user-generated content (comments, customer-contributed posts):

- The AST already excludes raw HTML. That's most of the safety story.
- **Validate node types** against an allowlist before rendering. Drop `embedded-resource-block` and similar unless you've explicitly built handling.
- **Validate hyperlinks** — block `javascript:` URIs, validate the host on `entry-hyperlink` and `asset-hyperlink` resolution.
- **Don't trust author-supplied alt text on assets** — a user could put XSS in there if you ever dropped it into HTML. The renderer puts it into `alt` attributes which are safe.

For trusted editor content (the typical Slalom case), this is over-engineering. Skip the deep allowlist and trust the model.

## Rendering on the server vs. client

Render Rich Text in server components when possible:

- Smaller client bundle (the renderer is ~30KB minified).
- Fewer hydration concerns.
- Embedded entries can be async server components with their own data fetching (see `contentful-react-wrapper`).

If you must render client-side (a real-time editor preview, an in-page comment thread):

- Bundle the renderer once.
- Memoize the options object.
- Be deliberate about embedded entry fetches; they shouldn't refetch on every render.

## Plain-text extraction

For excerpts, search indexing, or AI summaries, you usually want plain text:

```ts
import { documentToPlainTextString } from "@contentful/rich-text-plain-text-renderer";
const summary = documentToPlainTextString(article.body.json).slice(0, 280);
```

## Common pitfalls

- **Raw HTML elements in the renderer.** Skips the design system. Use typography components.
- **Embedded entries that don't resolve.** Querying `body { json }` without the `links` block. The renderer gets node references with no resolved target.
- **Re-querying the same embedded entry on every render.** Embedded `CallToActionFromContentful` does its own fetch; the page also fetched the same entry via `links`. Pick one path; don't re-fetch.
- **Heading levels not aligned with the page hierarchy.** Authors use `heading-1` inside an article body that already has an `h1` for the page title. Restrict to `heading-2` and below at the model.
- **Blockquote styling forgotten.** Looks like default browser `<blockquote>` (italic, indented gray). Map to a design-system component.
- **Asset blocks at full-width without responsive parameters.** Use the Images API in the asset renderer; don't ship raw originals.
- **Tables.** Rich Text supports tables since 2022, but they're easy to author poorly. If you allow them, render through a `<DataTable>` that handles overflow scroll on mobile.

## How this skill relates to others

- When to use Rich Text vs. Markdown vs. references → `contentful-content-model`.
- The wrapper that hands the document to the renderer → `contentful-react-wrapper`.
- The query that fetches `body { json, links }` → `contentful-graphql`.
- Image-heavy bodies and the Images API → `contentful-delivery-optimization`.
- Custom Rich Text editor controls → `contentful-app-framework`.
- Locale-specific rendering when an asset URL or entry differs by locale → `contentful-localization`.

## Source material

- Rich Text concept: https://www.contentful.com/developers/docs/concepts/rich-text/
- Rich Text types: https://www.npmjs.com/package/@contentful/rich-text-types
- React renderer: https://www.npmjs.com/package/@contentful/rich-text-react-renderer
- Plain-text renderer: https://www.npmjs.com/package/@contentful/rich-text-plain-text-renderer
- Plugin reference: `../../references/contentful-foundations.md`
