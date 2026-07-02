# Viewport internal deploy overlay — CLIProxyAPI

Viewport-internal deployment overlay for [CLIProxyAPI](https://github.com/router-for-me/CLIProxyAPI).
It does not modify upstream source — it only adds a `deploy/` folder with a hardened compose file.

## What this is

- **Pinned image:** `eceasy/cli-proxy-api:v7.2.48` (never `:latest`).
- **Internal-only:** every published port binds to `127.0.0.1` (loopback). Nothing is exposed on `0.0.0.0`.
- **Secret via env:** `MANAGEMENT_PASSWORD` is required and injected from the environment — never hardcoded.
- Keeps the upstream volumes: `config.yaml`, `auths`, `logs`. Adds `restart: unless-stopped`.

## Deploy on dokploy-new

1. Create a Compose service in dokploy-new.
2. Git source = this fork (`viewport-corp/fork-CLIProxyAPI`).
3. Compose path = `deploy/docker-compose.yml`.
4. Provide the env below (see `deploy/.env.example`).

## Env

| Var | Required | Notes |
|-----|----------|-------|
| `MANAGEMENT_PASSWORD` | yes | Management API password. Injected via env, never committed. |
| `DEPLOY` | no | Optional deploy marker passed to the container. |
| `CLI_PROXY_CONFIG_PATH` | no | Config volume source (default `./config.yaml`). |
| `CLI_PROXY_AUTH_PATH` | no | Auth volume source (default `./auths`). |
| `CLI_PROXY_LOG_PATH` | no | Logs volume source (default `./logs`). |

---

Follows locked Viewport external-OSS process: fork → clone → upstream → viewport/deploy overlay → PR.
