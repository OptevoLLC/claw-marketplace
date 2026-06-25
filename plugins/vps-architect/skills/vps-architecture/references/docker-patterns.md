# Docker Patterns

Standard patterns for deploying and managing containerized services on the owner's VPS.

---

## Compose file conventions

Every service gets its own `docker-compose.yml` in `/srv/<service>/`.

```yaml
# /srv/example/docker-compose.yml
services:
  example:
    image: vendor/example:1            # pin the major version, not :latest
    container_name: example            # predictable name for Caddy + logs
    restart: unless-stopped            # survives reboots; stays down if you stop it
    networks:
      - srv-net
    ports:
      - "127.0.0.1:8080:8080"          # bind to localhost; Caddy fronts it
    volumes:
      - ./data:/data                   # co-located, easy to back up
    env_file:
      - .env                           # secrets here, never in the compose file
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8080/health"]
      interval: 30s
      timeout: 5s
      retries: 3
      start_period: 10s
    logging:                           # cap logs or json-file grows until the disk is full
      driver: json-file
      options:
        max-size: "10m"
        max-file: "3"
    deploy:
      resources:
        limits:
          memory: 512M                 # a runaway container must not OOM the box
          cpus: "1.0"                  # ...and must not pin every core

networks:
  srv-net:
    external: true
```

`deploy.resources.limits` IS honored by `docker compose` outside Swarm — this is a common
myth. Always set **both** `memory` and `cpus`.

## Startup ordering with healthchecks

When a service needs another to be *ready* (not just started), gate it on the dependency's
healthcheck — don't just `depends_on` the container:

```yaml
  app:
    depends_on:
      db:
        condition: service_healthy     # waits for db's healthcheck to pass
```

Always include a healthcheck when the image supports it (HTTP `curl -f`, or `pg_isready` /
`redis-cli ping` for datastores) and a `start_period` so slow-booting services aren't marked
unhealthy early. Healthchecks are also what let you *verify* a deploy instead of guessing.

## The shared network

Create it once during bootstrap, before any service: `docker network create srv-net`. All
services join `srv-net`; Caddy routes to any container by `container_name`, and services
reach each other by name. **Bind public-facing ports to `127.0.0.1`** so the only way in is
through Caddy (which adds HTTPS). Internal-only services need no ports at all.

## Logging — don't fill the disk

The default `json-file` driver grows **unbounded**; on a small box this silently fills the
disk and the box dies. Two layers of defense:

1. **Per service:** the `logging:` block above (`max-size: 10m`, `max-file: 3`).
2. **Daemon-wide default** (the higher-leverage move — covers every container, even ones you
   forget). Write `/etc/docker/daemon.json`:
   ```json
   { "log-driver": "json-file", "log-opts": { "max-size": "10m", "max-file": "3" } }
   ```
   then `systemctl restart docker`. Set this during bootstrap.

## Resource headroom (4 GB box)

The host OOM-killer ignores container boundaries — if total RAM is exhausted it may kill the
wrong process. Keep **20–30% of RAM unallocated** across all `memory` limits, and give every
service a `cpus` limit so one can't starve the others.

## Secrets & env

Per-service `.env` files; generate strong values with `openssl rand -base64 24`. Never put
secrets in the compose file — they leak via `docker inspect`. (A central secrets vault,
**Infisical**, arrives in Week 3; until then per-service `.env` is correct.)

## Volumes

Prefer **bind mounts** (`./data:/app/data`) for anything you want to back up or let the owner
see — visible on the host and captured by the nightly `/srv` backup. Use named volumes only
for internal state nobody needs to touch. Mind container UID/GID vs. host ownership; set
`user: "1000:1000"` where the image supports it (a positive default, not just an anti-pattern
to avoid).

## Image pinning

Pin the **major version** (`postgres:16-alpine`), not bare `latest` (could jump majors) and
not an exact patch (`16.2`, misses security fixes). For the few **internet-exposed,
security-sensitive** images you may additionally pin by digest
(`caddy:2-alpine@sha256:...`) to defend against tag-overwrite supply-chain attacks — note
this blocks auto-patching, so use it sparingly, not as a default.

## The Docker socket — read-only proxy by default

The Docker socket is **root-equivalent host access**. Some dashboards (Homepage, Dozzle) want
it to show container status. Do **not** mount `/var/run/docker.sock` into them directly.
Default to a **read-only socket proxy** and point the dashboard at it:

```yaml
# /srv/docker-socket-proxy/docker-compose.yml
services:
  docker-socket-proxy:
    image: 11notes/docker-socket-proxy:latest   # MIT, rootless, distroless, read-only
    container_name: docker-socket-proxy
    restart: unless-stopped
    networks: [srv-net]
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
    # exposes a filtered, read-only API at tcp://docker-socket-proxy:2375 on srv-net
```
Then set the dashboard's Docker host to `tcp://docker-socket-proxy:2375`. Teach the owner the
honest caveat: a read-only socket makes exploitation *harder, not impossible* — so only the
dashboard that needs it gets even this.

## Common operations

```bash
cd /srv/<service> && docker compose up -d          # deploy
docker compose ps                                  # status (in service dir)
docker compose logs -f                             # logs (in service dir)
docker compose pull && docker compose up -d        # update
docker ps --format "table {{.Names}}\t{{.Status}}" # all containers
docker stats --no-stream                           # resource use
docker image prune -f                              # reclaim space
```

## Anti-patterns (don't)

- **One giant compose file** — kills independent management; one change risks everything.
- **Publishing every port to `0.0.0.0`** — bypasses Caddy's HTTPS. Bind to `127.0.0.1`.
- **No log limits** — unbounded `json-file` fills the disk and downs the box.
- **No memory/cpu limit** — one leak can take the whole box down.
- **Mounting the raw Docker socket** — root-equivalent. Use the read-only proxy, only where needed.
