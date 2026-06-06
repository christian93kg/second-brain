---
id: decisions
type: decision
status: active
created: 2026-05-28
updated: 2026-05-28
tags:
  - system
  - decisions
---

# Decisions

Decision log with revisit triggers. One row per decision. Grouped by domain.

**Status values:** `[DECIDED]` | `[OPEN]` | `[REVISED]` | `[SUPERSEDED]` | `[DORMANT]`

**ID prefixes** — pick a single-letter prefix per domain so IDs sort and group naturally. Register them here as you set them up:

| Prefix | Domain |
|---|---|
| (example) C | career |
| (example) F | finance |
| (example) B | body |
| | |

If a decision series grows past ~10 entries, consider a sub-prefix (e.g., `T-G#` under `T#`) to split a sub-category off. Document the sub-prefix below the table when you introduce it.

---

## Decisions

> One table per domain, or one big table sorted by ID — your call. Sample format below; delete the example row before adding your first real one.

| # | Decision | Status | Date | Why | Depends On | Revisit If |
|---|----------|--------|------|-----|------------|------------|
| EX1 | <!-- [EXAMPLE — delete me] Accept remote-first offer at Company X --> | [DECIDED] | 2026-05-28 | Salary + flexibility align with §1 hard constraints | [[example_claims#offer_terms]] | Company changes return-to-office policy |

---

*Decisions cite claims. Claims cite sources. If a `depends_on:` claim gets superseded, this decision needs a `decision-review` pass — see `_skills/decision-review.md`.*
