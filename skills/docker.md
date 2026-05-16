---
name: docker
description: Start or manage Docker Compose services. Triggers on "docker", "start docker", "docker compose up", "start containers".
---

# Docker Compose

## Trigger phrases

- "start docker for **ProjectName**", "docker compose up"
- "start containers", "rebuild containers"

## Steps

1. **Resolve target project** from `local/repos.json`.
2. **Ensure Docker is running.** Run `docker info`. If it fails:
   - macOS: `open -a Docker` and wait ~15s
   - Then retry `docker info`
3. **Start services:**
   - Default: `docker compose up -d`
   - With rebuild: `docker compose up -d --build` (if user said "rebuild" or "build")
   - Foreground: `docker compose up` (if user wants logs)
4. **Confirm** the stack is up. Mention `docker compose logs -f` and `docker compose down`.
