# Wiki Schema — Librarian Operating Manual

This file is the source of truth for how this wiki is structured and maintained.
Every LLM operating on this wiki MUST read and follow this document.

---

## Directory Layout

| Path | Purpose |
|---|---|
| wiki/SCHEMA.md | This file. Defines all conventions. |
| wiki/index.md | Content catalog. Updated on every ingest. Table format only. |
| wiki/log.md | Append-only chronological log. Never delete entries. |
| wiki/overview.md | Evolving narrative synthesis of all themes. |
| wiki/sources/ | One summary page per ingested source (document, CV, article). |
| wiki/entities/ | Pages for specific people, organisations, tools, and projects. |
| wiki/concepts/ | Pages for ideas, methods, techniques, and frameworks. |
| wiki/analyses/ | Syntheses and answers filed from good queries. |

---

## Page Formats

### Source Page (`wiki/sources/<slug>.md`)
Created for every new ingested document.
```markdown
---
type: source
date: YYYY-MM-DD
title: <descriptive title>
original_file: raw_sources/<filename>
---

## Summary
One paragraph overview.

## Key Facts
- Bullet list of important extractable facts.

## Entities Mentioned
- Links to entity pages.

## Concepts Mentioned
- Links to concept pages.
```

### Entity Page (`wiki/entities/<name>.md`)
```markdown
---
type: entity
entity_type: person | org | tool | project
---

## Overview
Who / what this is.

## Key Facts
Specific, citable facts.

## Sources
- Links to source pages that mention this entity.
```

### Concept Page (`wiki/concepts/<name>.md`)
```markdown
---
type: concept
---

## Definition
Clear, concise definition.

## How It Applies Here
Context within this wiki.

## Sources
- Links to source pages.
```

### Analysis Page (`wiki/analyses/<slug>.md`)
Created when a query produces a high/medium confidence answer worth preserving.
```markdown
---
type: analysis
date: YYYY-MM-DD
question: <the original question>
confidence: high | medium
---

## Answer
The synthesized answer.

## Sources
- Links to wiki pages cited.
```

---

## Ingest Workflow

On every new message or document, the Librarian MUST:

1. Read this SCHEMA.md and wiki/index.md.
2. Create `wiki/sources/<slug>.md` summarising the ingested content.
3. Update or create `wiki/entities/<name>.md` for every person, org, tool, or project.
4. Update or create `wiki/concepts/<name>.md` for every key idea or method.
5. Update `wiki/overview.md` if the source materially changes the big picture.
6. Update `wiki/index.md` — strict table, exact paths, no hallucinated entries.
7. Return a `log_entry` in the format:
   `## [YYYY-MM-DD] ingest | <title>\n<1-2 sentence summary>`

---

## Index Format (STRICT)

wiki/index.md must always be a markdown table:

```markdown
| File | Summary |
|---|---|
| wiki/sources/example.md | One-line description. |
```

Rules:
- The `File` column must be the exact repo-relative path.
- NEVER list a file that does not exist or was not created in this session.
- NEVER use bullet lists, headings, or any other format.

---

## Log Format (STRICT)

Every entry starts with `## [YYYY-MM-DD] <operation> | <title>`.
Operations: `ingest` | `query` | `lint`

Example:
```
## [2026-04-14] ingest | CV_DUYNGUYEN.pdf
Ingested Duy Nguyen's CV. Updated entities/duy-nguyen.md and sources/cv-duynguyen.md.
```
