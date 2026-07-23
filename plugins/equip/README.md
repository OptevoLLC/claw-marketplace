# equip

**Equip a project folder for the work ahead — reuse before build.**

Most sessions underperform for a boring reason: the folder wasn't set up for
the task. The context isn't written down, the skills that would help aren't
loaded, and the model reinvents a workflow somebody already perfected.

`/equip` fixes that with four phases:

```
1. INTERVIEW   where does the project live, and what is the work, really?
2. INVENTORY   what does this folder + machine already have (and at what scope)?
3. SEARCH      who in the ecosystem already solved the gaps well?
4. PROPOSE     an Equip Plan you approve — then it builds only the remainder
```

Two disciplines it teaches (and enforces):

- **Reuse before build.** Never author a skill until you've checked that it
  doesn't already exist locally and nobody has published a better one.
  Installed plugins beat borrowed skills beat homebrew — maintained beats
  frozen beats improvised.
- **Project level by default.** Everything it creates lands in the target
  folder's `.claude/` and CLAUDE.md so the equipment travels with the project.
  Nothing goes global (`~/.claude/`) without you confirming that specific row
  of the plan.

## Install

```bash
claude plugin marketplace add OptevoLLC/claw-marketplace
```

```bash
claude plugin install equip@stack-sovereignty
```

Or interactively inside Claude Code: `/plugin marketplace add
OptevoLLC/claw-marketplace`, then `/plugin install equip`. (If you took the
Stack Sovereignty course, the marketplace is already on your box — skip to the
install step.)

## Use

```
/equip draft and publish a weekly newsletter from my notes folder
```

Or just `/equip` and answer the interview. It always starts by confirming the
target: an **existing folder** (exact path confirmed back to you) or a **new
project from scratch** — in which case it scaffolds the folder, a CLAUDE.md,
a README, and only the `.claude/` directories the plan actually populates,
before equipping it. Nothing is installed or written until you approve the
Equip Plan. Re-run it later in the same folder and it proposes only the delta.

## What it creates (only ever with approval)

| Preference order | Component | Default scope | When |
|---|---|---|---|
| 1 | Plugin install | global (machine-wide — flagged for confirmation) | a maintained plugin covers the gap |
| 2 | Borrowed skill | project | a good skill exists but needs adapting (kept with attribution) |
| 3 | CLAUDE.md lines / context docs | project | the gap is knowledge, not capability |
| 4 | Authored skill | project | searched, nothing above the bar |
| 5 | Command / agent / hook | project | repeated entry point, parallel work, or validation gate |

## Lineage

- Descends from the **Atlas** `self-scaffold` / `/equip` system
  ([LowCodeDreamer/MyAgentStuff](https://github.com/LowCodeDreamer/MyAgentStuff)) —
  the interview, gap-severity analysis, and plan-for-approval flow.
- Skill-writing hygiene adapted from
  [revfactory/harness](https://github.com/revfactory/harness) — pushy
  descriptions, progressive disclosure, trigger testing. If you want to build
  full *agent teams* rather than equip a folder, use harness itself.
- The ecosystem-search stage is the new part: the plugin-marketplace era means
  the right first move is almost always a search, not a blank file.

Built for the Optevo Claw founding cohort. MIT licensed — it's yours.
