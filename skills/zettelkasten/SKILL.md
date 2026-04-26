---
name: zettelkasten
description: Use when creating Zettelkasten-style notes from raw input (ideas, URLs, text snippets, fleeting notes). Also use when analysing an existing vault or directory to find orphan notes, suggest missing links, or build Maps of Content. Triggered by: "노트 만들어", "정리해줘", "저장해줘", "연결해줘", "MOC 만들어줘".
---

# Zettelkasten

Two modes: **capture** (raw input → typed note) and **connect** (vault analysis → links + MOC).

Reference files in this skill directory:
- `note-types.md` — templates and quality criteria for each note type
- `naming.md` — frontmatter schema and filename conventions

## Mode Detection

```
Input is raw material (idea, URL, text, fleeting note to process)?  → CAPTURE MODE
Input asks to analyse/organise a vault or directory?                → CONNECT MODE
Ambiguous?                                                          → Ask: "새 노트를 만들까요, 아니면 기존 노트들을 연결할까요?"
```

## Capture Mode

### 1. Detect Note Type

| Signal | Type |
|--------|------|
| Casual phrase, "문득", "생각났는데", unpolished idea | Fleeting |
| Mentions a specific source (책, 논문, 영상, URL) | Literature |
| Single declarative concept stated clearly | Permanent |
| "X에 관련된 노트 모아줘" or list of topics | MOC |
| Ambiguous | Ask: "Fleeting(빠른 캡처) vs Permanent(완성된 아이디어) 중 어느 쪽인가요?" |

### 2. Generate Note

Apply the template from `note-types.md` for the detected type.
Use filename and frontmatter rules from `naming.md`.

If the vault is accessible (path provided or `obsidian version` exits 0):
- Scan existing note titles and tags for related keywords
- Add candidates to the `links:` frontmatter field

### 3. Resolve Target Folder

Before writing, determine the correct folder using this discovery sequence:

```
obsidian version exits 0?
  → obsidian list (or equivalent) to get vault folder list → match heuristics below

vault path known?
  → find <vault> -maxdepth 1 -type d → match heuristics below

neither?
  → ask: "노트를 저장할 폴더 경로를 알려주세요 (예: inbox, notes/fleeting 등)"
```

**Folder heuristics** (see `naming.md` for full keyword list):

| Note type | Match keywords in folder name |
|-----------|-------------------------------|
| Fleeting | inbox, capture, fleeting, quick, scratch, 임시 |
| Permanent / Literature | notes, permanent, zettel, slipbox, ideas, 영구, 노트 |
| MOC | moc, map, index, overview, topics, 맵, 인덱스 |

Resolution rules:
- **Exact single match** → use it without asking
- **Multiple candidates** → show list, ask user to pick (one question only)
- **No match** → use vault root; print: "일치하는 폴더가 없어 vault 루트에 저장합니다"

Cache the resolved mapping for the rest of the session.

### 4. Write File

```
obsidian version exits 0?   → obsidian create name="<filename>" content="<body>"
vault path known?           → Bash: write to <vault>/<resolved_folder>/<filename>.md
neither?                    → Output markdown block; print: "저장 경로: <resolved_folder>/<filename>.md"
```

## Connect Mode

### 1. Scan Vault

```bash
find <vault_path> -name "*.md" | head -200
```

Parse frontmatter (`type`, `tags`, `links`) and extract `[[wikilinks]]` from body.
Build: note list, existing link pairs, tag groups.

### 2. Analyse

Run all three checks:

**a) Orphan detection**
Permanent notes with 0 incoming links from any other note.

**b) Keyword clustering**
Note pairs that share 3+ significant terms in title/tags but have no explicit link.

**c) Atomicity check**
Notes with > 500 words or 3+ H2 sections — flag as possible split candidates.

### 3. Present Findings Before Changing Anything

```
고아 노트 (N): [[A]], [[B]], [[C]]
연결 제안 (N): [[A]] → [[B]] (공통 키워드: X), [[C]] → [[D]] (태그: Y)
분리 권장 (N): [[E]] — 두 아이디어 포함: "X", "Y"
```

If no issues found: "연결 상태가 좋습니다. 고아 노트나 연결 후보가 없습니다."

### 4. Apply Changes

- **Single file**: apply directly without confirmation
- **3+ files**: show full list → ask "적용할까요?" → apply on confirmation

```bash
# obsidian CLI (preferred)
obsidian update file="<name>" content="<updated body with [[links]]>"

# Bash fallback: append to links: array in frontmatter
```

For MOC creation: use the MOC template from `note-types.md`.

## Error Handling

| Situation | Action |
|-----------|--------|
| Note type ambiguous | Ask one clarifying question, then generate |
| Existing file at target path | Warn → ask to overwrite or choose new name |
| Vault inaccessible (no path, no obsidian CLI) | Output markdown + suggested save path |
| Note > 500 words during capture | Flag: "이 노트는 두 아이디어를 담고 있을 수 있습니다. 분리할까요?" |
| No existing notes to link to | Skip link suggestion step silently |

## Common Mistakes

- Non-atomic Permanent notes: always check — does the title state exactly ONE claim?
- Missing frontmatter: include full frontmatter even for Fleeting notes
- Obsidian-specific syntax when tool is unknown: use standard `[[wikilink]]` — works across all tools
