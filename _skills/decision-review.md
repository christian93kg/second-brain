---
id: skill_decision-review
type: system
status: active
created: 2026-05-28
updated: 2026-05-28
tags:
  - system
  - skill
aliases:
  - decision-review
---

# decision-review

**Goal:** Walk the decision log, check every revisit trigger, surface decisions that need re-examination.

**Trigger:** Weekly, or immediately when an `ingest` supersedes a claim cited by any decision.

**Steps:**

1. Read the decision log:
   ```
   obsidian read file="_decisions.md"
   ```
2. For each decision with `status: open` or `status: decided`:
   - Check `revisit_on` date. If past today, flag with reason "scheduled revisit due."
   - Check each claim id in `depends_on:`. For each, read the claim and check if `status: superseded`:
     ```
     obsidian property:get name="status" file="<claim id>"
     ```
     If any dependency is superseded, flag the decision with reason "dependency changed."
   - Check `revisit_if:` — if it names a claim, and that claim has been updated since the decision's `updated:` date, flag.
3. Append flagged decisions to `GAPS.md` under "Overdue decision reviews" (merge with lint output if lint just ran — don't duplicate).
4. For each flagged decision, draft a short "needs review" summary for the owner: which decision, why it's flagged, what changed.
5. Log the run:
   ```
   obsidian append file="log.md" content="- <YYYY-MM-DD> decision-review: <N flagged>"
   ```

**Never:**
- Automatically change a decision's status. Always surface for owner action.
- Skip a decision because its reasoning is old. The whole point is to check old reasoning against new facts.
