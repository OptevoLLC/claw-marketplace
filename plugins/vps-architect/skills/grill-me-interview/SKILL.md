---
name: grill-me-interview
description: >
  Run a short, friendly "grill me" interview to learn who the operator is and what they
  actually want from their box — then store it so everything you do afterward is tuned to
  them. Use at first run / right after the box is set up, or whenever the user says "get
  to know me", "interview me", "grill me", "personalize this", "set up my profile", or
  asks for recommendations you don't yet have the context to make well. The point is
  owned personalization: the profile lives on THEIR box and travels with them, instead of
  being trapped in a vendor.
---

# The "Grill Me" Interview

Adapted from Matt Pocock's "grill me" pattern (published as `grill-me` and `grilling`
in [mattpocock/skills](https://github.com/mattpocock/skills)): instead of guessing what
the user wants, *interview* them — one question at a time, digging where it's
interesting — then write down what you learned so you can act on their specific frame
from here on.

## The rules of a good grilling

1. **One question at a time.** Never fire a checklist. Ask, listen, then ask the next thing
   based on what they said. A wall of questions makes a non-technical person freeze.
2. **Dig when it's interesting.** If they say "I run a small consulting practice," follow up:
   "What eats the most time you wish it didn't?" The gold is in the follow-up.
3. **Stay warm and short.** ~6–8 questions total. This is a friendly conversation, not an
   onboarding form. Reassure: "no wrong answers, and we can change any of this later."
4. **Reflect back.** After a few answers, say what you're hearing ("so it sounds like
   invoicing and chasing payments is the real pain") and let them correct you. This is
   *direct-and-verify* applied to *them* — they steer.

## What to find out (adapt the wording to them)

- **Who they are / what they do** — their business or work, solo or team.
- **What they want to *own*** — the one or two tools/bills they'd most like off their plate.
- **Their biggest time-sink or frustration** — where an agent could actually help.
- **Their comfort level** — how technical they feel (so you calibrate how much you explain).
- **What "a win this course" looks like** to them — one concrete outcome.
- **Anything off-limits / sensitive** — data they don't want touched, hard constraints.

## Store it so it actually gets used

Write the result to **two places on the box**, then confirm with the user:
1. A human-readable profile at `~/profile.md` (or `/srv/_docs/operator-profile.md`) — the
   durable record they own and can edit.
2. The **agent's working context** — append a concise summary to the box's `CLAUDE.md`
   (or the equivalent the on-box agent reads each session) so future sessions start already
   knowing them. Keep it tight: who they are, what they want to own, their comfort level,
   their definition of a win.

Then say: "I've saved what I learned to your box — it's yours, it travels with you, and I'll
tune what I do to it from here. You can edit it anytime in your file manager."

## Use it from here on

- When recommending apps (see the `self-hosted-infrastructure-landscape` skill), filter to
  *their* stated pains and comfort level — lead with their best first pick.
- When explaining things, match the depth to their stated comfort.
- Re-grill lightly when their situation changes (new project, new goal). Personalization
  compounds — that's the whole point of owning it.
