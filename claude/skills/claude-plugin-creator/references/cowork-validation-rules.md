# Cowork Validation Rules

Cowork's drag-and-drop plugin installer ("Settings → Customize → Personal Plugins") enforces stricter validation than the Anthropic skill spec documents. A plugin that passes Claude Code's validator may still fail Cowork with a generic **"Plugin validation failed"** error message.

This document records the validation rules established by binary-search bisection across 12 test plugins (May 2026). When in doubt about why a plugin fails, walk this checklist top to bottom.

## The validated working recipe

A plugin matching all of these constraints will install in Cowork:

### Plugin manifest (`<plugin>/.claude-plugin/plugin.json`)

```json
{
  "name": "<plugin-name>",
  "version": "0.1.0",
  "description": "Up to 441 characters. No special character restrictions found, but keep it tight.",
  "author": {
    "name": "Your Name",
    "email": "you@example.com"
  },
  "license": "MIT",
  "keywords": [
    "max", "of", "seven", "keywords", "total", "stay", "under"
  ]
}
```

**Hard rules:**
- `description` length ≤ **441 characters** (matches working obsidian.plugin)
- `keywords` count ≤ **7 items** (matches working obsidian.plugin)
- `name` is lowercase, kebab-case, matches the folder name
- No empty optional fields — omit `homepage`, `license`, etc. rather than including as `""`

### SKILL.md frontmatter and body

```markdown
---
name: <plugin>-<skill-name>
description: >
  Folded multi-line description. Keep under ~441 characters (Cowork's
  effective ceiling on SKILL.md descriptions, despite the documented 1024
  spec limit). The description drives Claude's matching, so be specific
  about when to invoke this skill. Mention triggering phrases.

# Optional Obsidian-readable extras (don't break Cowork — confirmed working)
type: skill
project: skills-library
plugin: <plugin>
aliases: [<plugin>-<skill-name>]
tags: [type/skill, plugin/<plugin>, topic/<topic>]
status: active
---

## Role & Identity

(Body starts with H2 — never H1.)
```

**Hard rules:**
- Body must **not** start with H1 (`# Title`). Start with H2 (`## Section`) or prose.
- `description` should use folded `>` format (not plain scalar) when long. Plain scalars on one line with em-dashes, slashes, or parens can fail YAML parsing.
- **`description` ≤ ~550 characters in Cowork (target ≤ 441).** Anthropic's documented limit is 1024, but Cowork's installer is stricter. Confirmed by bisection (May 2026): obsidian-overview at 546 chars installs; claude-plugin-creator at 775 chars and claude-orchestrator at 941 chars both failed with the generic "Plugin validation failed" error, even with a fully compliant manifest. Cap at 441 to match the manifest rule.
- Avoid arrows (`→ ↔ ⇒`) and box-drawing (`─ │ ├ └`) inside the description field. They don't break YAML but compound risk with length.
- Frontmatter extras (`type`, `project`, `plugin`, `aliases`, `tags`, `status`) are confirmed safe.

### Per-skill folder structure

```
<plugin>/skills/<plugin>-<skill-name>/
├── SKILL.md
└── references/
    └── <something>.md   ← even a placeholder is fine
```

**Soft rule:** every skill folder should have a `references/` subfolder. Cowork hasn't been observed to fail when this is absent, but every working plugin in this marketplace has one — match the pattern for safety.

## The validation failure modes (what to suspect first)

When you get "Plugin validation failed," walk this list:

| Order | Check | Fix |
|---|---|---|
| **0** | **ZIP wraps content in a directory (`<plugin>/README.md` instead of `README.md`)** | **#1 silent-fail cause.** Rebuild with `cd <staging> && zip -r ../<plugin>.plugin .` (note the trailing dot). Verify with `unzip -l <plugin>.plugin` — top entries must be `README.md`, `.claude-plugin/`, `skills/`, etc., NOT `<plugin-name>/`. Confirmed May 2026: every plugin in this marketplace failed for this reason; obsidian was the only one built without the wrapper. |
| 1 | Manifest description > 441 chars | Trim |
| 2 | Manifest keywords > 7 | Trim |
| 3 | Manifest has empty optional fields (`""`) | Remove the fields |
| 4 | Any SKILL.md description > ~550 chars | Trim to ≤ 441. The documented spec says ≤1024 but Cowork rejects much earlier. |
| 5 | Any SKILL.md body starts with `# Title` | Demote to H2, keep H2+ structure |
| 6 | ZIP has 0-byte files (OneDrive interference) | Force-materialize and re-stage |
| 7 | Plain-scalar description with special chars | Convert to folded `>` format |
| 8 | Arrows or box-drawing inside SKILL.md description | Remove — compounds risk with length |

If all of those check out, build a minimum-viable test plugin (1 skill, name + description only, no agent, no commands) and verify it uploads. Then add components incrementally to find what breaks.

## Bisection record (May 2026)

For posterity, the test sequence that established these rules:

| Test | What was in it | Result | What it told us |
|---|---|---|---|
| `algolia-test` | 1 skill, minimal manifest, no agent/commands/refs | ✓ | Minimum viable plugin works |
| `algolia-test-3` | 12 skills, stripped frontmatter, minimal manifest | ✓ | Skill count is fine |
| `algolia-test-4` | + plugin-level references | ✓ | Plugin-level refs are fine |
| `algolia-test-5` | + agent file | ✓ | Agent file is fine |
| `algolia-test-6` | (instead) + 5 slash commands | ✓ | Commands are fine |
| `algolia-test-7` | All of the above with **minimal manifest** | ✓ | Content together is fine |
| `algolia-test-8` | All + **full algolia manifest** (625-char desc, 12 keywords, with arrows) | ✗ | Manifest fields are the issue |
| `algolia-test-9` | Same as 8 minus arrows (`→` and `↔`) | ✗ | Arrows aren't the issue |
| `algolia-test-10` | Long description **only** (no author/license/keywords) | ✓ | Long description alone is fine |
| `algolia-test-11` | Short description + author/license/2 keywords | ✓ | author/license are fine |
| `algolia-test-12` | **Full manifest, 320-char desc, 7 keywords** | ✓ | This is the recipe |

**Conclusion:** the issue is the **interaction** of long description + many keywords + supplementary fields. Each in isolation is fine; the combination breaks the validator. Capping description at 441 and keywords at 7 (matching obsidian's working profile) is the safe pattern.

## What we never confirmed

- The exact threshold between 441 and 625 chars where description starts failing
- The exact threshold between 7 and 12 keywords
- Whether specific characters in the description matter beyond the length

If you need to push these limits, build a binary-search test plugin (long-but-not-too-long description, then halve the failures). The pattern from this bisection: failures are silent, so the test plugins must be small enough to upload quickly.

## Cowork vs. Claude Code — different validators

Important: these rules apply to **Cowork's drag-and-drop installer specifically**. Claude Code's CLI marketplace install (`claude plugin install <name>@<marketplace>`) uses different validation and is more permissive. A plugin that fails Cowork might install fine via Claude Code.

If a plugin is intended for both surfaces, validate against the **stricter Cowork rules**. If it's Claude Code only, Anthropic's documented limits (description ≤ 1024 chars, no other formal caps) are the working constraint.

## Future-proofing

When Anthropic publishes updated Cowork plugin validation rules, this file should be revised. The bisection record above is empirical — the underlying rules may relax (or tighten) in future Cowork versions.
