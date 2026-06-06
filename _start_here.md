---
id: start_here
type: profile
domain: system
status: draft
created: 2026-05-28
updated: 2026-05-28
tags:
  - system
  - profile
  - dashboard
aliases:
  - Start Here
  - Dashboard
  - Home
  - Master Profile
---

# Life Optimization Vault — Master Profile

> [!abstract] One person. One system. Every decision connected.
> This file owns the profile, the current plan, cross-domain chains, current priorities, and navigation. **Per-domain current state lives in each `<domain>_overview.md`** — this file points to them, it does not duplicate them. Single-value dashboards live in `quick_reference.md`. If a domain overview contradicts this file's §1 profile or §2 plan, flag it — `_start_here.md` is authoritative for profile and plan. For per-domain current state, the domain overview is authoritative.

> [!important] For Claude
> Read `CLAUDE.md` → `SKILLS.md` → `conventions.md` → this file → `GAPS.md` → `_lessons.md` at the start of every session. When answering questions, start here for profile/plan, then follow the §3 pointer to the relevant `<domain>_overview.md`. Never answer without reading this file first.

> [!warning] DRAFT — fill me in
> This is the starter template. Status stays `draft` until you've filled in §1 (Owner) and §6 (Current Priorities). After that, set `status: active` in the frontmatter. See `BOOTSTRAP.md` for the recommended first-30-minutes pass.

> [!warning] Assume this vault could become public
> You're handing this vault to an AI and (probably) syncing it to the cloud. Treat everything you write as if it could one day be public — anything given to an internet-connected tool can leak, be indexed, or be retained after you delete it. **Only record what you'd be fine seeing attached to your name in public.** That's not a limitation: a vault of public-safe, decision-relevant facts is still enormously useful. Where your comfort line falls is your call. When in doubt, generalize — "manages a chronic condition" beats a diagnosis and dosages.

---

## 1. The Owner

### Identity
<!-- Who are you, at the bare minimum a future-you-or-Claude needs to know? Initials, rough age, region, household, profession in one line — at a detail level you're comfortable making public (initials and ranges are fine). Keep it tight — this is the prelude to the rest of the file, not a memoir. -->

- 

### Philosophy
<!-- Two or three sentences on what drives the vault. What outcome are you optimizing for? What are you NOT optimizing for? This is the load-bearing answer — every decision later cites it implicitly. -->

- 

### Context that shapes your decisions (optional, public-safe)
<!-- Anything that drives your choices and that you're comfortable recording — work situation, life stage, location constraints, or things you're managing around (health, family, finances) stated at a high level. Summary-level only; skip anything you wouldn't want public. If a topic deserves real depth later, it goes in that domain's `claims/` folder, not here. Delete this subsection if nothing applies. -->

- 

### Hard constraints (non-negotiables)
<!-- Things that cannot change without rewriting the whole plan. Examples: "must stay in region X for now," "a fixed time commitment through date Y," "a minimum-income requirement," "no relocation while a family member is in care." If something here changes, that's a profile-rewrite event. -->

- 

### Preferences (soft)
<!-- Things you'd prefer but would trade off against the hard constraints. Climate, language preferences, urban vs. rural, etc. Used to break ties in analyses. -->

- 

---

## 2. The Plan

### Primary pathway
<!-- The single most-likely path forward over the next 12–36 months. One paragraph. Cite the analysis or decision that committed to it (you'll create these as your vault grows). -->

- 

### Alternatives (secondary, tertiary)
<!-- One paragraph each for the next two most-likely paths. These are not abandoned — they're held in reserve and revisited if the primary path's preconditions break. -->

- 

### Confidence levels
<!-- One line per pathway: how sure are you, what would change your mind? -->

- Primary: 
- Secondary: 
- Tertiary: 

### Not locked in (open questions)
<!-- What's still being researched / decided. Each item should map to an entry in `_decisions.md` with `status: open` or to a research item the next GUI session will tackle. -->

- 

---

## 3. Domains — Pointers

> One line per domain pointing to its `<domain>_overview.md`. Don't put per-domain state here — that's the overview's job.

| Domain | Overview file | Status |
|---|---|---|
| <!-- example: career --> | <!-- [[career_overview]] --> | <!-- active --> |
| | | |
| | | |

**Note:** delete the `_example_domain/` folder once you've picked your real domains and created overview files for them. The example exists to show the shape, not to suggest you keep it.

---

## 4. Cross-Domain Chains

> Many real decisions span domains. Career affects finance affects location affects body affects travel. List the chains you keep finding yourself walking. Once a chain stabilizes, file the analysis that walks it under one of the domains and link from here.

- 
- 

---

## 5. Quick Reference

> Single-value dashboard lives in `quick_reference.md`. Don't duplicate values here. Point at it:

- See [[quick_reference]] for current single-value facts (income, target dates, key metrics).

---

## 6. Current Priorities

### Active right now
<!-- 1–5 things you're actively working on this week. Update freely — this is the most-changing section. -->

- 

### Next 6 months
<!-- 3–7 things you expect to land before the end of the half. -->

- 

### Critical path items
<!-- The 1–3 things that, if blocked, block everything else. -->

- 

---

## 7. Navigation

> If you want X, go to Y.

| Want | Go to |
|---|---|
| Profile / hard constraints | §1 above |
| Current plan | §2 above |
| State of a specific domain | §3 → that domain's overview |
| What's decided / why | `_decisions.md` |
| Current gaps / overdue reviews | `GAPS.md` |
| Workflows Claude runs | `SKILLS.md` |
| Conventions for filing | `conventions.md` |
| What changed recently | `log.md` |

---

*Keep this file under 250 lines. If it grows past that, something has leaked into it that belongs in a domain overview, the decisions log, or a research file. Move it out — see `conventions.md` §10.*
