# Optevo Claw — Stack Sovereignty marketplace

The versioned home for the **Stack Sovereignty** course plugins. Students install from
here in Week 1 and update each week as new capability accretes.

> Stop renting your software. Start owning your stack.

## Install (what students run)

```bash
# in Claude Code on your VPS
/plugins
# add this marketplace, then install the plugin:
claude plugin marketplace add OptevoLLC/claw-marketplace
claude plugin install vps-architect@stack-sovereignty
# then:
/reload
```

## What's here

| Plugin | What it gives your agent |
|---|---|
| **vps-architect** | "Your primitives, packaged." The architecture + stewardship judgment to build and run your box well (`vps-architecture`), the curated infrastructure landscape of what you can run and what it replaces (`self-hosted-infrastructure-landscape`), plus the operational skills your agent uses to hand you a task when blocked and to interview you so it works in your context. Grows one skill per course week. |

## Layout

```
claw-marketplace/
├── .claude-plugin/marketplace.json     # lists the plugin(s)
└── plugins/
    └── vps-architect/
        ├── .claude-plugin/plugin.json  # name, version (semver)
        ├── skills/
        │   ├── vps-architecture/                       # CORE: architecture + stewardship
        │   │   ├── references/  (docker, filesystem, networking, security-and-ops)
        │   │   └── templates/   (service-manifest, inventory)
        │   ├── self-hosted-infrastructure-landscape/   # Week-1 taught skill (what to run)
        │   ├── vikunja-assign-to-human/                # operational (D5)
        │   └── grill-me-interview/                     # operational (D11)
        └── CHANGELOG.md
```

## Releasing

Each course week that adds a taught skill: bump `plugin.json` version + `marketplace.json`,
add a CHANGELOG entry, then tag:

```bash
claude plugin tag plugins/vps-architect    # creates vps-architect--vX.Y.Z, validates manifests agree
```

Students pick up updates with `claude plugin update vps-architect`.

## Status

**Validating** — v0.1.0 is being proven end-to-end on a throwaway droplet before the
founding cohort. See the course build session for the validation checklist.
