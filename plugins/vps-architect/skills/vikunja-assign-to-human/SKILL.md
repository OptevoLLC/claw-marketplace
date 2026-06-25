---
name: vikunja-assign-to-human
description: >
  Hand a clear, do-able task back to the human when you (the agent) are blocked and
  need them to do something you can't — generate a key on their own laptop, click a
  confirmation link, make an account, decide between options, paste something only they
  have. Use whenever you're about to stall waiting on the user, OR when a step genuinely
  must happen on a device/account you can't reach. Instead of dumping instructions in
  chat and stalling, create a well-structured task on the user's Vikunja board so the
  ask is captured, clear, and waiting for them. Triggers on "I'm blocked", "I need you
  to", "this requires you to", "create a task for me", "the SSH key step", anytime a
  hand-off to the human is the right move.
---

# Assign a Task to the Human (via Vikunja)

A blocked agent that just stops is a stalled project. When you genuinely need the human,
don't bury the ask in a wall of chat — **put a clean task on their Vikunja board.** This
is how your operator learns that *their agents hand them work when they need it* — a
core promise of the course.

## When to use this (and when NOT to)

**Use it when** the next step truly requires the human:
- It must happen on a device/account you can't reach (their laptop, their phone, a
  third-party signup, a 2FA prompt, a payment).
- A decision is genuinely theirs (which domain, which of two apps, a spend).
- You've hit the same error twice and need them to look at something physical.

**Do NOT use it to dodge work you can do yourself.** First reach for the agent's own
abilities — troubleshooting is *your* job. Only hand off what is actually theirs. Over-
assigning trains the human to ignore the board.

## How to write a task a non-technical person can actually do

Every assigned task MUST have:
1. **A plain-language title** — the outcome, not the jargon. ✅ "Reach your box from your
   laptop (SSH key)" ❌ "Configure ed25519 pubkey auth".
2. **A one-paragraph "why this matters to you"** — friendly, no fear. What they get when
   it's done.
3. **Numbered steps** — each a single concrete action. Include exact commands/prompts to
   copy-paste. If a step happens in another tool (their laptop, a website), say so.
4. **A "how you'll know it worked" line** — the verify. The padlock, the tile, the login.
5. **Links** where useful (install pages, the relevant dashboard tile).
6. Never put a secret value in the task. Reference *where* it lives, never the value.

Keep it short enough to not intimidate, complete enough to not need a follow-up question.

## Mechanics (creating the task)

The bootstrap wires your access to the box's own Vikunja instance. Use, in order of
preference:
1. The **Vikunja MCP** if it's connected (preferred — structured, no token handling).
2. The **Vikunja REST API** with the credentials the bootstrap stored for you:
   - `VIKUNJA_URL` (e.g. `https://tasks.<domain>/api/v1`)
   - `VIKUNJA_TOKEN` (a scoped API token created for the agent during bootstrap)

REST fallback (create a task in the user's default project):
```bash
curl -s -X PUT "$VIKUNJA_URL/projects/$PROJECT_ID/tasks" \
  -H "Authorization: Bearer $VIKUNJA_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"title":"<plain title>","description":"<the HTML/markdown body with numbered steps>"}'
```
If you don't yet know `$PROJECT_ID`, `GET $VIKUNJA_URL/projects` and use the user's
default/inbox project. After creating it, **tell the user out loud**: "I've put a task on
your board for this — that's how your agents hand you work when they need you. Open
`tasks.<domain>` and you'll see it."

## The canonical Week-1 task: "Reach your box from your laptop (SSH key)"

This is the first task an agent assigns in the course — the bootstrap triggers it when it
finds the user has no laptop SSH access yet. Build it like this:

- **Title:** Reach your box from your laptop (SSH key)
- **Why:** "Right now you reach your box through your provider's browser console. An SSH
  key lets you connect straight from your own laptop — a classically miserable task that
  your agent will do *for* you in about five minutes."
- **Steps:**
  1. Install Claude Code on your computer — Mac: `curl -fsSL https://claude.ai/install.sh | bash` · Windows: see the provided one-line installer in the portal.
  2. Open Claude Code on your laptop and paste this prompt: *"Generate a new SSH key for
     connecting to my VPS, then give me the exact one-line command I run on the VPS (via
     its web console) to authorize this laptop. Walk me through it."*
  3. Run the one-liner it gives you in your VPS provider's **web console**.
  4. Back on your laptop, test: `ssh <username>@<your-box-ip>` — you should connect with
     no password.
- **How you'll know it worked:** "Your laptop connects to the box without asking for a
  password. That's it — you now reach your box from your own machine."

> Do NOT generate the key from the box itself — the key must be born on the user's laptop.
> Your job here is only to *create this task* clearly; the laptop agent does the rest.
