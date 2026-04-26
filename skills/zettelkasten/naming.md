# Naming & Frontmatter Reference

## Frontmatter Schema

All notes require this frontmatter block:

~~~yaml
---
title: "Concept-oriented title"
date: YYYY-MM-DD
type: fleeting | literature | permanent | moc
tags: [tag1, tag2]
source: ""        # literature only: URL or citation
links: []         # outbound wikilinks without [[ ]] brackets
---
~~~

## Filename Conventions

| Type | Pattern | Example |
|------|---------|---------|
| Permanent | `kebab-case-title.md` | `flow-state-requires-clear-goals.md` |
| MOC | `kebab-case-topic-moc.md` | `productivity-moc.md` |
| Literature | `YYYY-MM-DD-source-title.md` | `2026-04-26-atomic-habits.md` |
| Fleeting | `YYYY-MM-DD-HHMM-fleeting.md` | `2026-04-26-1430-fleeting.md` |

## Folder Targets

Folder names vary by system. The skill discovers the correct folder by scanning the vault and matching keywords:

| Note type | Keyword patterns (case-insensitive, partial match OK) |
|-----------|------------------------------------------------------|
| Fleeting | `inbox`, `capture`, `fleeting`, `quick`, `scratch`, `0-inbox`, `00-inbox`, `_inbox`, `임시` |
| Permanent / Literature | `notes`, `permanent`, `zettel`, `zettels`, `slipbox`, `ideas`, `01-notes`, `10-notes`, `영구`, `노트` |
| MOC | `moc`, `map`, `maps`, `index`, `indices`, `overview`, `topics`, `structure`, `맵`, `인덱스` |

**Fallback:** if no folder matches, write to vault root and notify the user.

**Common vault layouts this covers:**

```
vault/inbox/          vault/00-inbox/       vault/fleeting/
vault/notes/          vault/slipbox/        vault/10-permanent/
vault/moc/            vault/maps/           vault/overview/
```

## Title Guidelines

- **Permanent**: state a complete idea as a claim — "Flow state requires clear goals" not "Flow state notes"
- **Literature**: source title, optionally with key theme — "Atomic Habits — Habit Loop"
- **Fleeting**: brief label, no pressure — "idea about focus"
- **MOC**: topic name only — "Productivity" or "Machine Learning"
