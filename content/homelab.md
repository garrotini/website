---
date: 2026-10-02
lastmod: 2026-10-02
showTableOfContents: false
title: "Home Lab"
type: "page"
---

My home lab is where I do my learning!

It consists of a Linux server on repurposed hardware — an old laptop running a multi-service Docker stack that I use every day: media, music, and a document pipeline.

It's the real system behind "I break things on my own server so nothing breaks on a production one."

## Right Now

```text
                            ┌────────────────────────────┐
  laptop/desktop            │        HOME SERVER         │
  ───────────────           │  Debian 12 - old laptop    │
                            │                            │
  SSH (LAN)       ────────▶│  Docker ┌───────────────┐  │
  SSH over Tailscale  ────▶│         │ Jellyfin      │  │
                            │         │ Navidrome     │  │
                            │         │ Stirling-PDF  │  │
                            │         │ Downtify      │  │
                            │         └───────────────┘  │
                            │       systemd · UFW        │
                            └─────────────┬──────────────┘
                                          │ rsync
                                          ▼
                               config & document backups
```

## Services

### Running right now (Oct 2026)

- **Jellyfin** - media server, streaming my tv shows and movies library
- **Navidrome** - music streaming server, that I connect to using an Android client app called **Reasonus** (kind of a Spotify clone)
- **Stirling-PDF** - a great PDF toolbox complement to the cli-tool **pdfunite**
- **Downtify** - because sometimes it makes sense to store music locally

### Other useful apps, not containerized

- **docling** - document parsing pipeline (converting pdfs or yt videos to md has been great for learning!)
- **yt-dlp** - also, because sometimes it's easier to digest content if they're stored locally
- **ollama** - testing small local models. wouldn't it be nice to run local AI models on your consumer devices?
- **rsync** - great tool for copying and syncing between devices inside the same network (or even the same device. it's such a powerfull `cp` command!)

### Apps I tried in the past

- **OpenCloud** and **NextCloud** - in the search for finding a great file sync and sharing between devices
- **Syncthing** - same quest as mentioned before (sync across devices)
- **Immich** - Self-hosted photo library, a replacement for Google Photos
- **Paperless-ng** - I dream on having this service properly setup and fully functional for my needs
- **Pi-hole** - I messed up a few times with this. It blocks ads and tracker but it might affect other users on your network (still need to configure it properly)
- **Portainer** - it was great to see in a clean web page all the container management, but then I forced myself to be in the CLI

## What it taught me

- Linux administration (systemd, users, permissions, UFW firewall)
- Remote administration over SSH or SSH via Tailscale — no exposed ports
- Docker networking and volumes
- Full container lifecycle: updates, permissions, and health in my hands
- Backups of configuration and documents kept in sync with rsync
- Shell scripting for automation
- Backup strategies and cron jobs

## Next

- extending the lab with Terraform and Kubernetes
- testing Podman as a Docker alternative
- evaluating AWS vs Azure as my first cloud target.
