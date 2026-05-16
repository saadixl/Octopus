---
name: commit-push
description: Stage, commit, and push changes in a project. Triggers on "commit", "push", "commit and push", "save to git".
---

# Commit and push

## Trigger phrases

- "commit", "push", "commit and push **ProjectName**"
- "save to git", "push changes"

## Steps

1. **Resolve target project** from `local/repos.json` (by name, or `lastActive`).
2. **Run build/lint** if the project has a build script (`npm run build`). Fix errors before committing.
3. **`git status`** — if nothing to commit, say so and stop.
4. **`git add .`** (or specific files if the user specified).
5. **`git commit -m "<message>"`** — generate the message from the diff. Don't ask the user for one.
6. **`git push`** — always push after committing. No `--force` unless explicitly asked.

If push fails (no upstream), run `git push -u origin <branch>`.
