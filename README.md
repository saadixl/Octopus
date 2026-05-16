# Octopus

Manage all your projects from one repo using AI coding assistants. Open Octopus in **Cursor** or **Claude Code**, and run skills (commit, deploy, start dev server, etc.) against any registered project — without copying config into each one.

```
"commit and push MyApp"
"deploy Backend"
"start react for Dashboard"
```

The agent resolves the project path, reads cached context, and runs everything in the right directory.

## Why

- **One hub** — keep your AI assistant rules and skills in one place, use them everywhere.
- **Token-efficient** — project summaries are cached locally so the agent doesn't re-explore repos every session.
- **Shareable** — all personal data lives in `local/` (gitignored). The skills and rules are safe to share publicly.

## Quick start

### 1. Clone

```bash
git clone https://github.com/your-username/Octopus.git
cd Octopus
```

### 2. Create your local config

```bash
mkdir -p local/maps
cp examples/repos.json local/repos.json
cp examples/preferences.json local/preferences.json
```

Edit `local/repos.json` with your actual project paths:

```json
{
  "lastActive": "my-app",
  "projects": {
    "my-app": {
      "path": "/Users/you/Code/my-app",
      "added": "2025-01-01"
    }
  }
}
```

### 3. Open in your AI editor

**Cursor:** Open this folder. Rules in `.cursor/rules/` and skills in `skills/` are picked up automatically.

**Claude Code:** Open this folder. Instructions in `CLAUDE.md` are picked up automatically. Tell it to read the skills in `skills/` on first use.

### 4. Start using it

```
"add project MyApp"          → registers a project
"map project MyApp"          → generates a cached summary
"commit and push MyApp"      → stages, commits, pushes
"deploy MyApp"               → builds and deploys
"start MyApp"                → starts the dev server
"start docker for MyApp"     → docker compose up
```

If you skip the project name, Octopus uses the last active project.

## Repo structure

```
Octopus/
├── CLAUDE.md                    # Claude Code instructions (auto-loaded)
├── .cursor/rules/
│   └── octopus-core.mdc        # Cursor rules (auto-loaded)
├── skills/
│   ├── add-project.md          # Register a project
│   ├── map-project.md          # Cache a project summary
│   ├── commit-push.md          # Git commit and push
│   ├── deploy.md               # Build and deploy (Firebase/Vercel/Netlify)
│   ├── dev-server.md           # Start dev server
│   └── docker.md               # Docker Compose
├── examples/
│   ├── repos.json              # Example project registry
│   └── preferences.json        # Example preferences
└── local/                      # ← gitignored, your personal data
    ├── repos.json              # Your project registry
    ├── preferences.json        # Your response style preferences
    └── maps/
        └── MyProject.md        # Cached project summaries
```

## Skills reference

| Skill | Trigger phrases | What it does |
|-------|----------------|--------------|
| **add-project** | "add project", "register project", "use octopus" | Finds a repo on disk, saves its path, auto-generates a summary |
| **map-project** | "map project", "summarize project" | Creates a cached summary so the agent has instant context |
| **commit-push** | "commit", "push", "commit and push" | Builds, stages, commits with auto-generated message, pushes |
| **deploy** | "deploy", "ship", "publish" | Builds and deploys (auto-detects Firebase/Vercel/Netlify) |
| **dev-server** | "start", "dev", "run" | Starts the dev server (auto-detects npm start/dev/docker) |
| **docker** | "docker", "start docker", "start containers" | Starts Docker Compose services |

## How token caching works

When you first register a project, Octopus scans it lightly (README, package.json, folder structure) and writes a summary to `local/maps/<ProjectName>.md`. On every future session, the agent reads this cached file instead of re-exploring the repo — saving significant tokens.

To refresh a stale summary: `"refresh map for MyApp"`

## Customization

### Add your own skills

Create a new `.md` file in `skills/` with this format:

```markdown
---
name: my-skill
description: What it does and when to trigger it.
---

# My skill

## Trigger phrases
- "do the thing"

## Steps
1. Step one
2. Step two
```

Both Cursor and Claude Code will pick it up.

### Preferences

Edit `local/preferences.json`:

```json
{
  "verbosity": "short",    // tiny | short | normal | detailed
  "preferBullets": true
}
```

## Privacy

Everything in `local/` is gitignored:
- Your project paths stay on your machine
- Cached summaries stay on your machine
- No secrets or personal data are ever committed

The committed files (skills, rules, examples, README) contain no personal information and are safe to share publicly.
