---
description: Compose a brand guidelines deliverable — the document that consolidates strategy, naming, voice, identity, applications, and governance

# Project context
type: command
project: skills-library
plugin: brand
tags: [type/command, plugin/brand]
status: active
---

Compose a brand guidelines deliverable.

Steps:

1. **Inventory inputs.** Confirm the source content exists:
   - Strategy from [[brand-strategist]] — positioning, audience, narrative.
   - Naming from [[brand-naming]] — chosen name, rationale.
   - Voice from [[brand-voice-tone]] — voice attributes, tone register, filler-words ban.
   - Identity from [[brand-identity-system]] — logo, color, type, photography direction, iconography style.
   - Photography / illustration direction from [[design-imagery]].

2. **Fill gaps.** If anything is missing, route back to the relevant skill. Don't compose around incomplete content.

3. **Pick scale** via [[brand-guidelines-composer]]:
   - **One-pager** (small / scrappy brand): 1 page covering 3-attribute voice, logo + 3 colors + 2 fonts, one do/don't, one contact.
   - **Standard** (most B2B brands): 6–12 pages, all 9 chapters at moderate depth.
   - **Enterprise** (large brand, multi-product): 30–60 pages or full brand portal.

4. **Pick format:**
   - **PDF** (8.5×11 or A4) — default for distribution. Use [[design-document]] for layout, then `pdf` for generation.
   - **Web-published** (HTML / Notion / Confluence / brand portal) — living document, easier to update.
   - **Figma file** — designer-facing companion to the document.
   - **Mixed** — most enterprise.

5. **Outline the document** following the standard 9 chapters from [[brand-guidelines-composer]]:
   1. Brand foundation.
   2. Verbal identity.
   3. Logo.
   4. Color.
   5. Typography.
   6. Imagery and illustration.
   7. Motion (if applicable).
   8. Applications (web, mobile, deck, document, social, email).
   9. Governance.

6. **Compose each chapter** by lifting content from source skills. Don't rewrite from scratch — reference the source content.

7. **Add governance.** Document:
   - Brand owner (specific role / team).
   - Approval workflow.
   - Asset storage location.
   - Drift-flagging process.
   - Refresh cadence.

8. **Hand off:**
   - For PDF: route to [[design-document]] for visual layout, then `docx` and `pdf` skills for file generation.
   - For web-published: route to the publishing workflow (Notion / Confluence / brand portal).
   - For Figma: hand off to design-execution partner.

9. **Review with brand owner** before publishing.

Output structure (for a standard guidelines document):

```
# {Brand} Brand Guidelines — Version {X.X}

## 1. Brand foundation
- Mission, vision, values
- Personality and voice attributes (preview)
- Audiences
- Positioning summary

## 2. Verbal identity
- Tagline / positioning statement
- Voice (4 attributes with do/don't)
- Tone register
- Naming conventions
- Glossary
- Filler-words ban

## 3. Logo
- Primary mark + variants
- Clearspace and minimum size
- On-light / on-dark / on-color rules
- Misuse examples

## 4. Color
- Primary / secondary / neutrals / functional
- Tokens with contrast pairings
- Dark-mode rules

## 5. Typography
- Display / text / mono families
- Type scale
- Pairing and weight rules
- License coverage

## 6. Imagery and illustration
- Photography direction
- Illustration style
- Iconography style

## 7. Motion (if applicable)
- Easing curves
- Duration scale
- Reduced-motion behavior

## 8. Applications
- Web / mobile / deck / document / social / email examples

## 9. Governance
- Brand owner
- Approval workflow
- Asset storage
- Drift-flagging
- Refresh cadence
```
