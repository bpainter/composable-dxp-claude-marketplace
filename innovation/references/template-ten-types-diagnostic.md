---
type: reference
project: skills-library
scope: plugin
plugin: innovation
tags: [type/reference, plugin/innovation, scope/template, topic/doblin, topic/ten-types]
status: active
---

# Doblin Ten Types Diagnostic — Template

Two-part working canvas for using Doblin's Ten Types of Innovation (Keeley et al., 2013): (1) diagnose where the org is currently competing, and (2) design type-combinations that create sustained advantage.

## Part 1 — Diagnostic heatmap

For each of the ten types, score the org's *current* competitive position:

- **Investment intensity (0–5)** — capital, talent, attention being spent on this type
- **Distinctiveness (0–5)** — material difference from category benchmarks
- **Customer-perceptible (0–5)** — does the customer experience the difference?

Multiply for a **competitive weight** by type. Plot the heatmap.

```
═════════════════════════════════════════════════════════════════
  TEN TYPES DIAGNOSTIC — [Org / Practice]   [Date]
═════════════════════════════════════════════════════════════════

CONFIGURATION (internal)         Investment │ Distinct │ Perceptible │ Weight
─────────────────────────────    ──────────┼──────────┼─────────────┼───────
1. Profit Model                  [0–5]     │ [0–5]    │ [0–5]       │ [×]
2. Network                       [0–5]     │ [0–5]    │ [0–5]       │ [×]
3. Structure                     [0–5]     │ [0–5]    │ [0–5]       │ [×]
4. Process                       [0–5]     │ [0–5]    │ [0–5]       │ [×]

OFFERING                         Investment │ Distinct │ Perceptible │ Weight
─────────────────────────────    ──────────┼──────────┼─────────────┼───────
5. Product Performance           [0–5]     │ [0–5]    │ [0–5]       │ [×]
6. Product System                [0–5]     │ [0–5]    │ [0–5]       │ [×]

EXPERIENCE                       Investment │ Distinct │ Perceptible │ Weight
─────────────────────────────    ──────────┼──────────┼─────────────┼───────
7. Service                       [0–5]     │ [0–5]    │ [0–5]       │ [×]
8. Channel                       [0–5]     │ [0–5]    │ [0–5]       │ [×]
9. Brand                         [0–5]     │ [0–5]    │ [0–5]       │ [×]
10. Customer Engagement          [0–5]     │ [0–5]    │ [0–5]       │ [×]
```

### Pattern interpretation

After scoring, surface the dominant pattern:

- **Product-only** — high Type 5, near-zero on 1, 7, 10. Vulnerable to copying. Most common pattern.
- **Channel-led, product-thin** — high 8, low 5. Distribution advantage that erodes when product gaps surface.
- **Brand without backing** — high 9, low 3, 6, 7. Reputation outpaces operating model.
- **Configuration-deep** — high 1, 2, 3, 4. Often most defensible (Costco, Southwest pattern). Hardest to articulate.
- **Experience-led** — high 7, 8, 9, 10, lower 5. Hospitality, retail, services pattern.

### Diagnostic prompts

1. **What's the dominant pattern?** Most enterprises score 1–2 types high and 6–7 types invisible.
2. **Which Configuration types are under-invested?** Configuration-side innovation (1–4) is hardest to copy. Most orgs ignore it.
3. **Which Experience types are weakest?** Experience-side innovation (7–10) is most customer-perceptible.
4. **Where are competitors strong that we are weak?** This is where structural threat lives.
5. **Where could we be distinctive without massive investment?** Often Customer Engagement (10) is the cheapest place to differentiate.

## Part 2 — Type-combination canvas (generative)

Once diagnostic surfaces gaps, brainstorm across all ten types for a target outcome:

```
═════════════════════════════════════════════════════════════════
  TEN TYPES COMBINATION CANVAS
  Target: [customer outcome / job to be done]
═════════════════════════════════════════════════════════════════

Type 1 — Profit Model:
  •
  •
  •

Type 2 — Network:
  •
  •
  •

Type 3 — Structure:
  •
  •
  •

Type 4 — Process:
  •
  •
  •

Type 5 — Product Performance:
  •
  •
  •

Type 6 — Product System:
  •
  •
  •

Type 7 — Service:
  •
  •
  •

Type 8 — Channel:
  •
  •
  •

Type 9 — Brand:
  •
  •
  •

Type 10 — Customer Engagement:
  •
  •
  •

═════════════════════════════════════════════════════════════════
SELECTED COMBINATION (3–5 types):
  ▶
  ▶
  ▶
  ▶

WHY THIS COMBINATION:

WHY THIS IS HARD TO COPY:
═════════════════════════════════════════════════════════════════
```

### Combination patterns that produce predictable system effects

- **Type 1 + Type 2** — platform / marketplace plays
- **Type 3 + Type 4** — operating-model innovation; flag explicitly when bet looks like an OM play, not a product
- **Type 6 + Type 7** — features become complete experiences ("make it stick")
- **Type 9 + Type 10** — community-led / DTC plays
- **Type 8 + Type 10** — bypassing intermediaries via direct relationship
- **Type 5 + Type 6 + Type 7** — premium experience triple (Apple-like)

### Three-type wins (canonical examples to seed brainstorming)

| Company | Combination | Why it stuck |
|---|---|---|
| Apple iPod era | 5 + 6 + 9 + 7 | Product + System + Brand + Service: decade-long lead |
| Costco | 1 + 4 + 8 + 2 | Membership + low-SKU process + warehouse + member-supplier net |
| Cirque du Soleil | 5 + 8 + 9 + 10 | Reinvented product, residency channel, brand, community |
| Method | 9 + 8 + 5 + 10 | Brand + Target retail + formulation + community |
| Stripe | 5 + 4 + 7 + 10 | Product + dev-friendly process + concierge support + community |

## Anti-pattern: feature accumulation

If the canvas keeps producing more Type 5 ideas, force the team away from Product Performance and into Configuration types. The default cognitive bias of product orgs is to innovate on features — the empirical reality is that combination innovations win.

## Hand-off

Output of this template feeds:

- `innovation-typologist` — owns this work
- `innovation-strategist` — uses combination output to set ambition mix
- `innovation-monetization-strategist` — Type 1 (Profit Model) is their domain
- `brand-strategist` (sister plugin) — Type 9 (Brand) work
- `cx-` plugin — Type 10 (Customer Engagement) work
