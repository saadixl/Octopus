---
name: use-octopus
description: Register a repo so you can use skills from this Octopus repo without copying them. When the user says use octopus, use skills, use skill, or register repo, ask for the repo name, find it on the machine, and store a reference with path and details.
---

# Use Octopus (register a repo)

Use this when you want to **work from the Octopus repo** but run skill workflows (commit-push, start-react, deploy) in another repo. No need to copy skills into each project—we keep a reference here and run commands in the target repo when needed.

## When to Use

- User says **use octopus**, **use skills**, **use skill**, or **register repo**
- User wants to add a repo so they can use skills from this repo against it
- User wants to “point” this skills repo at a project by name

## Workflow

### 1. Ask for the repo name

Reply with something like:

- “Which project do you want to use Octopus with? Give me the project name.”
- Or: “What’s the name of the repo on your machine?”

If the user already included a repo name (e.g. “use octopus with Ventory”), do **not** ask again—use that name directly.

### 2. Find the repo on the machine

Once the user gives a name:

1. **Check the reference file first**  
   Read `memory/referenced-repos.json` in this repo (Octopus). If that file doesn’t exist, create it (same format as `memory/referenced-repos.example.json`). If that repo name is already in the file, confirm and stop (or ask if they want to update the path).

2. **If not in the reference file, search for the repo:**
   - **Sibling of this repo:** List the parent directory of Octopus and look for a folder matching the name (case-insensitive).
   - **Common roots:** If not found as sibling, try listing `~/Code/Personal`, `~/Code`, or the parent of the current workspace and look for a directory with the given name.
   - Use terminal commands such as:
     - `ls` the parent of the current workspace
     - or `find` with a reasonable root (e.g. user’s Code or Home) and `-maxdepth` to avoid deep scans, matching the repo name.

3. **If you find exactly one matching directory** that looks like a repo (e.g. has `.git` or package.json), treat it as the repo path. Resolve to an absolute path.

4. **If you find none or several:**  
   Ask the user for the full path (e.g. “I couldn’t find a unique repo named X. What’s the full path to that repo on your machine?”).

### 3. Create or update the reference

- Write to **`memory/referenced-repos.json`** in Octopus. This file is in `.gitignore`, so paths are never committed. If the file doesn’t exist, create it using the format in `memory/referenced-repos.example.json`.

- **Format:**

```json
{
  "repos": {
    "RepoName": {
      "path": "/absolute/path/to/repo",
      "addedAt": "2025-02-26"
    }
  }
}
```

- **Add or update the entry** for the repo name the user gave:
  - Use the exact name they gave (or a canonical form).
  - `path`: absolute path to the repo directory.
  - `addedAt`: today’s date in ISO or YYYY-MM-DD (update when path changes).

- Write the updated JSON back to `memory/referenced-repos.json` (preserve any other repos already in the file).

### 4. Confirm to the user

Tell them:

- The repo **&lt;name&gt;** is registered.
- Path stored: **&lt;path&gt;**.
- Short reminder: “You can keep using Cursor from this Octopus repo. When you ask to commit, start React, or deploy, tell me the repo name and I’ll run the steps in that repo using the reference here.”

### 5. Auto-map the project (one-time)

After registration (or when the repo name already exists), ensure the project has a saved summary:

- Check for: `memory/project/<RepoName>/summary.md`
- If it exists: read it and give a short recap (1–3 bullets), then stop.
- If it does not exist: run the **map-project** workflow to generate and save it, then tell the user where it was saved.

## Summary

1. User says “use skills” → ask for **repo name**.
2. User gives name → **find** repo (reference file, then sibling/common paths; else ask for path).
3. **Write** `memory/referenced-repos.json` with `repos.<name>.path` and `addedAt`.
4. **Confirm** registration and remind how to use it (e.g. “commit and push &lt;name&gt;”, “start React for &lt;name&gt;”).
5. If no saved summary exists, **generate** `memory/project/<name>/summary.md` (map-project).

## Notes

- **Always run from this repo (Octopus).** The reference file lives here; commands for the other repo are run by `cd`-ing to the stored path (or running from that path) when the user asks for a skill for that repo.
- **Other skills (commit-push, start-react, start-docker-compose, firebase-build-deploy):** When the user specifies a repo by name, look up the name in `memory/referenced-repos.json` and run the skill steps in that repo’s path instead of the current workspace.
