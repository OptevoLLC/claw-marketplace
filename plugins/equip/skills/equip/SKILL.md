---
name: equip
description: Equip a project folder for a task — interview the user, inventory existing skills/context, search the ecosystem for well-regarded skills, and only author what's missing, with an approved plan before anything is created. Recommends a process methodology (planf3, grilling, wayfinder, superpowers, BMAD — or none) and defaults everything to project scope. Use for /equip, "equip this project", "set up this folder for X", "just set me up", "start a new project for X", "what am I missing for this task", "what process/framework should I use", "gap analysis", or any new kind of work in an unequipped folder. Also for the post-plan pass — "equip against the plan", "re-equip", or a spec/plan file pointed at to derive skill needs from. Equips the session only; never does the project work itself.
---

# Equip — reuse before build

Prepare a project folder so a Claude session does its best work on a specific
kind of task. The core discipline: **never author a skill until you have
checked that (a) it doesn't already exist here and (b) nobody in the ecosystem
has already written a better one.** The output of a run is an approved
**Equip Plan**, then the smallest set of changes that closes the gaps:
installed plugins, borrowed skills, context docs, authored skills, agents,
commands — in that order of preference.

## The four phases

```
1. INTERVIEW   what is the work, really — and how should it run?
2. INVENTORY   what do we already have, right here?
3. SEARCH      who has already solved the gaps well?
4. PROPOSE     plan → approval → build only the remainder
```

Never skip 2 or 3 to jump to authoring. A maintained community skill beats a
one-session homebrew, and an installed plugin beats both for upgrades.

## Non-negotiables (any mode, any phase)

1. **The plan gate.** Present the Equip Plan and wait for explicit approval
   before creating or installing anything. Easy mode compresses the plan to a
   summary; it never removes the yes.
2. **Project scope by default.** Components live in the target folder's
   `.claude/` and CLAUDE.md, so equipment travels with the project, can't
   pollute unrelated sessions, and is visible to anyone who opens the folder.
   Global (`~/.claude/` — and note any `claude plugin install` lands
   user-scope, machine-wide) only when the capability is genuinely
   cross-project AND the user confirms that specifically-flagged row. For a
   project-specific capability, borrowing a skill into the project beats a
   machine-wide plugin install.
3. **Equip ends where the work begins.** The deliverable is an equipped
   session — never the work itself, and never the project's plan. Don't run
   the methodology you install (equip *installs* planf3; a fresh session
   *runs* it), don't draft the first work product "while you're here", don't
   write specs. End every run with a handoff: what was set up, and the exact
   next move to begin the work. This boundary exists because field use showed
   sessions drifting from "set up for the newsletter" into "draft the
   newsletter" — and it's what makes equip compose cleanly with whatever
   methodology follows.

## Two modes: easy and expert

Establish first how involved the user wants to be. Infer it from phrasing
when clear ("just set me up" → easy; "let's optimize this properly" →
expert); otherwise ask one plain question:

> "Want me to just set this up with good defaults (**easy**), or shall we go
> through the choices together and tune it (**expert**)?"

| | Easy | Expert |
|---|---|---|
| Interview | 2–3 plain-language questions; sensible defaults fill the rest | full interview, every dimension discussed |
| Methodology | you pick from the menu, one-line why | walk the menu, compare options, user chooses |
| Plan | compact summary, one approval | full table, per-row rationale, rejected alternatives |
| Explainer | offered, simple language | offered, includes the tuning knobs |

The non-negotiables hold in both modes — easy mode compresses conversation,
not diligence. Still run the full inventory and ecosystem search; just don't
narrate them. Honor mid-flight switches: "just decide for me" → drop to easy;
"wait, what were the options?" → open that decision up.

## Equip is two moments, not one

Equipping happens twice around a plan:

1. **Pre-plan equip** (the default) — before any plan exists. Necessarily
   approximate: equip the *general* capabilities the interview surfaces
   (domain context, sources, methodology, foundational skills) and don't
   chase fine-grained skills you're guessing at. Say so in the plan:
   "specific needs will surface once you've planned — re-equip then."
2. **Post-plan re-equip** — after the methodology has produced a plan, spec,
   or ticket map, run equip *against that document*: derive concrete
   capabilities from the actual planned steps (a real API to integrate, a
   file format to parse, a validation the spec demands). This is where the
   skills nobody could anticipate pre-planning get added.

The post-plan form has its own entry point — the `/equip-plan` command — and
also triggers when the user says "equip against the plan", points at a
spec/plan file, or re-runs equip in a folder whose methodology has produced
planning artifacts (a `specs/` dir, a wayfinder map) since the last run.

Any repeat run — post-plan or otherwise — **diffs instead of rebuilding**:
re-run the inventory, show what changed, propose only the delta, and never
overwrite an existing skill without showing the diff and getting approval.

## Phase 1 — Interview

Mode first — it sets the depth of everything below. If the task came as an
argument, confirm your understanding instead of re-asking. Then cover
(easy: 2–3 questions, expert: up to 5–6):

- **Where the project lives** — ask and confirm before touching anything;
  don't assume the current directory:
  - **Existing folder** — which one? Confirm the exact path back to the user.
  - **From scratch** — agree the parent location and folder name; Phase 4
    scaffolds folder + `.claude/` tree + context docs before equipping.
- **The work itself** — what gets produced or changed? What does "done" look
  like?
- **Frequency** — one-off, or recurring? (Recurring work justifies commands
  and skills; one-offs usually just need context.)
- **Inputs and sources** — what files, services, or accounts does the work
  touch? (Reveals MCP/auth needs.)
- **Quality bar** — is there a review/validation step worth automating?
- **Who runs it** — just this user, or teammates/students who need the
  folder to be self-explanatory?
- **Process weight** — how should the work itself run? Read
  `references/methodology-menu.md`, profile the task (size × ambiguity ×
  stakes), and ask with a concrete recommendation: none, spec-first
  plan/execute (planf3), adversarial plan review (grilling /
  grill-with-docs), ticket-mapped exploration (wayfinder),
  brainstorm-to-delivery (superpowers), or full team simulation (BMAD).
  Lightest thing that fits; a chosen framework becomes an install row in the
  plan like any other component.

Distill the answers into 3–7 named **required capabilities** (e.g. "draft X
threads in brand voice", "publish to Ghost", "validate frontmatter"). These
drive everything downstream.

**Post-plan pass:** when a plan/spec/ticket map exists, skip most of this —
the document IS the interview. Derive capabilities from the concrete planned
steps, then proceed to inventory as normal.

## Phase 2 — Inventory

Take stock of what already covers each capability, nearest to farthest:

1. **This session's loaded skills** — the available-skills list you can see.
2. **Project level** — `.claude/skills|agents|commands/` and
   `.claude/settings.json` hooks in the target folder and its parents. (A
   from-scratch project has none — still inventory what the parent tree and
   global level supply.)
3. **Global (user) level** — `~/.claude/skills/`, `~/.claude/agents/`,
   user-level CLAUDE.md.
4. **Installed plugins** — `claude plugin list` (or
   `~/.claude/plugins/installed_plugins.json`). User-scoped: present
   everywhere on this machine.
5. **Context cascade** — CLAUDE.md files from the target folder to root.
6. **Connected MCP servers** — tools already reachable for the named sources.

Mark each capability **covered** (name the component AND its scope — project
/ global / plugin / session), **partial**, or **gap**. Scope matters here: a
capability covered only globally won't travel with the folder — when a
teammate or student must get it by opening the project, treat global-only
coverage as *partial* and consider a project-level copy or pointer.

Classify each gap:

| Severity | Meaning |
|----------|---------|
| Critical | The task cannot be done well without it |
| Enhancing | Would significantly improve quality or speed |
| Nice-to-have | Minor convenience — default to skipping |

## Phase 3 — Search the ecosystem

For every Critical and Enhancing gap, look for an existing, well-regarded
solution before considering authoring. Full protocol and vetting checklist:
`references/ecosystem-search.md`. In brief:

1. Official plugin marketplace (`anthropics/claude-plugins-official`) and
   marketplaces already added on this machine.
2. GitHub skill collections — capability + "claude skill" / "SKILL.md";
   anthropics/skills and the known community repos.
3. Vet before adopting: alive, readable, right-sized, safe, licensed.

For each gap record a verdict: **install** (plugin), **borrow** (copy into
`.claude/skills/` with attribution), or **author** (nothing suitable exists —
and say why the found options were rejected).

The chosen methodology goes through the same funnel: the menu in
`references/methodology-menu.md` is a vetted starting point, but confirm the
source is still alive, and re-search if the need matches nothing on it.

## Phase 4 — Propose, then build

Present the **Equip Plan** and wait for approval:

```markdown
## Equip Plan: {task}

**Target:** {confirmed path} ({existing folder | new — will be scaffolded})
**Methodology:** {none | planf3 | ...} — {one-line why; what was rejected as
too heavy/light}

### Already covered
| Capability | Covered by | Scope |
|---|---|---|

### To add
| # | Capability | Action | Component | Scope | Source / rationale |
|---|---|---|---|---|---|
| 1 | publish to Ghost | install | ghost plugin | global (plugin) | official marketplace, maintained |
| 2 | brand voice rules | context doc | CLAUDE.md section | project | project-specific, nothing to reuse |
| 3 | thread drafting | author | skills/x-threads/ | project | searched; found nothing above bar |

### Skipped (nice-to-have)
- ...
```

Flag any `global` row when presenting ("row 1 installs machine-wide — OK?").
In easy mode, present the same plan as a short summary — "I'll install X
(works everywhere on your machine — OK?), add a Y skill to this folder, note
your brand rules in CLAUDE.md" — one approval, then build. The full table
still exists; show it if asked.

After approval, build in dependency order:

- **Scaffold first, if from scratch** — the agreed folder with minimum viable
  agentics and context: `CLAUDE.md` (purpose, constraints, pointers — facts
  only; the *how* goes in skills), `README.md` (for humans), and only the
  `.claude/` and `docs/` directories a plan row actually populates. No
  speculative directories.
- **Install** — `claude plugin marketplace add <repo>` +
  `claude plugin install <name>@<marketplace>`.
- **Borrow** — copy into the project's `.claude/skills/<name>/`, add a
  `source:` frontmatter line crediting the origin, trim what doesn't apply.
- **Context docs** — project CLAUDE.md additions or a `docs/` reference file.
- **Author** — per `references/skill-authoring.md`: pushy description, lean
  body, progressive disclosure, then trigger-test.
- **Agents / commands / hooks** — only when the decision tree says so.

### Component decision tree

Prefer the lightest component that does the job:

- Knowledge or repeatable method the session needs → **skill**
- Fact or constraint every session in this folder needs → **CLAUDE.md line**
- Repeated user entry point → **command** (thin: point at the skill)
- Isolated/parallel autonomous work → **agent**
- Deterministic validation of outputs → **hook**
- One-off task → none of the above; just do the work with context

### Verify and hand off

- Each authored skill: one realistic should-trigger prompt and one near-miss
  should-NOT-trigger prompt (see `references/skill-authoring.md`).
- Each install: confirm it appears in `claude plugin list`.
- Report: what was added, where, one line of "how to use it" each — and the
  handoff: the exact next move to begin the actual work (e.g. "fresh session,
  run /planf3 — then come back and re-equip against the spec").

### Explain (offered, never forced)

After the report, offer an explainer of what was just set up:

- **Quick breakdown** — one conversational message: each component in plain
  language, the gap it closes, how a session runs now, three copy-pasteable
  starter prompts.
- **HTML explainer** — the same content as a self-contained page saved to
  `docs/how-this-project-works.html`, so it outlives this conversation.
  Recommend it when the interview said others will use the folder.

Outline, format rules, and the CLAUDE.md pointer line:
`references/explainer-guide.md`. On a re-equip that changes the setup, offer
to regenerate the page.

## Lineage

Descends from the Atlas `self-scaffold`/`/equip` system
(LowCodeDreamer/MyAgentStuff); skill-writing hygiene borrowed from
revfactory/harness. For building whole *agent teams* rather than equipping a
folder, harness is the deeper tool — recommend it instead of reimplementing
it.
