---
name: start-docker-compose
description: Start a Docker Compose project. Use when the user asks to start Docker Compose, run docker compose, bring up containers, build images, or start the Docker stack.
---

# Start Docker Compose

## When to Use

- User asks to start Docker Compose, run docker compose, or bring up the stack
- User asks to start containers, run the Docker project, or spin up services with Docker Compose
- User asks to build (or rebuild) images before or when starting the stack

## Workflow

### 0. Target repo (when using from Octopus)

If the user specified a repo by name, look up that name in **`memory/referenced-repos.json`** in this workspace root. If found, run all following steps from that repo's path (e.g. `cd <path>` then run commands there). Otherwise use the current workspace.

### 1. Ensure Docker is running

Before running `docker compose`, check if the Docker daemon is available (e.g. run `docker info` or `docker version`). If it fails with "Cannot connect to the Docker daemon" or similar:

**macOS:** Start Docker Desktop (or the Docker app):

```bash
open -a Docker
```

**Windows:** Start Docker Desktop (e.g. from Start Menu or `Start-Process "C:\Program Files\Docker\Docker\Docker Desktop.exe"`).

Then wait for the daemon to be ready: either poll with `docker info` until it succeeds, or sleep a reasonable amount (e.g. 15–30 seconds) before running `docker compose up`. Request permissions that allow launching apps (e.g. `all`) when opening Docker so the app actually starts.

### 2. Start Docker Compose

From the project root (where `docker-compose.yml` or `compose.yml` lives):

**Build flag:** If the user wants to build or rebuild images (e.g. "start Docker Compose with build", "build and start", "rebuild images"), add `--build` to the command.

**Detached (recommended – runs in background):**

```bash
docker compose up -d
```

With build:

```bash
docker compose up -d --build
```

Containers start in the background. Logs can be viewed with `docker compose logs -f`.

**Foreground (see logs in terminal):** If the user wants to watch logs in the current terminal, run without `-d`:

```bash
docker compose up
```

With build:

```bash
docker compose up --build
```

Run this in the background if you use it from an automated flow so the terminal isn't blocked.

### 3. Confirm to the user

Tell them the stack is up. If relevant, mention how to view logs (`docker compose logs -f`) or stop (`docker compose down`).

## Summary

1. Resolve target repo from Octopus reference files if a repo name was given.
2. If Docker isn't running, start the Docker app (`open -a Docker` on macOS; Docker Desktop on Windows) and wait for the daemon to be ready.
3. From that repo root, run `docker compose up -d` (or `docker compose up` for foreground logs). Add `--build` if the user wants to build or rebuild images.
4. Confirm the stack is running and optionally remind about logs/down.

## Notes

- If the project uses the older `docker-compose` (hyphen) CLI, use `docker-compose up -d` (and `docker-compose up -d --build` when building) instead.
