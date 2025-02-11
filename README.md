# Media Streaming Service

Forked from [Automation Avenue](https://github.com/automation-avenue/youtube-39-arr-apps-1-click) and I'll try to better this as we go.

## From [Video 39](https://youtu.be/1eqPmDvMjLY?si=soWz9ggSyDqnz-n1) - Deploy ARR apps using just 1 command (full set with Jellyfin and qBittorrent !!!)

## To Prowlarr, Sonarr, Radarr stack only with Jellyfin and qBittorrent + Jellyseerr as request automation service!

### Useful Links:

-   [Servarr Wiki](https://wiki.servarr.com/)
-   [Trash Guides](https://trash-guides.info/)
-   [Ascii ART](https://patorjk.com/software/taag/#p=display&f=ANSI%20Shadow)

## Download and Unzip Files from GitHub

```bash
cd /home/marek/Downloads
unzip youtube-39-arr-apps-1-click
```

## Or you can just clone this repo, please be sure to have set up git to your server.

```bash
git clone https://github.com/ow3ndesu/media-server.git
cd media-server
```

## Environment Variables

Check your `.env` file, edit as you require.

## Set Permissions

Navigate to the folder specified in the `.env` file (e.g., `/media/Arr`, then go to `/media` as root) and run:

```bash
chown -R 1000:1000 Arr
```

Now, all services should be accessible.

## Installation Process

Ensure you are in the same folder as `docker-compose.yml` and `.env` file, then use the following commands:

```bash
sudo docker-compose up -d
```

## If something is not right, stop the container

Ensure you are in the same folder as `docker-compose.yml` and `.env` file, then use the following commands:

```bash
sudo docker-compose down
```

---

## Configuration

### qBittorrent

Since qBittorrent uses a temporary password, configure it first:

1. Find the qBittorrent container ID:
    ```bash
    sudo docker ps
    ```
2. Check logs for the temporary password:
    ```bash
    sudo docker logs <qbittorrent-container-id>
    ```
    Look for:
    ```
    The WebUI administrator username is: admin
    The WebUI administrator password was not set. A temporary password is provided for this session: <your-password>
    ```
3. Access qBittorrent at `http://localhost:8080` and log in.
4. Navigate to `Tools > Options > WebUI`, change the username and password, and tick **"Bypass authentication for clients on localhost"**.

### Prowlarr

1. Access Prowlarr at `http://localhost:9696`
2. Go to `Settings > Download Clients > +` and add qBittorrent.
3. Set the port to match qBittorrent’s WebUI port (default is `8080`).
4. Change `localhost` to the host machine's IP (`ip address` command).

### Sonarr

1. Access Sonarr at `http://localhost:8989`
2. Go to `Settings > Media Management > Add Root Folder`, set `/data/tvshows`.
3. Go to `Settings > Download Clients`, click `+`, add qBittorrent (same as Prowlarr).
4. Copy Sonarr’s API key (`Settings > General`), then in Prowlarr (`Settings > Apps`), click `+` and add Sonarr, replacing `localhost` with the host IP.
5. Enable backups: `Settings > General > Show Advanced > Backups > /data/Backup`.

### Radarr

1. Access Radarr at `http://localhost:7878`
2. Set `/data/movies` as the root folder (`Settings > Media Management`).
3. Add qBittorrent (`Settings > Download Clients > +`), same as Sonarr.
4. Copy Radarr’s API key and add it to Prowlarr (`Settings > Apps > +`).
5. Enable backups: `Settings > General > Show Advanced > Backups > /data/Backup`.

### Jellyfin

1. Access Jellyfin at `http://localhost:8096`
2. If port `1900` is in use (by `rygel`), remove it:
    ```bash
    sudo apt-get remove rygel
    sudo docker-compose up -d
    ```
3. Add media libraries in Jellyfin:
    - `/data/Movies`
    - `/data/TVShows`
    - `/data/Music`
    - `/data/Books`

### Jellyseerr (NEW!)

1. Access Jellyseerr at `http://localhost:5055`
2. Initial setup:
    - Sign in with Plex or create an admin account.
    - Go to `Settings > Services > +`, add Radarr and Sonarr.
    - Enter their API keys (from Radarr/Sonarr General Settings).
    - Change `localhost` to the host machine's IP.
3. Configure Jellyfin integration:
    - Go to `Settings > Media Server > +`, select Jellyfin.
    - Enter the API key (found in Jellyfin’s Admin settings under API keys).
    - Set the Jellyfin server URL.

### Final Steps

1. Go back to **Prowlarr**:
    - Click `Indexers` (top right) > `Add indexer`, search and add providers like `rarbg`, `yts`, etc.
    - Test and save each.
    - Click `Sync App Indexers` (next to `Add indexer`).
    - Ensure full sync (green status) in `Settings > Apps`.
2. Start adding content:
    - **Radarr:** Add movies, click `Search All`.
    - **Sonarr:** Add series, click `Search Monitored`.
    - **Jellyseerr:** Request media via its web UI, and it will automatically sync with Radarr/Sonarr.

---

### Your ARR + Jellyfin + Jellyseerr Stack is Now Ready!

Enjoy automated media management with seamless integration between all apps.
