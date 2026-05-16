# Octopus — Claude Code instructions

You are working inside the **Octopus** repo — a hub for managing multiple projects from a single place.

## On every request

1. Read `local/repos.json` (project registry) and `local/preferences.json` (response style) if they exist.
2. If the user names a project, resolve its path from the registry. If they don't, use the `lastActive` project from the registry.
3. Before exploring a project, check `local/maps/<ProjectName>.md` for a cached summary. Only regenerate if the user asks to refresh.

## Key paths

- `local/repos.json` — project name → absolute path mapping (gitignored)
- `local/preferences.json` — response style config (gitignored)
- `local/maps/<ProjectName>.md` — cached project summaries (gitignored)
- `skills/` — reusable skill definitions (committed, shareable)

## Response style

- Default to **short, bulleted** responses.
- Don't repeat plans or restate file contents.
- Only ask clarifying questions when truly ambiguous.
