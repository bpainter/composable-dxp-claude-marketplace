# Plugin Validator Script

A drop-in pre-flight check that runs the Cowork validation rules against a plugin source folder. Use it before every build to catch the common traps.

## Usage

```bash
# Run from anywhere
bash validate-plugin.sh /path/to/plugin-folder
```

Returns exit code 0 if all hard rules pass, non-zero if any fail. Soft warnings are flagged but don't fail the script.

## The script

Save this as `validate-plugin.sh` at the marketplace root:

```bash
#!/usr/bin/env bash
# Pre-flight validator for Cowork-bound plugins.
# Checks the empirically-established Cowork validation rules.
# See cowork-validation-rules.md for the full ruleset.

set -e

PLUGIN_DIR="${1:?Usage: $0 <plugin-folder>}"

if [ ! -d "$PLUGIN_DIR" ]; then
  echo "✗ $PLUGIN_DIR is not a directory"
  exit 1
fi

if [ ! -f "$PLUGIN_DIR/.claude-plugin/plugin.json" ]; then
  echo "✗ $PLUGIN_DIR/.claude-plugin/plugin.json missing"
  exit 1
fi

ERR=0
WARN=0

# === Manifest checks ===
echo "Checking manifest..."

DESC_LEN=$(python3 -c "
import json
d = json.load(open('$PLUGIN_DIR/.claude-plugin/plugin.json'))
print(len(d.get('description', '')))
")

KW_COUNT=$(python3 -c "
import json
d = json.load(open('$PLUGIN_DIR/.claude-plugin/plugin.json'))
print(len(d.get('keywords', [])))
")

EMPTY_FIELDS=$(python3 -c "
import json
d = json.load(open('$PLUGIN_DIR/.claude-plugin/plugin.json'))
empties = [k for k, v in d.items() if v == '']
print(','.join(empties) if empties else '')
")

if [ "$DESC_LEN" -gt 441 ]; then
  echo "  ✗ description $DESC_LEN chars > 441 (Cowork limit)"
  ERR=$((ERR + 1))
elif [ "$DESC_LEN" -gt 350 ]; then
  echo "  ⚠ description $DESC_LEN chars (close to 441 limit; safe but tight)"
  WARN=$((WARN + 1))
else
  echo "  ✓ description $DESC_LEN chars"
fi

if [ "$KW_COUNT" -gt 7 ]; then
  echo "  ✗ keywords $KW_COUNT > 7 (Cowork limit)"
  ERR=$((ERR + 1))
else
  echo "  ✓ keywords $KW_COUNT"
fi

if [ -n "$EMPTY_FIELDS" ]; then
  echo "  ✗ empty optional fields: $EMPTY_FIELDS (omit or fill)"
  ERR=$((ERR + 1))
else
  echo "  ✓ no empty optional fields"
fi

# === Skill checks ===
if [ -d "$PLUGIN_DIR/skills" ]; then
  echo "Checking skills..."
  
  for skill_md in "$PLUGIN_DIR"/skills/*/SKILL.md; do
    [ -f "$skill_md" ] || continue
    skill_name=$(basename "$(dirname "$skill_md")")
    
    # Check skill description length (≤1024)
    SKILL_DESC_LEN=$(python3 -c "
import re
content = open('$skill_md').read()
m = re.match(r'^---\n(.+?)\n---', content, re.DOTALL)
if m:
    fm = m.group(1)
    desc_m = re.search(r'^description:\s*>?\s*\n((?:\s+.+\n?)+)', fm, re.MULTILINE)
    if desc_m:
        desc = ' '.join(line.strip() for line in desc_m.group(1).split('\n')).strip()
    else:
        desc_m2 = re.search(r'^description:\s*(.+?)\$', fm, re.MULTILINE)
        desc = desc_m2.group(1).strip() if desc_m2 else ''
    print(len(desc))
else:
    print(0)
")
    
    if [ "$SKILL_DESC_LEN" -gt 550 ]; then
      echo "  ✗ $skill_name: description $SKILL_DESC_LEN chars > 550 (Cowork's effective ceiling — trim to ≤ 441)"
      ERR=$((ERR + 1))
    elif [ "$SKILL_DESC_LEN" -gt 441 ]; then
      echo "  ⚠ $skill_name: description $SKILL_DESC_LEN chars > 441 (close to Cowork's ceiling; obsidian works at 546 but tight)"
      WARN=$((WARN + 1))
    fi
    
    # Check for leading H1 in body (after frontmatter close)
    HAS_LEADING_H1=$(awk '
      /^---$/ { c++; if (c == 2) { in_body = 1; next } }
      in_body {
        if (/^[[:space:]]*$/) next
        if (/^# /) { print "yes"; exit }
        if (/^[^#]/) exit
      }
    ' "$skill_md")
    
    if [ "$HAS_LEADING_H1" = "yes" ]; then
      echo "  ✗ $skill_name: SKILL.md body starts with H1 (must use H2+)"
      ERR=$((ERR + 1))
    fi
    
    # Check for references/ subfolder (warning only)
    if [ ! -d "$(dirname "$skill_md")/references" ]; then
      echo "  ⚠ $skill_name: no references/ subfolder (recommended)"
      WARN=$((WARN + 1))
    fi
  done
fi

# === Agent checks ===
if [ -d "$PLUGIN_DIR/agents" ]; then
  echo "Checking agents..."
  for agent_md in "$PLUGIN_DIR"/agents/*.md; do
    [ -f "$agent_md" ] || continue
    [ "$(basename "$agent_md")" = "README.md" ] && continue
    
    AGENT_DESC_LEN=$(python3 -c "
import re
content = open('$agent_md').read()
m = re.match(r'^---\n(.+?)\n---', content, re.DOTALL)
if m:
    fm = m.group(1)
    desc_m = re.search(r'^description:\s*>?\s*\n((?:\s+.+\n?)+)', fm, re.MULTILINE)
    if desc_m:
        desc = ' '.join(line.strip() for line in desc_m.group(1).split('\n')).strip()
    else:
        desc_m2 = re.search(r'^description:\s*(.+?)\$', fm, re.MULTILINE)
        desc = desc_m2.group(1).strip() if desc_m2 else ''
    print(len(desc))
else:
    print(0)
")
    
    if [ "$AGENT_DESC_LEN" -gt 1024 ]; then
      echo "  ✗ $(basename "$agent_md"): description $AGENT_DESC_LEN chars > 1024"
      ERR=$((ERR + 1))
    fi
  done
fi

# === Summary ===
echo ""
if [ "$ERR" -gt 0 ]; then
  echo "✗ Validation FAILED — $ERR error(s), $WARN warning(s)"
  exit 1
else
  echo "✓ Validation passed — $WARN warning(s)"
  exit 0
fi
```

## Run it before every release

Add this to your build pipeline:

```bash
# 1. Validate the source folder
./validate-plugin.sh ./algolia
[ $? -ne 0 ] && exit 1

# 2. Build only if validation passed — CRITICAL: zip from inside the staging dir
STAGE=/tmp/algolia-build
rsync -a ./algolia/ "$STAGE/"
cd "$STAGE"
zip -qr ../algolia.plugin .              # NOTE the trailing dot — content at zip ROOT
mv ../algolia.plugin /path/to/Plugins/

# 3. Post-build verify: top-level zip entries should NOT be wrapped
unzip -l /path/to/Plugins/algolia.plugin | awk '{print $4}' | head -10
# Expected: README.md, .claude-plugin/, skills/, commands/  (etc.)
# WRONG:    algolia/README.md, algolia/.claude-plugin/, ...
```

**The single most catastrophic build mistake** is wrapping the plugin contents in a directory inside the zip. Cowork's installer rejects `algolia/README.md` at root; it expects `README.md` at root. obsidian.plugin works because it was zipped from inside the obsidian folder; every other plugin in this marketplace failed (May 2026) until rebuilt without the wrapper.

A post-build check belongs in the pipeline:

```bash
# Fails the build if top-level looks wrapped
TOP=$(unzip -l "$PLUGIN_FILE" | awk 'NR>3 && $4!="" {print $4}' | head -5 | grep -E '^[^/]+/' | head -1 | cut -d/ -f1)
if [ -n "$TOP" ] && [ "$TOP" != ".claude-plugin" ] && [ "$TOP" != "skills" ] && [ "$TOP" != "commands" ] && [ "$TOP" != "agents" ] && [ "$TOP" != "references" ]; then
  echo "FAIL: zip top-level appears wrapped in '$TOP/' — content must be at zip root"
  exit 1
fi
```

## What the script does NOT check

- ZIP integrity (run `unzip -l` after building)
- 0-byte files from OneDrive interference (force-materialize before staging)
- Cross-skill name collisions (rare, but worth a manual check)
- Slash command name collisions across plugins (Cowork warns at install time)

## Fixing failures

| Error | Fix |
|---|---|
| `description NN chars > 441` | Trim the manifest description |
| `keywords NN > 7` | Drop keywords down to 7 most distinctive |
| `empty optional fields: X` | Remove the fields entirely or fill them |
| `<skill>: description NN chars > 1024` | Trim the skill's frontmatter description |
| `<skill>: SKILL.md body starts with H1` | Demote the leading `# Title` to `## Title` or remove (the frontmatter `name` is the implicit title) |

The warnings (`⚠`) don't block but are worth addressing for consistency with the marketplace's working pattern.
