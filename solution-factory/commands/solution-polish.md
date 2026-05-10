---
description: Run Stage 3 (Polish) — generate visuals via the canonical toolchain (Napkin / Figma / PowerPoint native / higgsfield / Flaticon), format markdown to PPTX/PDF/HTML, apply brand. Optional Composable DXP microsite.
---

Invoke the `solution-factory-polish` skill. The user wants to convert the markdown deliverables into formatted output.

The skill runs 4 steps:

1. **Visual generation** — walks every `[VISUAL: ...]` tag in markdown drafts and routes per the canonical toolchain (`references/solution-factory-toolchain.md`):
   - Napkin for diagrams (corporate style)
   - Figma + Mermaid for flowcharts
   - PowerPoint native for charts
   - higgsfield (nano banana default, chatgpt 2 fallback) for imagery
   - higgsfield (Seedance) for video/animation
   - Flaticon for icons (lined or colored, never hand-drawn)

2. **Format to output** — uses PowerPoint MCP for 16:9 PPTX, Word MCP for 8.5×11 PDF. Files sit at top level of WIP folder alongside markdown sources.

3. **Apply brand** — Slalom default OR Composable DXP secondary based on intake. Avenir Next LT Pro body for Slalom; em-dash banned + bold-Lora-no-italic display for TFIC.

4. **Optional microsite** (Composable DXP Complex tier only) — Vercel password-protected static site OR SharePoint deployment. AskUserQuestion confirms whether to deploy.

Pre-checks higgsfield credit balance before image-heavy generation. Pauses for visual review before format-to-output. Surfaces brand violations (em-dash in TFIC, embedded illustrated icons surviving in Slalom default).
