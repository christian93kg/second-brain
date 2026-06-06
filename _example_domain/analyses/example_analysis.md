---
id: example_analysis
type: analysis
domain: example
status: draft
created: 2026-05-28
updated: 2026-05-28
cites: [current_recipe, grinder_setting]
tags:
  - example
  - analysis
---

# Example Analysis — V60 vs. Aeropress for Current Setup

> [!warning] EXAMPLE — delete with the rest of `_example_domain/`
> Shape demo for an `analysis` file. An analysis *combines cited claims* into a structured comparison. It never states new facts of its own — every factual cell points back to a claim.

Parent overview: [[example_overview]].

---

## Question

Given the current home setup ([[example_claims#current_recipe]], [[example_claims#grinder_setting]]), is the Aeropress a meaningfully different daily-driver than the V60, or are the two equivalent under the owner's preferences?

## Comparison

| Axis | V60 (current) | Aeropress | Winner |
|---|---|---|---|
| Brew time | ~3:30 ([[example_claims#current_recipe]]) | ~1:30 typical | Aeropress |
| Cup volume | 240g out of 270g in | ~200g (single-cup) | V60 |
| Cleanup | Filter + grounds → compost | Filter + puck → trash/compost | Tie |
| Required grind | 22 clicks ([[example_claims#grinder_setting]]) | 18 clicks (finer) | Tie (grinder handles both) |
| Bean economy | Standard | Slight favor — uses 1–2g less per cup | Aeropress |

## Conclusion

For the owner's current single-mug morning routine, the V60 and Aeropress are roughly equivalent on output quality. The Aeropress wins on brew time (≈2 minutes saved) and bean economy; the V60 wins on cup volume (about 40g more delivered).

A switch is only worth doing if morning time pressure (or bean budget) becomes a binding constraint. Currently neither does.

## Implications for decisions

- This analysis is the cited rationale for any `[DECIDED]` "stay on V60" row in `_decisions.md`.
- Revisit trigger: if [[example_claims#current_recipe]] changes (e.g., owner moves to a larger morning dose), re-run the cup-volume comparison.

---

*An analysis is the file type that links claims to decisions. If you change a cited claim, this analysis may go stale — `decision-review` will surface it.*
