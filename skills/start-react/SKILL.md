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

Resolve the target repo with minimal back-and-forth:

- If the user specified a repo by name, look it up in **`memory/referenced-repos.json`** and run in that path.
- If the user did **not** specify a repo, default to `memory/last-working-repos.json:lastActive` if present.
- Only ask which repo if neither is available.

### 1. Start the React dev server

From the project root:

```bash
npm start
```

- Runs in the **background** (do not block the terminal with a long-running process in the foreground unless the user prefers it).
- App is available at [http://localhost:3000](http://localhost:3000).

### 2. Optional: open the app / GitHub (only if asked)

Only open Safari/GitHub Desktop if the user explicitly asked to “open” them.

## Summary

1. Run `npm start` in the background.
2. If requested: open `http://localhost:3000` and/or the repo URL.
