---
name: pf-html-to-pdf
description: >
  Renders each composed slide HTML to a single PDF page at 1920×1080, then
  merges them into deck.pdf. Uses headless Chromium via Bash. Validates the
  output has the right page count and resolution.
type: skill
plugin: presentation-factory
tags: [type/skill, plugin/presentation-factory, scope/specialty, topic/pdf-export]
status: active
---

## When to invoke

Called from the Ship stage orchestrator after the user has approved the composed slides.

## Required context

Toolchain: [`../../references/presentation-factory-toolchain.md`](../../references/presentation-factory-toolchain.md). Foundations: [`../../references/presentation-factory-foundations.md`](../../references/presentation-factory-foundations.md).

## What this skill does

### Stage 1: Verify dependencies

Bash check:
```bash
which chromium || which google-chrome || which chrome
which pdftk || which python3 # python3 + pypdf as fallback
```

If neither Chromium nor Chrome is found, surface the issue. macOS install:
```bash
brew install --cask chromium
```

### Stage 2: Render each slide to a single-page PDF

For each `slides/NN-template.html` in the WIP folder:

```bash
chromium --headless --disable-gpu \
  --no-margins \
  --print-to-pdf="WIP/[deck]/_pdf/slide-NN.pdf" \
  --virtual-time-budget=3000 \
  --window-size=1920,1080 \
  --hide-scrollbars \
  --run-all-compositor-stages-before-draw \
  "file://$PWD/WIP/[deck]/slides/NN-template.html"
```

Notes:
- `--virtual-time-budget=3000` waits 3 seconds for any JS (the chart template uses vanilla JS to render the SVG bars)
- `--no-margins` prevents Chrome's default print margins
- `--window-size=1920,1080` matches the slide canvas exactly
- `--hide-scrollbars` removes any scrollbar artifacts

### Stage 3: Adjust the page size

Chromium's default `--print-to-pdf` uses letter size. The HTML body is 1920×1080 px. With CSS `@page { size: ... }` in the template, we can force exact dimensions. The slalom.css tokens already include the 1920×1080 body sizing.

Add to the composed slide HTML at compose time (in the composer skill's pass — already covered in the composer), inside `<style>`:
```css
@page {
  size: 1920px 1080px;
  margin: 0;
}
```

If pages still come out wrong, fall back to Puppeteer / Playwright via Node script:

```javascript
// puppeteer-render.js
const puppeteer = require('puppeteer');
(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.setViewport({ width: 1920, height: 1080, deviceScaleFactor: 2 });
  await page.goto(`file://${slidePath}`, { waitUntil: 'networkidle0' });
  await page.pdf({
    path: outPath,
    width: '1920px',
    height: '1080px',
    printBackground: true,
    margin: { top: 0, right: 0, bottom: 0, left: 0 },
  });
  await browser.close();
})();
```

### Stage 4: Merge slide PDFs into deck.pdf

```bash
cd WIP/[deck]/_pdf
pdftk slide-*.pdf cat output ../deck.pdf
```

Fallback if pdftk not installed:
```python
# merge.py
from pypdf import PdfWriter
writer = PdfWriter()
for f in sorted(os.listdir("WIP/[deck]/_pdf")):
    writer.append(f"WIP/[deck]/_pdf/{f}")
writer.write("WIP/[deck]/deck.pdf")
```

### Stage 5: Validate output

```bash
# Count pages — must match slide count
pdfinfo WIP/[deck]/deck.pdf | grep "Pages:"
# Or via Python: PdfReader("deck.pdf").pages length
```

Also: visually spot-check the first slide by extracting its rendered preview:
```bash
pdftoppm -f 1 -l 1 -png -r 96 WIP/[deck]/deck.pdf WIP/[deck]/_preview
# Produces _preview-01.png — show to user as a sanity check
```

### Stage 6: Clean up

After the merged `deck.pdf` is in place, remove the per-slide intermediate PDFs in `_pdf/` to keep WIP tidy (or leave them for debugging — config flag).

## Output

| File | Path |
|---|---|
| Merged deck | `WIP/[deck]/deck.pdf` |
| Preview thumbnail of slide 1 | `WIP/[deck]/_preview-01.png` (optional) |
| Render log | `WIP/[deck]/render.log` |

## Anti-patterns

- **Don't** rely on `--print-to-pdf` page dimensions defaulting correctly — always set `@page` in CSS and viewport in the headless command
- **Don't** skip the virtual-time-budget — JS-rendered slides (chart template) will appear blank otherwise
- **Don't** merge without sorting — `slide-*.pdf` glob sorts alphabetically; using NN-prefixed names ensures correct order
- **Don't** delete the slide HTML files after PDF export — they're needed for PPTX conversion (next skill)
