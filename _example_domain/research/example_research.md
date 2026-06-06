---
id: example_research
type: research
domain: example
status: draft
created: 2026-05-28
updated: 2026-05-28
source: example — synthesized for this kit, not a real research session
tags:
  - example
  - research
---

# Example Research — Pour-Over Extraction Variables

> [!warning] EXAMPLE — delete with the rest of `_example_domain/`
> Shape demo for a `research` file. A research file is the *input* layer — raw synthesized material brought in from a GUI session, before the `ingest` skill breaks it into atomic claims. Once claims have been extracted, the research file stays as a frozen reference; the claims become the load-bearing layer that overviews and analyses cite.

Parent overview: [[example_overview]].

---

## Source context

This file simulates what a research output looks like when the owner brings it back from a Claude GUI session and drops it into a domain folder. In a real session, this would be 1–10 KB of synthesized content from the owner's deep research run — citations inside, but not yet atomic.

## Key findings

Pour-over extraction is governed primarily by:

1. **Dose-to-water ratio** — typical range 1:15 to 1:17 for a balanced cup. Higher ratios extract less, producing brighter cups; lower ratios produce stronger, more bitter cups.
2. **Grind size** — finer grinds increase extraction surface area and slow flow rate. Most filters target a medium-fine setting.
3. **Water temperature** — 90–96°C window. Hotter water extracts more aggressively; lower temperatures favor sweetness over body.
4. **Total brew time** — extraction is time-bounded; pour-overs typically target 2:30–4:00 from first contact.
5. **Bloom** — initial 30–60g pour with 30–45s rest degasses the grounds, improving subsequent extraction uniformity.

## Implications

For a 18g / 270g recipe at ~3:30, the working point lies inside the published "balanced" envelope for V60-style filters. The grinder click setting is the primary lever available to the owner if extraction needs to shift.

## Ingest checklist

When this file is ingested per `_skills/ingest.md`, the following claims would be extracted:

- `pour_over_ratio_range` — 1:15 to 1:17 typical, dose-to-water
- `pour_over_temp_window` — 90–96°C standard brew temperature
- `pour_over_brew_time` — 2:30–4:00 typical for filter coffee
- `pour_over_bloom_protocol` — 30–60g first pour, 30–45s rest

After extraction, this file stays as `status: archived` (per `conventions.md` §6.1 in-place archive) — frozen as a reference input that future analyses can cite if needed, but no longer driving the overview.

---

*A research file is never the current-state document. The domain overview is. If this file and the overview ever contradict each other, the overview wins per `CLAUDE.md` Core Rule 4.*
