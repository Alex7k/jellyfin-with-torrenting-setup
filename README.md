# Jellyfin with Torrenting (Linux)

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

1. Run in a shell:

    ```bash
    sudo apt update -y
    sudo apt install -y docker.io docker-compose-plugin

    sudo systemctl enable --now docker
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
    docker compose pull
    docker compose up -d
    ```

1. Optional: Open web UI at http://localhost:8080 (user: "admin", password: "adminadmin") and change the password.
1. Go to web UIs of all services and set them up <!-- TODO: needs more documentation -->

## Usage

Use Jellyfin in your browser at 'IP:8096' (substitute 'IP'). It will guide you in setting it up. Make sure to make 2 libraries "Movies" (/mnt/media/movies) and "Shows" (/mnt/media/shows)

There is also a Jellyfin app for mobile devices
