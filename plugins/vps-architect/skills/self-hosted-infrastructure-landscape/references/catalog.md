# The app catalog — what each thing is and what it replaces

World-knowledge about self-hostable apps: the job each does, the bill or
chore it replaces, and whether it's a good first pick for a nervous beginner.
**This file never says what's on the box** — that's the inventory's job
(`/srv/_docs/inventory.md`, verified by `docker ps`).

## The course base stack

These ship with the course bootstrap. Whether they're on *this* box is the
inventory's call — use this table for the "what is it / what does it replace"
story once the inventory confirms what's installed.

| Service | Its one job | What it replaces |
|---|---|---|
| **Caddy** | The doorman — routes web requests + fetches real HTTPS certificates (the padlock). Invisible. | Manual SSL, nginx fiddling |
| **Homepage** | The dashboard / home base — every service as a clickable tile at `home.<domain>`. | Memorizing URLs; living in a SaaS dashboard |
| **CloudCLI** | The friendly browser/phone window where you talk to your agent (Claude Code). | The terminal you were afraid of |
| **code-server** | VS Code in the browser, for when you want to look at your files in an editor. | A local dev setup |
| **FileBrowser** | Familiar, Drive-style file manager, **hidden files shown** so the owner can see everything they own (`.env`, `CLAUDE.md`, `.claude/`). | Dropbox/Google Drive (for box files) |
| **Vikunja** | The task/project board — and where agents drop the owner a clear task when blocked. | ClickUp |
| **Nightly backups + `/srv/_docs/`** | The box copies itself nightly and writes its own plain-English notes as it builds. | "Hope nothing breaks"; undocumented setups |

An illustrative savings stack: a training/webinar business renting n8n +
Supabase + BBB + FreshBooks + ClickUp + Salesforce ≈ **$375/mo**; owned on
one VPS (+ a dedicated BBB box) ≈ **$68/mo** — roughly **5.5× cheaper,
~$3,700/yr**, and you own it. Every app below stacks more of that swap.

## Install-your-own (the weekend project fuel)

Apps an operator might add to feel the install-via-agent pattern. Pick 1–2
that match *their* work — don't over-install.

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
