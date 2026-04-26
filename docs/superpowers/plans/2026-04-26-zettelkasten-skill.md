# Zettelkasten Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create a `zettelkasten` Claude skill with capture mode (raw input → typed note) and connect mode (vault analysis → link/MOC suggestions).

**Architecture:** Three files under `skills/zettelkasten/` — `SKILL.md` (workflow), `note-types.md` (templates & quality criteria), `naming.md` (frontmatter schema & filename rules). SKILL.md references the two support files; they are loaded by Claude on demand.

**Tech Stack:** Markdown skill files, `obsidian` CLI (optional), Bash file operations (fallback)

**Spec:** `docs/superpowers/specs/2026-04-26-zettelkasten-skill-design.md`

**Status:** ✅ COMPLETE — All 7 tasks executed and verified (2026-04-26)

---

## Task 1: Create skill directory skeleton and update CLAUDE.md ✅

- [x] Create `skills/zettelkasten/` directory
- [x] Rewrite `CLAUDE.md` with skill documentation
- [x] `git init` + first commit (CLAUDE.md only)

---

## Task 2: Create `naming.md` ✅

- [x] Write frontmatter schema and filename conventions
- [x] Commit

---

## Task 3: Create `note-types.md` ✅

- [x] Write 4 note type templates (Fleeting, Literature, Permanent, MOC) with quality criteria
- [x] Commit

---

## Task 4: Create `SKILL.md` ✅

- [x] Write capture mode + connect mode workflow
- [x] Commit

---

## Task 5: Test capture mode ✅

- [x] Permanent type scenario: PASS (all 6 checks)
- [x] Literature type scenario: PASS (all 5 checks)

---

## Task 6: Test connect mode ✅

- [x] Fake vault with 3 orphan notes
- [x] All 5 checks PASS (orphan detection, keyword clustering, bulk-op gate)

---

## Task 7: Final commit ✅

- [x] Verified 3 skill files present
- [x] Git log shows 4 clean commits
