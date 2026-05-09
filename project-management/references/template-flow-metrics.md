---
type: reference
project: skills-library
scope: plugin
plugin: project-management
tags: [type/reference, plugin/project-management, scope/template, topic/flow, topic/metrics, source/reinertsen]
status: active
---

# Template — Flow Metrics & Economic Framework

**Source:** Reinertsen, *The Principles of Product Development Flow* (2009) — see [`book-product-development-flow.md`](book-product-development-flow.md). Use principle numbers (E1, Q12, B1, etc.) when citing.

A diagnostic worksheet that translates "we feel slow" into measurable flow metrics, prioritizes work using economic principles (Cost of Delay, CD3), and identifies the highest-leverage lever to pull. Use it when sprints feel theoretical, when WIP is climbing, when teams are utilizing 95%+ but missing dates, or when leadership is treating utilization as the metric to maximize.

## When to use it

- Quarterly flow diagnostic
- When sprints feel theoretical (planned but not delivered)
- When the team feels busier but ships less
- When leadership pushes to "increase utilization"
- Before a major reprioritization (CD3 workshop)
- When introducing kanban/WIP limits to a team

## The 5-section worksheet

### Section 1: Cost of Delay (CoD) and CD3

For each major work item / Initiative / project:

```markdown
# CD3 worksheet

| Item | CoD per week ($/wk) | Duration (wks) | CD3 = CoD ÷ Duration | Rank |
|---|---|---|---|---|
| Feature A | $50K | 4 | $12.5K/wk² | 3 |
| Initiative B | $200K | 12 | $16.7K/wk² | 2 |
| Project C | $80K | 2 | $40K/wk² | 1 |

## CoD components used (be explicit)
- Revenue lost (delayed acquisition / activation / conversion):
- Opportunity cost (alternative value foregone):
- Cost of risk realized (penalty for late, churn, etc.):

## Estimate confidence
- Order-of-magnitude (1×, 3×, 10×, 30× buckets) — typical for early-stage CD3
- Spreadsheet (estimated values) — for prioritization conversations
- Modeled (real numbers) — for major capital decisions
```

CD3 dominates raw CoD because shorter-duration items at similar CoD compound impact faster. The Reinertsen diagnostic: when work items have a 50:1 spread in CD3 (common in real backlogs), most prioritization debate is noise relative to the actual economic stakes.

### Section 2: Flow metrics (Little's Law applied)

Cycle time = WIP ÷ throughput. Measure all three; the diagnostic comes from the ratio.

```markdown
# Flow metrics — [team/project]

## Cycle time (per item, type-by-type)
- Story (avg): __ days
- Bug: __ days
- Spike: __ days

## WIP (per stage)
- Backlog (not counted in WIP)
- In-flight (designing): __
- In-flight (engineering): __
- In-flight (review): __
- In-flight (testing): __
- Total active WIP: __

## Throughput (last 4 weeks, count of items completed per week)
- Week 1: __
- Week 2: __
- Week 3: __
- Week 4: __
- Average: __

## Sanity check (Little's Law)
Cycle time should ≈ WIP ÷ throughput.
If observed cycle time >> calculated, expedites and queue jumping are inflating real cycle time.
```

### Section 3: Utilization vs. queue cost (Reinertsen Q3)

The exponential utilization curve. Plot or estimate:

```markdown
# Utilization curve

At 60% utilization: queue time is roughly 1.5× service time
At 80% utilization: queue time is roughly 4× service time
At 90% utilization: queue time is roughly 9× service time
At 95% utilization: queue time is roughly 19× service time
At 98% utilization: queue time is roughly 49× service time

(approximate M/M/1 queue behavior)

Current team utilization: __%
Estimated queue cost as multiplier of service time: __×

Translation: if engineering is at 95% utilization, every new request waits 19× the actual work time before being touched. This is why "everyone is busy" doesn't equal "we ship faster."
```

### Section 4: WIP-limit recommendation

```markdown
# WIP-limit recommendation

## Current state
| Stage | WIP today | WIP limit (none, soft, hard) |
|---|---|---|
| Designing | | |
| Engineering | | |
| Review | | |
| Testing | | |

## Recommended state (start aggressive — Reinertsen W1: 10:1 payoff for halving WIP)
| Stage | New WIP limit | Rationale |
|---|---|---|
| Designing | | |
| Engineering | | (typical: # of engineers ÷ 2 to ÷ 1) |
| Review | | (typical: 2–4 max) |
| Testing | | |

## Predicted impact
- Cycle time: __ → __ days
- Throughput: __ items/week (likely flat or slight increase initially; rises as queues drain)
- WIP visible to leadership: __ → __

## Rollout
- Week 1: announce limits; start enforcing softly (alerts when over)
- Week 2: enforce strictly (no new WIP until under)
- Week 3-4: review actual cycle time; adjust limits down if cycle time hasn't fallen
```

### Section 5: Cadence design

```markdown
# Cadence design

## Current meeting stack
| Meeting | Frequency | Duration | Attendees | Decision frequency |
|---|---|---|---|---|
| Daily standup | Daily | 15 min | 8 | 0 |
| Sprint planning | 2 wk | 2 hr | 8 | High |
| Sprint review | 2 wk | 1 hr | 12 | Medium |
| Retro | 2 wk | 1 hr | 8 | Low |
| Quarterly review | Q | 4 hr | 25 | High |

## Diagnostic
- Are decisions stuck waiting for a cadence point that's too far away?
- Are status meetings producing decisions, or just status?
- What's the longest decision-blocker right now? Is the cadence the cause?

## Recommendation
- Increase cadence on: [X] (because: ___)
- Decrease cadence on: [Y] (because: ___)
- Add cadence: [Z] (because: ___)
- Kill: [W] (because the work happens elsewhere)

## Reinertsen F9 reminder
Daily cadence beats weekly by ~3× for status-and-correction work because of harmonic effects on cycle time.
```

## The variability split (Reinertsen V1–V14)

Variability is not always waste. Two paths matter:

| Variability type | Example | Treat as | Reduce or preserve? |
|---|---|---|---|
| **Value variability** | Discovery experiments, where the bet is the value of the unknown | Information generator | Preserve |
| **Cost variability** | Random delays in CI pipeline, ticket queue chaos | Wasted queue time | Reduce |

Lean dogma's "eliminate variability" advice applies to cost variability. Applying it to value variability eliminates the experiments where learning lives.

When asked "should we reduce variability?", first ask: which variability?

## Decentralize vs. centralize map (Reinertsen D1–D11)

For each recurring decision in the team, score:

| Decision | Frequency | Impact | Information location | Push to edge or centralize? |
|---|---|---|---|---|
| Branch naming | High | Low | Team | Edge |
| New library introduction | Medium | Medium | Team | Edge with notify |
| Database schema change | Medium | High | Team + adjacent | Centralize (architecture review) |
| Quarterly priority shifts | Low | Very high | Leadership | Centralize |

The pattern: high-frequency + low-impact + local-info → push to edge. Low-frequency + high-impact + dispersed-info → centralize.

## Pitfalls (from book-product-development-flow.md)

- **Queue everything.** Treating every workflow as a queue when some are small enough to be informal. Queue thinking is an analytical tool, not a religion.
- **Suppress all variability.** As above — kills the experiments where learning lives.
- **Optimize utilization.** The single most common antipattern. Push utilization above 80% and watch queues explode.
- **CoD as priority list.** CD3 ranks; humans decide. The framework is input to a conversation, not a command.
- **WIP limits without team buy-in.** Enforced WIP limits without context create resentment and gaming. Pair with the why.
- **Cadence theater.** Meetings that exist because they're on the calendar, not because they generate decisions. Kill or reshape.
- **Mistaking throughput for output.** Throughput in items/week is a flow metric, not a value metric. High throughput on the wrong items is the build trap (`product-management-product-manager`).

## Hand-off downstream

- **CD3 ranking** → feeds prioritization for `project-management-sprint-planning` and `product-management-product-manager`
- **WIP limits** → enforced in the team's tool (Jira, Linear, Asana)
- **Cadence redesign** → integrated into the team's calendar
- **Variability classification** → informs which experiments stay loose and which get tightened

## Skills that use this template

- `project-management-flow-engineer` (primary)
- `project-management-sprint-planning` (CD3 ranking + WIP limits)
- `project-management-project-manager` (engagement-level flow governance)
- `product-management-product-manager` (CD3 for product Initiatives)
