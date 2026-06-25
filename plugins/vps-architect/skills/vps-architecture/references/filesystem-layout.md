# Filesystem Layout

Where everything goes on the owner's VPS, and why.

---

## Overview

```
/srv/                               Service deployments (your territory)
  _docs/                            Box documentation (keep current!)
    inventory.md                    What's running, URLs, where data lives
    network.md                      Subdomain map + Caddy summary
  caddy/                            Reverse proxy
    docker-compose.yml
    Caddyfile
    data/  config/                  Certs + Caddy state
  homepage/  vikunja/  cloudcli/    One directory per service, same shape:
  code-server/  filebrowser/
  <service>/
    docker-compose.yml
    .env                            Service-specific secrets
    data/                           Persistent data (bind-mounted, backed up)
    config/                         Mounted config files (if any)
    manifest.md                     Service doc (written after deploy)

/home/<user>/                       The owner's workspace (what they see)
  workspace/
    inputs/   outputs/   projects/
```

## Why `/srv/`

The Filesystem Hierarchy Standard reserves `/srv/` for "data for services provided by this
system" — the right home for Docker Compose stacks on Ubuntu. Co-locating compose + env +
data per service means:

- `ls /srv/` instantly shows what's deployed.
- `cp -r /srv/example /backup/` captures everything for that service.
- `rm -rf /srv/example` cleanly removes a service and all its data.
- Each service has an independent lifecycle.

## The `_docs/` directory — the box's memory

`/srv/_docs/inventory.md` is the **source of truth** for what's running; update it after
every deploy, removal, or change. `/srv/_docs/network.md` maps subdomains → services. These
are the first files you (or any future agent) read to understand the box, and they're how
the owner can *read* what they own. A box that documents itself is a box you can trust.

## The owner's workspace

`/home/<user>/` is what the owner sees in **FileBrowser** and **code-server** — their
content, not infrastructure. Keep `/srv` infrastructure out of their home. FileBrowser is
rooted here and **configured to show hidden files**, so the owner can see `.env`,
`CLAUDE.md`, and `.claude/` — seeing everything is part of owning the box.

## Permissions

- `/srv/` is managed by you (the agent's user; often root for Docker).
- `/home/<user>/` is owned by the owner's user.
- **FileBrowser and code-server run as the owner's user** so files created via the web UI
  have correct ownership.
