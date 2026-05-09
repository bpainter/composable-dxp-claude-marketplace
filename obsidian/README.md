---
type: readme
project: skills-library
scope: plugin
plugin: obsidian
tags: [type/readme, plugin/obsidian]
status: active
---

# Obsidian Plugin

**Opinionated toolkit for keeping an Obsidian vault organized, tagged correctly, and structurally healthy.**

## Skills (12)

### Advice skills

| Skill | What it does |
|---|---|
| [[obsidian-overview]] | Entry-point router and map for the whole plugin (load this first when the user mentions Obsidian). |
| [[obsidian-tag-taxonomist]] | Designs the tag system: what tags are *for*, naming, hierarchy, entry/exit criteria. |
| [[obsidian-tag-hygiene]] | Audits and cleans up an existing sprawling tag list. |
| [[obsidian-frontmatter-schema-designer]] | Designs Properties schemas per note type. |
| [[obsidian-property-vs-tag-vs-link]] | Decides which mechanism a piece of metadata belongs in. The most-invoked routing layer. |
| [[obsidian-folder-architect]] | Designs top-level folder structure (type-based, PARA, JD, flat). |
| [[obsidian-filename-conventions]] | Filename and title conventions per note type. |
| [[obsidian-moc-architect]] | Designs Maps of Content as the topical organizing layer. |
| [[obsidian-vault-audit-framework]] | Periodic vault health audits with prioritized fix plans. |

### Orchestrator-shaped skills (multi-step workflows)

| Skill | Workflow |
|---|---|
| [[obsidian-vault-health-sweep]] | Periodic full hygiene pass (audit → fixes → re-audit). 7 stages. |
| [[obsidian-tag-cleanup-flow]] | End-to-end tag work (taxonomy → audit → execute → document). 6 stages. |
| [[obsidian-vault-bootstrap]] | Set up a new vault from scratch (folders → schema → tags → templates → MOCs). 8 stages. |

## Agent

**`obsidian-agent`** — Senior practitioner who knows when to use each skill in this plugin and how to sequence them. Use this agent (instead of invoking skills directly) when a task spans multiple skills, requires sequencing, or needs expert-level judgment about which skill to apply for a given problem.

## Commands

Four slash commands for the most common workflows:

| Command | What it runs |
|---|---|
| `/audit` | `obsidian-vault-audit-framework` — produces baseline metrics and fix plan |
| `/sweep` | `obsidian-vault-health-sweep` — full quarterly hygiene pass |
| `/cleanup-tags` | `obsidian-tag-cleanup-flow` — end-to-end tag work |
| `/bootstrap` | `obsidian-vault-bootstrap` — set up a new vault |

## Shared references

Every skill in this plugin inherits from this category-level reference:

- **`references/obsidian-foundations.md`** — Vault concepts, the four organizing mechanisms (folders / properties / tags / links), and the opinionated stance the skills converge on.

## Opinionated stance

Every skill leans on the same defaults:

1. Links over folders for connection
2. Properties over inline metadata
3. Tags for facets, not topic taxonomies
4. Shallow folders, deep linking
5. MOCs replace folders as the topical layer
6. Atomic notes connect better
7. Search and Dataview beat rigid structure
8. Plain text first

Full discussion in `references/obsidian-foundations.md`.

## Conventions

- Each skill has a `SKILL.md` with YAML frontmatter and structured sections.
- Each skill has a `references/` folder with skill-specific reference files (1–3 each).
- Tone across all skills: direct, practical, builder-oriented. Match Bermon's voice. No hedging, no corporate-speak.
- Naming: every skill, agent, command, and reference uses an `obsidian-` prefix or lives in the obsidian/ folder so cross-plugin collisions are impossible.

## Folder structure

```
obsidian/
├── README.md                              # this file
├── .claude-plugin/
│   └── plugin.json                        # plugin manifest (v0.4.0)
├── references/                            # plugin-wide shared references
│   └── obsidian-foundations.md
├── skills/
│   ├── obsidian-overview/                  # advice skills (9)
│   │   ├── SKILL.md
│   │   └── references/
│   ├── obsidian-tag-taxonomist/
│   ├── ... (7 more)
│   ├── obsidian-vault-health-sweep/        # orchestrator-shaped skills (3)
│   ├── obsidian-tag-cleanup-flow/
│   └── obsidian-vault-bootstrap/
├── agents/
│   └── obsidian-agent.md                   # the senior practitioner
└── commands/
    ├── audit.md                            # /audit
    ├── sweep.md                            # /sweep
    ├── cleanup-tags.md                     # /cleanup-tags
    └── bootstrap.md                        # /bootstrap
```

## Install

This plugin is distributed as `.plugin` (a renamed ZIP). Drag the `.plugin` file into Claude Desktop's plugin install UI to install. See `../INSTALL.md` for full instructions.

## Roadmap

### v0.5 — Capture and lifecycle skills

Likely additions:
- `obsidian-capture` — friction-free capture into the inbox
- `obsidian-atomic-note-writer` — refining fleeting notes into atomic claims
- `obsidian-orphan-strategy`
- `obsidian-stale-note-strategy`

### v0.6 — Worker agents

Workers that *act* on the vault (read, scan, refactor):
- `obsidian-vault-scanner`
- `obsidian-tag-cleaner`
- `obsidian-frontmatter-normalizer`
- `obsidian-link-suggester`
- `obsidian-moc-generator`

### v0.7 — Output skills

Synthesis, draft assistance, periodic reviews.

## Part of the second-brain marketplace

See the [marketplace README](../README.md) for the full architecture: each plugin is self-contained; the marketplace at `80_Skills_and_Agents/` is the entry point for installation.

## License

MIT.
