---
id: skill_file-answer
type: system
status: active
created: 2026-05-28
updated: 2026-05-28
tags:
  - system
  - skill
aliases:
  - file-answer
---

# file-answer

**Goal:** Turn a valuable synthesized answer from a `query` session into a permanent vault page so the insight compounds.

**When to invoke:** When a query produces a synthesis that (a) isn't already captured in the vault, (b) the owner confirms is worth keeping, and (c) is generalizable beyond the specific question asked. Not every query. Only the ones where the answer itself becomes a future source.

**Steps:**

1. Decide the file type. Usually `analysis` (if it combined claims into a comparison) or `research` (if it represents new synthesized knowledge).
2. Create the new page:
   ```
   obsidian create name="<descriptive title>" content="---
id: <slug>
type: analysis
domain: <domain>
status: active
created: <date>
updated: <date>
cites: [<id>, <id>]
---

<body>" silent
   ```
3. Link it from the relevant domain overview:
   ```
   obsidian append file="<domain>_overview" content="- [[<new page>]] — <one-line>"
   ```
4. Add it to `index.md`:
   ```
   obsidian append file="index.md" content="- [[<new page>]] — <one-line>"
   ```
5. Verify bidirectional findability (per `CLAUDE.md` Core Rule 6). Same procedure as `ingest` Step 9, abbreviated:

   - **Top-down:** `_start_here.md` §3 → `<domain>_overview.md` → (subfolder `*_index.md` if applicable) → new file. Walk it; fix any missing pointer.
   - **Bottom-up:** `obsidian backlinks file="<new page>"` — expect at minimum `index.md` + the domain overview + the subfolder index (if one exists) + any analysis or decision that cites it.
   - **Cross-domain:** if the analysis combines claims from multiple domains (which is common — that's *why* the synthesis was worth filing), each affected overview gets a pointer, not just the home one.

   If either walk fails, the file is an orphan-in-waiting. Add the missing links before logging.

6. Log it:
   ```
   obsidian append file="log.md" content="- <YYYY-MM-DD> filed: <new page> (from query: <topic>)"
   ```

**Never:**
- File an answer that restates existing vault content without adding synthesis.
- Create the page without setting `cites:` to the files it rests on.
- Skip the `index.md` entry — orphan files are lint errors.
- Declare done before Step 5's two walks pass. See `CLAUDE.md` Core Rule 6.
