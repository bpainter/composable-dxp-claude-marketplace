---
type: reference
project: skills-library
scope: plugin
plugin: bynder
tags: [type/reference, plugin/bynder, scope/foundational, topic/connectors, topic/marketplace]
status: active
---
# Bynder Marketplace connector catalog

Quick reference for the OOTB connectors at https://marketplace.bynder.com/en-US/home. Skills cite this file to keep the build-vs-buy conversation grounded in what's actually available.

The Marketplace is real software — most connectors are configurable, click-installable, and work with no engineering. The agent should always check the Marketplace before recommending a custom build.

## CMS / DXP

| Connector | What it does | Slalom posture |
|---|---|---|
| **Contentful** | Bynder app for Contentful — embeds Compact View as a custom field control on entries, returns asset IDs + derivative URLs to Contentful field values | **Default**. Install OOTB; extend with custom Compact View settings (default filters, derivative selection) only when the editor experience demands it. See `bynder-contentful-pairing`. |
| **Sitecore** | Asset selection inside Sitecore's content editor; works for both XP and XM Cloud | Use as starting point on Sitecore engagements; expect customization for the experience editor |
| **Adobe Experience Manager** | DAM-to-DAM-ish — surfaces Bynder assets inside AEM's authoring environment | Useful when client has AEM as the page-authoring tool but Bynder as enterprise DAM. Tricky model — talk through asset-source-of-truth before installing. |
| **Drupal** | Asset picker module for Drupal 9+ | Solid; one of the more mature OOTB connectors |
| **WordPress** | Asset selection inside the WP block editor | Lightweight; fine for marketing-site WP installs |
| **Optimizely (formerly Episerver)** | Asset selection in Optimizely CMS | Good baseline; Optimizely engagements often need custom property mapping |
| **Magnolia** | Asset surfacing inside Magnolia's app launcher | Standard |

## Commerce

| Connector | What it does | Slalom posture |
|---|---|---|
| **Salesforce Commerce Cloud (SFCC / B2C)** | Bynder asset references on PDPs, content slots, marketing pages | Often the right answer for SFCC + Bynder pairing; less common in our composable practice but shows up |
| **commercetools** | Asset references in commercetools product, category, and content shapes | Pairs well with composable commerce stack — Bynder as DAM, commercetools as commerce, Contentful as CMS |
| **Shopify** | Bynder app for Shopify product images and theme assets | Only relevant on small-to-mid commerce engagements |
| **BigCommerce** | Similar to Shopify — product asset selection | Niche |

## Creative

| Connector | What it does | Slalom posture |
|---|---|---|
| **Adobe Creative Cloud (CC)** — Photoshop, Illustrator, InDesign | Surfaces Bynder assets inside the CC desktop apps; check-in/check-out on saves | High-value for clients with active design teams; demonstrates DAM ROI immediately |
| **Adobe Express** | Bynder asset library inside Adobe Express templates | Useful for marketing-team self-serve content creation |
| **Figma** | Plugin to drag Bynder assets into Figma frames | Designer-favorite; recommend it on every engagement with a Figma-using design team |

## Marketing automation / engagement

| Connector | What it does | Slalom posture |
|---|---|---|
| **HubSpot** | Asset selection inside HubSpot's email and landing-page builders | Good fit for mid-market clients on HubSpot |
| **Marketo Engage** | Asset surfacing in Marketo program assets | Standard for B2B marketing teams |
| **Salesforce Marketing Cloud** | Content Builder integration | Useful where the client has SFMC as the email/journey tool |
| **Mailchimp** | Lightweight asset picker | Niche |

## Productivity / collaboration

| Connector | What it does | Slalom posture |
|---|---|---|
| **Microsoft Teams** | Search and share Bynder assets directly in Teams | High-adoption value; install on every engagement |
| **Slack** | Same idea, Slack-native | Same — install reflexively |
| **Google Workspace** | Asset insertion in Docs, Slides, Sheets | Useful where the client lives in Google rather than Microsoft |
| **Microsoft 365** (Office) | Asset insertion in Word, PowerPoint, Outlook | Same — drives daily-use adoption |

## PIM (product information management)

| Connector | What it does | Slalom posture |
|---|---|---|
| **Akeneo** | Two-way asset references between Akeneo PIM and Bynder | Common in B2C retail with PIM + DAM + commerce stack |
| **Salsify** | Same idea | Less common for us; check support level before committing |
| **inRiver** | Same idea | B2B PIM scenarios |

## Workflow / planning

| Connector | What it does | Slalom posture |
|---|---|---|
| **Adobe Workfront** | Bynder Workflow ↔ Workfront task / project sync | High-value on creative-ops-heavy engagements |
| **monday.com** | Asset attachment from Bynder into monday boards | Useful for marketing-ops teams on monday |
| **Asana** | Similar to monday | Useful where Asana is the project tool |

## Social / publishing

| Connector | What it does | Slalom posture |
|---|---|---|
| **Hootsuite** | Pull Bynder assets into Hootsuite scheduled posts | Recommend wherever there's an active social team |
| **Sprinklr** | Same idea | Enterprise social-suite alternative |

## Decision framework: OOTB vs. custom

| Use the OOTB connector when... | Build custom (UCV embed) when... |
|---|---|
| The host system is on the Marketplace list | The host system isn't on the list (custom CMS, custom internal tool) |
| Default behavior matches the editorial workflow | Editorial workflow needs filters/derivatives the connector doesn't expose |
| Brand team is comfortable with the connector's UX | The connector's UX feels foreign to a key user group |
| No SLAs around how quickly metaproperty changes propagate | You need real-time UCV refresh on metaproperty change |
| Connector is actively maintained by the vendor | Connector is stale, deprecated, or community-maintained without support |

A common pattern: install the OOTB connector for the baseline editorial path, and **layer a custom Compact View embed** for advanced flows (e.g., the marketing team's bulk-publish dashboard). This is `bynder-contentful-pairing` in practice.

## Marketplace gotchas

- **Connector versioning is opaque.** The Marketplace listing tells you if a connector is "Verified by Bynder" but doesn't always tell you what version it's on or when it was last updated. Check the connector's own GitHub or docs before sizing a deployment around it.
- **Connector permissions and the host app's permissions are separate models.** A Bynder permission profile that lets a user see asset X has nothing to do with whether their Contentful role lets them edit the entry that references X. Audit both.
- **Connectors don't replicate webhooks.** If you need a metadata change in Bynder to invalidate a cache in the host system, you still wire that webhook yourself — the connector handles the picker UX, not the eventing.
- **Some connectors require Marketplace App + manual portal config.** The Contentful app, for example, is partly a Contentful app and partly a Bynder portal config (asset return URL, default filter). Always do both sides of the install.

## How this drives skill recommendations

When a skill in this plugin proposes integrating Bynder with a downstream system, the order of operations is:

1. Check this catalog for an OOTB connector → recommend it as the starting point.
2. Identify what the connector does NOT do for the target editorial flow.
3. If gaps are real, layer a custom Compact View embed for the gap-specific flows; keep the OOTB connector for the baseline.
4. Wire webhooks yourself for downstream sync; the connector won't.

See `bynder-marketplace-connectors` for the full skill treatment.

## Source

- Marketplace: https://marketplace.bynder.com/en-US/home
- Universal Compact View (the substrate for custom embeds): https://developers.bynder.com/universal-compact-view
