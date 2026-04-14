---
name: start-react
description: Starts the React development server and opens the app and GitHub. Use when the user asks to start React, start the React app, run the app, open development environment, or open GitHub.
---

# Start React

## When to Use

- User asks to start React, start the React app, or run the app locally
- User asks to open the development environment (for a React project)
- User asks to start the React dev server and open GitHub (or GitHub app)

## Workflow

### 0. Target repo (when using from Octopus)

If the user specified a repo by name, look up that name in **`memory/referenced-repos.json`** in this workspace root. If found, run all following steps from that repo’s path (e.g. `cd <path>` then run commands there). Otherwise use the current workspace.

### 1. Start the React dev server

From the project root:

```bash
npm start
```

- Runs in the **background** (do not block the terminal with a long-running process in the foreground unless the user prefers it).
- App is available at [http://localhost:3000](http://localhost:3000).

### 2. Open the app in Safari

After starting the dev server, open the URL in Safari so the user doesn't have to do it manually. Run this in the background (or after a short delay so the server is ready):

```bash
sleep 10 && open -a Safari http://localhost:3000
```

So: start the dev server in the background, then run the command above (also in the background) so Safari opens once the server is up.

### 3. Open GitHub

**Option A – GitHub Desktop (macOS):** Use the full path so the app reliably opens. Request **full permissions** when running:

```bash
open "/Applications/GitHub Desktop.app"
```

If the user doesn't have GitHub Desktop or it still doesn't open, use Option B.

**Option B – Repo on GitHub in Safari:** Open the repo in the browser (good fallback if Desktop doesn't open). Get the URL from `git remote -v` in the target repo and open it, e.g.:

```bash
open -a Safari "https://github.com/username/repo"
```

**Important:** Run these `open` commands with permissions that allow launching apps (e.g. `all`), otherwise they may not work.

**Windows:** Start-Process or the path to GitHub Desktop; or open the repo URL in the default browser.

### 4. Optional: open Safari manually if it didn't open

If Safari still didn't open, run:

```bash
open -a Safari http://localhost:3000
```

## Summary

1. Run `npm start` in the background.
2. Run `sleep 10 && open -a Safari http://localhost:3000` (in the background) so Safari opens when the server is ready.
3. Open GitHub: run `open "/Applications/GitHub Desktop.app"` (full path; use full permissions). If that doesn't open, run `open -a Safari "<repo-url>"` using the URL from `git remote -v` in the target repo.
