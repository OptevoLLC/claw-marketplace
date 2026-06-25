# Networking

Caddy reverse proxy, subdomain routing, certificates, DNS, and Tailscale.

---

## Caddy

Caddy is the reverse proxy: automatic HTTPS, simplest config. The Caddyfile lives at
`/srv/caddy/Caddyfile`.

```
{
    email {$ACME_EMAIL}
}

# define security headers once, import into every site
(security-headers) {
    header {
        Strict-Transport-Security "max-age=31536000; includeSubDomains"
        X-Content-Type-Options "nosniff"
        X-Frame-Options "SAMEORIGIN"
        Referrer-Policy "strict-origin-when-cross-origin"
        -Server
    }
}

# the owner's dashboard / home base
home.{$DOMAIN} {
    import security-headers
    reverse_proxy homepage:3000
}

agent.{$DOMAIN} { import security-headers
    reverse_proxy cloudcli:3008 }       # talk-to-your-agent window
code.{$DOMAIN}  { import security-headers
    reverse_proxy code-server:8443 }    # browser editor
files.{$DOMAIN} { import security-headers
    reverse_proxy filebrowser:80 }      # file manager
tasks.{$DOMAIN} { import security-headers
    reverse_proxy vikunja:3456 }        # task board
```

`{$DOMAIN}` / `{$ACME_EMAIL}` come from Caddy's `.env`. **Confirm each service's real internal
port** against its compose/image before writing the block. Only add `includeSubDomains` to
HSTS once *every* subdomain is confirmed serving HTTPS.

### Certificates — default to HTTP-01 per-subdomain (works out of the box)

This is the part to get right. The stock **`caddy:2-alpine`** image, with the per-subdomain
blocks above, obtains a **separate certificate per subdomain via the HTTP-01 challenge** on
first request — **no DNS API token, no custom build, nothing to configure.** This is the
default for this stack: simplest, and a per-host cert has a smaller blast radius than one
wildcard key.

**A wildcard *certificate* is a different, opt-in thing** and a common trap: it requires the
**DNS-01 challenge**, which the stock image **cannot do** — you'd have to build a custom Caddy
(`xcaddy` / the `caddy:builder` image) bundling a DNS-provider module (e.g.
`caddy-dns/cloudflare`) and supply a **scoped** API token (`Zone:DNS:Edit` + `Zone:Read` on
that one zone only — never the global key), injected via `.env`, never the Caddyfile. Only go
here if you specifically need a single wildcard cert; for this stack you don't.

> Note the distinction from DNS: a **wildcard DNS A record** (`*.domain → IP`) is just
> *resolution convenience* so new subdomains resolve without new DNS records. It works
> perfectly with HTTP-01 per-subdomain certs. Wildcard DNS ≠ wildcard cert.

### Caddy compose

```yaml
# /srv/caddy/docker-compose.yml
services:
  caddy:
    image: caddy:2-alpine
    container_name: caddy
    restart: unless-stopped
    ports: ["80:80", "443:443", "443:443/udp"]   # 443/udp = HTTP/3
    networks: [srv-net]
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - ./data:/data
      - ./config:/config
    env_file: [.env]
    logging: { driver: json-file, options: { max-size: "10m", max-file: "3" } }
networks:
  srv-net:
    external: true
```

### Adding a subdomain

1. Add a block: `newservice.{$DOMAIN} { import security-headers; reverse_proxy <container>:<port> }`
2. Reload, no downtime: `docker exec caddy caddy reload --config /etc/caddy/Caddyfile`
3. Caddy fetches the cert (HTTP-01) on first request. No DNS change if wildcard DNS is set.

### Keep it simple (Week-1 box)

Do **not** wrap services in an SSO portal (Authelia/Authentik) on a light Week-1 box — use
each app's own login plus Tailscale for private access. Internal-only services just get **no
Caddyfile entry** — reachable only inside `srv-net`.

## Wildcard DNS

```
{$DOMAIN}      →  <VPS_IP>     (A record)
*.{$DOMAIN}    →  <VPS_IP>     (A record, wildcard)
```

New services then need only a Caddyfile entry. Behind Cloudflare, set these records to
**DNS-only (grey cloud)** or Caddy's HTTP-01 challenge fails (this is the #1 cert pitfall).
Verify: `dig +short home.{$DOMAIN}` returns the box IP.

## Tailscale (private access + emergency path)

```bash
curl -fsSL https://tailscale.com/install.sh | sh
tailscale up
```

Reach services over the Tailscale IP (100.x.x.x) for things that shouldn't be public, or as
the way back in if Caddy/DNS ever breaks.

## Port allocation

Only Caddy publishes public ports (80, 443, 443/udp). Tailscale uses 41641/udp. Everything
else binds to `127.0.0.1` and is reached through Caddy.

## Troubleshooting

- **Connection refused:** container running? (`docker ps | grep <name>`) on `srv-net`?
  Caddyfile name+port correct? Did you reload Caddy?
- **Certificate won't issue:** does `dig <subdomain>.{$DOMAIN}` resolve to the box? Port 80
  open (HTTP-01 needs it)? Cloudflare set to DNS-only? Check Caddy logs for ACME errors.
- **502 Bad Gateway:** registered but not responding — check the container's health and logs,
  confirm the internal port in the Caddyfile.
