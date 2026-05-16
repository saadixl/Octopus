---
name: add-project
description: Register a project so Octopus can run skills against it. Triggers on "add project", "register project", "use octopus", or "use octopus with <name>".
---

# Add project

Register an external project so all other skills can target it by name.

## Trigger phrases

- "add project", "register project", "use octopus"
- "use octopus with **ProjectName**"

## Steps

1. **Get the project name.** If the user already said it, use it. Otherwise ask once.
2. **Check `local/repos.json`.** If the name already exists, confirm and stop.
3. **Find the repo on disk:**
   - Check sibling directories of this Octopus repo.
   - Try common roots: `~/Code`, `~/Code/Personal`, `~/Projects`.
   - Use `find` with `-maxdepth 3` if needed.
   - If not found, ask the user for the full path.
4. **Write to `local/repos.json`:**
   ```json
   {
     "lastActive": "ProjectName",
     "projects": {
       "ProjectName": {
         "path": "/absolute/path",
         "added": "YYYY-MM-DD"
       }
     }
   }
   ```
   Preserve existing entries. Set `lastActive` to the new project.
5. **Auto-map the project.** If `local/maps/ProjectName.md` doesn't exist, run the **map-project** skill to generate it.
6. **Confirm:** Tell the user the project is registered and give a 1-line recap if a map was generated.
