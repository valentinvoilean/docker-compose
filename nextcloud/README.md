# Nextcloud stack

Compose stack for Nextcloud with MariaDB and Redis. No secrets are stored in the repo; provide them via environment variables.

## Deploy from Git in Portainer

1. In Portainer: **Stacks** → **Add stack** → **Build from Git repository**.
2. Set **Repository URL** to your git repo (e.g. `https://github.com/youruser/docker-compose`).
3. Set **Compose path** to: `nextcloud/docker-compose.yml`.
4. In **Environment variables**, add at least:
   - `MYSQL_ROOT_PASSWORD` — root password for MariaDB.
   - `MYSQL_PASSWORD` — password for the `nextcloud` DB user.
5. Optionally set `MYSQL_DATABASE` and `MYSQL_USER` (defaults: `nextcloud`).
6. Deploy the stack.

Portainer will pull the compose file from Git and substitute these variables when deploying. Your secrets stay in Portainer and are never committed.

## Local deploy (optional)

```bash
cd nextcloud
cp .env.example .env
# Edit .env with your passwords
docker compose up -d
```
