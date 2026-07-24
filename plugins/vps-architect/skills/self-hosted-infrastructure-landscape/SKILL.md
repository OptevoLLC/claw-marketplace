---
name: self-hosted-infrastructure-landscape
description: >
  The curated map of what a non-technical operator can run on their own VPS and
  what each thing replaces. Use whenever the user asks "what could I run / install
  on my box?", "what's a self-hosted alternative to [SaaS]?", "what does this
  replace / what would it save me?", "should I install X?", or wants to pick an app
  for their weekend project. Grounds recommendations in an opinionated default-first
  catalog, knows which services are part of the base stack vs. arriving in a later
  course week (so it never installs something early), and frames every pick around
  "what bill does this kill and is it a good first pick for a beginner?"
---

# Self-Hosted Infrastructure Landscape

The agent-readable companion to the course's open-source app catalog. It answers,
for a **non-technical operator**, "what could live on my box, and what does each
thing replace?" — without sending them into self-hosting forums. Opinionated:
**recommend one default that works, then name alternatives for the curious.**

> Voice when talking to the operator: direct, anti-corporate, empowering. "Stop
> renting your software. Start owning your stack." Builder receipts — real jobs,
> real bills replaced. Never condescend; never bury them in jargon.

## The data lives in references — read the one the question needs

| File | Read when |
|------|-----------|
| `references/weekly-map.md` | **Before recommending or installing anything** — and for "should I install X?" |
| `references/catalog.md` | "What could I run?", "alternative to [SaaS]?", weekend-project picks |
| `references/base-stack.md` | "What's already on my box?", "what does this tile do?", the savings math |

These files change as the course evolves (new cohort, new week map, catalog
updates) — always read the file rather than answering from memory of it.

## How to answer

- **"What could I run?"** → read the catalog, filter to *their* stated use case
  (see their operator profile from the grill-me interview), lead with the
  good-first-pick flags. Recommend one default, name one alternative.
- **"What's a self-hosted alternative to X?"** → map X via the catalog: the
  default + one alternative + what it saves.
- **Before recommending or installing anything, check the weekly map.** If it
  arrives in a later week (a database, a secrets vault, automations), say so —
  "that's coming in Week N, and here's why we wait" — instead of installing it
  early. Week 1 stays infra-light on purpose.
- Pair every install with the standard lifecycle (own compose under
  `/srv/<service>/`, shared `srv-net`, Caddy subdomain, Homepage tile,
  `/srv/_docs/` manifest). The `vps-architecture` skill carries the deep
  how-to; this skill carries the *what & why*.

## "Is this worth running?" — the decision frame

Pitch this at non-technical judgment when they're unsure about an app:

1. **Does it kill a bill or a real chore?** If it doesn't replace something you
   pay for or dread, it's a toy — fine, but know that.
2. **Is it popular and maintained?** Lots of users + recent releases = problems
   are already solved and your agent can fix issues. Abandoned project = you're
   on your own.
3. **Is it a good *first* pick?** Single container, no database needed yet
   (remember: DB is Week 4), clear web UI. Save the heavy ones for when the box
   has grown.
4. **Install one, verify it, then decide on the next.** Don't bulk-install. The
   pattern — ask → it appears as a tile → you click and confirm — is the skill;
   repetition builds it.
