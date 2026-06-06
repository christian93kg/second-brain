---
id: skill_report
type: system
status: active
created: 2026-05-28
updated: 2026-05-28
tags:
  - system
  - skill
aliases:
  - report
---

# report

**Goal:** Refresh owner-facing summary artifacts after ingest or on a weekly cadence.

**Steps:**

## 1. Refresh `_start_here.md` Quick Reference
For each row in the profile's quick-reference table, confirm the underlying claim is current. Use transclusion where possible so the table updates automatically:
```markdown
| Current income | ![[claims_finance#current_income]] | [[finance_overview]] |
```
If transclusion isn't available, update the row manually and set the `updated:` property on `_start_here.md`:
```
obsidian property:set name="updated" value="<YYYY-MM-DD>" file="_start_here.md"
```

## 2. Refresh `index.md`
Ensure every file in the vault has a one-line entry. Find missing ones by comparing the vault file list with `index.md` contents. Append missing entries:
```
obsidian append file="index.md" content="- [[<file>]] — <one-line>"
```

## 3. Log the report
```
obsidian append file="log.md" content="- <YYYY-MM-DD> report: refreshed quick-ref, index"
```

**Never:**
- Rewrite `_start_here.md` from scratch. Update specific rows only.
- Regenerate `index.md` by deletion — always append missing entries and let lint surface any that shouldn't be there.
