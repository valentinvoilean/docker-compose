# MeTube (QNAP TS-264 / Portainer)

[MeTube](https://github.com/alexta69/metube) is a web UI for [yt-dlp](https://github.com/yt-dlp/yt-dlp). The TS-264 is **x86_64 (amd64)**; the official image is suitable for that platform.

## Portainer

1. Create a download folder on the NAS (e.g. `/share/Container/metube/downloads`) and note the **numeric** `PUID`/`PGID` of the account that should own those files (SSH: `id admin` or the user you use for shares).
2. **Stacks → Add stack** → paste the contents of `docker-compose.yml`.
3. Under **Environment variables**, add the variables from `.env.example` (or upload an env file), adjusting paths and `PUID`/`PGID`.
4. Deploy. Open `http://<NAS-IP>:8081` (or the port you set).

## Paths

QNAP paths usually look like `/share/...`. If downloads should land on a media share, set `METUBE_DOWNLOADS_HOST_PATH` to that folder instead.

The stack uses a **named Docker volume** (`metube_state`) for **`STATE_DIR`** (download queue / SQLite DB). That avoids QNAP share ACL quirks that often cause **`PermissionError: ... '/downloads/.metube/queue'`** when state lived on the share. **Finished videos still go to** the bind-mounted **`/downloads`** path on the NAS.

If you ever remove the stack but keep the volume, queue/history survives a redeploy. To wipe queue only, remove the `metube_state` volume in Portainer / Container Station.

## `PermissionError` on `/downloads` (not `.metube`)

If downloads fail with permission errors under **`/downloads`** (not the queue), align **`PUID`/`PGID`** with `id` on the QNAP for the user that should own that folder, fix ownership over SSH, and optionally set **`CHOWN_DIRS=false`** after a host `chown` if the entrypoint cannot change the share (see comments in `docker-compose.yml`).

## Updates

The image is rebuilt regularly with a current yt-dlp. In Portainer, use **Recreate** with **Pull latest image**, or enable Watchtower if you use it elsewhere.

## Optional: reverse proxy

If you terminate TLS on a proxy, set MeTube’s `URL_PREFIX` to match your subpath, or use a subdomain per the [upstream README](https://github.com/alexta69/metube/blob/master/README.md).
