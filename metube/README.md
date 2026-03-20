# MeTube (QNAP TS-264 / Portainer)

[MeTube](https://github.com/alexta69/metube) is a web UI for [yt-dlp](https://github.com/yt-dlp/yt-dlp). The TS-264 is **x86_64 (amd64)**; the official image is suitable for that platform.

## Portainer

1. Create a download folder on the NAS (e.g. `/share/Container/metube/downloads`) and note the **numeric** `PUID`/`PGID` of the account that should own those files (SSH: `id admin` or the user you use for shares).
2. **Stacks → Add stack** → paste the contents of `docker-compose.yml`.
3. Under **Environment variables**, add the variables from `.env.example` (or upload an env file), adjusting paths and `PUID`/`PGID`.
4. Deploy. Open `http://<NAS-IP>:8081` (or the port you set).

## Paths

QNAP paths usually look like `/share/...`. If downloads should land on a media share, set `METUBE_DOWNLOADS_HOST_PATH` to that folder instead.

## `PermissionError: ... '/downloads/.metube/queue'`

MeTube stores its download queue under **`downloads/.metube/`** on the bind-mounted folder. That directory must be writable by the user the container runs as (`PUID` / `PGID`).

1. **Align IDs with the NAS** — On the QNAP over SSH, run `id` (as the user that should own that folder). Set **`PUID` and `PGID` in Portainer to exactly those numbers**. If you use `1000` / `100` from the example but your `admin` user is e.g. `uid=500`, the container cannot write reliably.

2. **Fix ownership on the host** (SSH session on the QNAP, adjust path and numbers to match your `METUBE_DOWNLOADS_HOST_PATH` and `id`):

   ```bash
   mkdir -p /share/Container/metube/downloads
   chown -R 1000:100 /share/Container/metube/downloads
   chmod -R u+rwX,g+rwX,o-rwx /share/Container/metube/downloads
   ```

   Replace `1000:100` with your real `uid:gid` from `id`.

3. **If anything is left over from a failed start**, remove the state folder and redeploy (downloads in `.metube` are only queue/metadata):

   ```bash
   rm -rf /share/Container/metube/downloads/.metube
   ```

4. **Optional:** On some QNAP + Docker setups, `chown` inside the container does not behave like on a normal disk. After you have fixed ownership with step 2, set **`CHOWN_DIRS=false`** in the stack environment (see `docker-compose.yml`) so MeTube does not try to change ownership on every start.

After changes, recreate the stack in Portainer.

## Updates

The image is rebuilt regularly with a current yt-dlp. In Portainer, use **Recreate** with **Pull latest image**, or enable Watchtower if you use it elsewhere.

## Optional: reverse proxy

If you terminate TLS on a proxy, set MeTube’s `URL_PREFIX` to match your subpath, or use a subdomain per the [upstream README](https://github.com/alexta69/metube/blob/master/README.md).
