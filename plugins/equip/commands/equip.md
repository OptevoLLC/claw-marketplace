---
description: Equip a project folder for a task — interview, inventory, ecosystem search, then build only what's missing (project-level by default)
argument-hint: [task description]
---

Equip a project folder for the following task:

$ARGUMENTS

Use the `equip` skill and follow its four phases in order:

1. **Interview** — FIRST ask and confirm where the project lives: an existing
   folder (confirm the exact path — never assume the current directory) or a
   new folder to scaffold from scratch (agree parent location + name). Then,
   if the task description above is empty, ask what the work is; if present,
   confirm your understanding and fill the remaining interview points
   (frequency, sources, quality bar, audience) with at most a few questions.
   Then ask about PROCESS WEIGHT with a concrete recommendation from the
   skill's `references/methodology-menu.md`: none, spec-first plan/execute
   (planf3), plan grilling, wayfinder for large fuzzy work, superpowers, or
   BMAD — lightest thing that fits. Distill into named required capabilities.
2. **Inventory** — check loaded skills, the target project's `.claude/` tree,
   the global `~/.claude/` level, installed plugins, the CLAUDE.md cascade,
   and connected MCP servers. Mark each capability covered / partial / gap,
   with severity — and label every finding with its scope (project / global /
   plugin / session).
3. **Search** — for each real gap, search the ecosystem for existing
   well-regarded skills per the skill's `references/ecosystem-search.md`.
   Record install / borrow / author verdicts with rationale.
4. **Propose & build** — present the Equip Plan table (with its Methodology
   line and Scope column;
   every row defaults to project scope, and any global row must be flagged
   for explicit confirmation) and STOP for approval. Only after approval:
   scaffold the folder first if starting from scratch (CLAUDE.md + README +
   only the `.claude/` dirs the plan populates), then install, borrow, write
   context docs, and author remaining skills per
   `references/skill-authoring.md`, then verify triggers and report what was
   added, where, and how to use it. Finally, OFFER an explainer (per
   `references/explainer-guide.md`): a quick conversational breakdown, or a
   self-contained HTML page saved to `docs/how-this-project-works.html` in
   the project — recommend the HTML version when others will use the folder.
   Skip gracefully if declined.

Do not create any files or install anything before the plan is approved, and
never write to `~/.claude/` (global level) without the user explicitly
confirming that specific row.
