# Security & Operations

Hardening, backups, and resource care — pitched at a light, single-owner Week-1 box.

---

## Hardening (apply on every box)

**Firewall (UFW):**
```bash
ufw default deny incoming
ufw default allow outgoing
ufw allow 22/tcp     # SSH
ufw allow 80/tcp     # HTTP (Caddy)
ufw allow 443/tcp    # HTTPS (Caddy)
ufw allow 443/udp    # HTTP/3
ufw allow 41641/udp  # Tailscale
ufw enable
```

**Automatic security updates:**
```bash
apt install unattended-upgrades -y
dpkg-reconfigure -plow unattended-upgrades
```

**Non-root user:** the owner's user (e.g. `participant`) is in the `docker` group;
FileBrowser and code-server run as that user so files get the right ownership.

**SSH — do not lock the owner out.** A DigitalOcean droplet created with an SSH key already
has password login disabled. The owner reaches the box through the provider's **browser
console** and (soon) Tailscale, so SSH-from-their-laptop is a *guided bonus*, not a
requirement. If laptop SSH isn't set up yet, hand them the task via the
`vikunja-assign-to-human` skill (the key must be generated on *their* laptop, not the box) —
don't disable any access path they're currently relying on.

**fail2ban** (cheap, worth it on a public box):
```bash
apt install fail2ban -y && systemctl enable --now fail2ban
```

**Not on a Week-1 box:** an SSO gateway (Authelia/Authentik), Docker-socket proxies, Caddy
rate-limit plugins. These are real tools for later/heavier setups — note them, don't install
them now.

## Backups — the box copies itself nightly

The Week-1 box has no shared database, so the default is a nightly tar of the whole `/srv`
tree (compose files, configs, data, docs) plus the owner's `/home`, with a dump of Vikunja's
embedded database:

```bash
# /etc/cron.d/box-backup
0 2 * * * root tar czf /var/backups/srv_$(date +\%Y\%m\%d).tar.gz /srv/ 2>/dev/null
0 2 * * * root tar czf /var/backups/home_$(date +\%Y\%m\%d).tar.gz /home/ 2>/dev/null
0 2 * * * root docker exec vikunja-db pg_dump -U vikunja vikunja | gzip > /srv/vikunja/backups/vikunja_$(date +\%Y\%m\%d).sql.gz
# retention
0 4 * * * root find /var/backups -name "*.tar.gz" -mtime +7 -delete
0 4 * * * root find /srv/vikunja/backups -name "*.sql.gz" -mtime +14 -delete
```

This is local-only — it protects against accidental deletion, not box loss. Offsite backup
(Restic/Kopia to object storage) is a good later addition; mention it, don't force it Week 1.
Tell the owner in plain words what "your box backs itself up every night" means for them.

**Restore:** `cd / && tar xzf /var/backups/srv_<date>.tar.gz` for configs/data;
`gunzip < /srv/vikunja/backups/vikunja_<date>.sql.gz | docker exec -i vikunja-db psql -U vikunja vikunja`
for the task board.

## Resource care (small box, watch memory)

```bash
docker stats --no-stream --format "table {{.Name}}\t{{.CPUPerc}}\t{{.MemUsage}}"
df -h /
du -sh /srv/*/data 2>/dev/null | sort -h
```

Memory exhaustion is the most common silent failure on a 2-vCPU/4-GB box. Every service
carries a memory limit (see `docker-patterns.md`); if the box gets tight, find the largest
container, stop what isn't in use, and look for a steadily-growing (leaking) container.
