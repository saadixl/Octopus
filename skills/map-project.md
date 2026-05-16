---
name: map-project
description: Generate (or reuse) a cached project summary at local/maps/<ProjectName>.md. Triggers on "map project", "summarize project", or auto-runs after add-project when no map exists.
---

# Map project

Create a lightweight, reusable summary of a project so the agent doesn't re-explore it every session.

## Trigger phrases

- "map project", "summarize project", "refresh map for **X**"
- Auto-triggered by add-project when no map exists

## Storage

- Summaries live at `local/maps/<ProjectName>.md`
- If the file already exists, read it and give a short recap. **Do not regenerate** unless the user says "refresh".

## How to generate (first time or refresh)

Work fast — avoid deep scans.

1. **Read key files** in the target repo:
   - `README.md`, `package.json` (or `pyproject.toml`, `go.mod`, etc.)
   - Framework configs: `vite.config.*`, `next.config.*`, `tsconfig.json`, `docker-compose.yml`, `firebase.json`
2. **List top-level structure** + 1 level deep for key directories.
3. **Write `local/maps/<ProjectName>.md`** with these sections:
   - **Overview** — what the project is (1-2 sentences)
   - **Tech stack** — frameworks, libraries, language
   - **Structure** — top-level tree
   - **How to run** — dev, build, test commands
   - **Key features** — 3-8 bullets
   - **Config** — env var names (never values), config files
   - **Notes** — gotchas, quirks

Keep it under ~80 lines. No secrets. No large file dumps.
