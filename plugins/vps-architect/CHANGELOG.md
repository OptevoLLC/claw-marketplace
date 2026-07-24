# Changelog — vps-architect (Stack Sovereignty class plugin)

All notable changes per course week. Semver: each course week that adds a *taught* skill
bumps the minor version; fixes bump the patch.

## [0.2.3] — reality governs box state, not shipped files (2026-07-24)

`base-stack.md` removed: a static reference may not claim what's running on
the box — that's the inventory's job (`/srv/_docs/inventory.md`, verified by
`docker ps`). The landscape skill now routes by where truth lives: box state
→ reality; what-an-app-is/replaces → `catalog.md` (which absorbed the base
stack's durable "what it replaces" rows, explicitly framed as
bootstrap-ships-these, inventory-decides-if-present); install timing →
`weekly-map.md`, now with reality-outranks-the-map ordering (already
installed = already arrived; current week from `~/course/current` when
present; post-course, the map is history). Makes the skill durable beyond
the class artifact.

## [0.2.2] — landscape restructured: behavior vs. data (2026-07-24)

`self-hosted-infrastructure-landscape` split along the skill-vs-reference
line: SKILL.md now carries only *behavior* (how to answer, the
check-the-weekly-map-first rule, the decision frame) and the volatile data
moved to `references/` — `base-stack.md`, `weekly-map.md`, `catalog.md` —
with read-the-file-don't-answer-from-memory guidance. Per-cohort updates
(new week map, catalog additions, the portal `oss-cheatsheet` sync) are now
data-file edits that never touch behavior. `vps-architecture`'s lifecycle
step 1 points directly at the weekly-map reference.

## [0.2.1] — best-practices pass (2026-07-24)

Skill-creator audit across all six skills. Fixed: manifest version brought in
line with this changelog (plugin.json had stayed at 0.1.0 through 0.2.0);
`grill-me-interview`'s shipped "validate before shipping" source note resolved
with verified attribution (mattpocock/skills publishes `grill-me`/`grilling` —
confirmed 2026-07-24, closing that 0.1.0 pending-validation item);
`self-hosted-infrastructure-landscape`'s cross-reference to the build skill
corrected (`vps-architect` → `vps-architecture`) and its maintainer-facing
oss-cheatsheet sync note moved here (authors: keep the catalog in sync with the
portal's `oss-cheatsheet` — same source list, two faces, must not drift);
`vps-architecture`'s ~150-word Vikunja table cell unpacked into a "Vikunja
specifics" subsection and its description trimmed. No behavior changes.

## [0.2.0] — Week 2 (draft / pre-validation)

The "My Agent & Me" release. Handed off at the end of Class 2: Claude Code becomes the
student's **Hermes IT admin**, so Hermes operation after the first wizard run is a plain-
language conversation, never a terminal task.

### Added
- **`hermes-admin`** — the Week-2 *taught* skill (skill #2). Manage, configure, and
  optimize the Hermes agent + its profiles on the owner's box: health/troubleshooting
  (`status`/`doctor`/gateway/logs triage), settings + per-profile model right-sizing,
  profile lifecycle (the fleet: create/clone/rename/export), persona & briefing tuning,
  skills install + **class-skill mirroring into Hermes** (proposed mechanism for
  COURSE-BRIEF open item #6), memory inspection/correction (owned, readable files),
  gateway setup/repair (token steps arrive as Vikunja human-tasks), updates & backups.
  Hard rules: direct-and-verify after every change, never echo secrets, backup before
  change, plain-English three-line reporting for a non-technical owner.
  Includes **`references/agent-sources.md`** — the verified "where the pros publish
  their agents" shopping map (official + community collections + the Hermes hub, links
  verified 2026-07-06) and the fetch → vet-as-untrusted → interview-the-candidate →
  tailor → verify workflow. Powers the Class-2 "never hire from scratch" move and the
  weekend hire-#2 project. Re-verify links each cohort.

- **`course-briefings`** *(operational)* — the P-AGENTDROP convention (see
  `PORTAL-EXPERIENCE.md` §4.3): teaches the on-box agent to read the `~/course/`
  room where the portal publishes weekly drops (recap · personal recap · project ·
  prompts · shelf), to lead with the owner's personal recap as context, to keep
  Hermes profiles in the loop, and to treat missing drops as facts rather than gaps
  to fill. Makes "the portal feeds your staff" real on the student side; the
  portal-side publisher is build item P-AGENTDROP.

### Pending validation (before Class 2)
- Run the full admin loop against a real student-shaped install (Docker layout): detect
  install → profile create/clone → persona edit → skill mirror + `/reload-skills` →
  gateway repair → memory summary.
- `course-briefings`: publish one real drop end-to-end (portal → `~/course/` → Vikunja
  announce → agent answers "what's my project this week?" correctly).
- Confirm the Hermes→Claude Code delegation path used in the class demo (movement 4/4).
- Derek: bless skill-mirroring as the open-item-#6 mechanism; confirm student install
  surface (Docker vs. native) and demo gateway (recommend cohort Discord).

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
