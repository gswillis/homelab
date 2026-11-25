# Media library (Jellyfin)

Last updated: 2026-11-25

Plan for the Jellyfin server: where media lives, how to name it, how to grow it, and what to back up. Jellyfin replaced Plex in the plan on 2026-11-25. See D14 and D15 in `docs/architecture.md`.

## Where this stands

- There is no media library yet, so no media drive is needed yet.
- The plan is to grow a library over time. Buy the media drive when there are files to put on it.
- Jellyfin is free and open source, needs no vendor account, and includes hardware transcoding at no cost.

## Principles

- Media lives on its own drive, never on the photo drive.
- Jellyfin runs in its own container or VM named `jellyfin`, with a reserved address.
- Local network only. No port forwarding, and keep Jellyfin's automatic port mapping (UPnP) off, along with UPnP on the eero.
- Media is mostly replaceable. Photos are not. Spend backup effort accordingly.

## Growing the library

Jellyfin plays files you have. Ways to build a library:

- Discs you own, ripped to files. Check the rules where you live, because copying discs can be restricted.
- DRM-free purchases, such as music from stores that sell plain files.
- Home videos and your own recordings.
- Public-domain and Creative Commons material.

Purchases locked inside a streaming service's own app cannot be added to Jellyfin.

Start small. A few files are enough to learn the folder layout and test playback before the library gets big.

## Sizing worksheet

Fill in as the library grows.

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

If the library grows past what one extra drive can hold, revisit the NAS option (Option B) in `docs/architecture.md`.

## Folder layout

Jellyfin matches files by folder and file name, so names matter. Use these patterns:

```text
media/
  Movies/
    Movie Name (2010)/
      Movie Name (2010).mkv
  Shows/
    Show Name (2008)/
      Season 01/
        Show Name S01E01.mkv
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
- Check Jellyfin's documentation for the current naming rules before importing a lot of files.

## Backup tiers

| Tier | What | Where |
|---|---|---|
| 1 | Photos | Live copy, external drive, encrypted cloud copy |
| 2 | Jellyfin database and configs | Proxmox VM backups on the external drive, configs in git |
| 3 | Media files | No cloud copy. Keep original discs or purchase records. Copy to the external drive only if there is space |

## Jellyfin setup checklist

- [ ] Create the `jellyfin` container or VM and reserve a fixed address in the eero app
- [ ] Run the setup wizard and create the admin account. Store the login in the password manager
- [ ] Create a separate account for each household member
- [ ] In networking settings, keep automatic port mapping (UPnP) off
- [ ] Add libraries pointing at the folders above
- [ ] If you need hardware transcoding, pass the Intel integrated graphics through to the container or VM
- [ ] Test direct play from a TV, and note any client that transcodes

## Clients

You have one TV to watch on: a Samsung Smart TV.

### Samsung TV and the Jellyfin app

- Jellyfin has an official app in Samsung's Tizen app store for TVs from 2021 or later (Tizen 6.0 or higher). It launched in a limited set of markets first, so availability may vary.
- Older Samsung TVs (2020 or earlier) can still run Jellyfin by sideloading it. That needs Developer Mode on the TV and a community installer tool. Follow Jellyfin's own guide and treat third-party tools with care.
- If neither works, a small streaming device with a Jellyfin app is the fallback.

Steps:

1. Find the TV's model code (Settings, then Support, then About This TV) and look it up to confirm the model year.
2. Open the TV's app store and search for Jellyfin.
3. If it is not there and the TV is a 2021 or later model, check again later or check the TV's software updates.
4. If the TV is older, choose between sideloading and a streaming device.

### Client table

| Device | Model and year | Wired or Wi-Fi | Jellyfin app? | Notes |
|---|---|---|---|---|
| Samsung Smart TV | | | Check the app store | |

4K files can use a lot of bandwidth, so prefer Ethernet for the TV, or a strong Wi-Fi signal. The TV must be on your main network, because devices on the eero guest network may not reach the server.

## Streaming services stay separate

Netflix, Hulu, HBO Max, Prime Video, Disney+, YouTube, YouTube TV and Paramount lock their content to their own apps. None of it can go into Jellyfin, and Jellyfin does not replace them. Keep using those apps on the TV as you do now.

After Pi-hole becomes the network's DNS, open each of those apps and check that it still works. If one breaks, allowlist the domain it needs in Pi-hole, or turn Custom DNS off in the eero app to go back.
