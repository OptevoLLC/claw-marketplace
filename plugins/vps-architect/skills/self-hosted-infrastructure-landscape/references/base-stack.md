# The base stack (Week 1 — already on the box)

What stood up during the bootstrap. Each is a thing the owner was renting or
doing badly, now owned.

| Service | Its one job | What it replaces |
|---|---|---|
| **Caddy** | The doorman — routes web requests + fetches real HTTPS certificates (the padlock). Invisible. | Manual SSL, nginx fiddling |
| **Homepage** | The dashboard / home base — every service as a clickable tile at `home.<domain>`. | Memorizing URLs; living in a SaaS dashboard |
| **CloudCLI** | The friendly browser/phone window where you talk to your agent (Claude Code). | The terminal you were afraid of |
| **code-server** | VS Code in the browser, for when you want to look at your files in an editor. | A local dev setup |
| **FileBrowser** | Familiar, Drive-style file manager — see/open/upload/create files, **hidden files shown** so you can see everything you own (`.env`, `CLAUDE.md`, `.claude/`). | Dropbox/Google Drive (for box files) |
| **Vikunja** | Your task/project board — and where your agents drop *you* a clear task when they're blocked. | ClickUp |
| **Nightly backups + `/srv/_docs/`** | The box copies itself nightly and writes its own plain-English notes as it builds. | "Hope nothing breaks"; undocumented setups |

## The headline savings math

A training/webinar business renting n8n + Supabase + BBB + FreshBooks +
ClickUp + Salesforce ≈ **$375/mo**; owned on one VPS (+ a dedicated BBB box)
≈ **$68/mo** — roughly **5.5× cheaper, ~$3,700/yr**, and you own it. Every
app in the catalog stacks more of that swap.
