---
id: example_overview
type: overview
domain: example
status: draft
created: 2026-05-28
updated: 2026-05-28
tags:
  - example
  - moc
aliases:
  - Example Overview
---

# Example Domain Overview — Home Coffee Setup

> [!warning] EXAMPLE — delete this folder when you create your real domains
> This folder is a shape demo. The topic — "home coffee setup" — was chosen because it's mundane and obviously *not* a real life-optimization domain. The point is to show how an overview, claim, analysis, and research file fit together, so you can model your own domains on the same shape. **Delete `_example_domain/` (and its entry from `index.md`) once you have real domains.**

---

## Current State

A short prose paragraph describing what's true *right now* in this domain. Cite the underlying claim, don't restate it.

The setup is a small batch pour-over rig: 18g dose, 30g first pour, 270g total, ~3:30 brew time. See [[example_claims#current_recipe]] for the exact parameters and source.

## Architecture

How the domain is organized — the conceptual map, not the file map. A Mermaid diagram or short prose. Example:

```
Beans → Grind → Bloom → Pour → Cup
              ↘ (dose, water temp govern extraction)
```

## Key Decisions

Pointer to decisions logged in `_decisions.md` that affect this domain. Don't duplicate the decisions here; one row each with rationale link.

| Decision | Status | Rationale |
|---|---|---|
| <!-- e.g., Stay on V60 vs. switch to Aeropress --> | <!-- [DECIDED] --> | <!-- See [[example_analysis]] --> |

## Cross-Domain Links

Other domains this one touches. Each link goes to *that* domain's overview, not directly into its claim files.

- (none — example domain is intentionally isolated)

## Open Questions

What's not yet decided in this domain. Each item should map to a research prompt or an open row in `_decisions.md`.

- Whether a finer grind would improve extraction at current dose
- Whether bloom volume should track ambient humidity

## Files in This Folder

- [[example_claims]] — atomic claims about the current recipe and the grinder
- [[example_analysis]] — comparison of V60 vs. Aeropress for this owner
- [[example_research]] — the original research file ingested before claims were extracted

---

*This file is the current-state document for the example domain. If something here contradicts `_start_here.md` §1 (profile) or §2 (plan), the profile wins — flag the contradiction. For per-domain state, this file is authoritative.*
