# Methodology menu — how heavy should the process be?

Skills and context decide *what the session knows*. Methodology decides *how
the work runs*: straight execution, plan-then-execute, or a full multi-phase
delivery loop. Picking the right weight is a quality lever on par with any
skill — and over-picking is as costly as under-picking, because heavy process
on a small task burns the session on ceremony.

Ask during the interview, recommend based on the profile below, and put the
choice (including "none") in the Equip Plan.

## Selection heuristic

| Task profile | Recommend |
|---|---|
| Small, clear, one sitting | **None** — just equip context + skills and go |
| Medium, clear goal, correctness matters | **planf3** — spec first, then execute |
| Plan exists but feels shaky / high stakes | add **grilling** or **grill-with-docs** before building |
| Large and fuzzy — goal clear, path unknown | **wayfinder** — map it as investigation tickets |
| Idea-stage → shipped thing, wants guided process end-to-end | **superpowers** |
| Multi-week product build, wants full team simulation | **BMAD** — the heaviest; only when the others are clearly too small |

Two rules for recommending, especially to non-technical operators:

1. **Lightest thing that fits.** Escalate weight only when the task profile
   demands it, and say why in the plan.
2. **These compose.** Grilling stress-tests a planf3 spec; wayfinder tickets
   can each run as small planf3 loops. Recommend a primary, mention at most
   one add-on.

## The menu

Stars/popularity move fast — treat this menu as a prior and re-search when
currency matters (same protocol as `ecosystem-search.md`).

### planf3 — spec-first plan/execute
- **Source:** [disler/planf3](https://github.com/disler/planf3) (IndyDevDan)
- **Shape:** interview → concise implementation plan written to a `specs/`
  directory → human review → execute against the spec.
- **Best for:** the workhorse tier — any task big enough that "just start
  typing" loses the thread, small enough that a ticket map is overkill.

### grilling / grill-with-docs — adversarial plan review
- **Source:** [mattpocock/skills](https://github.com/mattpocock/skills)
  (`skills/productivity/grilling`, `skills/engineering/grill-with-docs`)
- **Shape:** the agent relentlessly questions the plan (grill-with-docs:
  grounded in the actual library/API docs) until shared understanding.
- **Best for:** an add-on before committing to any plan, not a standalone
  loop. Cheap insurance on anything high-stakes.

### wayfinder — ticket-mapped exploration
- **Source:** [mattpocock/skills](https://github.com/mattpocock/skills)
  (`skills/engineering/wayfinder`)
- **Shape:** work too big/foggy for one plan becomes a shared map of
  investigation tickets, resolved one at a time until the way is clear.
- **Best for:** large ambiguous efforts — a course, a migration, a product
  area — where the first job is discovering the shape of the work.

### superpowers — brainstorm-to-delivery framework
- **Source:** [obra/superpowers](https://github.com/obra/superpowers)
  (marketplace: obra/superpowers-marketplace)
- **Shape:** a full software-delivery methodology as a skill framework:
  brainstorm → plan → implement (TDD, subagent-driven) → review.
- **Best for:** "I have an idea and want to end with a shipped thing" with
  guided process the whole way. Big install — adopt the methodology, don't
  cherry-pick two skills from it.

### BMAD — full agile team simulation
- **Source:** [bmad-code-org/BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD)
- **Shape:** agent personas (analyst, PM, architect, dev, QA...) producing
  real agile artifacts (PRD, architecture, stories) through heavy
  plan/execute loops.
- **Best for:** multi-week product builds where the artifacts themselves
  matter. The heaviest option — for a first project it is almost always too
  much; prefer planf3 or superpowers.

### Orchestration styles (no install needed)
Sometimes the right answer is not a framework but a *style* the session
already supports: subagent fan-out for parallel research, a plan-approve gate
(plan mode), or a simple write-spec-first discipline borrowed from planf3
without the plugin. Prefer a style over an install when the task only needs
the shape, not the tooling.

## Recording the choice

The Equip Plan carries a **Methodology** line even when the answer is "none":

```markdown
**Methodology:** planf3 (spec-first) + grilling before build — task is
medium-sized with a clear goal; wayfinder rejected as overkill.
```

A methodology install is a plan row like any other (scope: usually global via
plugin — flag it), and rejected candidates get a one-line reason, same as the
ecosystem search.
