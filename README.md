# Jellyfin with Torrenting (Linux)

## Prerequisites:

- Linux machine (tested on Debian)

## Introduction

With this setup, you'll have a personal streaming service where you can request movies/shows with a click of a button and have them automatically torrented so that you can watch them in a Netflix-like web UI.
If you wish to use this setup with a network share, use branch "network-share-attached-setup"

## Services

- **jellyseerr:** lets user pick browse/choose media and forwards requests to radarr/sonarr
- **radarr:** (for movies) + sonarr (for shows): monitor requested episodes, making requests to prowlarr
- **prowlarr:** finds torrents appropriate to radarr/sonarr's needs and sends to qbittorrent
- **qbittorrent:** downloads torrents (radarr/sonarr will see that it finished and rename/organize the files)
- **jellyfin:** web UI to display whatever is in the media folder

## Folder Layout Example

```txt
/srv/media-server/media
├── movies
│   └── The Human Centipede (First Sequence) (2009)
└── shows
    └── Black Mirror
        ├── Season 1
        ├── Season 2
        └── Season 3

/srv/media-server/torrents
├── completed
│   ├── Black.Mirror.S01.1080p.WEBRip.DD2.0.x264-CasStudio[rartv]
│   ├── Black.Mirror.S02.1080p.WEBRip.DD2.0.x264-CasStudio[rartv]
│   ├── Black.Mirror.S03.1080p.BluRay.x264-SHORTBREHD[rartv]
│   └── The.Human.Centipede.First.Sequence.2009.1080p.BluRay.x264.DTS-WiKi[rartv]
└── incomplete
    └── Midsommar 2019 Directors Cut 1080p BluRay REMUX AVC DTS-HD MA 5.1-EPSiLON [RiCK]
```

## Setup

1. Run:

    ```bash
    curl -fsSL https://get.docker.com/ | sh # install docker (skip if you already have it)

    sudo usermod -aG docker "$USER" # so you don't have to use sudo for docker. log out/in afterwards.

    # create folder structure
    sudo mkdir -p /srv/media-server/media/{movies,shows}
    sudo mkdir -p /srv/media-server/torrents/incomplete
    sudo mkdir -p /srv/media-server/appdata/{jellyfin,jellyseerr,radarr,sonarr,prowlarr,qbittorrent}

    sudo chown -R "$(id -u):$(id -g)" /srv/media-server

    # copy custom config into qbittorrent
    sudo mkdir -p /srv/media-server/appdata/qbittorrent/qBittorrent
    sudo cp -f qBittorrent.conf /srv/media-server/appdata/qbittorrent/qBittorrent/qBittorrent.conf # overwrite config file

    # start
    docker compose up -d
    ```

1. Optional: Open web UI at http://localhost:8080 (user: "admin", password: "adminadmin") and change the password.
1. Go to web UIs of all services and set them up [Internal note: more documentation needed] <!-- TODO: needs more documentation -->

## Optional: Caddy reverse proxy

If you already run Caddy, you can attach these services to an external `caddy_net` (or change the name if you already have one) (this preserving host port access)

```bash
docker network create caddy_net # one-time
docker compose -f docker-compose.yml -f docker-compose.caddy.yml up -d
```

## Usage

Use Jellyfin in your browser at '\<ip>:8096' (substitute 'IP') (or over domain if you [use caddy](#optional-caddy-reverse-proxy)). It will guide you in setting it up. Make sure to create 2 libraries: "Movies" (/mnt/media/movies) and "Shows" (/mnt/media/shows)

Tip: There is a Jellyfin desktop app and mobile app. They may have better compatibility with certain media formats than web which leads to less need for transcoding
