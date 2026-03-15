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
├── referenced-repos.example.json   # Example format; copy to referenced-repos.json to use
├── referenced-repos.json          # Your registered repos (gitignored, not committed)
├── use-skills/
│   └── SKILL.md
├── commit-push/
│   └── SKILL.md
├── start-react/
│   └── SKILL.md
├── start-docker-compose/
│   └── SKILL.md
└── firebase-build-deploy/
    └── SKILL.md
```

## Skills

| Skill | What it does |
|-------|----------------|
| **use-skills** | Register a repo by name. You say the repo name; the agent finds it and stores the path in `referenced-repos.json` (gitignored). No need to copy skills into that repo. |
| **commit-push** | Run `npm run build`, then `git add`, `git commit` (message from changes), and `git push`. |
| **start-react** | Start the React dev server (`npm start`), open the app in Safari, and open GitHub (Desktop or repo in browser). |
| **start-docker-compose** | Start the Docker Compose stack (`docker compose up -d` or foreground). |
| **firebase-build-deploy** | Run `npm run build` and `firebase deploy` (or `--only hosting`) for Firebase Hosting. |

## Recommended: use skills from this repo (no copy)

**Open Cursor in this repo (Octopus).** You stay here; the agent runs commands in your other repos using stored paths.

### 1. Register a repo (one-time per repo)

- Say: **“use skills”** (or “use skill” / “register repo”).
- When asked, give the **repo name** (e.g. your project folder name).
- The agent finds it and adds it to **`referenced-repos.json`** at the root of Octopus. That file is in `.gitignore`, so your paths are never committed.

If you don’t have `referenced-repos.json` yet, copy from the example:  
`cp referenced-repos.example.json referenced-repos.json`  
(or the agent will create it when you register your first repo).

### 2. Run skills by repo name

Say what you want and **include the repo name**:

| You say | Skill used |
|--------|------------|
| “Commit and push **MyProject**” | commit-push (in that repo’s path) |
| “Start React for **MyProject**” | start-react (in that repo’s path) |
| “Start Docker Compose for **MyProject**” | start-docker-compose (in that repo’s path) |
| “Deploy **MyProject**” | firebase-build-deploy (in that repo’s path) |

Registered repos and their paths are in **`referenced-repos.json`** at the root of this repo (gitignored).

## Alternative: copy or symlink skills into a project

If you prefer to open Cursor in each project and have skills there, copy or symlink the skill folders into that project’s `.cursor/skills/` directory.

### Copy

From your project root (replace with your path to Octopus):

```bash
mkdir -p .cursor/skills
cp -r /path/to/Octopus/commit-push .cursor/skills/
cp -r /path/to/Octopus/start-react .cursor/skills/
cp -r /path/to/Octopus/start-docker-compose .cursor/skills/
cp -r /path/to/Octopus/firebase-build-deploy .cursor/skills/
```

### Symlink (updates in Octopus apply everywhere)

```bash
mkdir -p .cursor/skills
ln -s /path/to/Octopus/commit-push .cursor/skills/commit-push
ln -s /path/to/Octopus/start-react .cursor/skills/start-react
ln -s /path/to/Octopus/start-docker-compose .cursor/skills/start-docker-compose
ln -s /path/to/Octopus/firebase-build-deploy .cursor/skills/firebase-build-deploy
```

With copy/symlink, you run skills in the **current** workspace (that project). The “target repo” step in each skill (and `referenced-repos.json`) is only used when you work from the Octopus repo and pass a repo name.

## Customizing

- **start-react**: The skill can open the repo in the browser; use the URL from `git remote -v` in the target repo or edit the skill to point to your GitHub URL.
- **firebase-build-deploy**: Assumes a React app with `build/` and Firebase Hosting; adjust if your project differs.
