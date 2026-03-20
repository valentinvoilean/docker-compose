# MeTube (QNAP TS-264 / Portainer)

[MeTube](https://github.com/alexta69/metube) is a web UI for [yt-dlp](https://github.com/yt-dlp/yt-dlp). The TS-264 is **x86_64 (amd64)**; the official image is suitable for that platform.

## Portainer

1. Create a download folder on the NAS (e.g. `/share/Container/metube/downloads`) and note the **numeric** `PUID`/`PGID` of the account that should own those files (SSH: `id admin` or the user you use for shares).
2. **Stacks → Add stack** → paste the contents of `docker-compose.yml`.
3. Under **Environment variables**, add the variables from `.env.example` (or upload an env file), adjusting paths and `PUID`/`PGID`.
4. Deploy. Open `http://<NAS-IP>:8081` (or the port you set).

## Paths

QNAP paths usually look like `/share/...`. If downloads should land on a media share, set `METUBE_DOWNLOADS_HOST_PATH` to that folder instead.

## Updates

The image is rebuilt regularly with a current yt-dlp. In Portainer, use **Recreate** with **Pull latest image**, or enable Watchtower if you use it elsewhere.

## Optional: reverse proxy

If you terminate TLS on a proxy, set MeTube’s `URL_PREFIX` to match your subpath, or use a subdomain per the [upstream README](https://github.com/alexta69/metube/blob/master/README.md).
