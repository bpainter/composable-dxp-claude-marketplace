---
name: bynder-compact-view
description: Embed the Bynder Universal Compact View (UCV) — the asset picker — into custom apps, Contentful field controls, Salesforce Lightning components, internal tools, and any other host application that needs a "let editors pick a Bynder asset" UX. Covers UCV configuration (theme, default filters, allowed asset types, derivative selection), the two auth modes (OAuth flow vs. token flow), event handling via postMessage, host-side state integration, and the cases where UCV beats an OOTB Marketplace connector. Use this skill when there's no Marketplace connector for the host system, when the OOTB connector exists but doesn't expose the configuration the editorial flow needs, or when building a bespoke editor where the asset picker is part of the differentiated UX.

# Project context
type: skill
project: skills-library
plugin: bynder
aliases: [bynder-compact-view]
tags: [type/skill, plugin/bynder, topic/bynder, topic/compact-view, topic/integration]
status: active
---

# Universal Compact View (Bynder asset picker embed)

This skill puts you in the role of an integration engineer who's wired UCV into half a dozen host systems and knows the patterns. Default posture: check the Marketplace first; if there's a working OOTB connector, use it. UCV is the answer when the connector doesn't exist or doesn't fit.

UCV is Bynder's embeddable asset picker — an iframe that runs inside a host application, lets the user search/browse/filter Bynder assets, and returns selected assets (with metadata + derivative URLs) to the host via postMessage. Documented at https://developers.bynder.com/universal-compact-view.

Pair with `bynder-marketplace-connectors` (always check first), `bynder-contentful-pairing` (the most common UCV target), `bynder-derivatives` (which derivative the picker returns), `bynder-permissions-workflow` (token / scope strategy), and `bynder-asset-model` (default filters that pre-narrow the picker).

## When to use this skill

- Building a custom CMS extension where editors pick Bynder assets.
- The Marketplace doesn't have a connector for the host system.
- The Marketplace connector exists but doesn't expose configuration the editorial flow needs (default filters, derivative selection, custom theming).
- Embedding asset selection into a Salesforce Lightning component, a custom React internal tool, a Drupal block, an in-house dashboard.
- Layering a custom UCV embed on top of an OOTB connector (e.g., a "bulk publish" dashboard alongside the standard Contentful UCV embed).
- Diagnosing why a UCV embed is misbehaving (auth, postMessage, derivative-selection, theme).

## Core posture

- **Marketplace first, UCV second.** The OOTB Bynder–Contentful app uses UCV under the hood; if it fits, use it. UCV is the answer when it doesn't.
- **Pick the auth mode deliberately.** OAuth flow when editors have Bynder accounts; token flow when the host backend brokers access.
- **Configure defaults that match the editorial intent.** Filter to the relevant asset types, the relevant brand, the relevant approved-status. Editors shouldn't see assets they can't legally use.
- **Return the right derivative.** UCV can return original asset URL, a specific derivative, or all derivative URLs. Decide in advance.
- **Treat postMessage like an API contract.** Lock the message shape; version the integration if the host application's data model changes.
- **Permission-gate at the token level.** A UCV embed in a marketing app inherits the user's Bynder permissions. A token-flow embed inherits the *backend's* permissions. Choose accordingly.

## Architecture

### The two auth modes

**OAuth flow** — editor authenticates inline. Best when:
- Each editor has a Bynder account.
- You want per-editor permission enforcement (a junior editor sees only campaign assets; a senior editor sees everything).
- You're embedding inside a long-lived editor session (Contentful, Salesforce CRM).

```
Host loads UCV iframe
  → UCV detects no token
  → User clicks "log in to Bynder"
  → OAuth redirect dance (handled by UCV)
  → Token issued in iframe context
  → Picker renders
```

**Token flow** — backend mints a token, hands to UCV. Best when:
- Editors don't have direct Bynder accounts.
- The host system has its own user model.
- You need a single audit trail at the backend level.

```
Host backend → Bynder token endpoint (Client Credentials or impersonation)
  → Backend passes token to host frontend
  → Frontend instantiates UCV with the token
  → Picker renders without auth UI
```

Slalom default: OAuth flow when integrating into editor-facing tools where the editor has a Bynder account. Token flow when integrating into non-editorial surfaces (dashboards, batch tools).

### The basic embed

Install the package and instantiate:

```ts
import BynderCompactView from '@bynder/compact-view';

BynderCompactView.open({
  language: 'en_US',
  portal: { url: 'https://your-portal.bynder.com' },
  mode: 'SingleSelect', // or 'MultiSelect'
  assetTypes: ['image', 'video'],
  defaultSearchTerm: '',
  hideLimitedUseAssets: true,
  selectedAssets: [],
  onSuccess: (assets, additionalInfo) => {
    // assets is an array of selected asset metadata
    // additionalInfo has any extra context (e.g., the derivative selected)
    handleSelection(assets);
  },
  onClose: () => {
    cleanupModalUI();
  },
});
```

The package handles the iframe lifecycle, the postMessage event handling, and the modal scaffolding. The host app provides the callbacks.

### Configuration that matters

| Option | Use |
|---|---|
| `mode` | `SingleSelect` for fields that take one asset; `MultiSelect` for galleries / batches |
| `assetTypes` | Limit to relevant types — image-only field shouldn't show videos |
| `defaultSearchTerm` | Pre-populate search (e.g., the entry's tags joined into a search term) |
| `hideLimitedUseAssets` | True for editorial flows (don't show "internal-only" assets) |
| `selectedAssets` | Pre-select for edit-existing flows |
| `theme` | Light/dark + primary color match for visual integration |
| `language` | Localize the picker UI |

For richer config — default metaproperty filters, derivative-set selection — pass via the `assetFieldSelection` and `defaultMetapropertyOptions` configuration on Bynder's portal side, then reference them in the embed config. The portal config + embed config interact; you usually need both.

### Derivative selection

The big decision: what URL flows back to the host?

| Mode | Returns | Use |
|---|---|---|
| `original` | Original asset URL | Print, download, source-master flows |
| Specific derivative | One named derivative URL (e.g., `web_hero_lg`) | Most production CMS flows |
| All derivatives | An object of `{ name → URL }` | When the host wants flexibility (e.g., responsive image components that pick at render time) |

Slalom default for CMS pairing: return the asset ID + the canonical derivative for the field, plus a small set of responsive variants. Don't return all derivatives by default — payload bloats and the host has to choose anyway.

### postMessage contract

UCV emits messages to the host via `window.postMessage`:

```ts
{
  type: 'BYNDER_ASSET_SELECTED',
  payload: {
    assets: [
      {
        id: '...',
        name: '...',
        type: 'image',
        thumbnails: { /* derivative urls */ },
        property_AssetCategory: ['Marketing'],
        // ... other property_* and system fields
      }
    ],
    additionalInfo: { /* derivative chosen, etc. */ }
  }
}
```

The package abstracts this — you write `onSuccess`. But when something's wrong, debug at the postMessage level. Open dev tools, watch messages, see what UCV is actually emitting.

## Custom field control patterns

### Contentful (custom version of the OOTB app)

When the OOTB Contentful Bynder app doesn't quite fit (e.g., needs hard-coded brand filter, custom derivative-selection UI, validation), build a custom field control via the Contentful App SDK that wraps UCV:

```tsx
// SingleAssetField.tsx
import { useFieldValue } from '@contentful/react-apps-toolkit';
import BynderCompactView from '@bynder/compact-view';

export function BynderSingleAssetField() {
  const [value, setValue] = useFieldValue<BynderAssetReference>();

  function openPicker() {
    BynderCompactView.open({
      mode: 'SingleSelect',
      assetTypes: ['image'],
      portal: { url: PORTAL_URL },
      hideLimitedUseAssets: true,
      defaultMetapropertyOptions: [BRAND_OPTION_ID, RIGHTS_CLEARED_OPTION_ID],
      onSuccess: (assets) => {
        const a = assets[0];
        setValue({
          assetId: a.id,
          name: a.name,
          derivatives: {
            heroLg: a.thumbnails.web_hero_lg,
            cardMd: a.thumbnails.web_card_md,
          },
          alt: a.description,
        });
      },
    });
  }

  return (
    <div>
      {value ? <PreviewCard asset={value} onChange={openPicker} /> : <Button onClick={openPicker}>Pick asset</Button>}
    </div>
  );
}
```

The Contentful field stores a small JSON envelope (asset ID + the derivative URLs the front end actually needs + alt). Don't store the full Bynder asset object — it goes stale.

See `bynder-contentful-pairing` for the full pattern.

### Salesforce Lightning component

UCV runs in a Lightning Web Component via an iframe wrapper. Auth is usually token flow — Salesforce backend mints a token via Apex callout to Bynder's OAuth endpoint, passes to the LWC.

### Custom internal tool

For React or Vue host apps without a published connector: same pattern as Contentful. Drop the package, configure, handle the callback, persist what you need.

## Sync with downstream state

The hard part of UCV is *not* the picker — it's keeping the host's stored asset reference in sync with the underlying Bynder asset.

Three strategies:

1. **Store reference + cached derivatives. Re-resolve on webhook.** Host stores `{ assetId, derivatives: {...}, version }`. A `bynder-webhooks-events` receiver invalidates and re-fetches when `asset.updated` fires.
2. **Store reference only. Resolve at render time.** Host stores `{ assetId }`. Front end fetches derivative URLs on render. Latency cost; freshness benefit.
3. **Store reference + derivatives. Stale is acceptable.** Host stores everything; never re-syncs. Simple; assets that update in Bynder don't reflect downstream until manually re-picked.

Slalom default: strategy 1. Webhook-driven invalidation gives you fresh-enough state without the per-render fetch cost.

## Theming

UCV can be themed to match the host's brand:

```ts
BynderCompactView.open({
  theme: {
    colorButtonPrimary: '#7C3AED',
    colorPrimary: '#1F2937',
    fontFamily: 'Inter, -apple-system, sans-serif',
  },
  // ...
});
```

Match the host's primary brand color and font. The picker shouldn't look like a foreign body.

## Output formats

### UCV embed plan

```
# Bynder UCV Embed Plan: [Host system]

## Why UCV (vs. OOTB)
[What the OOTB connector doesn't do; why custom is justified.]

## Auth mode
[OAuth flow | Token flow — and why.]

## Picker configuration
- Mode: [Single | Multi]
- Asset types: [image | video | document | ...]
- Default filters: [metaproperty options]
- Hide limited-use: [true | false]
- Theme: [primary, secondary, font]

## Derivative selection
[What URL(s) flow back to the host.]

## Host state shape
[The JSON envelope persisted in the host CMS / DB.]

## Sync strategy
[Strategy 1 / 2 / 3 from above; webhook events subscribed.]

## Permission posture
[Who can use the picker; what their token / OAuth scope grants.]

## Test plan
[Pick + persist + render; pick + update in Bynder + invalidate; pick + delete in Bynder + handle gracefully.]
```

## Common anti-patterns to call out

- **Reaching for UCV when an OOTB connector exists.** Check the Marketplace first.
- **Returning all derivatives by default.** Bloats host payloads; commit to the named ones the field actually needs.
- **Storing the full asset object in the host DB.** Goes stale. Store reference + the few derivatives + a version pointer.
- **No webhook-invalidation strategy.** Asset updates in Bynder never reflect downstream — editors lose trust.
- **Token-flow with a long-lived token.** The backend mints; the picker holds; the token outlives the session. Use short-lived tokens.
- **OAuth-flow without `hideLimitedUseAssets`.** Editors see assets they shouldn't legally use; rights problems waiting to happen.
- **Defaults set in code only, no portal config.** UCV reads both; setting defaults only in the embed misses the portal's filter governance.
- **No fallback for "asset deleted in Bynder".** Host renders broken image. Detect 404 on derivative resolve, surface clearly.

## Constraints

- Do not propose UCV when there's a working, supported OOTB connector for the host system without explicitly explaining why the connector is insufficient.
- Do not propose returning the full Bynder asset object to the host — return a curated envelope.
- Do not skip the sync-with-downstream design — webhook strategy is part of the embed plan, not an afterthought.
- Do not theme inconsistently — picker should match host's primary brand.
- Do not store auth tokens in browser-accessible storage; broker through the host backend.

## How this skill relates to others

- The first-check Marketplace catalog → `bynder-marketplace-connectors`.
- The most common UCV target (Contentful) → `bynder-contentful-pairing`.
- The webhook events that drive sync → `bynder-webhooks-events`.
- The derivatives the picker returns → `bynder-derivatives`.
- The metaproperty filters that pre-narrow the picker → `bynder-asset-model`.
- The permissions / scopes the picker token carries → `bynder-permissions-workflow`.
- The SDK calls used in the host backend for token broker / re-fetch → `bynder-js-sdk`.

## Source material

- Universal Compact View docs: https://developers.bynder.com/universal-compact-view
- Compact View NPM package: `@bynder/compact-view`
- Plugin reference: `../../references/marketplace-connectors.md`
- Plugin reference: `../../references/api-surface.md`
- Plugin reference: `../../references/bynder-foundations.md`
