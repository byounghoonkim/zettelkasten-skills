# Zettelkasten Skill Design

**Date:** 2026-04-26  
**Status:** Approved

---

## Overview

A Claude skill that applies modern digital Zettelkasten methodology to local markdown note systems. Supports two modes: **capture** (turning raw material into properly typed notes) and **connect** (analysing an existing vault to surface links and build Maps of Content).

The skill is tool-agnostic: it works with any `.md`-based system and uses Obsidian CLI when available, falling back to Bash file operations, and finally to formatted markdown output.

---

## Methodology Foundation

Based on **Andy Matuschak's Evergreen Notes** as the core principle, with **Tiago Forte's CODE workflow** (Capture → Organize → Distill → Express) as the processing flow.

### Note Types

| Type | Role | Lifespan |
|------|------|----------|
| **Fleeting** | Quick capture, unprocessed idea | Temporary (inbox) |
| **Literature** | Key content from a specific source, in own words | Semi-permanent |
| **Permanent (Evergreen)** | One atomic idea, readable independently | Permanent |
| **MOC** (Map of Content) | Index/navigation linking related Permanent notes | Permanent, grows over time |

### Evergreen Note Principles
- **Atomic**: one idea per note — if a note answers two questions, split it
- **Concept-oriented titles**: "Flow state requires clear goals" not "My notes on flow"
- **Densely linked**: every Permanent note links to related notes
- **Own words**: never copy-paste — restate to verify understanding

---

## File Structure

```
skills/zettelkasten/
  SKILL.md       # Workflow for both modes (capture / connect)
  note-types.md  # Type definitions, quality criteria, examples
  naming.md      # Filename, ID, and frontmatter schema
```

---

## Skill Architecture

### Trigger Conditions (in SKILL.md description)

**Capture mode** — activated when:
- User provides raw material: idea, URL, text snippet, conversation excerpt
- User provides a Fleeting note to process into Permanent
- User says "노트 만들어", "정리해줘", "저장해줘" or equivalent

**Connect mode** — activated when:
- User asks to analyse or organise an existing vault/directory
- User asks for link suggestions, orphan detection, or MOC creation
- User says "연결해줘", "정리해줘 (vault 대상)", "MOC 만들어줘"

---

## Capture Mode Workflow

```
1. DETECT note type from input signals:
   "방금 생각난 건데..." → Fleeting
   "이 논문/글/책에서..." → Literature
   Single clear idea statement → Permanent
   "X에 관련된 노트 모아줘" → MOC

2. GENERATE note:
   - frontmatter: title, date, type, tags, links (see naming.md)
   - Body: apply type-specific template (see note-types.md)
   - Suggest candidate links from existing notes if vault accessible

3. WRITE file (priority order):
   a. obsidian CLI available (test: `obsidian version` exits 0) → obsidian create name="..." content="..."
   b. User provided a vault path or cwd is inside a known vault → Bash write to resolved path
   c. Neither → output markdown block + recommended save path
```

### Input → Type mapping

| Signal | Detected type |
|--------|--------------|
| Casual phrase, "문득", "생각났는데" | Fleeting |
| Source reference (책, 논문, 영상 제목) | Literature |
| Single declarative concept | Permanent |
| List of related topics / "모아줘" | MOC |
| Ambiguous | Ask: "Fleeting(빠른 캡처) vs Permanent(완성된 아이디어) 중 어느 쪽인가요?" |

---

## Connect Mode Workflow

```
1. SCAN vault or directory:
   - List all .md files
   - Parse frontmatter for type, tags, links
   - Build adjacency map (existing [[wikilinks]] and tags)

2. ANALYSE:
   a. Orphan detection — Permanent notes with 0 backlinks
   b. Keyword clustering — notes sharing 3+ common terms but no link
   c. Atomicity check — notes > 500 words flagged for possible split

3. OUTPUT (in order of confidence):
   a. Link suggestions: "[[A]] → [[B]] (shared concept: X)"
   b. MOC draft: if 4+ notes cluster around a theme, generate MOC skeleton
   c. Split suggestions: "이 노트는 두 아이디어를 담고 있습니다: X, Y"

4. APPLY changes (with user confirmation for bulk ops):
   - Single file: apply directly
   - 3+ files: show list → ask "적용할까요?" → apply on confirmation
   - obsidian CLI: obsidian update to add wikilinks
   - Bash: append to links: array in frontmatter
   - Output: show diff-style changes for manual application
```

---

## naming.md Contents (Reference File)

### Frontmatter Schema

```yaml
---
title: "Concept-oriented title"
date: YYYY-MM-DD
type: fleeting | literature | permanent | moc
tags: [tag1, tag2]
source: ""        # literature only: URL or citation
links: []         # explicit outbound wikilinks
---
```

### Filename Convention

- Permanent & MOC: `kebab-case-title.md` (concept-oriented, timeless)
- Literature: `YYYY-MM-DD-source-title.md`
- Fleeting: `YYYY-MM-DD-HHMM-fleeting.md` (timestamp so inbox sorts chronologically)

### Folder Structure (minimal, tool-agnostic)

```
vault/
  inbox/        # Fleeting notes land here
  notes/        # Permanent + Literature
  moc/          # Maps of Content
```

---

## note-types.md Contents (Reference File)

### Fleeting Note
**Purpose:** Capture without friction. Don't edit later — process or delete within 48h.  
**Quality check:** Does it capture the idea before you forget it? That's enough.

### Literature Note
**Purpose:** Record what a source says, entirely in your own words.  
**Quality check:** Could you explain this to someone without looking at the source?

### Permanent (Evergreen) Note
**Purpose:** One atomic, standalone idea. Title = claim.  
**Quality check:** Does the title state a complete idea? Is the body ≤ 300 words? Does it link to ≥ 2 other notes?

### MOC (Map of Content)
**Purpose:** Navigation hub for a topic cluster. Not an essay — a curated index.  
**Quality check:** Would a new reader know where to start and how notes relate?

---

## Error Handling & Edge Cases

| Situation | Behaviour |
|-----------|-----------|
| Note type ambiguous | Ask one clarifying question before generating |
| Vault not accessible (no path provided, obsidian CLI absent) | Fall back to markdown output with path suggestion |
| Note exceeds atomicity threshold (>500 words, multiple H2 sections) | Flag and ask "split into N notes?" |
| Existing file at target path | Warn, ask to overwrite or choose new name |
| No existing notes to link | Skip link suggestion step silently |
| 3+ files to modify (bulk op) | Show full list first, ask "적용할까요?" before writing |

---

## Out of Scope

- Spaced repetition / Anki integration (separate skill)
- PDF/ebook ingestion pipeline
- Sync or version control of notes
- Tag taxonomy management
