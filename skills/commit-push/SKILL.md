---
name: commit-push
description: Commits changes to git and pushes to the remote (GitHub). Use when the user asks to commit, push, commit and push, save to GitHub, or push to GitHub.
---

# Commit and Push to GitHub

## When to Use

- User asks to **commit** (with or without "and push")
- User asks to **push** to GitHub
- User asks to save changes to GitHub or push changes
- User says "commit and push" or "push my changes"

## Workflow

### 0. Target repo (when using from Octopus)

Resolve the target repo with minimal back-and-forth:

- If the user specified a repo by name, look it up in **`memory/referenced-repos.json`** and run in that path.
- If the user did **not** specify a repo, default to `memory/last-working-repos.json:lastActive` if present.
- Only ask which repo if neither is available.

### 1. Check and fix ESLint errors first

**Before committing, ensure there are no lint errors.** Run the build (which runs ESLint in this project):

```bash
npm run build
```

- If the build **fails** due to ESLint (e.g. unused variables, missing deps): fix those errors in the code, then run `npm run build` again until it passes.
- Only proceed to commit once the build succeeds (or there are no lint errors).
- If the user only said "push" and did not ask to commit, you can skip this step.

### 2. Check status

```bash
git status
```

- See what's modified, staged, or untracked.
- If there's nothing to commit, say so and stop.

### 3. Stage changes

- **All changes:** `git add .`
- **Specific paths:** `git add <path1> <path2>`
- **Previously discussed files:** Stage only what the user asked to commit.

Avoid committing build artifacts, env files with secrets, or `node_modules` (ensure they're in `.gitignore`).

### 4. Commit

**Generate the commit message from the changes.** Do not ask the user for a message. Inspect what was changed (e.g. `git diff --staged` or the files you modified in this session) and write a short, clear message that describes the change (e.g. "Add prayer times city selector", "Fix lint in Home.js", "Update start-dev skill to open Safari"). Only use a user-provided message if they explicitly gave one.

```bash
git commit -m "Your generated message here"
```

- Use `-m` with a single, concise message (imperative mood: "Add …", "Fix …", "Update …").
- No force flags; don't amend or rewrite history unless the user asks.

### 5. Push

**Always push after committing.** Do not ask; run `git push` as part of the workflow.

```bash
git push
```

- Default is `git push origin <current-branch>`.
- **Do not** use `--force` (or `-f`) unless the user explicitly asks.
- If push fails (e.g. rejected, or need to set upstream), fix the issue or tell the user what to run (e.g. `git push -u origin <branch>`).

## Summary

1. **Run `npm run build`** → fix any ESLint (or build) errors before committing.
2. `git status` → confirm there's something to commit.
3. `git add .` (or specific paths).
4. `git commit -m "<message>"` — **generate the message from the changes** (do not ask the user).
5. **Always run `git push`** after committing (no force unless requested).

## Notes

- **Commit always includes push:** Whenever you commit, push immediately after. Do not ask if the user wants to push.
- If the user only said "push" (no commit), assume commits are already done; run `git push` (and run `git status` first if you need to check).
