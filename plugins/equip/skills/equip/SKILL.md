---
name: equip
description: Equip a project folder for a task. Interviews the user about the work and confirms where the project lives (existing folder, or a new folder scaffolded from scratch), recommends a process weight/methodology for the work (none, spec-first like planf3, plan grilling, wayfinder for large fuzzy efforts, superpowers, BMAD), inventories skills/agents/commands/context already available at project and global level, searches marketplaces and GitHub for well-regarded existing skills to fill gaps, and only authors what is genuinely missing — defaulting everything to project level. Use whenever the user runs /equip, says "equip this project", "set up this folder for X", "start a new project for X", "what am I missing for this task", "what process/framework should I use for this", "scaffold my session", "gap analysis", or is starting a new kind of work in a folder that has no supporting infrastructure yet. Also use for follow-ups: "re-equip", "update the equipment", "we added a new task type".
---

# Equip — reuse before build

Prepare a project folder so a Claude session does its best work on a specific
kind of task. The core discipline: **never author a skill until you have
checked that (a) it doesn't already exist here and (b) nobody in the ecosystem
has already written a better one.**

The output of an equip run is an approved **Equip Plan** and then the smallest
set of changes that closes the gaps: installed plugins, borrowed skills,
authored skills, context docs, agents, commands — in that order of preference.

## Scope: project level by default

Equipment belongs to the project. Every component you create — skills, agents,
commands, hooks, context docs — goes in the **target folder's** `.claude/` and
CLAUDE.md, not in `~/.claude/`, so it travels with the folder, can't pollute
unrelated sessions, and is visible to anyone who opens the project.

Go **global** (`~/.claude/skills/`, `~/.claude/agents/`, user-level CLAUDE.md)
only when BOTH hold: the capability is genuinely cross-project (voice, personal
conventions, machine-wide infrastructure), AND the user explicitly confirms
after you flag it. Never silently write to user level.

Two machine realities to state honestly in the plan:

- `claude plugin install` lands at **user scope** on this machine — the plugin
  becomes available everywhere, not just here. That's fine for genuinely
  general tools; for a project-specific capability, prefer **borrowing** the
  skill into the project's `.claude/skills/` instead.
- The inventory must label what it finds by scope (session / project / global /
  plugin), because "already covered globally" and "already covered in this
  folder" have different implications for portability.

The Equip Plan table carries a **Scope** column, and any `global` row must be
called out for confirmation before building.

## The four phases

```
1. INTERVIEW   what is the work, really?
2. INVENTORY   what do we already have, right here?
3. SEARCH      who has already solved the gaps well?
4. PROPOSE     plan → approval → build only the remainder
```

Never skip 2 or 3 to jump to authoring. Writing a skill from scratch is the
*last* resort, not the default — a maintained community skill beats a
one-session homebrew, and an installed plugin beats both for upgrades.

### Phase 1 — Interview

If the task was given as an argument, confirm your understanding instead of
re-asking. Otherwise interview briefly — 3 to 5 questions, not a questionnaire.
You need:

- **Where the project lives** — ALWAYS ask and confirm this first, never
  assume the current directory:
  - **Existing folder** — which one? Confirm the exact path back to the user
    before touching it.
  - **From scratch** — a new project folder to create. Agree the parent
    location and folder name; the build phase will scaffold folder + agentics
    (`.claude/` tree) + context docs before equipping (see Phase 4).
- **The work itself** — what gets produced or changed? What does "done" look like?
- **Frequency** — one-off, or will this session type recur? (Recurring work
  justifies commands and skills; one-offs usually just need context.)
- **Inputs and sources** — what files, services, or accounts does the work
  touch? (This reveals MCP/auth needs.)
- **Quality bar** — is there a review/validation step worth automating?
- **Who runs it** — just this user, or teammates/students who will need the
  folder to be self-explanatory?
- **Process weight** — how should the work itself run? Read
  `references/methodology-menu.md`, profile the task (size × ambiguity ×
  stakes), and ASK the user with a concrete recommendation: none (just go),
  spec-first plan/execute (planf3), adversarial plan review (grilling /
  grill-with-docs), ticket-mapped exploration for large fuzzy work
  (wayfinder), brainstorm-to-delivery (superpowers), or full team simulation
  (BMAD). Default to the lightest thing that fits; a chosen framework becomes
  an install row in the plan like any other component.

Distill the answers into 3–7 named **required capabilities** (e.g. "draft X
threads in brand voice", "publish to Ghost", "validate frontmatter"). These
drive everything downstream.

### Phase 2 — Inventory

Take stock of what already covers each capability, from nearest to farthest:

1. **This session's loaded skills** — the available-skills list you can see.
2. **Project level** — `.claude/skills/`, `.claude/agents/`,
   `.claude/commands/`, `.claude/settings.json` hooks in the target folder and
   its parents. (A from-scratch project has none — the inventory is still
   worth running for what the parent tree and global level already supply.)
3. **Global (user) level** — `~/.claude/skills/`, `~/.claude/agents/`,
   user-level CLAUDE.md.
4. **Installed plugins** — `claude plugin list` (or read
   `~/.claude/plugins/installed_plugins.json`). These are user-scoped: present
   everywhere on this machine.
5. **Context cascade** — CLAUDE.md files from the target folder to root: what
   constraints and pointers already exist? What does the folder's location
   already supply?
6. **Connected MCP servers** — tools already reachable for the sources named
   in the interview.

Mark each required capability: **covered** (name the component AND its scope —
project / global / plugin / session), **partial** (what's missing), or
**gap**. Scope matters: a capability covered only at global level won't travel
with the folder — for anything a teammate or student must get by opening the
project, treat global-only coverage as *partial* and consider a project-level
copy or pointer.

Classify each gap:

| Severity | Meaning |
|----------|---------|
| Critical | The task cannot be done well without it |
| Enhancing | Would significantly improve quality or speed |
| Nice-to-have | Minor convenience — default to skipping in v1 |

### Phase 3 — Search the ecosystem

For every Critical and Enhancing gap, look for an existing, well-regarded
solution **before considering authoring**. Full search protocol and vetting
criteria: `references/ecosystem-search.md`. In brief:

1. Official plugin marketplace (`anthropics/claude-plugins-official`) and any
   marketplaces already added on this machine.
2. GitHub skill collections — search for the capability + "claude skill" /
   "SKILL.md"; check anthropics/skills and the well-known community repos.
3. Vet before adopting: recent commits, stars/forks as a signal (not proof),
   readable SKILL.md under ~500 lines, no secrets/exfil patterns, license.

For each gap record a verdict: **install** (plugin), **borrow** (copy a skill
into `.claude/skills/` with attribution), or **author** (nothing suitable
exists). Authoring must state *why* the found options were rejected.

The chosen methodology (if any) goes through the same funnel: the menu in
`references/methodology-menu.md` is a vetted starting point, but confirm the
source is still alive before installing, and re-search if the user's need
doesn't match anything on the menu.

### Phase 4 — Propose, then build

Present the **Equip Plan** as a markdown table and wait for explicit approval
before creating or installing anything:

```markdown
## Equip Plan: {task}

**Target:** {confirmed path} ({existing folder | new — will be scaffolded})
**Methodology:** {none | planf3 | wayfinder | ...} — {one-line why, and what
was rejected as too heavy/light}

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

Every row defaults to `project` scope. Flag any `global` row explicitly when
presenting the plan ("row 1 installs machine-wide — OK?") and get it confirmed
along with the plan itself.

After approval, build in dependency order:

- **Scaffold first, if from scratch** — before any equipping, create the
  agreed folder with the minimum viable agentics and context:
  - `CLAUDE.md` — purpose of the folder, constraints from the interview,
    pointers (facts only; the *how* goes in skills)
  - `README.md` — what this project is and how to work in it, for humans
  - `.claude/skills/` / `.claude/agents/` / `.claude/commands/` — created
    only when the plan actually puts something in them, never empty
  - `docs/` or `context/` — only if the plan includes context docs
  No speculative directories: every path created must be referenced by a plan
  row.

- **Install** — `claude plugin marketplace add <repo>` +
  `claude plugin install <name>@<marketplace>`. Remember: user scope,
  machine-wide — only for capabilities that deserve it.
- **Borrow** — copy into the *project's* `.claude/skills/<name>/`, add a
  `source:` line to the frontmatter crediting the origin repo, trim what
  doesn't apply.
- **Context docs** — additions to the project CLAUDE.md (constraints, facts,
  pointers only) or a `docs/` reference file the skills can point to.
- **Author** — follow `references/skill-authoring.md`: pushy description,
  lean body, progressive disclosure, then trigger-test it.
- **Agents / commands / hooks** — only when the decision tree below says so.

### Component decision tree

Prefer the lightest component that does the job:

- Knowledge or repeatable method the session needs → **skill**
- Fact or constraint every session in this folder needs → **CLAUDE.md line**
- Repeated user entry point → **command** (thin: point at the skill)
- Isolated/parallel autonomous work → **agent**
- Deterministic validation of outputs → **hook**
- One-off task → none of the above; just do the work with context

### Verify

- Each authored skill: run one realistic should-trigger prompt and one
  near-miss should-NOT-trigger prompt (see `references/skill-authoring.md`).
- Each install: confirm the skill/command appears (`claude plugin list`).
- Report: what was added, where, and the one-line "how to use it" for each.

## Re-equipping

On a repeat run in an already-equipped folder, diff instead of rebuilding:
re-run the inventory, show what changed since last time, and propose only the
delta. Never overwrite an existing skill without showing the diff and getting
approval.

## Lineage

This skill descends from the Atlas `self-scaffold`/`/equip` system
(LowCodeDreamer/MyAgentStuff) and borrows skill-writing hygiene from
revfactory/harness. For building whole *agent teams* rather than equipping a
folder, harness is the deeper tool — recommend it instead of reimplementing it.
