# Second Brain — a Life Optimization Vault starter kit

A curated, **LLM-maintained** knowledge base where your research flows into decisions about your
life — built for [Obsidian](https://obsidian.md) + [Claude Code](https://www.claude.com/claude-code).

The atomic unit isn't "a note." It's a **claim flowing into a decision**. A well-run vault can
answer four questions at any moment:

- What have I decided, and why?
- What's still open, and what's blocking it?
- Which facts are fresh, which are stale, which contradict each other?
- What should I research next?

Claude does the bookkeeping — cross-referencing, staleness tracking, contradiction-hunting — that
makes humans abandon wikis. You curate sources and ask good questions.

> The pattern was sketched by Andrej Karpathy (his note ships in `_inbox/karpathy_llm_wiki.md` as
> your first ingest). This kit is one implementation of it.

---

## What you get

- **The operating system** — governance rules (`CLAUDE.md`), workflows (`SKILLS.md` + `_skills/`),
  conventions, and a frontmatter schema.
- **Templates for every file type** — profile, claims, analyses, decisions, research, logs.
- **A worked example domain** (`_example_domain/`, "Home Coffee Setup") showing the shape.
- **A 30-minute onboarding** (`BOOTSTRAP.md`) Claude walks you through.

You bring the content. The system files run the show.

> [!IMPORTANT]
> **Assume this vault could become public.** You're handing it to an AI and (probably) syncing it
> to the cloud — treat everything you write as if it could one day be public. **Only record what
> you'd be fine seeing attached to your name in public.** A vault of public-safe, decision-relevant
> facts is still enormously useful. When in doubt, generalize.

---

## Install (about 10 minutes)

### 1. Get your own copy
Click **Use this template ▸ Create a new repository** at the top of this page, name it (e.g.
`my-brain`), then download or clone it.

- **Clone:** `git clone https://github.com/<you>/my-brain.git ~/my-brain`
- **Or** download the ZIP and unzip it to `~/my-brain`.

> ⚠️ Don't put the folder inside a cloud-synced directory like iCloud Drive, OneDrive, or Dropbox —
> those fight with Obsidian. A plain folder in your home directory (`~/my-brain`) is best.

### 2. Install the apps
| App | How |
|---|---|
| **Obsidian** | [obsidian.md](https://obsidian.md) — free. |
| **Claude Code** | [claude.com/claude-code](https://www.claude.com/claude-code) — sign in with your Anthropic account (Pro is fine). |
| **Obsidian CLI** | [help.obsidian.md/cli](https://help.obsidian.md/cli) — lets Claude *write* to the vault and keeps Obsidian's index in sync. *Optional for week one; reading works without it.* |

### 3. Open the folder as a vault
Obsidian → **Open folder as vault** → pick `~/my-brain`. You should see `BOOTSTRAP.md`,
`CLAUDE.md`, etc. in the file tree.

### 4. Open Claude Code in the vault
| OS | Command |
|---|---|
| **macOS / Linux** | `cd ~/my-brain && claude` |
| **Windows** | open a terminal in the folder, then `claude` |

Claude auto-loads `CLAUDE.md` on startup.

### 5. Add the Obsidian skills (one time)
Inside Claude Code, run:
```
/plugin marketplace add kepano/obsidian-skills
/plugin install obsidian@obsidian-skills
```
These teach Claude to drive Obsidian properly (Markdown, Bases, Canvas, and the CLI). Made by
Steph Ango (Obsidian's CEO).

### 6. Start
Tell Claude:
> **Walk me through BOOTSTRAP.md.**

Claude reads the guide, helps you fill in your profile, and runs your first ingest. Follow its lead.

---

## How it works (the four-layer flowdown)

```
Profile   →   Claims    →   Analyses   →   Decisions
(who)         (facts)       (compare)      (commit)
```

Information flows strictly downward; each layer cites the one above it. That's what makes
contradictions detectable and decisions reviewable. Full rules in `CLAUDE.md`.

---

## Credits & license

- **Pattern:** Andrej Karpathy's LLM-maintained wiki sketch (`_inbox/karpathy_llm_wiki.md`).
- **Obsidian skills:** [kepano/obsidian-skills](https://github.com/kepano/obsidian-skills) by Steph Ango (MIT).
- **License:** MIT — see `LICENSE`.
