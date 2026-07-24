# Changelog — equip

## 0.5.1 (2026-07-24)

Best-practices pass (skill-creator audit). Description cut from ~230 to ~120
words. Structure consolidated: scattered invariants merged into one
"Non-negotiables" section (plan gate, project scope, work boundary — each
with its why); duplicate "Re-equipping" section folded into "Two moments";
phases moved up. Command file slimmed to a thin pointer so the skill is the
single source of truth. Unexplained ALL-CAPS softened. No behavior changes
intended.

## 0.5.0 (2026-07-24)

Guardrail from first field test: equip ends where the work begins. Hard
boundary — never start the project task, never write the plan/spec (install
planf3, don't run it), always end with a handoff to the first real move.
Equip formalized as two moments: pre-plan pass (general capabilities,
deliberately approximate) and post-plan re-equip ("equip against the plan" —
the spec/ticket map becomes the interview; derive concrete skills from
planned steps, propose only the delta).

## 0.4.0 (2026-07-24)

Easy/expert mode, established first (inferred from phrasing or one plain
question). Easy: 2–3 questions, agent-picked defaults and methodology,
compact plan summary, one approval. Expert: full interview, menu walked,
per-row rationale and scope discussion. Invariant in both: approval gate,
global-scope flags, full inventory + ecosystem search — easy compresses
conversation, never diligence. Mid-flight switching honored.

## 0.3.0 (2026-07-24)

Optional explainer phase: after the build report, offer a plain-language
breakdown of what was set up — conversational, or a self-contained HTML page
saved to `docs/how-this-project-works.html` in the project (recommended when
teammates/students will use the folder). New `references/explainer-guide.md`
with the six-section outline, offline-HTML rules, and CLAUDE.md pointer;
re-equips offer to regenerate the page.

## 0.2.0 (2026-07-24)

Methodology as a fifth equip dimension: the interview now profiles the task
(size × ambiguity × stakes) and recommends a process weight — none, planf3
(disler), grilling / grill-with-docs and wayfinder (mattpocock/skills),
superpowers (obra), or BMAD — always the lightest that fits. New
`references/methodology-menu.md`; Equip Plan gains a Methodology line;
framework installs go through the same vetting/approval funnel as skills.

## 0.1.0 (2026-07-23) — draft

Initial draft. Four-phase equip loop (interview → inventory → ecosystem
search → propose & build), project-scope-by-default policy, existing-folder
vs from-scratch placement, Equip Plan approval gate.

Lineage: Atlas `self-scaffold`/`/equip` (LowCodeDreamer/MyAgentStuff) +
skill-writing hygiene from revfactory/harness + new ecosystem-search stage.
