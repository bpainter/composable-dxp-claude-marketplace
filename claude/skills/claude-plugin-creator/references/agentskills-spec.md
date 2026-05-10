# Agent Skills canonical spec digest

The Agent Skills specification at https://agentskills.io is the authoritative format for `SKILL.md` files. This file digests the parts of the spec and best-practices guides most relevant to authoring skills for Claude plugins.

Sources:

- Specification: https://agentskills.io/specification
- Best practices: https://agentskills.io/skill-creation/best-practices
- Optimizing descriptions: https://agentskills.io/skill-creation/optimizing-descriptions
- Evaluating skills: https://agentskills.io/skill-creation/evaluating-skills
- Adding skills support (client implementor's view): https://agentskills.io/client-implementation/adding-skills-support

## Canonical SKILL.md format

```markdown
---
name: my-skill-name
description: >
  One paragraph describing when this skill applies. Use imperative phrasing
  ("Use this skill when..."). Include trigger phrases the user might say.
license: MIT
metadata:
  type: skill
  project: <project-name>
  plugin: <plugin-name>
  tags: [type/skill, plugin/<plugin>, topic/<topic>]
---

## Section heading (H2)

Body content...
```

### Required frontmatter fields

| Field | Type | Constraint |
|---|---|---|
| `name` | string | 1–64 chars, lowercase a-z and hyphens, no consecutive hyphens, must match parent directory name |
| `description` | string | 1–1024 chars per spec. **Cowork installer rejects above ~550 — target ≤ 441 for Cowork compatibility.** |

### Optional spec-defined frontmatter fields

| Field | Type | Purpose |
|---|---|---|
| `license` | string (SPDX identifier) | Skill license |
| `compatibility` | string or object | Environment requirements (Python version, OS, etc.) |
| `metadata` | mapping | Custom keys nested here for namespace isolation |
| `allowed-tools` | list of strings | Experimental: limit which tools the skill can use |

### Custom keys

Per the spec: "we recommend making your key names reasonably unique to avoid accidental conflicts." The canonical pattern nests custom keys under `metadata:`:

```yaml
metadata:
  type: skill
  project: skills-library
  plugin: claude
  aliases: [my-skill]
  tags: [type/skill, plugin/claude]
  status: active
```

Top-level custom keys work in lenient YAML parsers (and in obsidian-pattern skills) but `metadata:` nesting is the canonical convention.

## Body content

The spec places no format restrictions on the body. Recommendations:

- Keep `SKILL.md` under 500 lines and 5,000 tokens.
- Push detail to `references/`, `scripts/`, or `assets/` subfolders. Tell the agent when to load each file ("Read `references/api-errors.md` if the API returns a non-200 status code").
- Don't start the body with H1 — the `name` field is the implicit title. Cowork's parser explicitly rejects leading H1.

## Progressive disclosure

The spec defines three loading tiers:

| Tier | What loads | When | Token cost |
|---|---|---|---|
| 1. Catalog | `name` + `description` only | Session start | ~50–100 tokens per skill |
| 2. Instructions | Full `SKILL.md` body | When skill is activated | < 5,000 tokens (recommended) |
| 3. Resources | `scripts/`, `references/`, `assets/` | When body explicitly references them | Varies |

Design the body to assume tier 1 already happened. The user's prompt matched the description; now the body's job is to teach methodology.

## Description craft (for skill triggering)

The description is what drives Claude's matching. Optimize it for trigger accuracy:

### Use imperative phrasing

Lead with "Use this skill when..." instead of "This skill helps with...". Imperative phrasing ties the skill to user intent.

Bad: "This skill provides guidance for working with PDFs."

Good: "Use this skill when the user wants to extract text, fill forms, or merge PDF files."

### Mention concrete trigger phrases

Include the actual words a user is likely to type. If your skill handles vault organization, say "vault audit," "tag cleanup," "organize my notes" — not just "knowledge management."

### Be specific about scope

A description that's too broad over-triggers (loads on irrelevant prompts). Too narrow under-triggers (misses the user's real intent). Test both directions: prompts that should trigger AND prompts that shouldn't.

### Keep it under the Cowork ceiling

Spec allows 1024 chars. Cowork installer rejects above ~550. Target ≤ 441 for safety. Cut adjectives and meta-language; keep concrete triggers.

## Body content best practices

### Add what the agent lacks; omit what it knows

Focus on what the agent wouldn't know without your skill: project-specific conventions, domain procedures, non-obvious edge cases, the specific tools or APIs to use. Don't explain what a PDF is or how HTTP works.

Ask of each line: "Would the agent get this wrong without this instruction?" If no, cut it.

### Match specificity to fragility

For flexible tasks, explain the why and let the agent decide. For fragile operations (migrations, destructive actions), be prescriptive about exact sequences.

### Provide defaults, not menus

Don't list five equivalent options. Pick a default; mention alternatives only when they apply.

Bad: "You can use pypdf, pdfplumber, PyMuPDF, or pdf2image..."

Good: "Use pdfplumber for text extraction. For scanned PDFs requiring OCR, use pdf2image with pytesseract instead."

### Favor procedures over declarations

A skill should teach how to approach a class of problems, not what to produce for one instance. Procedures generalize; specific answers don't.

### Gotchas section

The highest-leverage content in many skills is a list of environment-specific facts that defy reasonable assumptions. Concrete corrections, not general advice.

Example:

```markdown
## Gotchas

- The `users` table uses soft deletes. Queries must include
  `WHERE deleted_at IS NULL` or results include deactivated accounts.
- The user ID is `user_id` in the database, `uid` in the auth service,
  and `accountId` in the billing API. All three refer to the same value.
- The `/health` endpoint returns 200 as long as the web server is running.
  Use `/ready` to check full service health.
```

Add a gotcha every time the agent makes a correctable mistake. This is the most direct way to improve a skill iteratively.

### Output format templates

For specific output formats, provide a concrete template inline. Agents pattern-match against templates more reliably than against prose descriptions.

### Validation loops

Instruct the agent to validate its own work before moving on: do the work, run a check (script, reference checklist, or self-review), fix issues, repeat until validation passes.

### Plan-validate-execute

For batch or destructive operations: produce an intermediate plan in a structured format, validate it against a source of truth, then execute. The validator script is what makes this reliable.

## Bundled scripts

When iterating on a skill, watch for the agent reinventing the same logic across runs (parsing a format, building a chart, validating output). That's a signal to write a tested script in `scripts/` and reference it from the body.

## Spec compliance vs Cowork compliance

| Concern | Spec says | Cowork installer says |
|---|---|---|
| `description` length | ≤ 1024 chars | ≤ ~550 chars (target ≤ 441) |
| Body leading H1 | Allowed | Forbidden |
| Custom frontmatter keys | Top-level allowed (lenient parser); `metadata:` recommended | Both work in practice |
| Manifest description length | Not constrained | ≤ 441 chars |
| Manifest keywords count | Not constrained | ≤ 7 items |
| Empty optional manifest fields | Lenient | Rejects `""` values; omit instead |

A skill that targets only Claude Code (CLI) can use the full spec ceiling. A skill that needs to install via Cowork's drag-and-drop installer must honor the Cowork constraints.

## Evaluation

Once a skill works, iterate against real test cases:

- Prompts that should trigger the skill — confirm it loads.
- Prompts that should not trigger — confirm it doesn't.
- Prompts that should trigger this skill but not adjacent skills — confirm precision.
- Prompts in the skill's wheelhouse with edge cases — confirm output quality.

Read the agent's execution traces, not just final outputs. Wasted steps usually mean instructions are too vague (agent tries multiple approaches), don't apply (agent follows them anyway), or present too many options without a default.

## Cross-client compatibility

The agentskills.io spec exists as a cross-client standard. Claude Code, Cowork, and other agents all consume the same `SKILL.md` format. Authoring to the canonical spec maximizes portability — a skill you write for Cowork should work in Claude Code, in third-party agents that scan `.agents/skills/`, and in any future client that adopts the spec.

The Cowork-specific constraints in `cowork-validation-rules.md` are tighter than the spec, not different from it. A Cowork-compliant skill is also spec-compliant.
