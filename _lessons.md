---
id: _lessons
type: system
status: active
created: 2026-05-28
updated: 2026-06-11
tags:
  - system
  - meta
---

# Lessons

Living ledger of corrections from the owner. Read at session start alongside `GAPS.md`. Goal per `CLAUDE.md`: same mistake never twice.

**This file is the distilled digest** — one short entry per lesson, rule-first. Full original entries (incident detail, reasoning, code) live verbatim in `_lessons_archive.md` under the **same headings**; open `[[_lessons_archive#<heading>]]` when the digest isn't enough.

---

## Format

- **New lessons:** append in full under **Your Lessons** (heading = the pattern; body = **Why:** + **How to apply:** blocks), newest at the bottom.
- **During lint:** condense entries older than ~30 days to digest form and move the full text verbatim (same heading) to `_lessons_archive.md`.
- Never delete a lesson from either file — the history is the value. If a lesson is superseded, strike through the heading and add `→ see <new heading>`.

---

## About these starter lessons

The lessons below were inherited from another vault's ledger — *universal* gotchas (shell quoting, CLI discipline, search-before-claiming, etc.) that the original vault owner ran into during early Claude Code work and that any Claude-Code-driven vault is likely to hit. They ship already digested; the full versions are in `_lessons_archive.md`. Read them once, absorb the patterns, and let them act as guardrails during your first weeks.

Your own corrections — the ones specific to *your* workflow, *your* vault, *your* preferences — go under **Your Lessons** at the bottom, so you can tell at a glance which lessons are inherited starter wisdom vs. ones you earned through your own use.

---

## Inherited starter lessons

### Use a quoted heredoc when an `obsidian append` or `create` body contains backticks, apostrophes, or `$`

Single-line `content="..."` arguments get shell-evaluated — backticks trigger command substitution, `$` triggers expansion, and inline-code references silently vanish. Build content via a quoted-sentinel heredoc (`<<'SENTINEL_END'`) into a variable, then pass `content="$content"`; pick a sentinel unique to the call site, never bare `EOF`. (Superseded in practice by the Write-tool-tempfile pattern below.)


### Use the Obsidian CLI, not `Edit` or `Write`, for vault file mutations

"No clean CLI primitive for this operation" is not a fall-through case. Read the full current content, construct the new body, `obsidian create overwrite path=...` — or restructure the file so `append`/`prepend` fits. Never reach for `Edit`/`Write` on vault files; verify destructive overwrites with `obsidian read`.


### Propagate confirmed state changes without asking

When the owner verbally confirms a state change ("yes, the offer was accepted"), fix **all** stale references to the prior state in the same turn — the confirmation IS the trigger. Counter-pattern (still ask first): structural changes (schema, domain port, overview rebuild) or propagation touching unrelated decisions.


### Always cite a claim with `[[file#claim_id]]` — never bare `[[claim_id]]` or `[[file.claim_id]]`

Bare ids resolve to nonexistent top-level notes; dot syntax is treated as part of the filename. Both break the citation chain silently. Resolve the claim to its parent file and link `[[file#claim_id]]`; run `obsidian unresolved` after any new claim-citation write — a 1-second guard.


### `obsidian create overwrite` requires `path=` only — never both `name=` and `path=`

`name=` alongside `overwrite` triggers conflict handling and silently creates a numbered duplicate (`<file> 1.md`) while the original stays untouched. Overwrite by `path=` alone. Note `name=` also resolves against the vault root — after a `move`, targeting by `name=` spawns a fresh root duplicate; the CLI answering `Created:` instead of `Overwrote:` is the tell.


### Don't construct vault file content via shell heredoc + `$TMPDIR`; build the tempfile via the `Write` tool first

An unset `$TMPDIR` makes `$content` resolve empty — and `create overwrite` then blanks the target file. Standing write pattern: (1) `Write` tool → explicit absolute path under `/tmp/claude/`; (2) separate Bash call `content=$(cat /tmp/claude/<file>); obsidian create overwrite path=... content="$content" silent`; (3) **always** verify with `obsidian read` — the CLI's success message is not proof of content.


### `obsidian create overwrite` silently fails on large content (>~30 KB); use `obsidian eval` (in-app `app.vault.modify` + Node `fs`) for big or targeted writes

Large argv content cold-boots a fresh instance and never writes (exit 0 or 124, both lying). Two safe patterns that keep large content off argv: (1) targeted edit — `eval` IIFE reads the file in-app, `split().join()`, `app.vault.modify`; (2) large body — `Write` tool → `/tmp/claude/body.md`, then `eval` with `require('fs').readFileSync` + `modify`. Gotchas: the param is `code=`; wrap statements in `(async()=>{...})()`. `eval` *is* the CLI — Sync propagates correctly. Snippets in `_lessons_archive.md`.


### Research-file synthesis tables are frozen against the owner-state they were written for — verify applicability before reusing them

Research files are frozen at GUI-session time and embed assumptions about the owner's state. Before reusing a synthesis table in an answer, check its embedded assumptions against `_start_here.md` §1 and the domain overview's current state; if anything drifted, adjust in the *answer* — the research file stays frozen, overviews win on conflict.


### Ask for measured values before percentiling owner-specific physiology

Modeled estimates for personal physiology can be off by full deciles vs. actual measurements. For *what-is-true-now* questions, check the vault for measured values; if none, ask "do you have measured values for this?" before percentiling. Modeled estimates are for before-data planning and hypotheticals only.


### Search the vault before claiming you lack context on a topic

A `/clear` wipes *conversation* context, not the *vault*. Before saying "I don't have context on X": grep/glob the obvious keywords and check `index.md`. Especially when the owner says "continue" — that word presupposes a prior artifact exists; find it first.


### Never `print(json.dumps(<config>))` on credential-bearing configs — and the same caution applies to `cat`, `grep -A/-B`, `head`, `tail`, `less` on per-line-JSON or YAML credential files

Any broad-output read of a credential-bearing config lands keys and tokens in the transcript. Use a targeted read: locate the entry without dumping it, print only named non-credential fields, echo lengths (not values) for credential-shaped fields. Before any dump-like call, ask: "would a credential land in this output?"


### Distinguish "fix" from "design refinement" when a side effect surfaces

**Bug fix** (the config doesn't do what was asked) → just fix it. **Design refinement** (the config does what was asked, but the use case has unconsidered implications) → surface the tradeoff and options; the owner picks. Diagnostic: "is there a defensible alternative a reasonable person would prefer?" Yes → ask; no → fix. Mechanical fact-propagation on confirmed state changes stays autonomous.


### Under `set -euo pipefail`, `cmd | head -c N` (or any early-closing reader) silently exits the script via SIGPIPE

The early-closing reader sends the upstream SIGPIPE (exit 141); `pipefail` + `set -e` kill the script with no message. Defuse with `|| true` inside the command substitution, prefer a single-process generator (`openssl rand`), or locally `set +o pipefail`.


### Don't write files into containers/VMs via nested `bash -lc 'cat <<MARKER ... MARKER'` — `pct push` / `scp` or base64 round-trip instead

The nested heredoc's closing marker jams against the outer closing quote, so the inner bash slurps through EOF and writes garbage. Build the file on the host, then `pct push` (LXC) / `scp` (VM) — or base64-encode on the host and decode inside. Diagnostic: `here-document at line 1 delimited by end-of-file`.

---

## Your Lessons

> Append your own corrections below as they come up, in full: heading (the pattern), then `Why:` and `How to apply:` blocks. Lint condenses entries older than ~30 days into digest form and moves the full text to `_lessons_archive.md`.

<!-- new entries go here, newest at the bottom -->
