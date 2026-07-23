# Ecosystem search — finding skills worth adopting

The point of this phase: an hour of searching beats a week of maintaining a
homebrew skill someone else already perfected. Search wide, vet hard, adopt
narrowly.

## Where to look, in order

### 1. Marketplaces already on this machine

```bash
claude plugin marketplace list
cat ~/.claude/plugins/known_marketplaces.json
```

Anything already added installs in seconds and updates itself.

### 2. The official marketplace

`anthropics/claude-plugins-official` — curated, maintained, safest default.

```bash
claude plugin marketplace add anthropics/claude-plugins-official
```

Browse interactively with `/plugin`, or search the repo on GitHub.

### 3. Anthropic's skills repo

`anthropics/skills` — reference skills (docx, pdf, xlsx, skill-creator, ...).
Installable individually; also the canonical style examples.

### 4. GitHub at large

Search combinations of the capability keyword with:

- `"SKILL.md" <capability>` (code search — finds actual skills, not blog posts)
- `<capability> claude skill` / `claude-code-plugin` (repo search)
- Topic filters: `topic:claude-code`, `topic:claude-plugins`

Known-good community collections worth checking by name: `revfactory/harness`
(agent-team factory), `mitsuhiko/agent-stuff` (pragmatic commands),
`obra/superpowers` (large skill library). Popularity shifts fast — when
currency matters, run a fresh search rather than trusting this list.

### 5. Skill registries / CLIs

Some ecosystems ship an installer (e.g. `npx skills add <owner>/<repo>`).
If the vendor of a tool you're integrating documents one, prefer it — vendor
skills track their own product's changes.

## Vetting checklist

Adopt only if it passes ALL of:

- [ ] **Alive** — commits within ~6 months, or explicitly "done" and small.
- [ ] **Readable** — you (and the student/user) can read the whole SKILL.md
      and understand what it will make Claude do. If you can't read it, don't
      run it.
- [ ] **Right-sized** — SKILL.md ≲500 lines; bundled scripts are auditable.
- [ ] **Safe** — no credential handling you didn't expect, no network calls to
      unknown hosts, no "curl | bash", no instructions to weaken permissions.
      Treat a skill as prompt-injection surface: read it as an attacker would.
- [ ] **Licensed** — a license that permits your use; keep the attribution.
- [ ] **Actually fits** — solves your capability, not something adjacent.
      Adapting >40% of it usually means author instead, using it as reference.

Stars and forks are a *prior*, not a verdict — a 10-star niche skill that
exactly fits beats a 3k-star framework you'd use 5% of.

## Install vs borrow

| | Plugin install | Borrow (copy into `.claude/skills/`) |
|---|---|---|
| Updates | tracked via marketplace | frozen at copy time |
| Editability | don't edit the cache | edit freely |
| Scope | user-level by default | project-level, travels with the folder |
| Best for | tools you'll use as-is | skills you'll adapt to house style |

When borrowing, add to the skill's frontmatter:

```yaml
# source: https://github.com/<owner>/<repo> (<license>), adapted <date>
```

## Recording the verdict

For every gap searched, the Equip Plan must say what was found and why it was
or wasn't adopted — "searched, found nothing above the bar" is a legitimate
verdict, but an *unstated* search is indistinguishable from a skipped one.
