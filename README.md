# Pinchflat fork

Fork of kieraneglin/pinchflat, kept whole, then packaged as standalone desktop apps. Mac first, because alex is on Mac.

Upstream: https://github.com/kieraneglin/pinchflat
License: AGPL-3.0, see Source/LICENSE
Fork point: 67d8bd5598f4461c5435bcc66fc8d02b00c607cb, 2025-12-16, Fix spelling of 'Unaffected' in help texts (#838)

## Layout

- Source: the original Pinchflat fork, unmodified except for FORK-NOTE.md. Start here to run it as upstream intended, with Docker.
- Mac: standalone Mac app work. Active now.
- Win: standalone Windows app work. Not started.
- Linux: standalone Linux app work. Not started.

Work on a standalone app happens in its own dir. Source stays clean so upstream updates can be merged in.

## What Pinchflat does

Self hosted app for downloading YouTube content, built on yt-dlp. Set rules for channels or playlists and it checks for new content and downloads it. It supports Shorts and livestreams, audio only, SponsorBlock, RSS feeds, and naming for Plex, Jellyfin, and Kodi. It downloads to disk. It is not a video player.

## Status

Mac scaffold only so far. No .app or .dmg built yet. A Mac build needs a Mac. It cannot be fully built and tested on a Linux VM.
