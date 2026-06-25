# Service Manifest: [SERVICE NAME]

## Overview
- **Purpose:** [What this does and why it's on the box — in plain words]
- **Image:** [Docker image + version, e.g. vikunja/vikunja:latest]
- **Project:** [Upstream URL]
- **Deployed:** [Date] · **Last updated:** [Date]

## Access
- **URL:** [e.g. tasks.example.com]
- **Internal:** [container_name:port on srv-net]
- **Auth:** [How the owner logs in — first-login account, etc. Never paste secret values.]

## Configuration
- **Compose:** /srv/[service]/docker-compose.yml
- **Data:** /srv/[service]/data/
- **Env:** /srv/[service]/.env  (secrets — referenced, never printed)
- **Notable settings:** [Anything non-default worth knowing]

## Dependencies
- **Requires:** [srv-net, and any other service]
- **Required by:** [Anything that depends on this]

## Backup
- **What:** [Paths/volumes that matter]
- **Covered by:** [Nightly /srv tar · DB dump · etc.]

## Notes
[Gotchas, known issues, planned changes]
