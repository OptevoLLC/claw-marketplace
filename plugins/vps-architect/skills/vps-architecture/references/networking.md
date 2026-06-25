# Networking

Caddy reverse proxy, subdomain routing, wildcard DNS, and Tailscale.

---

## Caddy

Caddy is the reverse proxy: automatic HTTPS via Let's Encrypt, simplest config. The
Caddyfile lives at `/srv/caddy/Caddyfile`.

```
{
    email {$ACME_EMAIL}
}

# the owner's dashboard / home base
home.{$DOMAIN} {
    reverse_proxy homepage:3000
}

# talk-to-your-agent window
agent.{$DOMAIN} {
    reverse_proxy cloudcli:3008
}

# browser editor
code.{$DOMAIN} {
    reverse_proxy code-server:8443
}

# file manager
files.{$DOMAIN} {
    reverse_proxy filebrowser:80
}

# task board
tasks.{$DOMAIN} {
    reverse_proxy vikunja:3456
}
```

`{$DOMAIN}` and `{$ACME_EMAIL}` come from Caddy's `.env`. **Confirm each service's real
internal port** against its compose/image before writing the block.

### Caddy compose

```yaml
# /srv/caddy/docker-compose.yml
services:
  caddy:
    image: caddy:2-alpine
    container_name: caddy
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
      - "443:443/udp"      # HTTP/3
    networks: [srv-net]
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - ./data:/data
      - ./config:/config
    env_file: [.env]
networks:
  srv-net:
    external: true
```

### Adding a subdomain

1. Add a block: `newservice.{$DOMAIN} { reverse_proxy <container_name>:<port> }`
2. Reload with no downtime: `docker exec caddy caddy reload --config /etc/caddy/Caddyfile`
3. Caddy fetches the certificate on first request. No DNS change needed if wildcard DNS is set.

### Keep it simple (Week-1 box)

Do **not** wrap services in an SSO portal (Authelia/Authentik) on a light Week-1 box — use
each app's own login plus Tailscale for private access. SSO is an advanced, later addition.
Internal-only services (e.g. a database an app brings with it) just get **no Caddyfile
entry** — they stay reachable only inside `srv-net`.

## Wildcard DNS

One pair of records at the DNS provider points every subdomain at the box:

```
{$DOMAIN}      →  <VPS_IP>     (A record)
*.{$DOMAIN}    →  <VPS_IP>     (A record, wildcard)
```

New services then need only a Caddyfile entry — no DNS change. If the domain is behind
Cloudflare, set these records to **DNS-only (grey cloud)** or Caddy's HTTP-01 challenge
fails. Verify: `dig +short home.{$DOMAIN}` returns the box IP.

## Tailscale (private access + emergency path)

```bash
curl -fsSL https://tailscale.com/install.sh | sh
tailscale up
```

Reach services over the Tailscale IP (100.x.x.x) for things that shouldn't be public, or as
the way back in if Caddy/DNS ever breaks.

## Port allocation

Only Caddy publishes public ports (80, 443, 443/udp). Tailscale uses 41641/udp. Everything
else binds to `127.0.0.1` and is reached through Caddy. Nothing else should be exposed.

## Troubleshooting

- **Connection refused on a subdomain:** container running? (`docker ps | grep <name>`) on
  `srv-net`? Caddyfile name+port correct? Did you reload Caddy?
- **Certificate won't issue:** does `dig <subdomain>.{$DOMAIN}` resolve? Is port 80 open
  (Caddy needs it for the challenge)? Check Caddy logs for ACME errors. Cloudflare proxy on?
- **502 Bad Gateway:** registered but not responding — check the container's health and logs
  and confirm the internal port in the Caddyfile.
