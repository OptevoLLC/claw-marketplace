# Authoring a skill (the last resort, done well)

Condensed hygiene rules, distilled from Anthropic's skill-creator guidance and
revfactory/harness's skill-writing guide. Read this before writing any new
SKILL.md in the "author" step of an equip run.

## Structure

```
skill-name/
├── SKILL.md            # required: frontmatter + body
├── references/         # docs loaded only when needed
├── scripts/            # runnable helpers (executed, not loaded)
└── assets/             # templates, images used in outputs
```

## The description is the trigger — write it pushy

The `description:` frontmatter is the ONLY thing the model sees before
deciding to load the skill. Claude triggers conservatively, so be aggressive:

- Say what the skill does AND list concrete trigger situations, including
  casual phrasings ("set up my folder", not just "scaffold workspace").
- Include follow-up phrasings ("re-run", "update", "fix the X part").
- Distinguish from near neighbors: if a similar skill exists, say when NOT to
  use this one.

Bad: `Processes PDF documents.`
Good: `Read, split, merge, OCR and watermark PDF files. Use whenever the user
mentions a .pdf file or asks for a PDF deliverable.`

## Body principles

| Principle | Why |
|---|---|
| Explain *why*, not just ALWAYS/NEVER | A model that knows the reason handles edge cases right |
| Stay under ~500 lines | Context is a public good; move detail to `references/` with "read X when Y" pointers |
| Generalize | Rules overfitted to one example fail on the next input |
| Imperative voice | "Run the inventory" beats "the inventory could be run" |
| Bundle repeated code | If every run rewrites the same helper, ship it in `scripts/` |

## Progressive disclosure

Three loading tiers — budget accordingly:

1. **Metadata** (name + description): always in context. ~100 words.
2. **SKILL.md body**: loaded on trigger. <500 lines.
3. **references/**: loaded only when the body says to. Unbounded. Files over
   ~300 lines get a table of contents at the top.

Per-variant material (per-cloud, per-framework, per-brand) goes in separate
reference files so only the relevant one loads.

## Trigger testing

Before calling an authored skill done:

1. Write 3+ **should-trigger** prompts — formal, casual, and implicit
   phrasings a real user would type.
2. Write 3+ **should-NOT-trigger** near-misses — prompts with overlapping
   keywords where a *different* tool is correct. ("Extract this xlsx chart as
   PNG" must not trigger a spreadsheet-editing skill.) Obviously-unrelated
   prompts are worthless as tests.
3. Check for collisions with skills already in the folder's cascade.

If a should-trigger fails, push the description harder; if a near-miss fires,
add the distinguishing "do not use for X" clause.

## Iteration

Ship lean, then evolve from feedback. When the same correction comes up twice,
fold it into the skill — generalized, not as a bolted-on special case. Track
notable changes in the project CLAUDE.md changelog if the folder keeps one.
