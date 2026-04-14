---
name: firebase-build-deploy
description: For this project, deploy means deploying to Firebase Hosting. Builds the React app and deploys to Firebase. Use when the user asks to deploy, build and deploy, publish, ship, or release the project, or when setting up or troubleshooting deployment.
---

# Build and Deploy (Firebase)

**For this project, "deploy" means deploy to Firebase Hosting.** There is no other deploy target.

## When to Use

- User asks to **deploy** (interpret as Firebase)
- User asks to build and deploy, publish, ship, or release
- User asks to fix or run deployment
- User mentions Firebase deployment or hosting

## Prerequisites

- Firebase CLI installed: `npm install -g firebase-tools` (or use `npx firebase`)
- User logged in: `firebase login` (agent may need to prompt if not logged in)
- Project uses `firebase.json` with `hosting.public: "build"` and SPA rewrites

## Workflow

### 0. Target repo (when using from Octopus)

Resolve the target repo with minimal back-and-forth:

- If the user specified a repo by name, look it up in **`memory/referenced-repos.json`** and run in that path.
- If the user did **not** specify a repo, default to `memory/last-working-repos.json:lastActive` if present.
- Only ask which repo if neither is available.

### 1. Build the app

From the project root:

```bash
npm run build
```

- Output goes to the `build/` folder (used by Firebase Hosting).
- If the build fails, fix lint/compile errors before deploying.

### 2. Deploy to Firebase Hosting

```bash
firebase deploy
```

Or deploy only hosting:

```bash
firebase deploy --only hosting
```

- First run may require `firebase use <project-id>` if the project isn't linked; check `.firebaserc` or ask the user for the project ID.
- Do not commit `.firebaserc` if it contains sensitive data; it's normally safe to commit project ID.

### 3. Optional: one-shot with npx

If Firebase CLI might not be installed globally:

```bash
npm run build && npx firebase deploy --only hosting
```

## Project-specific config

- **Build output**: `build/` (Create React App default)
- **Hosting**: `firebase.json` points `hosting.public` to `build`, with rewrites so `**` → `/index.html` (SPA)
- **Database**: If `database.rules` are in `firebase.json`, `firebase deploy` will deploy rules too; use `--only hosting` to deploy only the site

## Common issues

| Issue | Action |
|-------|--------|
| Build fails (e.g. lint) | Fix reported errors; often `process.env.CI` causes lint-as-error in build |
| Not logged in | Run `firebase login` (browser); agent cannot complete login for the user |
| No project selected | Run `firebase use <project-id>` or `firebase use --add` to link a project |
| 404 on refresh | Confirm `firebase.json` has the `rewrites` rule sending `**` to `/index.html` |

## Checklist

- [ ] `npm run build` succeeds
- [ ] Firebase CLI available (`firebase` or `npx firebase`)
- [ ] User is logged in and project is selected (if not, prompt or document)
- [ ] Run `firebase deploy` or `firebase deploy --only hosting`
