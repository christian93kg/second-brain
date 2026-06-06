---
id: _lessons
type: system
status: active
created: 2026-05-28
updated: 2026-05-28
tags:
  - system
  - meta
---

# Lessons

Living ledger of corrections from the owner. Appended to after any correction. Read at session start alongside `GAPS.md`. Goal per `CLAUDE.md`: same mistake never twice.

---

## Format

Each lesson is one H2 heading (the pattern), with two short blocks under it:

- **Why:** the reason the owner gave or the incident that prompted it.
- **How to apply:** when/where this kicks in.

Keep entries short. If a lesson is superseded, strike through the heading and add `→ see <new heading>`. Do not delete lessons — the history is the value.

---

## About these starter lessons

The lessons above the divider were inherited from another vault's ledger — they are *universal* gotchas (shell quoting, CLI discipline, search-before-claiming, etc.) that the original vault owner ran into during early Claude Code work and that any Claude-Code-driven vault is likely to hit. Read them once, absorb the patterns, and let them act as guardrails for Claude during your first weeks.

Your own corrections — the ones specific to *your* workflow, *your* vault, *your* preferences — go below the divider. The two parts stay separate so you can tell at a glance which lessons are inherited starter wisdom vs. ones you earned through your own use.

---

## Inherited starter lessons

### Use a quoted heredoc when an `obsidian append` or `create` body contains backticks, apostrophes, or `$`

**Why:** Single-line `content="..."` arguments to the CLI get shell-evaluated — backticks trigger command substitution, `$` triggers variable expansion. A content body with inline-code references like `` `some_file.md` `` will silently lose them.

**How to apply:** Build the content via a quoted heredoc (`<<'SENTINEL_END'`) with a sentinel marker unique to the call site, capture it into a shell variable, then pass `content="$content"`. The single-quoted sentinel disables expansion inside the heredoc; passing `"$content"` only expands the variable once, with no re-evaluation of its contents. Pick a sentinel that won't appear in the body (avoid bare `EOF` if you're writing about heredocs themselves).


### Use the Obsidian CLI, not `Edit` or `Write`, for vault file mutations

**Why:** `CLAUDE.md` mandates the CLI as the only sanctioned mutation channel — the CLI keeps Obsidian's index in sync, preserves wikilinks, and prevents the owner and assistant from disagreeing on vault contents. "No clean CLI primitive for this operation" is not a fall-through case; it's a case for `obsidian create overwrite` with the full new body, or for restructuring the target file so `append`/`prepend` is sufficient.

**How to apply:** When you want to insert a line into an existing file and the CLI has no `insert-at-line` primitive: (1) read the full current content, (2) construct the desired new content, (3) `obsidian create path="<file>" content="<combined>" overwrite silent`, (4) verify with `obsidian read`. If the file is well-structured (sections with clear boundaries), `append` or `prepend` is usually sufficient — restructuring slightly to enable that is preferable to reaching for `Edit`.


### Propagate confirmed state changes without asking

**Why:** When the owner verbally confirms a state change ("yes, the offer was accepted," "yes, that lab was drawn"), all stale references to the prior state in the affected file(s) should update in the same turn. Flagging the stale line and asking "want me to fix that too?" wastes a round-trip on a mechanical correction.

**How to apply:** Whenever the owner verbally confirms something about a vault file is now true / false / changed, propagate the correction across the file in the same turn. The verbal confirmation IS the trigger to fix — do not require a second explicit instruction. **Counter-pattern (still ask):** structural changes (schema, domain port, overview rebuild) or propagation that would touch unrelated decisions. Mid-file factual corrections to a single section are not in this category.


### Always cite a claim with `[[file#claim_id]]` — never bare `[[claim_id]]` or `[[file.claim_id]]`

**Why:** Bare `[[claim_id]]` resolves to a non-existent top-level note, not the section anchor inside the claim file. `[[file.claim_id]]` treats the `.` as part of the file basename and also fails. Both forms break the citation chain silently and pollute the unresolved-link signal until lint runs.

**How to apply:** Before writing a wikilink to a claim, mentally resolve it to its parent claim file. If the target is a section heading inside a claim file, it MUST use `#`. When in doubt, `obsidian search query="<claim_id>"` finds the parent file. After any new claim-citation write, run `obsidian unresolved` once — it's a 1-second guard against this exact mistake.


### `obsidian create overwrite` requires `path=` only — never both `name=` and `path=`

**Why:** When `path=` and `name=` are both passed alongside `overwrite`, the CLI's conflict-handling suffixes the new file with a number instead of honoring the overwrite flag, silently creating `<file> 1.md` next to the original. The original is untouched, the intended overwrite never happens.

**How to apply:** For in-place rewrites, pass **only** `path="<full/path/to/file.md>"`. Drop the `name=` argument entirely.


### Don't construct vault file content via shell heredoc + `$TMPDIR`; build the tempfile via the `Write` tool first

**Why:** `$TMPDIR` is not always set in the shell the tool runs in. When it's empty, a `cat > "$TMPDIR/<file>" <<'END' ... END` resolves to `cat > /<file>`, which fails with permission denied, and the subsequent `content=$(cat "$TMPDIR/<file>")` resolves to empty — at which point `obsidian create overwrite content="$content"` happily overwrites the target file with nothing. The CLI's success message is not proof of correct content; only a follow-up `obsidian read` is.

**How to apply:** For non-trivial content (multi-line, backticks, interpolation hazards):

1. Use the `Write` tool to land the content in an explicit absolute path under `/tmp/claude/`. Do not rely on `$TMPDIR` expansion.
2. In a separate Bash call: `content=$(cat /tmp/claude/<file>); obsidian create overwrite path=<target> content="$content" silent`.
3. **Always** follow a destructive `obsidian create overwrite` with `obsidian read` to verify the new content landed.


### Research-file synthesis tables are frozen against the owner-state they were written for — verify applicability before reusing them

**Why:** Research files reflect what was true when the GUI session produced them. Synthesis tables (per-X ratings, timeline estimates, dose-response recommendations) typically embed assumptions about the owner's state at that time. Reusing the synthesis literally in a future query — without checking whether the underlying assumptions still hold — can produce confidently wrong answers calibrated to a prior version of the owner.

**How to apply:** Before reusing a research file's *synthesis tables* in an answer, read the file's intro paragraph and per-row notes for embedded assumptions about the owner's state. Cross-check those assumptions against `_start_here.md` §1, the relevant domain overview's current-state section, and the conversation. If anything has drifted, the synthesis layer needs adjusting in the answer — not in the research file. Research stays frozen; overviews win on conflict per `CLAUDE.md` flowdown.


### Ask for measured values before percentiling owner-specific physiology

**Why:** Modeled estimates for personal physiology (labs, biomarkers, BF%, performance metrics, sleep architecture) can be off by full deciles vs. actual measured values. Answering "how high above average is X for me?" from a model — when the owner has actual labs you didn't ask about — is a misframe waiting to happen.

**How to apply:** Before giving any percentile / population-comparison / "how am I tracking" answer about owner-specific physiology, ask: "do you have measured values for this?" Modeled estimates are appropriate for *what-to-expect-before-data* questions (planning, hypotheticals). For *what-is-true-now* questions, defer to measured data — and if the measured data isn't in the vault, ask before percentiling. The cost of asking is one turn.


### Search the vault before claiming you lack context on a topic

**Why:** A `/clear` or fresh session wipes *conversation* context, not the *vault*. "I don't recall" or "we didn't talk about this" is not a final answer — the vault is the durable memory. Treating absent-from-context as absent-from-vault, and asking the owner to re-explain, makes the assistant look broken when the answer was sitting one search away.

**How to apply:** Before telling the owner you lack information on something, grep/glob the vault for the obvious keywords (domain name, project name, hardware names, person names) — and check `index.md`. Especially when the owner says "continue" — that word presupposes a prior artifact exists. Find it first.


### Never `print(json.dumps(<config>))` on credential-bearing configs — and the same caution applies to `cat`, `grep -A/-B`, `head`, `tail`, `less` on per-line-JSON or YAML credential files

**Why:** Service-config files often carry API keys, OAuth tokens, or refresh secrets in the same dict as the non-credential fields you want to read. Dumping the whole entry — or pulling adjacent context with `grep -A/-B` on per-line-JSON — lands those credentials in the conversation transcript. Rotation + audit are the cleanup cost.

**How to apply:** Before reading any field from a credential-bearing config (`~/.<app>.json`, `~/.<app>/config.json`, `.storage/core.config_entries` style files, env dumps, `secrets.yaml`, supervisor add-on `info` responses), structure the read as a targeted Python expression:

```python
import json
d = json.load(open("/path/to/config"))
e = next((x for x in d["entries"] if x.get("domain") == "<target>"), None)
print("entry_id:", e["entry_id"])
print("modified_at:", e.get("modified_at"))
print("non-cred keys:", sorted(k for k in e["data"] if k not in {"api_key", "token", "secret", "password"}))
```

If a field might be credential-shaped but you only need its existence/length, print `len(e["data"]["<field>"])`, not the value. Before any `cat | head | grep | less` on a credential-bearing path, ask: "is my command going to bring more than my one target field into stdout?" If yes or unsure, switch to the targeted-read pattern.


### Distinguish "fix" from "design refinement" when a side effect surfaces

**Why:** When a side effect of one configuration shows up during testing, two cases blur together: (a) the config doesn't do what was asked — just fix it; (b) the config *does* what was asked, but the owner hadn't considered the implication — surface the tradeoff. Treating case (b) as case (a) means unilaterally re-architecting around an assumption about the owner's preference, which often gets reverted.

**How to apply:** Before "fixing" a side effect, ask: *is there a defensible alternative that a reasonable person would prefer?* If yes, surface options to the owner. If no — the right behavior is unambiguous — just fix it. Bug-fix and state-correction are mechanical; design-refinement is judgment. **Counter-pattern (still autonomous):** mechanical fact-propagation when the owner verbally confirms a state change — that lives in the mechanical bucket.


### Under `set -euo pipefail`, `cmd | head -c N` (or any early-closing reader) silently exits the script via SIGPIPE

**Why:** `head -c N` closes its stdin after N bytes; the upstream `cmd` gets SIGPIPE and exits 141; `pipefail` propagates the non-zero through the pipeline; `set -e` kills the script with no error message. Symptom: the script exits cleanly right after the prior `log` / `echo` line. The owner sees the script "succeed" but downstream steps don't run.

**How to apply:** Never write `cmd | head -c N` (or any pipeline ending in an early-closing reader) inside `set -o pipefail` without a defuse. Options in preference order:

1. Suffix the pipeline with `|| true` inside the command substitution: `PW="$(LC_ALL=C tr -dc 'A-Za-z0-9' </dev/urandom | head -c 20 || true)"`.
2. Use a single-process generator that doesn't pipe: `openssl rand -hex 10` or `openssl rand -base64 15`.
3. Locally disable pipefail: `set +o pipefail; PW="$(... | head -c 20)"; set -o pipefail`.


### Don't write files into containers/VMs via nested `bash -lc 'cat <<MARKER ... MARKER'` — `pct push` / `scp` or base64 round-trip instead

**Why:** Nested heredocs inside `bash -lc '...'` jam the closing marker against the outer closing quote. Bash inside the container parses the heredoc, never sees the marker on its own newline-ended line, slurps through EOF, and writes garbage to the target file. Diagnostic: `here-document at line 1 delimited by end-of-file (wanted MARKER)`.

**How to apply:** For file delivery into LXCs / VMs / remote shells, build the file on the host (the outer-shell heredoc terminator gets its newline naturally because it's a real shell line), then `pct push` (LXC) or `scp` (VM) it in. If that's not available, base64-encode on the host and decode in the container. Avoid nested heredocs as a delivery mechanism — they look concise but are brittle to the layer beneath the wrapper.

---

## Your Lessons

> Append your own corrections below as they come up. Format: H2 heading (the pattern), then `Why:` and `How to apply:` blocks. Same shape as the inherited lessons above.

<!-- new entries go here, newest at the bottom -->
