---
id: SKILLS
type: system
status: active
created: 2026-05-28
updated: 2026-05-28
tags:
  - system
  - skills
  - index
aliases:
  - Vault Operating Skills
---

# Vault Operating Skills

Index of operating procedures for the vault. Each skill lists when to invoke it and links to a detail file in `_skills/` containing the full procedure. **Every action in vault skills goes through the Obsidian CLI.** No direct file I/O.

Read this file after `CLAUDE.md` at session start. **When a procedure is invoked, read its detail file before executing.** When the owner asks a question, you will almost always be running `query`. When they bring back deep research from a GUI session, you will be running `ingest`. When things feel off, run `lint`.

---

## Skill Index

| Skill | When to invoke | Output | Detail |
|---|---|---|---|
| query | Owner asks a question | An answer with citations, optionally filed | [[_skills/query]] |
| ingest | Deep research arrives from a GUI session | Claims filed, overviews updated, superseded files marked, **bidirectional findability verified** (CLAUDE.md Core Rule 6), log entry | [[_skills/ingest]] |
| file-answer | A synthesized query answer is worth keeping | New page linked into the domain, entry in `index.md`, **bidirectional findability verified** | [[_skills/file-answer]] |
| lint | On demand, after every ingest, weekly cadence | `GAPS.md` auto-sections refreshed below delimiter; manual sections preserved | [[_skills/lint]] |
| decision-review | Weekly, or when ingest touches a cited claim | Flagged decisions in `GAPS.md` | [[_skills/decision-review]] |
| report | After ingest, weekly | `_start_here.md` quick reference refreshed, `index.md` updated | [[_skills/report]] |
| confirm-sweep | Before any sweep touching 3+ files | Explicit mental-model restatement; no edits until owner confirms | [[_skills/confirm-sweep]] |

---

## CLI reference

The Obsidian CLI is the only sanctioned write channel for vault files. Common commands:

```
obsidian read file="name"                    # read by wikilink-style name
obsidian read path="folder/note.md"          # read by exact path
obsidian create name="Title" content="..." silent
obsidian append file="note" content="..."
obsidian prepend file="note" content="..."
obsidian search query="term" limit=20
obsidian search:context query="term"
obsidian property:set name="key" value="val" file="note"
obsidian property:get name="key" file="note"
obsidian backlinks file="note"
obsidian unresolved
obsidian tags sort=count counts
```

Run `obsidian help` for the full reference once installed. See `BOOTSTRAP.md` for the install pointer.

**Tips:**
- Always pass `silent` on `create`/`append` so notes don't steal focus in the editor
- Pass `overwrite` to `create` to replace an existing file (use `path=` only, not `name=` — see `_lessons.md`)
- Use `format=json` for structured output you need to parse

---

## When in doubt

Run `lint`. The `GAPS.md` output is the single best signal for what the vault needs next — stale files to verify, orphans to archive, decisions to revisit, research prompts to draft for the next GUI session.

A well-maintained vault should have a short, stable `GAPS.md` and a long, boring `log.md`. If `GAPS.md` keeps growing between lint runs, the vault is drifting faster than it's being maintained. If `log.md` stops growing, the vault is stagnant.

---

## References

Foundational external material about the wiki pattern this vault implements.

| File | What it is |
|---|---|
| [[karpathy_llm_wiki]] | Andrej Karpathy's original sketch of the LLM-maintained markdown wiki pattern (raw sources → LLM-owned wiki → schema config). This vault is one implementation of that pattern. Lives in `_inbox/` at delivery — promote or harvest on your first ingest. |
