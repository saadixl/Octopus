---
name: deploy
description: Build and deploy a project. Triggers on "deploy", "build and deploy", "ship", "publish".
---

# Deploy

## Trigger phrases

- "deploy **ProjectName**", "build and deploy", "ship", "publish"

## Steps

1. **Resolve target project** from `local/repos.json`.
2. **Build** — run the project's build command (`npm run build` or equivalent).
   - If it fails, fix errors and retry.
3. **Deploy** — detect the deploy target from project files:
   - `firebase.json` → `npx firebase deploy --only hosting`
   - `vercel.json` or `.vercel/` → `npx vercel --prod`
   - `netlify.toml` → `npx netlify deploy --prod`
   - `fly.toml` → `fly deploy`
   - If unclear, ask the user.
4. **Confirm** with the deployed URL if available.
