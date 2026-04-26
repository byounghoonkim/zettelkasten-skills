# Note Types Reference

## Fleeting Note

**Purpose:** Capture without friction. Process or delete within 48 h.
**Quality check:** Did it save the idea before you forgot it? That's enough.

```markdown
---
title: "{{brief label}}"
date: {{YYYY-MM-DD}}
type: fleeting
tags: []
links: []
---

{{raw thought, no polish needed}}

→ process by: {{YYYY-MM-DD + 2 days}}
```

---

## Literature Note

**Purpose:** Record what a specific source says, entirely in your own words.
**Quality check:** Can you explain this without looking at the source?

```markdown
---
title: "{{Source Title}} — Key Ideas"
date: {{YYYY-MM-DD}}
type: literature
tags: [{{topic}}]
source: "{{URL or citation}}"
links: []
---

## Core ideas

- {{idea 1 in your words}}
- {{idea 2 in your words}}

## Questions raised

- {{what this made you wonder}}

## Related notes

- [[{{related-permanent}}]]
```

---

## Permanent (Evergreen) Note

**Purpose:** One atomic, standalone idea. Title = a complete claim.
**Quality check (all must pass):**
- Title states exactly one complete idea?
- Body ≤ 300 words?
- Links to ≥ 2 other notes?
- Readable without any other context?

```markdown
---
title: "{{Claim as a complete sentence}}"
date: {{YYYY-MM-DD}}
type: permanent
tags: [{{concept}}]
links: [{{related-note-1}}, {{related-note-2}}]
---

{{1–3 paragraphs explaining the idea in your own words}}

## Evidence / examples

- {{concrete example or reference}}

## Related

- [[{{related-note-1}}]]
- [[{{related-note-2}}]]
```

---

## MOC (Map of Content)

**Purpose:** Navigation hub for a topic cluster. A curated index, not an essay.
**Quality check:** Would a new reader know where to start and how notes relate?

```markdown
---
title: "{{Topic}} — Map of Content"
date: {{YYYY-MM-DD}}
type: moc
tags: [{{topic}}]
links: [{{permanent-1}}, {{permanent-2}}]
---

## Core ideas

- [[{{permanent-1}}]] — {{one-line summary}}
- [[{{permanent-2}}]] — {{one-line summary}}

## Subtopics

- [[{{moc-subtopic}}]]

## Entry points

- Start with [[{{best-first-note}}]]
```
