---
description: Re-equip a project against its finished plan/spec — derive the specific skills the planned steps need, propose only the delta
argument-hint: [path to plan/spec, if not obvious]
---

Use the `equip` skill in its **post-plan re-equip** mode for this project.

Plan/spec to equip against: $ARGUMENTS

If no path was given, look for the folder's planning artifacts (a `specs/`
directory, a wayfinder map, a PLAN/spec markdown the methodology produced) —
and if none exists, say so and suggest running `/equip` (pre-plan) or the
chosen methodology first, rather than inventing a plan.

The document is the interview — skip the standard questions. Read the plan,
derive concrete capabilities from the actual planned steps (real APIs to
integrate, file formats to parse, validations the spec demands), inventory
what the folder already has, search the ecosystem for the gaps, then present
the delta-only Equip Plan and stop for approval before creating or
installing anything.

The skill's non-negotiables apply unchanged: project scope by default,
flagged confirmation for anything global, never overwrite an existing skill
without showing the diff — and equip the session, then stop. Do not start
executing the plan; end with a handoff to its first step.
