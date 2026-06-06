---
id: web_clipper_template
type: system
status: active
created: 2026-05-28
updated: 2026-05-28
tags: [system, web-clipper, template, reference]
aliases: [Web Clipper Template]
---

# Web Clipper — Note Content Template

This is the body template for Obsidian Web Clipper, configured to drop clips into `_inbox/`. See `conventions.md` §11 for the inbox triage workflow and `_skills/ingest.md` Step 0 for the promote / harvest / archive decision.

**To update Web Clipper:** open the extension → template settings → Note Content field → paste from the line starting `# {{title}}` through the line `{{content}}` inclusive. Do NOT include this header text or the frontmatter above.

---

# {{title}}

> {{description}}

**Link:** [{{title}}]({{url}})
**Site:** {{domain}}
**Author:** {{author}}
**Published:** {{published}}
**Clipped:** {{date|date:"YYYY-MM-DD HH:mm"}}

---

## Triage
<!-- fill in at ingest, before deciding promote / harvest / archive -->

- **Why kept:** 
- **Domain:** 
- **Verdict:** <!-- promote | harvest | archive -->
- **Notes:** 

---

{{content}}
