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
| **vps-architect** | "Your primitives, packaged." The curated self-hosted infrastructure landscape (what you can run + what it replaces) plus the operational skills your on-box agent uses to set up and run your box. Grows one skill per course week. |

## Layout

```
claw-marketplace/
├── .claude-plugin/marketplace.json     # lists the plugin(s)
└── plugins/
    └── vps-architect/
        ├── .claude-plugin/plugin.json  # name, version (semver)
        ├── skills/
        │   ├── self-hosted-infrastructure-landscape/   # Week-1 taught skill
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
