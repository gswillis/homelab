# Media library (Plex)

Last updated: 2026-11-25

Plan for the Plex server: where media lives, how to name it, how big the drive must be, and what to back up. Plex status is **Proposed**. See D14 and D15 in `docs/architecture.md`.

## Principles

- Media lives on its own drive, never on the photo drive.
- Plex runs in its own container or VM named `plex`, with a reserved address.
- Local network only. Turn **Remote Access off** in Plex, and keep UPnP off on the eero.
- Media is mostly replaceable. Photos are not. Spend backup effort accordingly.

## Fill in before buying a media drive

| Type | Approximate amount now | Approximate size now | Notes |
|---|---|---|---|
| Movies | | | |
| TV shows | | | |
| Music | | | |
| Other (home videos, concerts, audiobooks) | | | |
| **Total now** | | | |
| **Plan for (total plus about 50%)** | | | |

Rough file sizes, to estimate if you do not know:

| Media | Typical size |
|---|---|
| 1080p movie | 4 to 15 GB |
| 4K movie | 30 to 80 GB |
| TV episode, 1080p | 0.5 to 3 GB |
| Music album, lossless | about 0.3 GB |
| Music album, MP3 | about 0.1 GB |

## Folder layout

Plex matches files by folder and file name, so names matter. Use these patterns:

```text
media/
  Movies/
    Movie Name (2010)/
      Movie Name (2010).mkv
  TV Shows/
    Show Name (2008)/
      Season 01/
        Show Name - s01e01.mkv
  Music/
    Artist Name/
      Album Name/
        01 - Track Name.flac
  Other/
    Home Videos/
```

- One folder per movie, one folder per show with a folder per season.
- Put the year in the movie and show folder names.
- Keep each library type in its own top-level folder.

## Backup tiers

| Tier | What | Where |
|---|---|---|
| 1 | Photos | Live copy, external drive, encrypted cloud copy |
| 2 | Plex database and configs | Proxmox VM backups on the external drive, configs in git |
| 3 | Media files | No cloud copy. Keep original discs or purchase records. Copy to the external drive only if there is space |

## Plex settings checklist

- [ ] Create a plex.tv account and store the login in the password manager
- [ ] Claim the server from a device on the home network
- [ ] Turn Remote Access off
- [ ] Add libraries pointing at the folders above
- [ ] Reserve a fixed address for the `plex` container in the eero app
- [ ] Test direct play from a TV, and note any client that transcodes
- [ ] Only then decide whether a Plex Pass is worth it

## Clients

| Device | Wired or Wi-Fi | Plays directly? | Notes |
|---|---|---|---|
| | | | |

4K files can use a lot of bandwidth, so prefer wired clients or a strong Wi-Fi signal for those.
