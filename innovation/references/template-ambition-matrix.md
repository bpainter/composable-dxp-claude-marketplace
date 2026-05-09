---
type: reference
project: skills-library
scope: plugin
plugin: innovation
tags: [type/reference, plugin/innovation, scope/template, topic/ambition-matrix]
status: active
---

# Innovation Ambition Matrix — Template

Risk-based portfolio map (Nagji & Tuff, *Managing Your Innovation Portfolio*, HBR May 2012). Pairs with the Three Horizons canvas — Three Horizons sorts by *time*, Ambition Matrix sorts by *risk*. They're not the same axis.

## When to use

- Portfolio audits (alongside Three Horizons canvas)
- "Are we taking enough risk?" diagnostics
- Investor / board narrative on innovation profile
- M&A and partnership rationale work

## The matrix

```
                          OFFERING
              Existing offering   ←→   New offering
            ┌────────────────────┬────────────────────┐
   Existing │                    │                    │
   market   │       CORE         │    ADJACENT        │
   /        │   (incremental)    │   (extension)      │
   customer │                    │                    │
            ├────────────────────┼────────────────────┤
   New      │                    │                    │
   market   │     ADJACENT       │  TRANSFORMATIONAL  │
   /        │   (extension)      │   (disruptive)     │
   customer │                    │                    │
            └────────────────────┴────────────────────┘
                          MARKET / CUSTOMER
```

Three zones:

- **Core** — improvements to existing offerings for existing customers. Lowest risk; often best ROI per bet; cumulatively bounded growth potential.
- **Adjacent** — extension along one axis (new offering OR new customer, not both). Higher risk; growth potential proportionate.
- **Transformational** — new offerings *and* new customers/markets. Highest risk; highest potential; new business creation.

## Default allocation

Nagji & Tuff's empirical finding from high-performing portfolios:

| Zone | Capital % | Returns % |
|---|---|---|
| Core | ~70% | ~10% |
| Adjacent | ~20% | ~20% |
| Transformational | ~10% | ~70% |

The returns ratio inverts the capital ratio. Most long-run value comes from the smallest zone.

## Mapping current bets

For each active initiative, classify across the matrix:

| Initiative | Offering axis | Market axis | Zone | Investment |
|---|---|---|---|---|
| | Existing / New | Existing / New | Core / Adjacent / Transformational | $[X] |

Then aggregate:

```
Capital deployed by zone:
  Core:             $[X]M  ([N]%)   Target: 70%
  Adjacent:         $[X]M  ([N]%)   Target: 20%
  Transformational: $[X]M  ([N]%)   Target: 10%
```

## Diagnostic prompts

1. **Risk profile.** What is the org's actual risk profile vs. its stated mandate? "We're an innovator" + 95% Core capital = mandate confusion.
2. **Adjacent gap.** Many portfolios have Core and a few Transformational moonshots, with little Adjacent. Adjacent is often where the real growth lives.
3. **Transformational depth.** If there are Transformational bets, are they real (capitalized, sponsored, gated for learning) or window-dressing?
4. **Type-pairing.** Cross-reference with Doblin Ten Types (`template-ten-types-diagnostic.md`). Most Core innovation is product-performance only; most Transformational wins combine 3+ types.
5. **Customer-known vs. customer-unknown.** New-customer columns require fundamentally different validation — start from JTBD discovery, not analog of current customer behavior.

## Three-horizons crosswalk

The two frames don't fully overlap. Be explicit:

- An **H1 (0–12 mo) bet** can be Core *or* Adjacent (a quick adjacent extension is plausible) — rarely Transformational
- An **H2 (1–3 yr) bet** is most often Adjacent, sometimes Transformational
- An **H3 (3+ yr) bet** is usually Transformational, sometimes Adjacent

Use the Ambition Matrix to *risk-classify* each bet, then the Three Horizons canvas to *time-stage* it. The combination tells you which governance, which value math, and which kill criteria fit.

## Hand-off

Pair with:

- `template-three-horizons-canvas.md` — time-based portfolio view
- `template-ten-types-diagnostic.md` — for type-combination depth on Adjacent/Transformational bets
- `template-value-engineering-canvas.md` — value math (NPV for Core, real options for Transformational)

Skill owners:

- `innovation-strategist` — uses for ambition-mix decisions
- `innovation-portfolio-architect` — uses to differentiate gate criteria across zones
- `innovation-typologist` — uses to design type-combinations for Adjacent / Transformational bets
