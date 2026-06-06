---
id: skill_confirm-sweep
type: system
status: active
created: 2026-05-28
updated: 2026-05-28
tags:
  - system
  - skill
aliases:
  - confirm-sweep
---

# confirm-sweep

**Goal:** Catch assumption errors before they propagate across multiple files. A mandatory pre-check, not a standalone workflow.

**When:** Before running `ingest`, a domain port, a cross-domain walk, or any sweep touching 3+ files. Also before executing a plan that will produce such a sweep.

**Steps:**

1. State what you believe each key figure in scope means. Pull the values from `_start_here.md` or the relevant overview; do not restate from memory.
2. Name the file types in the folders you're about to touch and what each one is for (e.g., `research/` holds deep-research synthesis material brought from a GUI session — not content for you to generate).
3. Name the tools you'll use for each write. For vault files, this is always an Obsidian CLI command — never `Edit`, `Write`, or any direct filesystem mechanism.
4. Flag anything ambiguous and **wait for owner confirmation** before proceeding.
5. If confirmation modifies scope, restart at step 1.

**Rationale:** The most expensive vault errors come from silent propagation — one wrong assumption about what a figure means, or what a folder is for, cascading across many files in a single sweep. An explicit mental-model restatement is cheap insurance against that class of error.

**Output:** A restatement written directly to the owner. No vault side effects until confirmation.
