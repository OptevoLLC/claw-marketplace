# Box Inventory

> Source of truth for this VPS. Update after every deploy, removal, or change.
> Regenerate the Services table partly from live state (`docker ps`,
> `docker network inspect srv-net`) so it can't silently drift from reality.

## Box
- **Provider / plan:** [e.g. DigitalOcean, 2 vCPU / 4 GB / 80 GB]
- **OS:** Ubuntu 24.04 LTS
- **IP:** [public IP]
- **Domain:** [example.com] · **DNS:** [provider, wildcard *.example.com → IP]

## Network
- **Docker network:** srv-net (external)
- **Reverse proxy:** Caddy (auto HTTPS)
- **VPN:** Tailscale [100.x.x.x]
- **Firewall:** UFW — 22, 80, 443, 443/udp, 41641/udp

## Services (base stack)

| Service | URL | Container | Internal port | Status |
|---------|-----|-----------|---------------|--------|
| Caddy | — | caddy | 80,443 | Running |
| Homepage | home.example.com | homepage | 3000 | Running |
| CloudCLI | agent.example.com | cloudcli | 3008 | Running |
| code-server | code.example.com | code-server | 8443 | Running |
| FileBrowser | files.example.com | filebrowser | 80 | Running |
| Vikunja | tasks.example.com | vikunja | 3456 | Running |
| Vikunja DB | — | vikunja-db | 5432 | Running |

> Coming in later weeks (NOT installed yet): Hermes (wk2) · Infisical (wk3) ·
> Supabase (wk4) · n8n (wk5) · GitHub/plugins (wk6).

## Backups
- **Nightly /srv + /home tar:** [Configured / Not] → /var/backups, 7-day retention
- **Vikunja DB dump:** [Configured / Not] → /srv/vikunja/backups, 14-day retention
- **Offsite:** [Configured / Not]

## Users
- **root:** infrastructure (the on-box Claude Code agent)
- **[participant]:** owner workspace, FileBrowser, code-server

## Change log
| Date | Change | By |
|------|--------|----|
| [date] | Initial bootstrap | Claude Code (on-box) |
