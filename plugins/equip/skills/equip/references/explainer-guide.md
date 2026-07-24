# Explainer guide — making the equipment understandable

Equipment the user doesn't understand is equipment they won't use — or worse,
will work around. After the build report, always OFFER an explainer; never
force one. Three response options:

1. **Quick breakdown** — conversational, right in the chat.
2. **HTML explainer** — a page saved into the project, for keeps.
3. **No thanks** — respect it, move on.

If the interview said teammates/students will use the folder, recommend the
HTML version: it travels with the project and works for people who weren't in
this conversation.

## What every explainer covers (both formats)

Write for the audience from the interview — a non-technical operator gets no
unexplained jargon; an engineer gets less hand-holding. Cover, in order:

1. **What you asked for** — one sentence restating the task.
2. **What was set up and why** — each component in plain language:
   *"a skill = instructions Claude loads automatically when the topic comes
   up; we added one for X because Y."* Name the gap each one closes.
3. **The folder map** — what lives where and what's safe to edit.
4. **How a session runs now** — the methodology in action: what to type,
   what Claude will do, where you approve. If planf3 was chosen, show the
   spec-review-execute rhythm; if none, say "just describe the work."
5. **Your first three prompts** — concrete, copy-pasteable starters for this
   exact project.
6. **What's global vs project** — what came along machine-wide (plugins) vs
   what lives only in this folder, and why that matters.

## Conversational breakdown

Keep it to one message. Use the six points above as the skeleton, but prose —
not a document dumped into chat. End by asking if any part deserves a deeper
dive.

## HTML explainer

- **Location:** `docs/how-this-project-works.html` in the target project
  (create `docs/` if the plan didn't already).
- **Self-contained:** single file, inline CSS, no external fonts/scripts/CDNs
  — it must open from disk, offline, forever.
- **Structure:** the six sections above, plus a header with the project name,
  equip date, and methodology badge. A simple folder-tree block beats a
  diagram. Copy-pasteable prompts go in styled `<code>` blocks.
- **Tone:** written to the *user*, not about the system ("When you want a new
  newsletter issue, open this folder and say…").
- **Keep it current:** on a re-equip that changes the setup, offer to
  regenerate the page; note the date so staleness is visible.

Also drop one pointer line in the project CLAUDE.md so future sessions know
it exists:

```markdown
- `docs/how-this-project-works.html` — human-facing explainer of this
  folder's equipment (regenerate on re-equip).
```
