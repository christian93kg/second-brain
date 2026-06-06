---
id: example_claims
type: claim
domain: example
status: draft
created: 2026-05-28
updated: 2026-05-28
tags:
  - example
  - claim
---

# Example Claims — Home Coffee Setup

> [!warning] EXAMPLE — delete with the rest of `_example_domain/`
> Shape demo for the atomic-claim format. Each `###` block is one claim with frontmatter-shaped metadata in its body. Cited by [[example_analysis]] and [[example_overview]].

Parent overview: [[example_overview]].

---

### current_recipe
**Assertion:** Current home brew is 18g dose / 30g first pour / 270g total / ~3:30 brew time on a Hario V60 with #2 paper filters.
**Source:** owner statement, 2026-05-28
**Confidence:** high
**Verified:** 2026-05-28
**Notes:** Verified weekly by owner. Grind setting tracked in [[example_claims#grinder_setting]].

### grinder_setting
**Assertion:** Comandante C40 set to 22 clicks for the current V60 recipe; one click finer if beans are >2 weeks past roast date.
**Source:** Comandante "brewing guide" PDF + owner experimentation
**Confidence:** medium
**Verified:** 2026-05-28
**Notes:** The 22-click baseline is the published recommendation for V60; the adjust-for-age rule is owner heuristic and might not generalize.

### offer_terms
**Assertion:** Sample claim referenced from `_decisions.md` example row — placeholder for shape only.
**Source:** placeholder
**Confidence:** low
**Verified:** 2026-05-28
**Notes:** This claim exists so the example `[EX1]` decision in `_decisions.md` has a working citation target. Delete with the rest of the example domain.

---

*Each claim is atomic: one assertion, one source. If you want to combine multiple claims into a conclusion, that goes in an `analysis` file (see [[example_analysis]]), not here.*
