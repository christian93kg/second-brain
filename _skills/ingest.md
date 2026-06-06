---
id: skill_ingest
type: system
status: active
created: 2026-05-28
updated: 2026-05-28
tags:
  - system
  - skill
aliases:
  - ingest
---

# ingest

**Goal:** Absorb new research (usually a deep research output from the owner's Claude GUI session) into the vault, fanning updates out across everything it touches.

**Trigger:** Owner says "I have new research on X," pastes a document, or adds a `research`-type file to a domain folder.

**Steps:**

## 0. Inbox triage (only when source is `_inbox/`)

If the trigger is a Web Clipper file in `_inbox/`, decide one of three before doing anything else:

- **Promote** — the clip is substantive research. Rename to `<domain>/research/<topic>_research.md`, set proper `domain`, flip `status: active`, drop the `clip` / `inbox` / `unprocessed` tags, then continue with Step 1 against the new file.
- **Harvest** — the clip contains 1–3 discrete facts but isn't research-tier. Extract claims directly per Step 4 into existing claim files; then move the source to `_archive/_inbox/` with `status: archived`. Skip Step 6 (no overview update for the archived source).
- **Archive** — not useful. Move to `_archive/_inbox/` with `status: archived`. Stop. No log entry needed beyond the move.

Always log the triage outcome:
```
obsidian append file="log.md" content="- <YYYY-MM-DD> inbox triage: <clip name> → <promoted|harvested|archived>"
```

If trigger is not `_inbox/` (owner pasted research, or research file already lives in a domain folder), skip Step 0 entirely.

## 1. Read the incoming material
```
obsidian read file="<incoming research file>"
```
Or read the owner's pasted text directly.

## 2. Identify affected domains
A single research output can touch multiple domains. Example: a city research file touches `locations`, `finance` (cost of living), `body` (medication legality), and `career` (time zone, remote work).

## 3. Find what the vault already has on the topic
```
obsidian search query="<topic>" limit=20
obsidian search:context query="<key phrase>"
obsidian backlinks file="<relevant overview>"
```

## 4. Extract claims
Each discrete fact becomes a claim entry. Claims live in topic-specific files (e.g., `locations/claims/zurich_claims.md`). Use one of:

**Append to an existing claims file:**
```
obsidian append file="<domain>/claims/<topic>_claims.md" content="### <claim_id>
**Assertion:** ...
**Source:** ...
**Confidence:** high
**Verified:** <YYYY-MM-DD>
"
```

**Create a new claims file:**
```
obsidian create name="<topic>_claims" content="---
id: <topic>_claims
type: claim
domain: <domain>
status: active
created: <date>
updated: <date>
---

### <claim_id>
..." silent
```

Claim body format:
```markdown
### <claim_id>
**Assertion:** One sentence stating the fact.
**Source:** <url, publication, or owner's GUI session prompt id>
**Confidence:** high | medium | low
**Verified:** YYYY-MM-DD
**Notes:** (optional — caveats, context)
```

## 5. Mark superseded claims
If a new claim replaces an older one, mark the old claim and set the `superseded_by` pointer:
```
obsidian property:set name="status" value="superseded" file="<old>"
obsidian property:set name="superseded_by" value="<new id>" file="<old>"
```
And on the new one:
```
obsidian property:set name="supersedes" value="<old id>" file="<new>"
```

## 6. Update the domain overview
Reflect new current state. Do **not** paste research into the overview — link to claims instead.
```
obsidian append file="<domain>_overview" content="..."
obsidian property:set name="updated" value="<YYYY-MM-DD>" file="<domain>_overview"
```

## 7. Update `_start_here.md` only if profile-level facts changed
Profile data = things in the top-level facts table: income, target dates, current certifications, active priorities. Most ingests should NOT touch `_start_here.md`.
```
obsidian append file="_start_here.md" content="..."
```

## 8. Update `index.md`
Every new page gets a one-line entry:
```
obsidian append file="index.md" content="- [[<new page>]] — <one-line description>"
```

## 9. Verify bidirectional findability (per `CLAUDE.md` Core Rule 6)

The ingest is not done until the new content is reachable **top-down and bottom-up** through the vault. Run both walks explicitly:

**Top-down walk** — start at `_start_here.md` and try to reach the new file using only links:

1. `_start_here.md` §3 Domains row for the affected domain → `<domain>_overview.md`.
2. From the overview, can you click into the new claim file / research file / analysis? If the overview points only at a stale list, **add the pointer now**.
3. If the new file lives in a subfolder with ≥5 files, that subfolder's `*_index.md` must list it (per `conventions.md` §9). If the index is missing the entry — or the subfolder just crossed the 5-file threshold and has no index yet — **add it now**.
4. `index.md` has the one-line entry (Step 8 above).

If any step fails, fix it before moving on. The walk must succeed cold — starting from `_start_here.md`, no prior knowledge of the filename.

**Bottom-up walk** — start at the new file and confirm the ladder back up:

```
obsidian backlinks file="<new page>"
```

Expected inbound links, at minimum:

- `index.md` — always.
- `<domain>_overview.md` — always (or whichever overview owns the topic).
- The subfolder `*_index.md` — if one exists in the file's parent folder.
- Any decision in `_decisions.md` that cites it (when the ingest extracted decision-relevant claims).
- Each *other* affected domain's overview (cross-domain ingests — see below).

If the backlinks output is missing any of these, the inbound link is missing somewhere upstream. Fix it.

**Cross-domain coverage** — if Step 2 identified more than one affected domain, every affected `<domain>_overview.md` gets a pointer. Don't punt cross-domain facts to a single "home" overview and rely on lateral discovery — `_start_here.md` §4 Cross-Domain Chains is for the dependency map, not for individual file discovery.

**The test:** a future Claude session starting cold from `_start_here.md` can reach this file in ≤3 clicks; a future Claude session looking at this file can reach `_start_here.md` in ≤3 backlinks. If either fails, the filing is incomplete.

## 10. Log the ingest
```
obsidian append file="log.md" content="- <YYYY-MM-DD> ingest: <topic> → <files touched>"
```

## 11. Run lint
The ingest likely introduced new staleness, potentially new contradictions, and may have fired revisit triggers. Run `lint` now.

## 12. Check cross-domain impact
Walk the cross-domain chains in `_start_here.md`. An ingest into `body` may affect `locations` (drug legality), which affects `finance` (cost of living at new candidate cities), which affects `career` (remote work feasibility). If a cross-domain effect is plausible, surface it to the owner as a question — don't fan out into other domains without permission.

**Never:**
- Drop research into the vault unprocessed. Every ingest must fan out to affected overviews.
- Create a new claim without a source.
- Update the domain overview without also updating its `updated:` frontmatter.
- Skip the log entry.
- Declare done before Step 9's two walks pass. "I edited the files" ≠ "the file is findable from zero." See `CLAUDE.md` Core Rule 6.
