# Jellyfin with Torrenting (Linux)

## Introduction

With this setup, you'll have a personal streaming service where you can request movies/shows with a click of a button and it will automatically torrent them so that you can watch them in a Netflix-like web UI.
This setup is intended to be used with a network share for file storage. You could skip some steps if you just want to store files locally.

## Services

- **jellyseerr:** lets user pick browse/choose media and forwards requests to radarr/sonarr
- **radarr:** (for movies) + sonarr (for shows): monitor requested episodes, making requests to prowlarr
- **prowlarr:** finds torrents appropriate to radarr/sonarr's needs and sends to qbittorrent
- **qbittorrent:** downloads torrents (radarr/sonarr will see that it finished and rename/organize the files)
- **jellyfin:** web UI to display whatever is in the media folder

## Folder Layout Example

```txt
/mnt/media
├── movies
│   └── The Human Centipede (First Sequence) (2009)
└── shows
    └── Black Mirror
        ├── Season 1
        ├── Season 2
        └── Season 3

/mnt/torrents
├── completed
│   ├── Black.Mirror.S01.1080p.WEBRip.DD2.0.x264-CasStudio[rartv]
│   ├── Black.Mirror.S02.1080p.WEBRip.DD2.0.x264-CasStudio[rartv]
│   ├── Black.Mirror.S03.1080p.BluRay.x264-SHORTBREHD[rartv]
│   └── The.Human.Centipede.First.Sequence.2009.1080p.BluRay.x264.DTS-WiKi[rartv]
└── incomplete
    └── Midsommar 2019 Directors Cut 1080p BluRay REMUX AVC DTS-HD MA 5.1-EPSiLON [RiCK]
```

## Setup

1. Before running, fill out "password=" in smbcredentials file
1. Run commands in [setup](setup) (I recommend running commands one by one)

## Usage

Use Jellyfin in your browser at 'IP:8096' (substitute 'IP'). It will guide you in setting it up. Make sure to make 2 libraries "Movies" (/mnt/media/movies) and "Shows" (/mnt/media/shows)

There is also a Jellyfin app for mobile devices
