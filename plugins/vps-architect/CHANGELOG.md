# Changelog — vps-architect (Stack Sovereignty class plugin)

All notable changes per course week. Semver: each course week that adds a *taught* skill
bumps the minor version; fixes bump the patch.

## [0.1.0] — Week 1 (unreleased / validating)

The founding install. Students add this plugin to Claude Code on their box in Week 1.

### Added
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
