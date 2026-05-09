---
type: reference
project: skills-library
scope: plugin
plugin: behavioral-economics
tags: [type/reference, plugin/behavioral-economics, scope/template, topic/prioritization, source/slalom]
status: active
---

# Template — Use-Case Scoring & Prioritization

**Source:** Slalom Behavioral Design Model, pp. 37–38. See `2026-05_Slalom-Behavioral-Design-Model.md` in `40_Library/Solution_Briefs/`.

After Stage 3 generates a portfolio of candidate interventions, this template scores each one on **Customer Value × Business Value** and plots them on a 2×2-style grid to decide what to test first, what to deprioritize, and what to reshape.

## When to use it

- After the choice architecture is mapped and 3+ candidate interventions are on the table
- Before committing to a single A/B test or pilot — the scoring forces explicit comparison
- As a workshop deliverable to align stakeholders on prioritization
- Whenever the team is tempted to test "all of them" — the grid makes trade-offs visible

## The four customer-value criteria

Each scored 0–3 (0 = no change, 1 = some, 2 = moderate, 3 = significant).

| Criterion | What "no change" looks like | What "significant" looks like |
|---|---|---|
| **Clear and Relevant Information Shown** | Information is unclear and irrelevant; causes confusion | Information is exceptionally clear and directly relevant; no confusion possible |
| **Aligned with Behavioral Drivers** | Doesn't align with user's behavioral drivers, preferences, or motivations | Perfect alignment; addresses all preferences and motivations |
| **Simplicity & Optimized Info** | Overly complex, not optimized; difficult to process | Exceptionally simple and fully optimized; easy to understand and act on |
| **Positive Economic Impact** | Negative financial impact — increases stress, impedes financial benefit | Significant economic impact; powerful nudges drive substantial cost savings or financial improvements |

## The four business-value criteria

| Criterion | What "no change" looks like | What "significant" looks like |
|---|---|---|
| **Strategic Alignment** | Doesn't align with strategic goals, mission, or values | Perfect alignment with strategic goals, mission, and values |
| **Enhanced Customer Engagement** | Little to no impact; doesn't resonate with customer motivations | Significant enhancement in engagement; deep resonance with customer motivations |
| **Long-term Relationship Building** | Hinders long-term relationship building; decreased loyalty and trust | Substantial success in building long-term relationships, loyalty, trust, and retention |
| **Financial Impact** | Negative financial impact — decreased revenue and profitability | Significant positive financial impact — increased revenue, profitability, conversion, consumption |

## The 2×2 grid

Plot interventions on a 12×12 grid (max score on each axis = 4 criteria × 3 = 12).

```
                                                  12
                                                   │
                                Consider how to    │   Most promising
                                add customer       │   interventions.
                                value.             │   Test first.
                                                   │
                Business        ───────────────────┼───────────────────
                  Value                            │
                                Not enough         │   Consider whether
                                value.             │   this is the right
                                Deprioritize.      │   intervention, or if
                                                   │   it's at the wrong step
                                                   │   in the flow.
                                                   │
                                                   0──────────── 12
                                                       Customer Value
```

| Quadrant | Action |
|---|---|
| **Top-right** (high customer + high business value) | **Test first.** These are the most promising interventions. |
| **Top-left** (low customer + high business value) | Consider how to modify the intervention to provide more customer value. If you can't, deprioritize — high-business-value-only interventions tend to drift into sludge. |
| **Bottom-right** (high customer + low business value) | Consider whether this is the right intervention, or whether the business framing is wrong (or whether the intervention is at the wrong step in the flow). |
| **Bottom-left** (low both) | Deprioritize. |

## Template (copy this into your deliverable)

```markdown
# Use-Case Scoring — [Initiative Name]

## Interventions evaluated
| ID | Intervention | Source (Choice Arch step / Tactic) |
|---|---|---|
| 1 | [name] | [link to template-choice-architecture step] |
| 2 | [name] | [link] |
| 3 | [name] | [link] |
| ... | | |

## Customer-Value Scoring

| Intervention | Clear & Relevant Info | Aligned w/ Behavioral Drivers | Simplicity & Optimized Info | Positive Economic Impact | **Total (0-12)** |
|---|---|---|---|---|---|
| 1. | | | | | |
| 2. | | | | | |
| 3. | | | | | |

## Business-Value Scoring

| Intervention | Strategic Alignment | Enhanced Customer Engagement | Long-term Relationship Building | Financial Impact | **Total (0-12)** |
|---|---|---|---|---|---|
| 1. | | | | | |
| 2. | | | | | |
| 3. | | | | | |

## Quadrant Plot
[A 12x12 grid with each intervention plotted at its (Customer-Value, Business-Value) coordinate. Tools: Miro, FigJam, Excel scatter chart, or hand-drawn.]

## Decisions
| Intervention | Quadrant | Decision |
|---|---|---|
| 1. | Top-right | Test first |
| 2. | Top-left | Modify to add customer value (specifically: ...) |
| 3. | Bottom-right | Right idea, wrong step — try at Step N instead |
| ... | | |

## Test queue
Order the interventions selected for testing. Each entry feeds Use-Case Validation (Stage 4).
1. [Intervention name] — testing to be detailed in `template-use-case-validation.md` instance
2. [Intervention name] — ...
```

## Worked example — Co-morbid patient enrollment redesign

Six interventions on the table. After scoring (0-3 each):

| Intervention | Cust V | Biz V | Quadrant | Decision |
|---|---|---|---|---|
| 1. Personalized default care plan | 11 | 10 | Top-right | **Test first** |
| 2. Reduce options 24 → 3 (partitioning) | 10 | 9 | Top-right | **Test second** |
| 3. AI self-service replacing phone tree | 9 | 11 | Top-right | **Test third** |
| 4. Loss-aversion framing of benefit choice | 8 | 7 | Top-right | Bundle with #1 |
| 5. Sunk-cost reminder ("you've already started") | 4 | 8 | Top-left | Modify — current frame feels manipulative |
| 6. Detailed all-options list with attribute table | 6 | 3 | Bottom-right | Wrong intervention; replaced by #2 |

## Pitfalls

- **Score inflation.** Teams default to scoring every intervention 2–3. Use the descriptions for "0" and "1" to anchor scoring. If everything is "high impact," the grid is useless.
- **Single scorer.** Score in a group of 3+, then discuss disagreements. Lone scoring drifts toward the scorer's preferred intervention.
- **Treating the grid as a final decision.** The grid is a forcing function for conversation, not a verdict. Bottom-right interventions often have a story that the grid doesn't capture.
- **Skipping the customer side when business case is strong.** A high-business-value intervention with low customer value will eventually erode trust and undo the business gain. The two axes are coupled in the long run.
- **Hidden ethics violations.** A tempting top-right intervention may exploit a bias in a way that fails the Stage 5 ethics gate. Run the gate alongside scoring, not after.

## Hand-off downstream

- Top-right interventions feed **Use-Case Validation** (`template-use-case-validation.md`) — turn each into a hypothesis-driven test
- All interventions feed the **Ethics gate** (Slalom p. 43, also covered in the validation template)

## Skills that use this template

- `behavioral-economics-choice-architect` (primary)
- `behavioral-economics-behavioral-economist` (cross-domain)
- `behavioral-economics-organizational-behavior-specialist` (workplace interventions)
- `consulting-management-consultant` (engagement-level prioritization)
- `project-management-project-manager` (sprint planning input when interventions become work items)
