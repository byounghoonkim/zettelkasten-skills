# zettelkasten-skills

Claude Code skill for applying modern digital Zettelkasten methodology to local markdown note systems. Works with Obsidian, plain `.md` files, or any directory of markdown notes.

## What it does

Two modes in one skill:

**Capture** — Give Claude a raw idea, URL, or text snippet and it produces a properly typed note with frontmatter, a concept-oriented title, and wikilink suggestions.

**Connect** — Point Claude at a vault or directory and it finds orphaned notes, surfaces missing links between related notes, and generates Maps of Content for topic clusters.

## Installation

### Option 1 — Plugin (recommended)

Clone the repo and load it as a plugin:

```bash
git clone https://github.com/byounghoonkim/zettelkasten-skills ~/.claude/plugins/zettelkasten-skills
```

Then add to your `~/.claude/settings.json`:

```json
{
  "pluginDirectories": ["~/.claude/plugins/zettelkasten-skills"]
}
```

The skill is available as `zettelkasten:zettelkasten` in any Claude Code session.

### Option 2 — Personal skill (all projects)

```bash
git clone https://github.com/byounghoonkim/zettelkasten-skills /tmp/zettelkasten-skills
cp -r /tmp/zettelkasten-skills/skills/zettelkasten ~/.claude/skills/
rm -rf /tmp/zettelkasten-skills
```

Available as `zettelkasten` in any Claude Code session.

### Option 3 — Project skill (this project only)

```bash
git clone https://github.com/byounghoonkim/zettelkasten-skills /tmp/zettelkasten-skills
mkdir -p .claude/skills
cp -r /tmp/zettelkasten-skills/skills/zettelkasten .claude/skills/
rm -rf /tmp/zettelkasten-skills
```

Available as `zettelkasten` in Claude Code sessions within this project.

### Verify installation

Open Claude Code and run:

```
/zettelkasten
```

Claude should respond describing capture and connect modes.

## Usage

### Capture mode

Trigger by giving Claude raw material to turn into a note:

```
"문득 생각났는데, 습관은 환경 설계로 바꾸는 게 의지력보다 훨씬 쉬운 것 같아. 노트 만들어줘."
```

```
"James Clear의 Atomic Habits에서 습관 루프 개념 정리해줘. 출처는 이 책이야."
```

```
"저장해줘 — 복잡한 시스템은 단순한 규칙에서 출현한다."
```

Claude automatically detects the note type, applies the right template, and writes the file (Obsidian CLI → Bash → markdown output, in that order).

### Connect mode

Trigger by pointing Claude at an existing vault:

```
"내 vault ~/notes에 있는 노트들 분석해서 연결 상태 알려줘."
```

```
"~/notes 에서 고아 노트 찾아서 연결해줘."
```

```
"습관 관련 노트들로 MOC 만들어줘."
```

Claude scans the vault, reports orphans and link candidates before touching anything, then asks for confirmation if 3+ files need updating.

## Note types

| Type | When to use | Lifespan |
|------|-------------|----------|
| **Fleeting** | Quick capture, unpolished idea | Temporary — process within 48h |
| **Literature** | Key ideas from a specific source, in your own words | Semi-permanent |
| **Permanent** | One atomic idea stated as a complete claim | Permanent |
| **MOC** | Navigation hub linking related Permanent notes | Permanent, grows over time |

## Vault folder structure

The skill automatically discovers your vault's folder layout — no configuration needed. It scans your vault and matches folder names by keyword:

| Note type | Recognised folder names |
|-----------|------------------------|
| Fleeting | `inbox`, `capture`, `fleeting`, `quick`, `scratch`, `0-inbox`, `임시` |
| Permanent / Literature | `notes`, `permanent`, `zettel`, `slipbox`, `ideas`, `영구`, `노트` |
| MOC | `moc`, `map`, `index`, `overview`, `topics`, `맵`, `인덱스` |

If multiple folders match, Claude asks you to pick. If none match, it writes to the vault root and tells you.

Common layouts that work out of the box:

```
vault/inbox/          vault/00-inbox/       vault/fleeting/
vault/notes/          vault/slipbox/        vault/10-permanent/
vault/moc/            vault/maps/           vault/overview/
```

## File write priority

1. **Obsidian CLI** — if `obsidian version` exits 0, uses `obsidian create`
2. **Bash** — if a vault path is provided, writes `.md` directly
3. **Output only** — prints the note as a markdown block with the suggested save path

## Skill files

```
skills/zettelkasten/
  SKILL.md        # Main workflow (both modes)
  note-types.md   # Templates and quality criteria per note type
  naming.md       # Frontmatter schema and filename conventions
```

## Methodology

Based on [Andy Matuschak's Evergreen Notes](https://notes.andymatuschak.org/Evergreen_notes) with [Tiago Forte's CODE workflow](https://fortelabs.com/blog/the-code-framework/) as the processing model. The core principle: **one idea per note, stated as a complete claim, densely linked**.
