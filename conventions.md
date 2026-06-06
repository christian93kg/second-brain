---
id: conventions
type: system
status: active
created: 2026-05-28
updated: 2026-05-28
tags:
  - system
  - conventions
---

# Vault Conventions

Operating conventions that extend `CLAUDE.md`. If `CLAUDE.md` is the contract, this file is the playbook. Claude reads this file alongside `CLAUDE.md` at session start when any claim-filing, porting, or structural decision is in play.

---

## 1. Claim Files — Format and Location

**Location:** `<domain>/claims/<topic>_claims.md`. One claim file per topic. Topics are granular — a single research session's worth of related facts. A claim file may hold 1–20 individual claims.

**Not used:**
- **Per-domain mega-files** (`career/claims.md`) — grow unwieldy, drown signal in size.
- **One-claim-one-file** (`claim_oscp_cost.md`) — fragmentation kills navigability.

**Per-topic is the readable middle.** One topic = one file = one research session's worth of facts.

**File structure:**

```markdown
---
id: <topic>_claims
type: claim
domain: <career|finance|body|...>
status: active
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [claim, <topic tags>]
---

# <Topic> Claims

One-line topic description. Who consumes these facts.

---

### <claim_id>
**Assertion:** One sentence stating the fact.
**Source:** <see §3 Provenance>
**Confidence:** high | medium | low
**Verified:** YYYY-MM-DD
**Notes:** (optional — caveats, context, exceptions)
```

**Claim ID format:** `<topic>_<specifier>`. Examples: `oscp_exam_cost`, `mha_remote_rate_current`. Short, snake_case, unique across the whole vault. IDs are cited by analyses and decisions via `cites: [<id>]`.

---

## 2. Canonical-Home Rule for Cross-Domain Facts

Every fact has exactly one canonical home. Other files that need it link or transclude.

**The rule:** Facts live where they are **generated**, not where they are *consumed*. Tiebreak by asking: "If this fact changed, which domain's owner would notice first?"

**Rationale:** Generation-home is stable (changes once, at the source). Consumption-home is unstable (changes every time another domain needs the fact). Generation-home also matches how research flows in — a research session is about a *topic*, not a *consumer*.

**When a fact seems to belong equally to two domains,** pick the one with the sharpest definition of ownership and add a transclusion or wikilink from the other.

---

## 3. Provenance Convention

Every claim names a source. Sources fall into five tiers:

1. **External authoritative** — published regulation, official vendor/government page, peer-reviewed study. Highest confidence. Cite URL + retrieval date.
2. **External secondary** — news article, industry blog, forum post. Medium confidence. Cite URL + retrieval date.
3. **Direct communication** — advisor email, vendor statement, HR reply. High confidence but narrow scope. Format: `<role>, <organization>, YYYY-MM-DD`.
4. **Owner statement** — something the owner reported from memory, internal knowledge, or personal circumstance. Used when no external source is practical (e.g., personal context or preferences the owner is recording from their own knowledge — only at a level they're comfortable being public). Format: `owner statement, YYYY-MM-DD`. Medium confidence by default; promote to high if the owner explicitly confirms a number is current.
5. **Research file** — a deep research document synthesized in a GUI session. Cite by file id. The research file itself must cite its sources inside.

**Never leave source blank.** If a claim cannot be sourced, mark `confidence: low`, `source: unverified`, and surface it in `GAPS.md`.

**Conversation-sourced claims** (facts the owner states during a Claude Code session, not from a deep research session) are valid — they just use tier 4 (`owner statement, YYYY-MM-DD`). This is how in-session facts get provenance without pretending to be research.

---

## 4. Canonical Home for Profile Data

The owner's profile (identity, philosophy, the context that shapes their decisions, hard constraints) lives in **`_start_here.md` §1**. It is **self-sourced** — the owner is the authoritative source for their own identity. Profile data is NOT duplicated in claim files. Domain overviews transclude or link when they need it.

**Exception:** Detail-heavy profile data, if the owner chooses to record it, may get **both** a summary in `_start_here.md` §1 AND a full claim file in the relevant domain's `claims/` folder. The summary is high-level; the claim file has the audit trail. (Keep everything public-safe — see the privacy posture in `BOOTSTRAP.md` / `_start_here.md` §1.)

---

## 5. File Naming Convention

- **System files:** `CLAUDE.md`, `SKILLS.md`, `conventions.md`, `_start_here.md`, `_decisions.md`, `quick_reference.md`, `index.md`, `log.md`, `GAPS.md`. Underscore prefix on `_decisions` / `_start_here` is a visual hint that these are dashboards. Keep the convention.
- **Domain overviews:** `<domain>_overview.md` at the domain folder root. Example: `career/career_overview.md`.
- **Claim files:** `<domain>/claims/<topic>_claims.md`.
- **Research files:** `<domain>/research/<topic>_research.md` once ported. On ingest from a GUI session, rename to this pattern.
- **Decision entries:** grouped by domain inside `_decisions.md`, one row per decision. Use a single-letter prefix per domain so decision IDs sort and group naturally (e.g., `C` career, `F` finance, `B` body, `T` travel). Pick prefixes when you set up your first few domains and add this table to a notes block in `_decisions.md` so the convention is visible.
- **Sub-prefixes** (e.g. `T-G#`) are useful when one decision series grows past ~10 entries and you want to split a sub-category off so the parent series stays uncluttered. Register the sub-prefix in `_decisions.md` notes when you introduce it, and explain in one sentence what falls into the sub-series versus the parent.

---

## 6. Archive Policy

Files that are superseded, obsolete, or replaced move to `_archive/<origin_folder>/`. They are not deleted — kept for diff/recovery. Two things mark an archived file:

1. Physically moved into `_archive/<origin>/`.
2. Its entry in `index.md` moved to the `Archived` section with a note explaining why.

`status: archived` frontmatter is optional — the folder placement IS the status. A file in `_archive/` is archived by definition.

### 6.1. In-Place Archival (Frozen Inputs)

Some files stay in their domain folder but stop driving decisions. The topic was retired, dormant, declined, or "moot under current plan" — yet the file is kept as research-tier reference because (a) the underlying research is still factually valid, (b) the topic might revive under different circumstances, or (c) cross-domain consumers still benefit from being able to read it.

**Convention:** flip the **frontmatter** to `status: archived` while leaving the **file in place**. The domain overview's prose owns the narrative ("dormant — kept as reference," "moot under current plan," "owner not using; research file remains a frozen input"). Frontmatter and prose stay synchronized via this flip.

This is the **second archival mode**, distinct from §6:

| Mode | File location | Frontmatter | `index.md` placement |
|---|---|---|---|
| §6 — Hard archive | Moved to `_archive/<origin>/` | Optional `status: archived` | Listed under `## Archived` |
| §6.1 — In-place archive (frozen input) | Stays in domain folder | **Required `status: archived`** | Stays in domain section, with a clarifying suffix like "(frozen input)" or "(dormant — kept as reference)" |

Use §6 when the file's content is genuinely obsolete and only kept for diff/recovery. Use §6.1 when the *topic* is frozen but the *file* is still consulted.

**Schema enum is strict.** `status` MUST be one of `active | draft | complete | superseded | archived` per `CLAUDE.md`. Off-schema values like `dormant`, `partial`, `closed` should be cleaned up: `dormant` → `archived` (§6.1) or `active` (if revived); `partial` → `draft`; `closed` → `archived` or `complete`. **Decisions** in `_decisions.md` use a richer vocabulary (`[DECIDED] | [OPEN] | [REVISED] | [DORMANT] | [SUPERSEDED]`) — that's a **decision-row** label, not a file frontmatter value. Don't mix them.

---

## 7. Frontmatter Schema Audit

`CLAUDE.md` defines the canonical schema. Imported files (e.g., from another vault, from research synthesized elsewhere) often arrive with fields that aren't valid here. Every imported file is in schema-violation state until conversion runs.

**Rule:** Schema conversion happens as part of the walk that touches each file. Don't do it as a separate pass — you'd be reading each file twice. When walking a file for claim extraction or content review, also run the property operations below.

**Schema audit checklist** to run on every imported file during its walk:

1. `id` field exists and matches filename (snake_case, no extension).
2. `type` is one of: `overview | claim | analysis | decision | research | log | system | reference | profile | plan`.
3. `domain` is set (except `type: system`).
4. `status` is one of: `active | draft | complete | superseded | archived` (strict — see §6.1 for `dormant`/`partial`/`closed` cleanup).
5. `created` and `updated` are both YYYY-MM-DD.
6. `tags` is a list.
7. For claims: `confidence`, `source`, `verified_on` are populated.
8. For analyses and decisions: `cites:` lists exist and resolve.
9. For decisions: `depends_on`, `revisit_on` or `revisit_if` are populated.

Use the CLI for the mechanical fixes:
```
obsidian property:remove name="<bad field>" file="<name>"
obsidian property:set name="type" value="overview" file="<name>"
obsidian property:set name="status" value="active" file="<name>"
obsidian property:set name="id" value="<name>" file="<name>"
obsidian property:set name="updated" value="<YYYY-MM-DD>" file="<name>"
```

**Completed schema fixes are logged in `log.md`** — not in a separate tracking file.

---

## 8. `cp -r` vs CLI for migration work

`CLAUDE.md` mandates using the Obsidian CLI for all vault operations. During **bulk migration from another vault** specifically, `cp -r` is permitted for staging large numbers of files because:

- The CLI has no bulk import command.
- Obsidian auto-reindexes on file changes within ~1s.
- The CLI rule exists to preserve semantic state (wikilinks, backlinks), not to police filesystem operations.

**After migration**, all edits go through the CLI (or — pragmatically — through `Edit` for surgical text changes when the CLI's inline-content-only constraint makes full-file overwrite impractical). New file creation during normal operation goes through `obsidian create`.

This is a narrow exception, logged in `log.md` each time it's exercised.

---

## 9. Per-Subfolder Index Files

Any subfolder inside a domain that holds **5+ research or reference files** should have a `<topic>_index.md` at the subfolder root. The index is the subfolder's entry point — Claude reads it before drilling into any child file.

**Shape:**
- `type: overview` (the index is a navigation/overview layer, not a claim, not research).
- `id: <foldername>_index` (or `<foldername>_research_index` if the folder is purely research-frozen).
- Body: one-paragraph **Purpose** (what the folder is and is NOT for); a **Files** table (filename → one-sentence topic → status); a one-line **Flowdown** (which overview this feeds); a short **Notes** section for load-bearing caveats (e.g., "frozen research, do not edit").
- Target length: 40–80 lines. An index is not a survey.

**Authoritative facts never live in an index.** The index points at the ceiling (`<domain>_overview.md` → `_start_here.md` §1). If a fact changes, the fact moves; the index only mentions it in passing.

**Exemption — small folders (<5 files).** An index adds no value; link directly from the domain overview.

**Exemption — plan-dominant folders.** If a subfolder is almost entirely plan files marked for deletion/rebuild, skip the index — the domain overview handles navigation until the rebuild lands.

Before reading multiple files in a subfolder, check for `*_index.md` at the subfolder root and read it first.

---

## 10. `_start_here.md` Scope

`_start_here.md` is the profile + plan + navigation dashboard. It owns:
- §1 Owner (identity, philosophy, decision-shaping context, hard constraints)
- §2 Plan (primary pathway, alternatives, confidence levels)
- §3 Domains (pointers only — one line per domain pointing to `<domain>_overview.md`)
- §4 Cross-Domain Chains
- §5 Current Priorities
- §6 Navigation

It does NOT own:
- Per-domain current state (that lives in domain overviews)
- Single-value dashboard (that lives in `quick_reference.md`)
- Decisions (that live in `_decisions.md`)

**If `_start_here.md` grows past ~250 lines, something has leaked into it that belongs elsewhere.** Move it out.

---

## 11. The `_inbox/` staging folder

Web Clipper (and your first ingest material) writes here. Files use `domain: inbox` (the **only** place this pseudo-domain is permitted) and `status: draft`. The frontmatter shape is otherwise standard: `type: research`, `id: clip_<YYYYMMDD>_<slug>`, with `source: <url>` carrying provenance.

**Triage discipline:** every clip is processed within 14 days via the `ingest` skill. Three outcomes:

1. **Promote** — rename and move to `<domain>/research/<topic>_research.md`, flip `status: active`, set proper `domain`. Then run normal ingest from Step 1.
2. **Harvest** — extract claims directly into existing claim files; hard-archive the source clip to `_archive/_inbox/`.
3. **Archive** — not useful. Move to `_archive/_inbox/`, set `status: archived`. No `index.md` entry needed for archived clips.

Lint flags any `_inbox/` file with `created` >14 days ago under `## Stale clips` in `GAPS.md`. The folder should normally hold 0–5 items; chronic accumulation means triage is being skipped.

**`domain: inbox` is not a real domain.** It does not appear in the staleness TTL table, does not get an overview file, and does not get cited by analyses. It exists solely as a holding pen for unclassified inbound material.

---

*This file is the playbook for scale. Update when conventions change.*
