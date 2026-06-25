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
      - .env                           # secrets live here, never in the compose file
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8080/health"]
      interval: 30s
      timeout: 5s
      retries: 3
      start_period: 10s
    deploy:
      resources:
        limits:
          memory: 512M                 # a runaway container must not OOM the box

networks:
  srv-net:
    external: true
```

## The shared network

Create it once during bootstrap, before any service:

```bash
docker network create srv-net
```

All services join `srv-net`. This lets Caddy route to any container by `container_name`,
and lets services reach each other by name. **Bind public-facing ports to `127.0.0.1`** so
the only way in is through Caddy (which adds HTTPS). Internal-only services need no ports at
all.

## Secrets & env

Per-service `.env` files; generate strong values with `openssl rand -base64 24`. Never put
secrets in the compose file — they leak via `docker inspect`. (A central secrets vault,
**Infisical**, arrives in Week 3; until then per-service `.env` is correct.)

## Volumes

Prefer **bind mounts** (`./data:/app/data`) for anything you want to back up or let the owner
see — they're visible on the host and captured by the nightly `/srv` backup. Use named
volumes only for internal container state nobody needs to touch. Watch container UID/GID vs.
host ownership; set `user: "1000:1000"` where the image supports it.

## Health checks

Always include one when the image supports it (HTTP `curl -f`, or `pg_isready` / `redis-cli
ping` for datastores). Docker and Homepage can then show real health, and you can verify a
deploy instead of guessing.

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
- **Running as root in-container** — use non-root images or `user:` where supported.
- **No memory limit** — one leak can take down the whole box. Set a limit on every service.
- **Mounting the Docker socket without reason** — it's root-equivalent host access. Only
  tools that truly need it (e.g. Homepage, read-only) should get it, mounted `:ro`.
