---
id: skill_query
type: system
status: active
created: 2026-05-28
updated: 2026-05-28
tags:
  - system
  - skill
aliases:
  - query
---

# query

**Goal:** Answer a question grounded in the vault, with citations, without drifting into generic advice.

**Steps:**

1. Read the profile:
   ```
   obsidian read file="_start_here.md"
   ```
2. Check the gap report for anything relevant to the question:
   ```
   obsidian read file="GAPS.md"
   ```
3. Identify the domain(s) involved. Read each relevant overview:
   ```
   obsidian read file="<domain>_overview"
   ```
4. Follow `cites:` and wikilinks to specific claims, analyses, or research:
   ```
   obsidian read file="<cited file>"
   ```
5. For any claim you rely on, check staleness:
   ```
   obsidian property:get name="verified_on" file="<claim>"
   obsidian property:get name="updated" file="<claim>"
   ```
   If the claim is past its domain TTL (see `CLAUDE.md`), flag it in the answer: "This is based on a claim verified 120 days ago; the TTL for this domain is 90 days — treat as stale."
6. Draft the answer. Every factual assertion names its source file with a wikilink.
7. If the answer synthesized something new and non-obvious that isn't already captured, offer `file-answer` to the owner.
8. Log the query:
   ```
   obsidian append file="log.md" content="- <YYYY-MM-DD> query: <topic> → cited <file1>, <file2>"
   ```

**Never:**
- Answer from general knowledge without grounding in vault files.
- Use `WebSearch` to fill gaps. If the vault can't answer, say so and propose a research prompt for the owner's GUI session.
- Paste content from one file into your answer without citing it.
