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

## How to use this skill

- When asked "what could I run?" → lead with the **install-your-own catalog** below,
  filtered to their stated use case, **good-first-pick flagged**.
- When asked "what's a self-hosted alternative to X?" → map X to the catalog; give the
  default + one alternative + what it saves.
- **Before recommending or installing anything, check the grows-weekly map.** If the
  thing they want arrives in a later week (a database, a secrets vault, automations),
  say so — "that's coming in Week N, and here's why we wait" — instead of installing it
  early. Week 1 stays infra-light on purpose.
- Pair installs with the standard lifecycle (own compose under `/srv/<service>/`, shared
  `srv-net`, Caddy subdomain, a Homepage tile, a `/srv/_docs/` manifest). The
  `vps-architecture` skill carries the deep how-to; this skill carries the *what & why*.

## The base stack (Week 1 — already on the box)

What stood up during the bootstrap. Each is a thing they were renting or doing badly,
now owned.

| Service | Its one job | What it replaces |
|---|---|---|
| **Caddy** | The doorman — routes web requests + fetches real HTTPS certificates (the padlock). Invisible. | Manual SSL, nginx fiddling |
| **Homepage** | The dashboard / home base — every service as a clickable tile at `home.<domain>`. | Memorizing URLs; living in a SaaS dashboard |
| **CloudCLI** | The friendly browser/phone window where you talk to your agent (Claude Code). | The terminal you were afraid of |
| **code-server** | VS Code in the browser, for when you want to look at your files in an editor. | A local dev setup |
| **FileBrowser** | Familiar, Drive-style file manager — see/open/upload/create files, **hidden files shown** so you can see everything you own (`.env`, `CLAUDE.md`, `.claude/`). | Dropbox/Google Drive (for box files) |
| **Vikunja** | Your task/project board — and where your agents drop *you* a clear task when they're blocked. | ClickUp |
| **Nightly backups + `/srv/_docs/`** | The box copies itself nightly and writes its own plain-English notes as it builds. | "Hope nothing breaks"; undocumented setups |

## The box grows weekly (do NOT install these early)

Each lands the week its payoff lands. If the operator asks for one of these in Week 1,
explain it's coming and why waiting is the point — don't install it.

| Week | Arrives | Why that week |
|---|---|---|
| 2 | **Hermes** (your always-on agent + chat gateway) | The agent you talk to from here on; sits above Claude Code. |
| 3 | **Infisical** (secrets/keys UI) | The first time you connect agents to your tools and need keys. |
| 4 | **Supabase** (Postgres + Studio) | When you build apps that need a database. The inspect-verify window. |
| 5 | **n8n** (visual automations) | Autonomous execution — automations your agent can build and you can watch run. |
| 6 | **GitHub / plugins + weekly-update agent** | Staying current; the take-away agent that briefs you weekly. |

So: **no database, no secrets vault, no automation engine in Week 1.** Named so they
know it's coming; not installed, so it's not their problem yet.

## The install-your-own catalog (the weekend project fuel)

OSS apps a student might add **now** to feel the install-via-agent pattern. Each: its
job, what it replaces (and the rough rented cost), and whether it's a good first pick
for a nervous beginner. Pick 1–2 that match *their* work — don't over-install.

| App | Its one job | Replaces (rough rent) | Good first pick? |
|---|---|---|---|
| **Nextcloud** | Files, calendar, contacts — your own cloud | Dropbox/Google Workspace (~$12/mo) | ⚠️ Powerful but heavier — fine if they want "my own Google Drive" |
| **Immich** | Photo backup + albums, phone auto-upload | Google Photos (~$10/mo) | ✅ High delight, very tangible |
| **Paperless-ngx** | Scan/store/search documents, auto-OCR | Paper drawers; DocuWare | ✅ Great for consultants/shop owners |
| **Uptime Kuma** | "Is my site up?" status monitoring + alerts | Pingdom/UptimeRobot (~$15/mo) | ✅ Tiny, instant payoff, very beginner-friendly |
| **Stirling-PDF** | Merge/split/sign/compress PDFs in the browser | Adobe Acrobat (~$20/mo) | ✅ Everyone has PDF chores |
| **Cal.com** | Booking/scheduling page | Calendly (~$12/mo) | ✅ Obvious win for anyone client-facing |
| **Docmost / Outline** | Team wiki / notes | Notion (~$10/mo) | ⚠️ Nice, slightly more setup |
| **Linkwarden** | Save/organize bookmarks + read-later | Pocket/Raindrop (~$3–5/mo) | ✅ Low-stakes, easy first install |
| **Ghost** | Newsletter + blog | Substack/Mailchimp (rev share / ~$13/mo) | ⚠️ For those building an audience |
| **Plausible** | Privacy-friendly website analytics | Google Analytics (free but it's the cost) | ⚠️ Only if they run a site |

> The base stack already shows the headline savings: a training/webinar business renting
> n8n + Supabase + BBB + FreshBooks + ClickUp + Salesforce ≈ **$375/mo**; owned on one
> VPS (+ a dedicated BBB box) ≈ **$68/mo** — roughly **5.5× cheaper, ~$3,700/yr**, and you
> own it. Every app above stacks more of that swap.

## "Is this worth running?" — the decision frame

Pitch this at non-technical judgment when they're unsure about an app:

1. **Does it kill a bill or a real chore?** If it doesn't replace something you pay for or
   dread, it's a toy — fine, but know that.
2. **Is it popular and maintained?** Lots of users + recent releases = problems are already
   solved and your agent can fix issues. Abandoned project = you're on your own.
3. **Is it a good *first* pick?** Single container, no database needed yet (remember: DB is
   Week 4), clear web UI. Save the heavy ones for when the box has grown.
4. **Install one, verify it, then decide on the next.** Don't bulk-install. The pattern —
   ask → it appears as a tile → you click and confirm — is the skill; repetition builds it.
