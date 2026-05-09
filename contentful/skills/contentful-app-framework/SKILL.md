---
name: contentful-app-framework
description: Build custom Contentful App Framework apps — UI extensions that live inside the Contentful web app, surfaced at field, sidebar, entry-editor, page, dialog, home, or config locations. Covers app definitions, App SDK patterns, hosting choices, parameter design, distribution, and the canonical Slalom-flavored app patterns (custom field editors, entry-side workflow tools, taxonomy pickers, AI-assisted authoring). Use this skill when the stock editor is missing a control authors keep asking for, when integrating with a third-party system needs editor-side surface area, when the team is evaluating "build vs. install from marketplace," or when an existing app is being extended. Trigger any time the question is "can we customize the editor experience" or "let's wrap an external service inside Contentful."

# Project context
type: skill
project: skills-library
plugin: contentful
aliases: [contentful-app-framework]
tags: [type/skill, plugin/contentful, topic/contentful, topic/app-framework]
status: active
---

# Contentful App Framework

The App Framework lets you embed custom React UIs inside Contentful's web app. It's the right answer when the stock authoring experience is missing something a Slalom client keeps asking for — a smarter slug picker, an in-context preview, an AI-assist drafting widget, a taxonomy selector that talks to an external classifier, an editorial dashboard.

This skill owns app design and lifecycle. Pair with `contentful-content-model` (custom field editors imply field decisions), `contentful-mcp-cli` (when AI-assist apps are in scope), `contentful-personalization` (the Personalization UI is itself an app on the marketplace), and `contentful-react-wrapper` (apps often consume the same content the front end does).

## When to use this skill

- Authors keep asking "can we just have a field that does X" and X isn't a stock control.
- A third-party service (DAM, translation, AI) needs editor-side surface area.
- Evaluating "build vs. install from the marketplace."
- Designing a custom workflow dashboard.
- Extending an existing app definition.
- Distributing an app across multiple client spaces or organizations.

## Before building: check the marketplace

Contentful's marketplace ([https://www.contentful.com/marketplace/apps/](https://www.contentful.com/marketplace/apps/)) ships pre-built apps for common needs. Default to install if a marketplace app fits. Common categories:

- **AI / Productivity** — AI Content Generator, Smart Translations, AI Image Generator
- **Translation / Localization** — Smartling, Phrase, Lokalise, GlobalLink Connect
- **DAM** — Cloudinary, Bynder, Frontify, Brandfolder
- **Search** — Algolia, Coveo
- **Analytics** — Optimizely, Amplitude, Mixpanel preview helpers
- **SEO** — Yoast-style helpers, Schema.org, GSC
- **Workflow** — Compose & Launch (first-party), GraphQL Playground (first-party), Tasks
- **Personalization** — Contentful Personalization (former Ninetailed; now first-party)

Build only when:

- No marketplace app fits.
- The marketplace app exists but its UX or data flow doesn't match the engagement.
- The integration is proprietary to the client.

See `../../references/marketplace-apps.md` for the curated list with Slalom annotations and default-install sets per engagement complexity.

## Architecture in one paragraph

An app definition (org-scoped) declares which **locations** the app can render in, which **parameters** it accepts, and where its UI is **hosted**. A space owner installs the app definition into a space and configures the per-installation parameters; from then on, the app's React UI renders in the configured locations using the App SDK, which exposes a typed API to talk to Contentful (CMA-scoped to the current user's permissions, plus the editor context).

## Locations — where apps render

Pick the right surface:

| Location | What it is | Use for |
|---|---|---|
| **Entry field (`entry-field`)** | Replaces or augments a field's editor | Custom slug pickers, AI-assist text fields, smart selectors, color pickers, validators |
| **Entry editor (`entry-editor`)** | Replaces the whole entry editing page | Full custom workflows for a content type — rare |
| **Entry sidebar (`entry-sidebar`)** | A widget in the entry sidebar | Workflow status, related-content suggestions, publish-time helpers |
| **Page (`page`)** | A standalone page in the Contentful web app | Dashboards, bulk operations, reports |
| **Dialog (`dialog`)** | A modal triggered by another location | Asset/entry pickers, confirmation flows |
| **Home (`home`)** | Tile on the space home | Onboarding, KPIs, links into custom apps |
| **App config (`app-config`)** | The configuration screen for the app itself | Where install-time parameters get set |

You can register one app for multiple locations. A "translation status" app might surface as `entry-sidebar` (per-entry status) and `page` (org-wide dashboard).

## App SDK essentials

The App SDK (`@contentful/app-sdk`) is what your React app talks to. Key surfaces:

```ts
import { init, locations } from "@contentful/app-sdk";

init((sdk) => {
  if (sdk.location.is(locations.LOCATION_ENTRY_FIELD)) {
    // sdk.field — get/set value, listen to changes
    // sdk.entry — sibling fields on the same entry
    // sdk.contentType — schema info
    // sdk.cma — CMA client scoped to current user
    // sdk.parameters — install + invocation parameters
    // sdk.window — resize the iframe
    renderFieldApp(sdk);
  }
  if (sdk.location.is(locations.LOCATION_ENTRY_SIDEBAR)) {
    renderSidebarApp(sdk);
  }
  // ...etc
});
```

Common patterns:

```ts
// Read the current field value
const value = sdk.field.getValue();

// Subscribe to external changes (collaborator typing, autosave)
const detach = sdk.field.onValueChanged((v) => setStateFromCMS(v));

// Write back
await sdk.field.setValue(newValue);

// Resize the iframe so the editor doesn't show scrollbars
sdk.window.startAutoResizer();

// Open a dialog (another location of the same app)
const result = await sdk.dialogs.openCurrentApp({
  position: "center",
  width: 800,
  parameters: { mode: "picker" },
});

// Use CMA scoped to the user
const entries = await sdk.cma.entry.getMany({ query: { content_type: "article" } });
```

The `cma` is **scoped to the current user's permissions** — if the user can't read drafts, neither can your app. Don't try to elevate; that's not what App Framework is for.

For React, the [`@contentful/react-apps-toolkit`](https://github.com/contentful/apps/tree/main/packages/react-apps-toolkit) gives you `useSDK()`, `useCMA()`, `useFieldValue()` hooks plus a `<SDKProvider>` that wires init for you.

## Forma 36 — Contentful's design system

Build apps with **Forma 36** (`@contentful/f36-components`). Reasons:

- Native look and feel inside the editor.
- Accessibility baked in.
- Skipping it is the fastest way to make an app feel bolted-on.

```tsx
import { Button, FormControl, TextInput } from "@contentful/f36-components";
```

The Forma 36 components are theme-aware; light/dark switches with the editor.

## Parameter design

Two types of parameters:

- **Installation parameters** — set when the space owner installs/configures the app. Stable across uses. Examples: API key for a translation service, default language.
- **Instance parameters** — set per-location-instance. Examples: which dropdown options this particular field should show; whether this sidebar widget shows the read-only summary or the full editor.

Design installation parameters as if they were environment variables; instance parameters as if they were per-component props.

```ts
// app definition (CMA call when registering the app)
{
  name: "Smart Slug",
  src: "https://your-app-host.example.com",
  locations: [
    { location: "entry-field", fieldTypes: [{ type: "Symbol" }] },
    { location: "app-config" },
  ],
  parameters: {
    installation: [
      { id: "modelEndpoint", type: "Symbol", required: true, name: "AI model endpoint" },
      { id: "apiKey", type: "Symbol", required: true, name: "API key" },
    ],
    instance: [
      { id: "sourceField", type: "Symbol", required: true, name: "Source field id" },
      { id: "maxLength", type: "Number", default: 60, name: "Max slug length" },
    ],
  },
}
```

## Hosting

Three options:

1. **Contentful's hosted apps service** — for apps with a built bundle. Upload via CLI; Contentful serves it from `app-host.contentful.com`. Default for first-party and marketplace apps. Lowest ops burden.
2. **Self-hosted** — point `src` at any HTTPS URL. Lets you ship a Next.js app, host on Vercel, deploy via the same pipeline as the front end. Best when the app needs server-side endpoints (calling external APIs that can't be called from the browser).
3. **Local dev** — `npm run create-contentful-app start` runs at `http://localhost:3000`; the app definition's `src` points there during development.

For Slalom delivery, hosted is the default for simple apps; self-hosted on Vercel for anything with server-side complexity.

## Lifecycle

```bash
# 1. Scaffold
pnpm create contentful-app
# Choose template (React + TS + Forma 36 is default)

# 2. Develop locally
cd my-app && pnpm install && pnpm start
# Update the app definition's src to your localhost URL while developing

# 3. Build
pnpm build

# 4. Upload (hosted) or deploy (self-hosted)
pnpm upload-ci   # bundles dist/ and uploads to Contentful

# 5. Install in a space
contentful app install --app-definition <APP_DEFINITION_ID>
```

The CLI handles app definition creation and updates.

## Distribution

- **Single-space**: install via CLI or UI in the target space. Done.
- **Multi-space within an org**: install in each space; the app definition is shared at org level.
- **Across organizations** (e.g., a Slalom delivery accelerator app reused across clients): the simplest path is to publish the app definition under each client's org. Slalom can keep a private bundle host (or Contentful-hosted) and a single source of truth, but the registration is org-scoped.
- **Marketplace publication**: requires Contentful review, branding, support commitments. Reach for this only when the app is a productized accelerator.

## Common app patterns

### Custom slug field with AI assistance

- Location: `entry-field` on `Symbol` types.
- Reads the source field via `sdk.entry.fields[id].getValue()`.
- Calls an AI endpoint (proxied through your backend so the API key stays server-side) to generate slug candidates.
- Writes the chosen slug back via `sdk.field.setValue()`.

### In-context preview

- Location: `entry-sidebar`.
- Reads `sdk.ids.entry`, `sdk.ids.contentType`, `sdk.contentType.sys.id`.
- Renders an iframe pointing at the front-end preview URL with the entry id as a query param. Front end uses Preview API + `draftMode()` to render.

### Taxonomy / external picker

- Location: `entry-field` on `Symbol` or `Array<Symbol>`.
- Calls an external taxonomy service.
- Renders a Forma 36 `<Autocomplete>` or `<MultipleSelect>`.
- Writes selections back as the field value.

### Workflow dashboard

- Location: `page`.
- Uses `sdk.cma` to query entries by tag, contentType, or status.
- Forma 36 `<Table>` of entries needing review, with quick-actions to open them.

### Personalization (first-party)

The Personalization UI is itself an App Framework app, packaged by Contentful (acquired from Ninetailed). See `contentful-personalization` for the runtime side.

## Security

- Apps run inside an iframe with `postMessage` between the app and the editor. The CMA token is brokered by the SDK; you never handle it directly.
- API keys for external services (translation, AI) belong on the server, not in the app bundle. Use installation parameters to pass them to your backend, but route external calls through a server endpoint of your own.
- HTTPS is mandatory for the `src`.
- App-config screens that take secrets must mark those parameter fields as secret in the definition; Contentful obscures them in the UI.

## Versioning

Contentful keeps a single "current" bundle per app definition. Updates are atomic. Implications:

- Use the hosted bundle pattern with `pnpm upload-ci`; rolling forward is fast.
- Don't ship breaking parameter changes without a migration plan — installations expect the prior parameter shape.

## Common pitfalls

- **Building before checking the marketplace.** Spend an hour on the marketplace; you may not need to build anything.
- **Using stock HTML instead of Forma 36.** App looks bolted-on; loses theme switching; misses accessibility.
- **Forgetting `startAutoResizer`.** App content gets clipped or shows scrollbars inside the editor iframe.
- **Calling external APIs from the browser with a key in the bundle.** Bundle is shipped; key is leaked. Always proxy.
- **Apps that mutate other entries silently.** Surprising side effects from a field-level app erode editor trust. Always show what the app is doing.
- **Skipping the `app-config` screen.** The space owner has to set parameters somewhere; without a config location, they're in the JSON definition only.
- **Not testing in dark mode.** Forma 36 helps, but custom canvas elements need a check.
- **Per-keystroke `setValue` calls.** Lots of versioned writes; collaborators get jumpy. Debounce.

## Output formats

### App proposal

```
# App: [Name]

## Problem
{what authoring pain it solves}

## Why build vs. install
{specific marketplace alternative considered, why it doesn't fit}

## Locations
- {location}: {what renders here}

## Parameters
- Installation: {list with types}
- Instance: {list with types}

## Data flow
{external services, API calls, where keys live}

## UX walkthrough
1. Author opens entry → ...
2. ...

## Hosting
- Hosted | Self-hosted (Vercel)
- Reasoning: {one line}

## Distribution
- Spaces: {list}
- Roll-forward plan: {if multi-space}

## Risks
{permissions, key handling, version drift}
```

## How this skill relates to others

- Field type / model implications → `contentful-content-model`.
- AI-assist apps that orchestrate via the Contentful MCP → `contentful-mcp-cli`.
- Personalization (first-party app) runtime → `contentful-personalization`.
- Translation apps and locale-aware behavior → `contentful-localization`.
- Apps that fetch the same content the front end does → `contentful-react-wrapper`.

## Source material

- App Framework overview: https://www.contentful.com/developers/docs/extensibility/app-framework/
- App SDK reference: https://www.contentful.com/developers/docs/extensibility/app-framework/sdk/
- Forma 36: https://f36.contentful.com/
- Apps GitHub (open source examples): https://github.com/contentful/apps
- create-contentful-app: https://github.com/contentful/create-contentful-app
- Marketplace: https://www.contentful.com/marketplace/apps/
- Plugin reference: `../../references/marketplace-apps.md`
