---
description: Run only Stage 3 (Polish) — generate visuals via the canonical toolchain, format markdown to PPTX/PDF, apply brand, confirm + produce flagged companions.
---

Invoke the `proposal-factory-polish` skill. The user wants to convert finished markdown into formatted output and produce any confirmed companions. The skill:

1. **Generate visuals** routed by the canonical toolchain:
   - Diagrams → Napkin.ai (corporate style for Slalom default; corporate/clean for Composable DXP)
   - Flowcharts / sequence diagrams → Figma + Mermaid
   - Charts → PowerPoint native
   - Imagery → higgsfield (nano banana default; chatgpt 2 fallback)
   - Animations / video segments → higgsfield Seedance
   - Icons → Flaticon (lined OR colored, one style across the proposal, no hand-drawn)

2. **Format to output by variant:**
   - SPRO / Small / Large-Slides → 16:9 PPTX via PowerPoint MCP
   - Large-Document → 8.5×11 PDF via Word MCP

3. **Apply brand:**
   - Slalom default → `70_Templates/Slalom_Brand_Assets/Slalom_PowerPoint_16x9_Aug2025.potx`
   - Composable DXP secondary → same base + TFIC tokens + voice rules (no em-dashes, no marketing-speak verbs)

4. **Confirm companions** with AskUserQuestion for each flagged at intake. For confirmed companions, hand off to `proposal-factory-companion-router` which:
   - **POC** → Website Factory plugin (Claude Design default; v0 or Figma Make alternative) → Vercel deploy with password
   - **Walkthrough video** → higgsfield (visuals) + Elevenlabs (voiceover) + editing
   - **Microsite** → design + frontend skills → Vercel deploy with password

5. **Speaker notes** (slide variants) — generate talk-track for the lead presenter.

Output: finalized files (PPTX / PDF) at the top level of `WIP/[ClientName_Descriptor]/`, alongside markdown sources. Plus companion outputs in `assets/companions/`. Self-review pause before Ship.
