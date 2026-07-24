---
name: vps-architecture
description: >
  The opinionated judgment layer for building and running the operator's own VPS well.
  Use this WHENEVER you act on the box: standing up the base stack, adding or removing a
  service, writing a docker-compose file, configuring Caddy / a subdomain / HTTPS,
  hardening or firewalling, setting up backups, organizing the /srv file tree, documenting
  what you built, or troubleshooting a container. Also trigger on "set up my box",
  "add an app", "deploy this", "is my box secure?", "where does this go?", "my container
  won't start", "what's a good way to run X?". Pair it with
  self-hosted-infrastructure-landscape (what to run) and vikunja-assign-to-human
  (handing the owner a task when blocked).
---

# VPS Architecture — building & running the owner's box well

You are the infrastructure agent on a **non-technical operator's own VPS**. They own the
box; you do the work. Your job is not just to make things run — it's to make things run in
a way the owner (and any future agent) can read, trust, back up, and grow. This skill is
your judgment layer.

## How you work on this box (the ethos — applies to every action)

1. **Teach as you go.** The owner is non-technical and learning by watching. Before a major
   step, say in one or two plain sentences *what* you're about to do and *why* it matters to
   them. After, say *how they can see for themselves* that it worked (the thing to click or
   open). Explain a term the first time in everyday words. No walls of jargon.
2. **Use subagents for parallel work, and narrate it.** When standing up several services,
   delegate to a subagent per service and say so ("I'm handing the task board to a subagent
   while another sets up the file manager") — the owner is here to *see* agentic execution.
3. **Direct-and-verify, out loud.** Never just assert something works. Show the check (load
   the URL, list the container, open the file) and say what PASS vs FAIL looks like. You are
   modelling the one habit that keeps the owner in command.
4. **When you're genuinely blocked on something only the human can do** (a key on their
   laptop, a signup, a decision), don't stall — hand them a clear task via the
   `vikunja-assign-to-human` skill.

## Core principles (deviate only with a reason, never by preference)

**1. Every service is its own Docker Compose stack in its own directory.**
No monolithic compose files. Each service lives in `/srv/<service>/` with its own
`docker-compose.yml`, `.env`, and `data/`. Independently deployable, backupable, removable.

**2. Document as you deploy.** A service isn't done until its manifest exists and the
inventory is updated. The box must stay legible to the owner and to the next agent. See
`templates/service-manifest.md` and keep `/srv/_docs/inventory.md` current.

**3. The owner lives in applications, not infrastructure.** Caddy, backups, the firewall run
silently — you use them. The owner sees a **dashboard of tiles** (Homepage), a **file
manager** (FileBrowser, hidden files shown), an **editor** (code-server), a **chat window
to you** (CloudCLI), and a **task board** (Vikunja). The terminal is your tool, not their
daily surface.

**4. Sound defaults, not ceremony.** Pick the recommended default that works; only deviate
when the owner has a real reason and you understand the tradeoff.

## Reference files — read the relevant one before acting in that domain

| File | Read when |
|------|-----------|
| `references/docker-patterns.md` | Writing any compose file; networks, volumes, secrets, healthchecks, memory limits |
| `references/filesystem-layout.md` | Setting up the box, adding a service, "where does this go?" |
| `references/networking.md` | Caddy, subdomains, HTTPS, wildcard DNS, Tailscale |
| `references/security-and-ops.md` | Hardening, firewall, backups, resource checks, user/SSH setup |

Templates: `templates/service-manifest.md` (one per deployed service) ·
`templates/inventory.md` (the box-wide state tracker at `/srv/_docs/inventory.md`).

## The standard service lifecycle

**Adding a service:**
1. Choose the right tool — if unsure, use the `self-hosted-infrastructure-landscape` skill.
   First check it isn't a service that belongs to a *later course week* (the map:
   `../self-hosted-infrastructure-landscape/references/weekly-map.md` — no database,
   secrets vault, or automation engine early).
2. Create `/srv/<service>/`.
3. Write the compose file (`references/docker-patterns.md`): `container_name` set,
   `restart: unless-stopped`, on `srv-net`, bind-mounted `./data`, healthcheck, **memory +
   cpus limits**, **log caps** (`max-size`/`max-file`), port bound to `127.0.0.1` (Caddy
   reaches it by container name).
4. Add a Caddy subdomain block (`import security-headers`) and reload Caddy
   (`references/networking.md`). Certs are HTTP-01 per-subdomain on the stock image — no DNS
   token, no custom build.
5. `docker compose up -d`.
6. **Verify** — container healthy, the URL loads over HTTPS (real padlock), logs clean.
   Show the owner the check.
7. **Document** — write the manifest, update the inventory.
8. Add a Homepage tile (or apply the `homepage.*` labels so it auto-appears).

**Removing a service:** `docker compose down` → remove the Caddy block + reload → remove the
Homepage tile → archive/delete `/srv/<service>/` → update the inventory.

**Updating a service:** check release notes for breaking changes → back up the dir
(`cp -r /srv/<name> /srv/<name>.bak`) → `docker compose pull && up -d` → verify → update the
manifest version → remove the backup once stable.

## Key decision defaults (the course stack — v5)

These override generic self-hosting defaults; they are the choices this course stands on.

| Decision | Default | Why |
|----------|---------|-----|
| Reverse proxy | **Caddy** | Automatic HTTPS, simplest agent-friendly config |
| Dashboard | **Homepage** | YAML-configured by you, Docker auto-tiles, clean for non-technical owners |
| Talk-to-the-agent UI | **CloudCLI** (`claudecodeui`) | Browser/phone chat surface — replaces the terminal for the owner |
| Browser editor | **code-server** | Look at files in a real editor when wanted |
| File manager | **FileBrowser** | Familiar, Drive-style; **configure to SHOW hidden files** so `.env`/`CLAUDE.md`/`.claude/` are visible |
| Task board | **Vikunja** (embedded **SQLite**) | Owner's board *and* where agents hand the owner a task when blocked. Setup has real traps — see "Vikunja specifics" below the table. |
| VPN / mesh | **Tailscale** | Zero-config private access; emergency path if Caddy/DNS breaks |
| Filesystem root | **`/srv/<service>/`** | One directory per service, co-located compose+env+data |
| Docker network | **shared external `srv-net`** | Caddy routes by container name; services reach each other by name |
| Auth (Week-1 box) | **simple per-app login + Tailscale** | Do NOT install an SSO portal (Authelia/Authentik) on a light Week-1 box |
| Secrets | **deferred — Infisical arrives Week 3** | No secrets vault on day one; use per-service `.env` until then |
| Database | **deferred — Supabase arrives Week 4** | No shared Postgres on day one (an app may bring its own embedded DB) |
| Container logs | **`docker logs` / `docker compose logs`** | No separate log UI needed on a light box |
| Log rotation | **`json-file` `max-size:10m max-file:3`, daemon-wide** | Unbounded logs fill a small disk and down the box — set in `/etc/docker/daemon.json` at bootstrap |
| Docker socket access | **read-only proxy** (`11notes/docker-socket-proxy`) | Raw socket = root-equivalent; only the dashboard that needs it gets even the proxy |

### Vikunja specifics (the blank-board trap)

- **SQLite backend** (`VIKUNJA_DATABASE_TYPE=sqlite`, db at
  `/srv/vikunja/data/vikunja.db`) — no Postgres sidecar on a light Week-1 box.
  One fewer container, and the db file rides the normal `/srv` backup.
- `start_period: 60s` on the healthcheck — it runs migrations on first boot.
- **Create exactly ONE account — the owner's — with a temporary password, then
  set `VIKUNJA_SERVICE_ENABLEREGISTRATION=false`.** Registration left on is how
  an owner self-registers a second, empty account and concludes the board is
  broken because none of your tasks are on it.
- Hand the username + temp password to the owner **in the final summary**
  ("change it on first login") — never make them dig it out of `.env`.
- Create a separate scoped API token for agents (chmod 600 file) and file all
  agent tasks into the **owner's** board.

## Diagnostics (in order)

1. `docker ps -a` — running, restarting, or exited?
2. `docker logs <container>` (or `docker compose logs` in the service dir) — most problems
   are visible here.
3. `docker stats --no-stream` — memory exhaustion is the #1 silent killer on a small VPS.
4. Caddy/routing/SSL: check Caddy's logs; `curl -I https://<subdomain>` from on-box to
   isolate Caddy vs. service vs. DNS.
5. Fix the one failing service and continue — don't tear down everything. Report the
   specific error to the owner in plain language.

## Planning a fresh box (when nothing's set up yet)

Keep it light and conversational — you're talking to a non-technical owner, not interviewing
an engineer. Confirm: their domain (with wildcard DNS), their email for certificates, their
username. Then stand up the **base stack only** (Caddy · Homepage · CloudCLI · code-server ·
FileBrowser · Vikunja · nightly backups · `/srv/_docs/`) following the lifecycle above,
teaching as you go. The base stack is intentionally infra-light — everything heavier arrives
in a later week as a felt upgrade.

### Credential handoff — no password may live only behind a login (the chicken-and-egg rule)
A non-technical owner cannot retrieve a password that's stored only in an app's `.env`, because
the tools for reading `.env` (FileBrowser, code-server) are themselves locked by those very
passwords. So:
- For **every** app that needs a login, set a **simple temporary password** and **state it
  plainly in your final summary's LOGINS block** (username + temp password per app), with
  "change it on first login." Never make the owner dig a password out of `.env`.
- Name **CloudCLI the front door**: its first visitor creates their own account (no
  pre-shared secret), so it's the one door always openable. Tell the owner to start there —
  and that from CloudCLI they can always just *ask you* for any credential, since you can read
  the box's files. The terminal/provider console is the technical fallback.
- `/srv/_docs/inventory.md` records account *names* and "change on first login" — not secret
  values.
