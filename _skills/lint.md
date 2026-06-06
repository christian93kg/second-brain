---
id: skill_lint
type: system
status: active
created: 2026-05-28
updated: 2026-05-28
tags:
  - system
  - skill
aliases:
  - lint
---

# lint

**Goal:** Detect drift, staleness, orphans, unresolved links, duplicated facts, broken citation chains, and overdue decision reviews. Refresh the auto-generated section of `GAPS.md` while preserving the owner's manual annotations.

**Trigger:** On demand. After every `ingest`. On a weekly cadence if the owner doesn't run it manually.

## Output model — merge, not overwrite

`GAPS.md` is a hybrid file. Two parts:

- **Above** the delimiter marker `<!-- LINT-AUTO-OUTPUT-BELOW -->`: owner-curated content (Open / Deferred / Watch buckets, dated ingest residuals, Resolved blocks awaiting fold-down). **Lint never touches this.**
- **Below** the delimiter: auto-derived sections (Stale, Orphans, Unresolved, Duplicated facts, Status drift, Broken citation chains, Overdue decision reviews, Missing claims, Stale clips). **Lint regenerates this entirely on each pass.**

If the marker is missing from `GAPS.md`, surface the error to the owner — do not guess a position. Do not overwrite the file.

**Procedure:**

1. Read full `GAPS.md`:
   ```
   obsidian read file="GAPS"
   ```
2. Locate the line containing `<!-- LINT-AUTO-OUTPUT-BELOW -->`. Capture everything from the start of the file through (and including) that marker line as the **preserved prefix**.
3. Run the eight checks below, each producing one section of new auto-output.
4. Concatenate: `<preserved prefix>` + newline + `<new auto-output>`.
5. Write via overwrite:
   ```
   obsidian create path="GAPS.md" content="<combined>" overwrite silent
   ```
6. Set the `updated:` frontmatter date:
   ```
   obsidian property:set name="updated" value="<YYYY-MM-DD>" file="GAPS"
   ```
7. Log the run:
   ```
   obsidian append file="log.md" content="- <YYYY-MM-DD> lint: <N stale, M orphans, K overdue, L unresolved>"
   ```

## Checks (all feed sections below the marker)

### 1. Stale files (per-domain TTLs)
For each file in the vault, read its `updated` frontmatter date and compare against the TTL for its `domain` (from `CLAUDE.md`'s staleness table). Flag anything past TTL.

### 2. Orphans (no backlinks, not cited by anything)
For each file in the vault, check inbound links:
```
obsidian backlinks file="<file>"
```
Files with zero backlinks that are not of `type: system`, `type: profile`, or linked from `index.md` are orphan candidates. Surface them for archival decision — don't auto-archive.

### 3. Unresolved wikilinks
```
obsidian unresolved
```
Every unresolved link is either a typo (fix it) or a planned-but-unwritten page (add to gaps). Both are lint output.

### 4. Duplicated facts
Heuristic only — perfect duplication detection requires structured claims. For now:
- Identify known profile values from `_start_here.md` (income, key dates, current certifications, target cities).
- Search for each value across the vault:
  ```
  obsidian search query="<value>" limit=50
  ```
- Any occurrence outside the canonical file is a duplication candidate. Flag it.

### 5. Status/update drift
Files marked `status: active` but with `updated` older than 180 days. These need either a refresh or a status downgrade.

### 6. Broken citation chains
For each `type: decision` and `type: analysis` file, read its `cites:` field and confirm each cited id resolves to an existing file:
```
obsidian read file="<cited id>"
```
Missing citations = broken chains = lint error.

### 7. Overdue decision reviews
```
obsidian read file="_decisions.md"
```
For each decision, check `revisit_on` date. If past today, flag. Also check each `depends_on` claim — if any has been superseded since the decision was made, flag.

### 8. Stale clips (inbox accumulation)
List any file in `_inbox/` with `created` more than 14 days ago. Triage SLA per `conventions.md` §11 is 14 days; anything older has slipped. Surface under `## Stale clips` in the auto-output. Inbox files are exempt from the §1 staleness check (TTL doesn't apply to `domain: inbox`).

## Vault Health Readout (top of auto-output)

The first section below the delimiter is a synthesized readout: a single 0–100 score, a per-layer breakdown, and a ranked top-fixes list. The detail sections below it remain as-is — the readout summarizes, it doesn't replace.

### Scoring layers and weights

Total weight sums to 100. Claim-extraction backlog is **not scored** — it's an intentional gradual buildout, not drift; report as info only.

| Layer | Weight | Score formula | Notes |
|---|---|---|---|
| Chain integrity | 20 | 100 − 20×(broken chains) | Floors at 0. Broken `cites:` / `depends_on:` = wrong decisions. |
| Decision currency | 15 | 100 − 10×(overdue reviews) | Floors at 0. Each fired but un-acted `revisit_on` / `revisit_if`. Owner-deferred decisions still count — deferral is a real cost. |
| Findability | 15 | 100 − 5×(orphans) − 2×(one-way attachments) | Core Rule 6. Floors at 0. |
| Freshness | 12 | 100 × (1 − stale/total) | Proportional to vault size. |
| Fact consistency | 12 | 100 − 10×(duplication hits) | Heuristic until claim layer matures; intentional transcludes excluded. |
| Status accuracy | 10 | 100 − 5×(status-drift files) | `status: active` past 180d. |
| Link integrity | 10 | 100 − 2×(unintentional unresolved) | Intentional unresolved (templates, `_archive/`, `log.md` historical) excluded. |
| Inbox hygiene | 6 | 100 − 10×(stale clips) | Each clip past 14d. |

Overall = sum of (layer score × weight / 100). Round to integer. Always show both the rounded overall and the breakdown — the rounded number is the immediate visual, the breakdown is where the work lives.

### Top fixes section

After the score table, list up to **5 ranked fixes** where each is one of:

- **Closes a category gap** — phrased as "+X pts" recoverable, computed from the formula. Rank by points-per-effort (decision-or-defer beats research-touch beats refactor). Skip fixes worth <0.3 pts unless they're trivially cheap.
- **Backlog progress markers** — e.g., "extract claims from N research files" — only if the backlog is moving (claim count changed since last pass). Frame as info, not score recovery.

Each fix names the file(s) or decision(s) to touch and a one-line "what action."

### Trend line

After the fixes section, one line: `Trend: <prev> → <curr> (Δ +N / −N) over <K> days.` Pull the prior score from the most-recent lint entry in `log.md`. If no prior pass has a score, write `Baseline pass — no prior to compare.`

## Auto-output section template

Everything below this point is what gets written **below the delimiter** on each lint run. Existing content below the delimiter from prior runs is replaced wholesale.

```markdown
<!-- LINT-AUTO-OUTPUT-BELOW -->

# Lint output — <YYYY-MM-DD>

## Vault Health: <N>/100

| Layer | Score | Weight | Status |
|---|---|---|---|
| <emoji> <layer name> | <N>% | <W> | <one-line state> |
| ... | | | |
| **Overall** | | | **<N>/100** |

**Backlog (info, not scored):** <claim count> / <research count> research files (<%>) have claim extraction.

## Top fixes to close the <100−N>-point gap

1. **<action>** (+X pts) — <what to touch> — <one-line why>
2. ...

**Trend:** <prev>/100 → <curr>/100 (Δ <±N>) over <K> days.

## Stale (past TTL)
- [[<file>]] — <domain>, verified <N> days ago (TTL: <M>)

## Orphans (no backlinks)
- [[<file>]] — candidate for archival

## Unresolved links
- `<link text>` (from [[<file>]])

## Possibly duplicated facts
- "<value>" appears in [[<file1>]] and [[<file2>]]; canonical: [[<file3>]]

## Status drift
- [[<file>]] — marked active but not updated in <N> days

## Broken citation chains
- [[<file>]] cites missing id `<id>`

## Overdue decision reviews
- [[<decision>]] — revisit trigger fired <N> days ago (<reason>)

## Missing claims (research with no extracted claims)
- [[<research file>]] — ingested <N> days ago, no claims filed

## Stale clips (inbox triage SLA exceeded)
- [[<clip>]] — clipped <N> days ago, still in `_inbox/`
```

Emoji convention: ✅ if score = 100, ⚠️ if 50–99, ❌ if <50. The emoji is decoration — the score is the signal.

Empty sections: write the heading with `*(none)*` underneath rather than omitting the heading. The template stays predictable across runs.

**Never:**
- Overwrite the entire `GAPS.md` file. Always preserve everything above the delimiter marker.
- Write the auto-output above the delimiter, or interleave it with manual content.
- Auto-archive orphans. Always surface for owner decision.
- Auto-delete stale files. Staleness ≠ wrongness.
- Rewrite `GAPS.md` without logging the lint pass.
- Edit content above the delimiter, even to "fix" formatting or move resolved items. That's the owner's territory.
