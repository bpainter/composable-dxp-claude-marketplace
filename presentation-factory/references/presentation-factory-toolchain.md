# Presentation Factory — Toolchain

The named tools and commands the plugin's skills use. Cited by the skills, not re-explained.

## Brand ingestion (Client brand mode only)

### From a brand styleguide PDF

1. User attaches the styleguide PDF to the chat.
2. Skill `pf-brand-router` reads the PDF (LLM vision).
3. Extracts:
   - Primary, secondary, accent colors (hex)
   - Display font name and body font name
   - Logo files (page references)
   - Voice characteristics
   - Banned patterns
4. Writes `WIP/[deck]/brand/tokens.css` with derived CSS variables (HSL channel format).
5. Saves logo as SVG/PNG to `WIP/[deck]/brand/logo.svg` (or `.png`).

### From a `.com` crawl

1. User provides the client URL.
2. Skill uses `WebFetch` on the homepage + 1-2 sub-pages (services or about).
3. Parses for:
   - `:root` CSS custom properties (often present in modern sites)
   - Heading / body font from CSS or computed styles
   - Logo `<img src>` patterns
   - Hero color and accent patterns observed across pages
4. Writes the same `tokens.css` + `logo.svg` to `WIP/[deck]/brand/`.

Fallback: if either source is missing fonts, default to system serif + system sans and note in the brief that fonts need designer review.

## Content authoring

The Draft stage uses these skills in sequence per slide section:

| Step | Skill |
|---|---|
| Frame the POV | `consulting:consulting-management-consultant` (overall message arc), `consulting:consulting-digital-strategist` (digital POV), or domain skill per topic |
| Draft the copy | `marketing:marketing-copywriter` or domain skill |
| Specify imagery / infographic prompts | `design:design-imagery` (advice on prompt language) + `design:design-visualization` (advice on chart/diagram choice) |
| Pick icons | `design:design-iconography` (one family per deck) |
| Apply brand voice | `brand:brand-voice-tone` |
| Strip AI tells | `consulting:consulting-humanize` (MANDATORY final pass) |
| Taste check | `design:design-taste` (the four checks: swap / squint / signature / token) |

Output is a single `10_Content.md` file per deck with this shape:

```markdown
# Deck: [name]
# Style: [informational | presentational]
# Brand: [slalom | composable-dxp | client:[name]]

## Slide 1 — Title — template: title.html

**Eyebrow**: [optional]
**Title**: Build *better tomorrows* for [Client]
**Subtitle**: [optional one-liner]
**Footer**: [optional: presenter, date, audience]

**Speaker notes**: [3-4 sentences for the presenter]

---

## Slide 2 — Agenda — template: agenda.html

**Eyebrow**: TODAY
**Title**: What we'll *cover*
**Items**:
1. [Item one]
2. [Item two]
…

**Speaker notes**: …

---

## Slide 3 — Section Divider — template: section-divider.html

**Eyebrow**: SECTION 1
**Title**: The *problem* in three parts

**Speaker notes**: …

---

## Slide 4 — Image-Led — template: image-led.html

**Eyebrow**: CONTEXT
**Title**: The market is *shifting* fast
**Body**: [1-2 sentence support]

**Image prompt** (Higgsfield, 1920×1080):
> Wide documentary shot of a modern retail environment with customers using
> AR-equipped phones to scan products on shelves. Natural light, slight motion
> blur on shoppers. Warm-cool color contrast. No screens visible from camera angle.
> Mood: dynamic, curious, optimistic. Avoid stock-photo clichés.

**Speaker notes**: …

---

…continued for every slide…
```

## Image / video generation

**Higgsfield MCP** is the default. The skill `pf-asset-orchestrator` calls it.

Tool name patterns (already loaded in this session as deferred tools):
- `mcp__79eb9064-..._generate_image` — stills (nano banana 2, chatgpt image 2)
- `mcp__79eb9064-..._generate_video` — video (seedance 2)
- `mcp__79eb9064-..._job_display` — poll job status
- `mcp__79eb9064-..._show_medias` — list generated media in workspace

Workflow:
1. Read the `**Image prompt**` blocks from `10_Content.md`
2. Submit each to Higgsfield via `generate_image`
3. Poll until ready
4. Download to `WIP/[deck]/assets/images/slide-NN-image.png`
5. Update `10_Content.md` with the local path

## Infographics

**Napkin.ai** is the default. No MCP — manual or browser-driven. The skill writes a Napkin-ready prompt to `WIP/[deck]/assets/infographics/slide-NN-prompt.md`, then prompts user to paste into Napkin and save the result back to that folder.

Alternative for code-generated diagrams: Mermaid.js inline in HTML (process, sequence, flow). Recharts/Chart.js or hand-built SVG (data viz).

## Icons

**Flaticon** is the default. The skill writes the icon search query to the content file:

```markdown
**Icon**: Flaticon search "discovery / magnifying glass / linear" — pick one from Lucide-equivalent linear style pack
```

Cache to `WIP/[deck]/assets/icons/`. Lucide icons (npm `lucide-static`) are an alternative for code-rendered icons.

## HTML composition

Each slide is a self-contained HTML file. The composer skill `pf-slide-composer`:
1. Loads the template HTML from `resources/templates/[name].html`
2. Loads the brand tokens from the appropriate `resources/tokens/[brand].css` (or `WIP/[deck]/brand/tokens.css` for client)
3. Injects tokens into the template's `<style>` block (in place of the placeholder `/* TOKENS_HERE */` marker)
4. Replaces every `data-slot="[name]"` element's text content / src / href with the values from `10_Content.md`
5. Writes to `WIP/[deck]/slides/NN-[template].html`

The index page `deck.html` lists all slides for review with thumbnail rendering (each slide rendered scaled-down in an iframe or img).

## HTML → PDF export

Headless Chromium via Bash:

```bash
# Per-slide PDF render:
chromium --headless --disable-gpu \
  --no-margins \
  --print-to-pdf=slide-01.pdf \
  --virtual-time-budget=2000 \
  --window-size=1920,1080 \
  file://WIP/[deck]/slides/01-title.html

# Merge all slide PDFs into one:
pdftk slide-*.pdf cat output deck.pdf
# or via Python pypdf if pdftk unavailable
```

Alternatively, use Puppeteer or Playwright Node script for more control. Each slide renders to a single PDF page sized 16:9 at 1920×1080 (or 13.33×7.5 inches at 144 DPI for print-quality 1920px width).

## PDF → editable PPTX

The skill `pf-pdf-to-pptx` uses the PowerPoint MCP. It:

1. Creates a new presentation: `mcp__PowerPoint..._create_presentation`
2. For each PDF page (each slide):
   - Adds a slide
   - Re-parses the source HTML to extract:
     - Text content + bounding box from each `data-slot` element
     - Images and their bounding boxes
     - The gradient / decorative elements (added as PowerPoint shapes)
   - Adds each text element via `add_text_to_slide` (editable text frames at the right coordinates)
   - Adds each image via `insert_image` (movable raster assets)
   - Adds shapes/gradients as PowerPoint shapes (movable)
3. Saves: `mcp__PowerPoint..._save_presentation` to `WIP/[deck]/deck.pptx`

Why this approach over a "PDF-as-image-per-slide" hack: text remains editable, assets remain movable, brand stays consistent. The PDF is the proof-of-render; the PPTX is the editable deliverable.

If the PowerPoint MCP can't faithfully reconstruct a specific complex slide (e.g. a hand-built SVG diagram), the fallback is: insert the slide as a high-resolution image, with text labels overlaid as editable text frames near where they appear.

## Promotion

Final deck location depends on the originating intent:

| Intent | Promote to |
|---|---|
| Internal training / playbook | `40_Library/Decks/[deck-name]/` |
| Pursuit deliverable | `20_Proposals/Active/[Pursuit]/Final/[deck-name].pptx` |
| Conference talk | `30_Talks/[YYYY]/[Event]/[deck-name].pptx` |
| Practice / solution deck | `10_Practice/Offerings/[Solution]/Decks/[deck-name].pptx` |
| Client-facing one-off | `20_Proposals/Active/[Client]/Decks/[deck-name].pptx` |

Promotion archives the WIP folder afterward.

## Browser-based MCPs available

Loaded in this session:
- `mcp__Claude_in_Chrome__*` — browser automation (useful for Napkin.ai manual flow)
- `mcp__PowerPoint__By_Anthropic__*` — PowerPoint MCP
- `mcp__Word__By_Anthropic__*` — Word MCP (not used in this plugin)
- `mcp__79eb9064-...` — Higgsfield MCP
- `mcp__67054d52-...` — Figma MCP (alternative for diagrams)

## Dependency notes

The plugin assumes:
- Chromium installed (for HTML → PDF). macOS: `brew install --cask chromium` or use Chrome if present
- `pdftk` or Python `pypdf` available (for merging slide PDFs)
- PowerPoint MCP available in the session
- Higgsfield MCP authenticated and credits available
- Fonts present at the documented paths
