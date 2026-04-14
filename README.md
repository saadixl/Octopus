# Octopus

Cursor Agent skills you can reuse across projects. Each skill is a `SKILL.md` file that tells the AI when and how to perform a task.

**Works with:**

- ✅ Cursor
- ✅ Claude Code

In Cursor, skills are picked up automatically. In Claude Code, tell it once (e.g. “use the skills in this repo” or point it at the skill files); it will follow the same instructions.

## Repo structure

```
Octopus/
├── README.md
├── .gitignore
├── skills/
│   ├── use-octopus/
│   │   └── SKILL.md
│   ├── commit-push/
│   │   └── SKILL.md
│   ├── start-react/
│   │   └── SKILL.md
│   ├── start-docker-compose/
│   │   └── SKILL.md
│   └── firebase-build-deploy/
│       └── SKILL.md
└── memory/
    ├── assistant-preferences.json      # Optional: response verbosity/preferences for the agent
    ├── referenced-repos.example.json  # Example format; copy to referenced-repos.json to use
    └── referenced-repos.json          # Your registered repos (gitignored, not committed)
```

## Skills

| Skill | What it does |
|-------|----------------|
| **use-octopus** | Register a project repo by name. You say the project name; the agent finds it and stores the path in `memory/referenced-repos.json` (gitignored). No need to copy skills into that repo. |
| **map-project** | Create (or reuse) a saved project summary under `memory/project/<ProjectName>/summary.md` so the agent doesn’t re-summarize the repo every time. |
| **commit-push** | Run `npm run build`, then `git add`, `git commit` (message from changes), and `git push`. |
| **start-react** | Start the React dev server (`npm start`), open the app in Safari, and open GitHub (Desktop or repo in browser). |
| **start-docker-compose** | Start the Docker Compose stack (`docker compose up -d` or foreground). |
| **firebase-build-deploy** | Run `npm run build` and `firebase deploy` (or `--only hosting`) for Firebase Hosting. |

## Recommended: use skills from this repo (no copy)

**Open Cursor in this repo (Octopus).** You stay here; the agent runs commands in your other repos using stored paths.

### 1. Register a repo (one-time per repo)

- Say: **“use octopus”** (or “use skills” / “use skill” / “register repo”).
- When asked, give the **repo name** (e.g. your project folder name).
- The agent finds it and adds it to **`memory/referenced-repos.json`**. That file is in `.gitignore`, so your paths are never committed.

If you don’t have `memory/referenced-repos.json` yet, copy from the example:  
`cp memory/referenced-repos.example.json memory/referenced-repos.json`  
(or the agent will create it when you register your first repo).

### 2. Run skills by repo name

Say what you want and **include the repo name**:

| You say | Skill used |
|--------|------------|
| “Commit and push **MyProject**” | commit-push (in that repo’s path) |
| “Start React for **MyProject**” | start-react (in that repo’s path) |
| “Start Docker Compose for **MyProject**” | start-docker-compose (in that repo’s path) |
| “Deploy **MyProject**” | firebase-build-deploy (in that repo’s path) |

Registered repos and their paths are in **`memory/referenced-repos.json`** (gitignored).

## Project memory: `memory/project/`

Octopus can keep **project-specific notes** (generated once, reused later) under:

- `memory/project/<ProjectName>/summary.md`

### How it’s used

- **First time** you “use octopus with <ProjectName>” (or you run “map project <ProjectName>”):
  - Octopus scans the repo (lightweight: README, package.json, key folders)
  - Writes a reusable summary to `memory/project/<ProjectName>/summary.md`
- **Next time**:
  - Octopus reads the existing summary instead of re-summarizing the repo

### Git / privacy

- `memory/project/*` is **gitignored** so summaries stay **local** and aren’t committed.
- `memory/project/.gitkeep` is kept so the folder exists in the repo.

## Optional: control response length

You can store your preference in `memory/assistant-preferences.json` (gitignored is optional; it’s safe to commit if it contains no secrets).

- **Short responses**: set `"verbosity": "short"`
- **Normal**: set `"verbosity": "normal"`
- **Detailed**: set `"verbosity": "detailed"`

To make the agent apply it, include it in your prompt, e.g.:

- “Follow `memory/assistant-preferences.json`. Start React for MyProject.”

## Optional: remember last active repo

If you want Octopus to default to the repo you worked on last (when you don’t specify a repo name), use:

- `memory/last-working-repos.json` (local; gitignored)
- `memory/last-working-repos.example.json` (template)

## Alternative: copy or symlink skills into a project

If you prefer to open Cursor in each project and have skills there, copy or symlink the skill folders into that project’s `.cursor/skills/` directory.

### Copy

From your project root (replace with your path to Octopus):

```bash
mkdir -p .cursor/skills
cp -r /path/to/Octopus/skills/commit-push .cursor/skills/
cp -r /path/to/Octopus/skills/start-react .cursor/skills/
cp -r /path/to/Octopus/skills/start-docker-compose .cursor/skills/
cp -r /path/to/Octopus/skills/firebase-build-deploy .cursor/skills/
```

### Symlink (updates in Octopus apply everywhere)

```bash
mkdir -p .cursor/skills
ln -s /path/to/Octopus/skills/commit-push .cursor/skills/commit-push
ln -s /path/to/Octopus/skills/start-react .cursor/skills/start-react
ln -s /path/to/Octopus/skills/start-docker-compose .cursor/skills/start-docker-compose
ln -s /path/to/Octopus/skills/firebase-build-deploy .cursor/skills/firebase-build-deploy
```

With copy/symlink, you run skills in the **current** workspace (that project). The “target repo” step in each skill (and `memory/referenced-repos.json`) is only used when you work from the Octopus repo and pass a repo name.

## Customizing

- **start-react**: The skill can open the repo in the browser; use the URL from `git remote -v` in the target repo or edit the skill to point to your GitHub URL.
- **firebase-build-deploy**: Assumes a React app with `build/` and Firebase Hosting; adjust if your project differs.
