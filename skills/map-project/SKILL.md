---
name: map-project
description: Create (or reuse) a saved project summary under memory/project/<ProjectName>/ so Cursor doesn't re-summarize the same repo every time. Runs when the user says map project, summarize project, project summary, or when use-octopus registers a repo that has no summary yet.
---

# Map Project (save a reusable summary)

This skill creates a **persistent, local** project summary in this Octopus repo under:

- `memory/project/<ProjectName>/summary.md`

That summary is **reused on future runs**, so you don’t have to re-explore the repo every time.

## When to use

- User says: **map project**, **summarize project**, **project summary**
- Or as a follow-up step after `use-octopus` registers a repo **and** it has no existing summary yet

## Inputs you need

- **Project name** (matches the key in `memory/referenced-repos.json`)
- **Project path** (from `memory/referenced-repos.json`)

If the project name isn’t provided, ask for it.

## Storage rules

1. Ensure `memory/project/` exists in Octopus.
2. Ensure `memory/project/<ProjectName>/` exists (create it).
3. Summary lives at `memory/project/<ProjectName>/summary.md`.
4. If `summary.md` exists already:
   - Read it
   - Provide a short recap to the user
   - Stop (do not regenerate unless the user explicitly asks to refresh)

## How to generate the summary (first time only)

Work efficiently and avoid deep scans.

### 1) High-level structure

- List the repo root (top level)
- If there are obvious app roots, list them too (e.g. `webapp/`, `frontend/`, `backend/`, `apps/`, `packages/`)

### 2) Identify the stack and entry points

Prefer reading (when present):

- `README.md`
- `package.json` (and `pnpm-lock.yaml` / `yarn.lock` / `package-lock.json`)
- Framework config files if present:
  - React/Vite/Next: `vite.config.*`, `next.config.*`, `craco.config.*`
  - TS: `tsconfig.json`
  - Lint/format: `.eslintrc*`, `eslint.config.*`, `.prettierrc*`
  - Backend: `Dockerfile`, `docker-compose.yml`, `requirements.txt`, `pyproject.toml`, `go.mod`, etc.

Capture:

- Primary frameworks/libraries
- Dev/start commands
- Build/deploy commands
- Any env var requirements (only names, never secrets)

### 3) Feature map (lightweight)

Use quick targeted searches to understand how the product works:

- Routing (e.g. `react-router`, Next routes, backend routers)
- Data layer (API client, Firebase, ORM, DB)
- Key “screens”/modules and how they connect

Aim for **“how the app works”**, not exhaustive file-by-file notes.

### 4) Output format

Write `memory/project/<ProjectName>/summary.md` with sections:

- **Overview** (what the project is)
- **Tech stack**
- **Repo structure** (top-level tree + 1 level deeper for key dirs)
- **How it runs** (dev/build/test)
- **Core flows / features** (3–8 bullets)
- **Configuration** (env var names, config files)
- **Notes / gotchas** (anything important discovered)

Keep it concise, but actionable.

## After writing the summary

- Tell the user where it was saved.
- On subsequent `use-octopus` for this project, the agent should read this file instead of re-summarizing.

