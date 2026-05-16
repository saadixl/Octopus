---
name: dev-server
description: Start the development server for a project. Triggers on "start", "dev", "run", "start react", "start dev server".
---

# Start dev server

## Trigger phrases

- "start **ProjectName**", "run **ProjectName**", "dev **ProjectName**"
- "start react", "start dev server"

## Steps

1. **Resolve target project** from `local/repos.json`.
2. **Detect the start command** from the project's `package.json` or cached map:
   - `npm run dev` (Vite, Next.js)
   - `npm start` (CRA, generic)
   - `docker compose up -d` (Docker projects)
   - Or read from the project map if available.
3. **Run in background** — don't block the terminal.
4. **Confirm** with the local URL (e.g. `http://localhost:3000`).

Only open browser/apps if the user explicitly asked.
