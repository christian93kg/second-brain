---
id: bootstrap
type: system
status: active
created: 2026-05-28
updated: 2026-06-11
tags:
  - system
  - bootstrap
  - onboarding
---

# BOOTSTRAP — Read This First

Welcome. This folder is a **starter kit** for a Life Optimization Vault: a curated, LLM-maintained knowledge base where research flows into decisions about your life. The pattern was sketched by Andrej Karpathy ([[karpathy_llm_wiki]] in `_inbox/`) and this kit is one implementation of it.

The kit gives you the operating system: governance rules (`CLAUDE.md`), workflows (`SKILLS.md`, `_skills/`), conventions (`conventions.md`), inherited lessons (`_lessons.md`), and templates for every file type you'll create. **Your content goes in. The system files run the show.**

Spend ~30 minutes following this file end-to-end before you do anything else. Then delete this file — your vault doesn't need it.

> [!warning] Assume this vault could become public
> You're handing this vault to an AI and (probably) syncing it to the cloud. Treat everything you write as if it could one day be public — anything given to an internet-connected tool can leak, be indexed, or be retained after you delete it. **Only record what you'd be fine seeing attached to your name in public.** That's not a limitation: a vault of public-safe, decision-relevant facts is still enormously useful. Where your comfort line falls is your call. When in doubt, generalize — "manages a chronic condition" beats a diagnosis and dosages.

---

## 1. What's in this folder

```
CLAUDE.md           — the contract. Auto-loaded by Claude every session.
SKILLS.md           — workflow index (query, ingest, lint, etc.)
conventions.md      — claim format, naming, archive policy
_start_here.md      — YOUR profile, plan, priorities. Fill this in first.
_decisions.md       — decision log, grows as you commit to things
_lessons.md         — inherited starter lessons (distilled digest) + a slot for your own
_lessons_archive.md — full text of digested lessons; grows as lint condenses old entries
index.md            — catalog of every page; updated by Claude on ingest
log.md              — append-only ops log
GAPS.md             — what's stale / orphaned / overdue; refreshed by `lint`
quick_reference.md  — single-value dashboard

_skills/            — full procedures for each workflow
_inbox/             — staging for new material; Karpathy wiki lives here as your first ingest
_example_domain/    — shape demo; delete once you have real domains
```

---

## 2. Prerequisites

Install (Obsidian, Claude Code, the Obsidian CLI, and the kepano `obsidian-skills` pack) is covered in **`README.md`** — do those steps first. By the time you're reading this *inside Claude*, you should already have:

- **Obsidian** open, with this folder opened as a vault (system files sit at the vault root so Claude finds them);
- **Claude Code** running in the vault root — it auto-loads `CLAUDE.md`;
- the **Obsidian CLI** installed (`obsidian help` works) — *optional for week one; only vault writes need it, reading works without it*;
- the **kepano skills** installed: `/plugin marketplace add kepano/obsidian-skills` then `/plugin install obsidian@obsidian-skills`.

Two quick checks in Claude:
- *"What files did you load at startup?"* → should list `CLAUDE.md`, `SKILLS.md`, `_start_here.md`, `GAPS.md`, `_lessons.md`.
- *"What skills do you have loaded?"* → should include `obsidian-cli`, `obsidian-markdown`, `obsidian-bases`, `json-canvas`, `defuddle`.

---

## 3. First 30 minutes

### Step 1 — Fill in `_start_here.md` §1 and §6 only
Open `_start_here.md`. **Read the "Assume this vault could become public" callout at the top first** — only record what you'd be comfortable being public. Then fill in:
- **§1 Owner** — identity, philosophy, hard constraints, and (optionally) the context that shapes your decisions. 5–15 lines total, at whatever level of detail you're comfortable with. The point is that §1 exists with real content so Claude has *something* to anchor on. A placeholder or a deleted subsection is fine.
- **§6 Current Priorities** — what you're working on this week + critical-path items.

Skip §2 (Plan), §4 (Cross-Domain Chains), and the rest. Those fill themselves in as your vault grows.

When §1 and §6 have real content, flip the frontmatter `status: draft` → `status: active`.

### Step 2 — Delete the `[EXAMPLE]` rows
Open these files and delete the rows marked `<!-- [EXAMPLE — delete me] -->`:
- `_decisions.md` — the `EX1` row
- `log.md` — the example block
- `quick_reference.md` — the example row

### Step 3 — Read `_lessons.md` once
Skim the inherited lessons. Don't memorize. Just absorb the patterns — they're gotchas earlier users ran into so you won't have to.

### Step 4 — Read `CLAUDE.md` once
Same — skim, don't memorize. You'll re-read it the first time something surprises you.

---

## 4. First real session — ingest the Karpathy wiki

The `_inbox/karpathy_llm_wiki.md` file is your first ingest target. It describes the wiki pattern this vault implements. Ingesting it teaches you the loop on real content (not synthetic examples) and gives you a foundational reference page in your vault.

Open Claude Code in the vault root and say:

> "Ingest `_inbox/karpathy_llm_wiki.md` per the ingest workflow in `SKILLS.md`. It's a reference-tier document, not domain-specific — file the promoted version under a `reference/` folder you create at the vault root, then break out 4–6 atomic claims about the pattern into a claims file. Update `index.md` and `log.md` per the workflow. Verify findability before declaring done."

Read what Claude does. Look at the new files. Open `log.md` and confirm the ingest line landed. Open `index.md` and confirm the new entries appear. **That's the loop** — every future ingest looks like this.

If the obsidian CLI isn't installed yet, Claude will tell you which steps it can't complete. That's fine for the first run — you'll re-do those steps with the CLI once it's installed.

---

## 5. Pick your domains

Look at `_start_here.md` §1 — the priorities and constraints you wrote. What buckets of life do they fall into? Career? Finance? Where you live? Health? Relationships? Travel? Each becomes a **domain** with its own folder, overview file, and growing collection of claims/analyses/research.

**Start with 2 or 3 domains, not 9.** You can always add more. Hard to subtract them once they're populated.

For each new domain:
1. Create `<domain>/` folder at the vault root.
2. Create `<domain>/<domain>_overview.md` using the shape of `_example_domain/example_overview.md` as a model.
3. Add a row to `_start_here.md` §3 Domains pointing at the new overview.
4. Add an entry under `## Your Domains` in `index.md`.

Delete `_example_domain/` and its `index.md` entries once your first real domain is set up — it's only there to model the shape.

---

## 6. Run lint after a week

Once you've done 3–5 ingests and built a couple of domains, run lint:

> "Run the lint skill per `_skills/lint.md`."

Claude will refresh the `GAPS.md` auto-output section: stale files, orphans, broken citations, overdue decision reviews. **A healthy vault has a short, stable `GAPS.md` and a long, boring `log.md`.** If `GAPS.md` keeps growing between lint passes, you're drifting faster than you're maintaining.

---

## 7. When you're stuck, look here

| Want | Read |
|---|---|
| What Claude must do | `CLAUDE.md` |
| What workflow to invoke | `SKILLS.md` |
| Detail on a workflow | `_skills/<skill>.md` |
| How to name a file / where it lives | `conventions.md` |
| What's broken right now | `GAPS.md` |
| Gotchas to avoid | `_lessons.md` |
| What changed recently | `log.md` |

---

## 8. What this kit does NOT include

- **Your profile.** Identity, constraints, priorities. You fill those in.
- **Your domains.** Career, finance, locations, etc. — pick your own based on what you're optimizing for.
- **Your decisions.** Empty log; grows as you commit.
- **Pre-made dashboards.** `quick_reference.md` is a template; populate it with your figures.
- **Automation.** Lint is a *procedure* described in `_skills/lint.md`, not an automated script. You ask Claude to run it. If you want it automated later, you can build a wrapper script — start with the manual procedure for at least a month so you understand what it's doing.

---

## 9. The mindset

You're not "using" Claude to take notes. You're running a **persistent compounding knowledge base** that Claude maintains for you. The vault gets richer with every source you add and every question you ask. Your job is to curate sources, direct the analysis, ask good questions. Claude's job is everything else.

The cost of maintenance — the bookkeeping, the cross-referencing, the staleness tracking — is what makes humans abandon wikis. Claude doesn't get bored. The wiki stays maintained because the maintenance cost is near zero.

The hard part is sourcing well and asking the right questions. Spend your attention there.

---

*Once you've finished steps 1–4, delete this file. Your vault has its own onboarding now — it's `_start_here.md`.*
