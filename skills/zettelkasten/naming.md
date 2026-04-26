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

```
vault/
  inbox/    # Fleeting notes land here
  notes/    # Permanent + Literature
  moc/      # Maps of Content
```

## Title Guidelines

- **Permanent**: state a complete idea as a claim — "Flow state requires clear goals" not "Flow state notes"
- **Literature**: source title, optionally with key theme — "Atomic Habits — Habit Loop"
- **Fleeting**: brief label, no pressure — "idea about focus"
- **MOC**: topic name only — "Productivity" or "Machine Learning"
