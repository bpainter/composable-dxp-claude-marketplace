# 80_Skills_and_Agents / Memory

Working memory and knowledge base for both Claude and ChatGPT.

## Files

| File | Purpose |
|---|---|
| `CLAUDE.md` | Working memory entry point for Claude. Read every session. |
| `CHATGPT.md` | Working memory entry point for ChatGPT. Pasted into custom instructions / GPT system prompt. |
| `people.md` | Decoded — names, nicknames, roles, key relationships. |
| `glossary.md` | Acronyms, codenames, internal terminology. |
| `practice.md` | How the practice runs — structure, cadence, priorities. |
| `preferences.md` | Style, tone, formatting preferences. |

## How they relate

`CLAUDE.md` and `CHATGPT.md` are entry points — short, top-level orientation. They reference the deeper files (`people.md`, `glossary.md`, `practice.md`, `preferences.md`) which carry the bulk of the context.

You should be able to dump `people.md` + `glossary.md` + `practice.md` + `preferences.md` into a fresh session and the model knows enough to be useful.

## Maintenance

- Update as a side effect of work — when you encounter new shorthand or new people, add them.
- Periodically (monthly?) prune anything stale.
- Keep these files in plain Markdown. No fancy templating; the value is in the prose.
