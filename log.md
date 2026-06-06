---
id: log
type: log
status: active
created: 2026-05-28
updated: 2026-05-28
tags:
  - system
  - log
---

# Log

Append-only operations log. One line per vault operation: ingest, query-filed, lint pass, decision-review, report, archive, port. Grep-friendly — sortable by date, searchable by action type.

**Format:** `- YYYY-MM-DD <action>: <one-line description>`

**Action types:** `ingest`, `query`, `filed`, `lint`, `decision-review`, `report`, `archive`, `port`, `edit`, `walk`, `inbox triage`.

---

<!-- Example lines (delete after your first real entries):

- 2026-05-28 setup: vault initialized from the brain/ starter kit; system files in place; _start_here.md drafted
- 2026-05-29 ingest: karpathy_llm_wiki → extracted to references/karpathy_llm_wiki.md; promoted from _inbox/
- 2026-05-29 lint: 0 stale, 1 orphan, 0 overdue, 2 unresolved (baseline pass)

-->

- 
