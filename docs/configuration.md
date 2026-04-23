---
icon: lucide/settings
---
# Configuration

The server can be configured via environment variables or a `.env` file.

## General

| Variable | Description | Default |
| --- | --- | --- |
| `DATA_DIR` | Directory for data files | `data` |
| `TRONBYT_HOST` | Listen address | All interfaces |
| `TRONBYT_PORT` | Listen port | `8000` |
| `TRONBYT_UNIX_SOCKET` | Path to Unix socket to listen on | |
| `TRONBYT_TRUSTED_PROXIES` | Trusted proxy CIDR ranges | `*` |
| `LOG_LEVEL` | Logging verbosity: `DEBUG`, `INFO`, `WARN`, `ERROR` | `INFO` |

## Database

| Variable | Description | Default |
| --- | --- | --- |
| `DB_DSN` | Database connection string. Supports SQLite, PostgreSQL, and MySQL. | `data/tronbyt.db` |

## Users & Registration

| Variable | Description | Default |
| --- | --- | --- |
| `ENABLE_USER_REGISTRATION` | Allow open user registration | `true` |
| `MAX_USERS` | Maximum number of user accounts (`0` for unlimited) | `0` |
| `SINGLE_USER_AUTO_LOGIN` | Skip login when only one user exists | `false` |

## Apps

| Variable | Description | Default |
| --- | --- | --- |
| `SYSTEM_APPS_REPO` | Git repository URL for system apps | `https://github.com/tronbyt/apps.git` |
| `SYSTEM_APPS_AUTO_REFRESH` | Automatically refresh the system apps repository | `false` |
| `GITHUB_TOKEN` | GitHub token for private app repositories | |

## HTTPS (TLS)

HTTPS can be enabled in two ways:

**Native TLS** — configure the key and certificate paths directly:

| Variable | Description | Default |
| --- | --- | --- |
| `TRONBYT_SSL_KEYFILE` | Path to TLS private key | |
| `TRONBYT_SSL_CERTFILE` | Path to TLS certificate | |

**Reverse proxy** — front the service with a reverse proxy like Caddy. See `docker-compose.https.yaml` in the [server repository](https://github.com/tronbyt/server) for an example.

!!! note

    After enabling HTTPS, update the health check to use the HTTPS URL:
    ```yaml
    test: ["CMD", "/app/tronbyt-server", "health", "https://tronbyt.example.com/health"]
    ```

## Cache

By default, an in-memory cache is used. For persistent caching across container restarts, configure Redis:

| Variable | Description | Default |
| --- | --- | --- |
| `REDIS_URL` | Redis connection string | |

## Monitoring

| Variable | Description | Default |
| --- | --- | --- |
| `ENABLE_PPROF` | Enable Go pprof routes at `/debug/pprof/` | `0` |
| `ENABLE_UPDATE_CHECKS` | Check for new releases on startup | `true` |

The `/metrics` endpoint exposes Prometheus-compatible metrics including:

- Application metrics (`tronbyt_*`) for renders, device activity, HTTP requests, and user/device/app counts
- GORM database connection pool stats
- Standard Go runtime metrics

```bash
curl http://localhost:8000/metrics
```

## CLI Commands

The `tronbyt-server` binary supports additional commands:

**Reset password:**

```bash
tronbyt-server reset-password <username> <new_password>
```

**Health check:**

```bash
# Default (http://localhost:8000/health)
tronbyt-server health

# Custom URL
tronbyt-server health https://your-tronbyt-server.com/health
```
