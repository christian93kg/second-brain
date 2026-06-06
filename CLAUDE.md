---
updated: 2026-05-28
---
# Life Optimization Vault — Claude Instructions

You are operating on a **life optimization vault**: a curated knowledge base where high-quality research flows down into decisions about a specific person's life. This is not a notes dump. Every file earns its place.

---

## Session-Start Read Order

At the beginning of every session, read these files in this exact order before answering anything:

1. **`CLAUDE.md`** (this file) — conventions and principles.
2. **`SKILLS.md`** — operating procedures (ingest, query, lint, decision-review, report, file-answer).
3. **`_start_here.md`** — the owner's profile, current state, and domain map.
4. **`GAPS.md`** — outstanding gaps, stale files, contradictions, and overdue decision reviews from the last lint pass.
5. **`_lessons.md`** — living ledger of corrections from the owner. Same goal: same mistake never twice.

If any of those five files are missing, say so and stop. Do not guess structure or fall back to general knowledge.

---

## What This Vault Is

A life optimization vault organizes research and decisions across life domains (examples: career, finance, where to live, health, travel, privacy, philosophy). Its atomic unit is not "a note" — it is a **claim flowing into a decision**. A well-run vault can answer four questions at any moment:

- What has been decided, and why?
- What is still open, and what is blocking it?
- Which facts are fresh, which are stale, and which contradict each other?
- What research is needed next?

Your job as the assistant is to **maintain that state** and help the owner make decisions against it — not to invent content, not to give generic advice, not to answer from memory.

---

## The Four-Layer Flowdown

Information flows strictly downward. Each layer cites the layer above it. Never skip layers.

```
Profile   →   Claims    →   Analyses   →   Decisions
(who)         (facts)       (compare)      (commit)
```

- **Profile** — who the owner is. Constraints, non-negotiables, identity, and whatever context shapes the owner's decisions — recorded only at a level they're comfortable being public. Lives in `_start_here.md`. **One canonical copy.** Every other file that needs profile data transcludes or links; nothing duplicates.
- **Claims** — atomic facts with provenance. Each claim has an assertion, a source, a confidence level (`high | medium | low`), and a verification date. Claims are the load-bearing layer — most contradictions live here, and most lint checks target them.
- **Analyses** — structured comparisons that cite claims to produce rankings, tradeoffs, scoring matrices, cost models. An analysis never states a fact; it only combines claims.
- **Decisions** — commitments with status (`decided | open | revised`), an analysis they depend on, and a revisit trigger (a date, or a claim whose change would reopen the decision). Logged in `_decisions.md`.

This flowdown is what makes contradictions detectable and decisions reviewable. If you find yourself wanting to shortcut it (e.g., writing a "fact" directly into a decision without a claim behind it), stop and create the claim first.

---

## Core Rules

1. **One fact, one home.** Never duplicate a value across files. Pick the canonical location; everywhere else, transclude (`![[file#section]]`) or link.
2. **Cite or flag.** Every claim states its source. If a source can't be produced, mark the claim `unverified` and surface it in lint.
3. **Freshness matters.** Every file has an `updated:` date. Treat anything past its domain's TTL as possibly stale — verify before recommending.
4. **Overviews beat research files.** Research files are inputs; overviews are current state. If they conflict, the overview wins. If the overview conflicts with `_start_here.md`, `_start_here.md` wins.
5. **Chains must close.** Decisions cite analyses; analyses cite claims; claims cite sources. Broken chains are lint errors, not judgment calls.
6. **Findability closes both ways.** Every new file must be reachable **top-down** (`_start_here.md` → `<domain>_overview.md` → subfolder `*_index.md` if present → file) **and bottom-up** (the file's backlinks resolve to `index.md`, the domain overview, and any subfolder index above it). The link ladder is the test: if you can't walk from `_start_here.md` to the file via links, and from the file back up via backlinks, the filing isn't done. Cross-domain content gets a pointer from *every* overview it touches. Operationalized in `SKILLS.md` → `ingest` and `file-answer` as an explicit verification step. **This rule fires on every ingest, every filed answer, every new page** — no exceptions.
7. **No orphans.** A file with no backlinks and no decision depending on it is a candidate for archival. Don't create speculative pages.
8. **Don't re-litigate decided items** unless the revisit trigger has fired or the owner explicitly reopens them.
9. **Research is prompted out, not generated in-session.** Deep research happens in a separate Claude GUI session where the owner controls the inputs. Do not use `WebSearch` for substantive research — the vault's quality depends on the owner curating what enters it. In-session, you synthesize from what's already in the vault or process research the owner brings back.

---

## Obsidian CLI

Vault mutations should go through an Obsidian CLI (the `obsidian` command), not direct filesystem writes. The CLI keeps Obsidian's index in sync with the filesystem; bypassing it can desync the graph view, break wikilinks the editor sees, and let the owner and assistant disagree on vault contents.

If you don't yet have the CLI installed, see `BOOTSTRAP.md` for the install pointer. Reading files via `Read`/`Grep`/`Glob` (or `obsidian read` / `obsidian search`) is always fine — the restriction is on **writes**.

See `SKILLS.md` for which CLI command to use in which workflow.

---

## Frontmatter Schema

Every file must have YAML frontmatter. Fields marked **required** apply to all files; fields marked *conditional* apply only to specific types.

```yaml
---
id: <stable slug>                       # required — used for citations
type: <see types table>                 # required
domain: <one of the domain tags>        # required (except type: system)
status: active | draft | complete | superseded | archived   # required
created: YYYY-MM-DD                     # required
updated: YYYY-MM-DD                     # required
tags: [<list>]                          # required
aliases: [<list>]                       # optional

# Conditional fields:
confidence: high | medium | low         # claims only
source: <url or citation>               # claims and research only
verified_on: YYYY-MM-DD                 # claims only
cites: [<id>, <id>]                     # analyses, decisions, plans
depends_on: [<id>, <id>]                # decisions, plans
revisit_on: YYYY-MM-DD                  # decisions, plans
revisit_if: <claim id>                  # decisions, plans — reopen if this changes
supersedes: <id>                        # files that replace older versions
superseded_by: <id>                     # files that have been replaced
---
```

### File Types

| `type` | Purpose |
|---|---|
| `profile` | The owner's profile. Exactly one file: `_start_here.md`. |
| `overview` | Domain entry point. Current state, open items, links to claims/analyses/decisions. |
| `claim` | An atomic fact with source, confidence, verification date. |
| `analysis` | Structured comparison built from cited claims. |
| `decision` | A commitment with status, depends_on, revisit trigger. Entries live in `_decisions.md`. |
| `plan` | Multi-domain implementation scaffold (e.g., a 14-week fitness block). Time-bound; references claims; slotted into by domain files. Revisits on completion date or when a depended-on claim materially changes. |
| `research` | Deep research output brought in from a GUI session. Raw synthesized material waiting to be broken into claims. |
| `log` | Temporal data: daily notes, weekly reviews. |
| `system` | Vault governance: `CLAUDE.md`, `SKILLS.md`, `index.md`, `log.md`, `GAPS.md`. |
| `reference` | Stable reference material. Rarely changes. |

---

## Staleness TTLs

Different domains age at different rates. Lint uses these to flag stale files. **Tune per vault** — the defaults below are a starting point. Edit this table when you understand how fast each of your domains actually drifts.

| Domain | TTL | Rationale |
|---|---|---|
| `finance` | 90 days | income/markets shift moderately; tax-treaty research doesn't drift in 30d |
| `career` | 90 days | job market, certifications, salary bands |
| `body` | 90 days | protocols, labs, routines |
| `privacy` | 90 days | threat landscape, data broker state |
| `locations` | 180 days | cities change slowly |
| `travel` | 180 days | gear, systems, logistics |
| `philosophy` | none | principles don't expire |

If a domain isn't listed here, default to 90 days. **Add new domains to this table as your vault grows** — the lint pass reads this table to flag staleness, so an undocumented domain falls back to the 90-day default.

---

## The System Files

| File | Role |
|---|---|
| `CLAUDE.md` | This file. Auto-loaded. The contract. |
| `SKILLS.md` | Operating procedures. Every workflow Claude runs is defined here. |
| `_start_here.md` | Owner's profile, current state, domain map. The one-pager the owner reads. |
| `index.md` | Content catalog. Every page in the vault, one-line description. Updated on every ingest. |
| `log.md` | Append-only operations log. One line per ingest, query-filed, lint run, report. Grep-friendly. |
| `GAPS.md` | Lint output. Stale, orphans, contradictions, overdue decision reviews, broken chains. Overwritten each lint run. |
| `_decisions.md` | Decision log with revisit triggers. May be promoted to the primary index over time. |

**`_start_here.md` and `index.md` are different files.** `_start_here.md` is the dashboard — profile + current priorities + domain map, read by the owner. `index.md` is the catalog — every page in the vault with a one-line summary, used by Claude for discovery. Do not merge them.

---

## Answering Questions

1. Start with `_start_here.md` for profile and current state.
2. Identify the domain(s). Read each overview file.
3. Drill into specific claims, analyses, or research only if the overview is insufficient. **Before reading multiple files in any subfolder, check for a `*_index.md` at the subfolder root and read it first** — it exists precisely to keep you from having to read every file in the folder. See `conventions.md` §9.
4. Cite file paths in every answer. "See `career/career_overview.md` certification table" beats "check your certs."
5. Flag staleness when it matters. "This claim was verified 120 days ago — past its TTL. Want me to mark it for reverification?"
6. Connect across domains. The vault's value is in the links, not the pages. A location question is also a finance question and a body question.
7. When a question produces a valuable synthesis that isn't already captured, offer to file it. See `SKILLS.md` → `file-answer`.

---

## Never

- Never bypass the Obsidian CLI for vault operations.
- Never duplicate profile data outside `_start_here.md`.
- Never answer from a single file without cross-checking the domain overview.
- Never give generic advice. Every answer is calibrated to the specific owner's profile.
- Never re-open decided items without the revisit trigger firing.
- Never run `WebSearch` for research. Research is curated by the owner.
- **Never solicit sensitive specifics.** Don't ask the owner for diagnoses, dosages, account numbers, credentials, or precise personal identifiers. Record only what the owner volunteers, at the level they choose. Treat the vault as if it could become public; if something the owner pastes looks sensitive, note it and offer to generalize before filing.
- Never write a claim without a source. If you don't have a source, don't write the claim.
- Never create a page without **both** an inbound link path from `_start_here.md` / a domain overview / a subfolder index **and** outbound links that anchor it back to its parent context. One-way attachment is an orphan in waiting — see Core Rule 6.

---

## Working Principles

Adapted from general Claude Code practice; applied to vault work, not software.

- **Plan first on non-trivial work.** Any task touching 3+ files, a domain port, a walk, or a schema change gets a written plan before edits. Simple single-file fixes don't.
- **Confirm mental model before multi-file sweeps.** Before editing 3+ files, restate (a) what the key figures in scope mean, (b) what file types exist in the folders you'll touch and their purpose, (c) which tools you'll use for writes. Wait for confirmation. Prevents silent-propagation errors where one wrong assumption cascades across many files.
- **Re-plan when sideways.** If a walk uncovers something that invalidates the plan, stop and re-scope. Don't push through a stale plan.
- **Subagents for exploration.** Use `Explore` / `general-purpose` agents for multi-file research, cross-domain scans, or "is X referenced anywhere" sweeps. Keeps the main context clean. One focused task per agent.
- **Lessons file.** After any correction from the owner, append the pattern to `_lessons.md`. Review at session start alongside `GAPS.md`. Goal: same mistake never twice.
- **Verify before "done".** A walk isn't complete until: inbound refs on surviving files are clean, `GAPS.md` is updated, `log.md` has the entry, and the ceiling (`_start_here.md` / domain overview) reflects the new state. "I edited the files" is not done.
- **Root cause over patch.** If a claim contradicts an overview, find *why* — don't just edit one side to match. If a wikilink is broken, find whether the target was renamed, deleted, or never existed.
- **Minimal impact.** Touch only what the task requires. A fix to one claim doesn't justify reformatting the file. A walk doesn't justify rewriting research.
- **Autonomous gap closure.** When `GAPS.md` lists something actionable and in-scope, just fix it. Don't narrate options for trivial cleanups.
- **Demand elegance on structural changes, not on fixes.** Before a schema change, domain port, or overview rebuild, ask "is there a cleaner shape?" Skip this on typo fixes and ref repairs.

---

*This file is the contract between the vault owner and the assistant. If any rule here conflicts with a behavior you're about to take, stop and ask.*
