# n8n Self-Hosted — Docker Setup

Self-hosted [n8n](https://n8n.io) workflow automation stack using Docker Compose, backed by PostgreSQL. Includes production hardening: resource limits, health checks, log rotation, and basic auth.

## Stack

| Service | Image | Purpose |
|---------|-------|---------|
| `n8n` | `n8n-custom:1.91.3` (built locally) | Workflow automation UI & engine |
| `postgres` | `postgres:16-alpine` | Persistent workflow/credential storage |

## Prerequisites

- Docker Desktop (or Docker Engine + Compose plugin)
- `openssl` available in your shell (for key generation)

## Quick Start

**1. Copy and configure the environment file:**

```bash
cp env.example .env
```

Then edit `.env` and fill in every `CHANGE_ME` value:

| Variable | Description |
|----------|-------------|
| `POSTGRES_PASSWORD` | Strong password for the PostgreSQL user |
| `N8N_ENCRYPTION_KEY` | 32-byte random hex — generate with `openssl rand -hex 32` |
| `N8N_BASIC_AUTH_USER` | Login username for the n8n UI (default: `admin`) |
| `N8N_BASIC_AUTH_PASSWORD` | Login password for the n8n UI |
| `WEBHOOK_URL` | Public URL used for incoming webhooks (update when behind a reverse proxy) |
| `N8N_EDITOR_BASE_URL` | Public URL for the editor (same as above in most setups) |
| `TZ` | Timezone, e.g. `America/New_York` (default: `UTC`) |

**2. Build and start the stack:**

```bash
docker compose up -d --build
```

**3. Open the editor:**

Navigate to [http://localhost:5678](http://localhost:5678) and log in with the credentials set in `.env`.

## Stopping & Restarting

```bash
# Stop containers (data is preserved in Docker volumes)
docker compose down

# Stop and remove all data (destructive)
docker compose down -v
```

## Data Persistence

Two named Docker volumes store all persistent data:

- `n8n_data` — n8n workflows, credentials, and settings (`/home/node/.n8n`)
- `postgres_data` — PostgreSQL database files

The `n8n_data/` directory in this folder is reserved for local bind-mount use if needed but is not mounted by default.

## Resource Limits

| Service | CPU | Memory (limit) | Memory (reserved) |
|---------|-----|----------------|-------------------|
| n8n | 1.0 core | 768 MB | 256 MB |
| postgres | — | 256 MB | — |

## Configuration Notes

- n8n is only bound to `127.0.0.1:5678` — it is **not** exposed publicly. Put it behind a reverse proxy (nginx, Caddy, Traefik) with TLS before exposing to the internet.
- Execution history is pruned automatically after **168 hours (7 days)** to keep the database lean.
- Telemetry and version notifications are disabled in the custom image.
- Log rotation is configured: n8n logs cap at 20 MB × 5 files; PostgreSQL at 10 MB × 3 files.

## File Structure

```
.
├── Dockerfile          # Custom n8n image (adds tzdata, curl, hardens permissions)
├── docker-compose.yml  # Service definitions for n8n + PostgreSQL
├── env.example         # Template for required environment variables
├── .env                # Your local secrets (never commit this)
└── n8n_data/           # Optional local data directory
```

## Upgrading n8n

Change `N8N_VERSION` in [docker-compose.yml](docker-compose.yml) (line 6), then rebuild:

```bash
docker compose up -d --build
```

Always back up your Docker volumes before upgrading.
