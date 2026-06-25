# Changelog — vps-architect (Stack Sovereignty class plugin)

All notable changes per course week. Semver: each course week that adds a *taught* skill
bumps the minor version; fixes bump the patch.

## [0.1.0] — Week 1 (unreleased / validating)

The founding install. Students add this plugin to Claude Code on their box in Week 1.

### Added
- **`vps-architecture`** — the **core** skill: the opinionated architecture + stewardship
  judgment for building and running the owner's box well (compose conventions, the `/srv`
  file tree, Caddy/networking, hardening tiers, nightly backups, document-as-you-deploy, the
  add/update/remove service lifecycle, the v5 course-stack defaults) with on-demand
  `references/` and `templates/`. Forked v5-clean from Optevo's production `vps-architect`
  skill (that box's copy left untouched) and tuned for a non-technical owner whose agent does
  the work — teach-as-you-go, subagent-driven, direct-and-verify. Research-hardened against
  current (2025–26) practice: daemon-wide log rotation (disk-fill protection), `cpus` limits +
  RAM-headroom rule, HTTP-01 per-subdomain certs as default (stock `caddy:2-alpine` can't
  issue wildcard certs) + reusable security-headers snippet, SSH lockout-safety protocol +
  `ed25519` + unattended auto-reboot, per-app embedded-DB backup safety + Kopia offsite, and a
  read-only Docker socket proxy (`11notes/docker-socket-proxy`, MIT) as default. External
  skills surveyed — none worth vendoring (Anthropic repos license-unshippable; permissive ones
  thin); our manifest discipline is ahead of the public skills.
- **`self-hosted-infrastructure-landscape`** — the Week-1 *taught* skill. The curated map of
  what a non-technical operator can run and what it replaces: the base stack, the
  grows-weekly map (so the agent never installs later-week services early), the
  install-your-own catalog (weekend-project fuel), and a beginner decision frame.
- **`vikunja-assign-to-human`** *(operational)* — lets the on-box agent hand the user a clear,
  do-able task on their Vikunja board when it's blocked. Powers the Week-1 SSH-key task.
- **`grill-me-interview`** *(operational)* — a short "grill me" interview the agent runs at
  first boot to learn the operator's frame and store an owned profile.

> Note on scope: the *taught* skill-per-week story counts `self-hosted-infrastructure-landscape`
> as skill #1. The two operational skills are bundled in Week 1 because the weekend bootstrap
> depends on them. Decision pending Derek: keep operational skills in this plugin vs. install
> them via the bootstrap onto the box.

### Pending validation (before cohort)
- End-to-end install on a clean droplet: `marketplace add` → `install` → `/reload` → all
  three skills load and trigger correctly.
- `grill-me-interview`: confirm Matt Pocock attribution + refine questions.
- `vikunja-assign-to-human`: confirm against the live Vikunja instance the bootstrap creates
  (MCP vs. REST token path).
