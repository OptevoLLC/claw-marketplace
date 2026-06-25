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

**Automatic security updates — and actually reboot for them:**
```bash
apt install unattended-upgrades -y
dpkg-reconfigure -plow unattended-upgrades
```
Then ensure patches that need a reboot actually land on this unattended box — in
`/etc/apt/apt.conf.d/50unattended-upgrades`:
```
Unattended-Upgrade::Automatic-Reboot "true";
Unattended-Upgrade::Automatic-Reboot-Time "03:00";
```
Without this, security patches install but the box keeps running the old kernel indefinitely.

**Non-root user:** the owner's user (e.g. `participant`) is in the `docker` group;
FileBrowser and code-server run as that user so files get the right ownership.

### SSH — harden WITHOUT locking the owner out (read before touching sshd)

Disabling password auth is correct, but a botched SSH change is the one mistake that bricks
access for a non-technical owner. Follow this protocol every time:

1. Prefer **`ed25519`** keys (`ssh-keygen -t ed25519`).
2. **Verify a working key-based login in a SECOND session BEFORE** you restart sshd. Never
   restart sshd on the strength of an untested config from your only session.
3. Set `PasswordAuthentication no` and `PermitRootLogin prohibit-password`, then
   `systemctl restart ssh`.
4. Know the out-of-band recovery path: the **provider's web console** (DigitalOcean/Hetzner)
   reaches the box even with SSH fully broken — but it logs in by *password*, so keep a strong
   root/sudo password set (or use the provider's rescue/console-injection) even though SSH
   passwords are off. This is the safety net.
5. If the owner has **no laptop key yet**, do NOT lock things down in a way that strands them —
   hand them the SSH-key setup as a task via `vikunja-assign-to-human` (the key is generated on
   *their* laptop), and leave the provider console path intact until it's done.

**fail2ban** (cheap, worth it on a public box):
```bash
apt install fail2ban -y && systemctl enable --now fail2ban
```
**Do NOT adopt CrowdSec on a single small box** — it's heavier and overkill here; fail2ban is
the right tool at this size. Also not on a Week-1 box: an SSO gateway (Authelia/Authentik) or
Caddy rate-limit plugins — real tools for later/heavier setups, not now.

## Backups — the box copies itself nightly

The bind-mount-everything-under-`/srv` layout makes a flat nightly tar sufficient for configs
and most data. The one correctness trap: **a live database file (SQLite/Postgres) tarred
mid-write can be torn/corrupt.** So snapshot each app's DB consistently first, then tar.

On the Week-1 base box the only stateful app is **Vikunja on SQLite** (db at
`/srv/vikunja/data/vikunja.db`). Take a consistent SQLite snapshot with `.backup` (atomic, no
downtime), then tar `/srv` and `/home`:

```bash
# /etc/cron.d/box-backup
0 2 * * * root docker run --rm -v /srv/vikunja/data:/d keinos/sqlite3 sqlite3 /d/vikunja.db ".backup /d/vikunja.bak" 2>/dev/null
5 2 * * * root tar czf /var/backups/srv_$(date +\%Y\%m\%d).tar.gz /srv/ 2>/dev/null
5 2 * * * root tar czf /var/backups/home_$(date +\%Y\%m\%d).tar.gz /home/ 2>/dev/null
# retention (14 days)
0 4 * * * root find /var/backups -name "*.tar.gz" -mtime +14 -delete
```

(If `sqlite3` is already on the host, `sqlite3 /srv/vikunja/data/vikunja.db ".backup …"`
works without the helper container.) **General rule (not a Postgres-shaped assumption):** for
any app that carries its own embedded DB, snapshot/dump it before the tar, or stop→tar→start
in the backup window. Most config/data is fine to tar live.

This is **local-only** — it protects against accidental deletion, not box loss. The first real
upgrade is **one offsite target**. For an agent-managed, non-technical owner, **Kopia** is the
better default (built-in web UI, native **pre-snapshot hooks** to run the per-app DB dumps
above, zstd compression, granular retention) to Backblaze B2 / S3; **Restic** is the
larger-community alternative. Pick one, pin it. Tell the owner in plain words what "your box
backs itself up every night" means for them.

**Restore:** `cd / && tar xzf /var/backups/srv_<date>.tar.gz` brings back every service's
config/data including Vikunja's SQLite db file — for the base box there's no separate DB
restore step. Restart the stacks afterward (`docker compose up -d` per `/srv/<service>/`).

## Resource care (small box, watch memory)

```bash
docker stats --no-stream --format "table {{.Name}}\t{{.CPUPerc}}\t{{.MemUsage}}"
df -h /
du -sh /srv/*/data 2>/dev/null | sort -h
```

Memory exhaustion is the most common silent failure on a 2-vCPU/4-GB box. Every service
carries `memory` + `cpus` limits (see `docker-patterns.md`) and you keep 20–30% RAM headroom;
if the box gets tight, find the largest container, stop what isn't in use, and look for a
steadily-growing (leaking) container. Watch disk too — capped logs (see `docker-patterns.md`)
are what keep `/` from filling.
