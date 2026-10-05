# qBittorrent (QNAP / Portainer)

1. Create the config directory and choose a downloads directory on the NAS.
2. In Portainer, create a stack from `docker-compose.yml` and set the variables in `.env.example` to match your NAS paths and numeric user/group IDs.
3. Deploy and open `http://<NAS-IP>:8080`, or the `QBITTORRENT_WEBUI_PORT` you selected. Find the temporary `admin` password in the container logs and change it in the Web UI settings.

The torrent listening port is published over both TCP and UDP. If you change either port, set the corresponding environment variable; the container's internal port must match the published port. For incoming peers outside your LAN, forward the torrent listening port on your router to the NAS.

[Image documentation](https://docs.linuxserver.io/images/docker-qbittorrent/)
