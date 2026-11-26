# Mac app

Standalone Mac packaging for the Pinchflat fork in Source. Mac first, because alex is on Mac.

## Goal

One Mac app that runs the whole Pinchflat server locally and shows its web UI in a native window, with no Docker and no browser tab to open first.

Plus the alex feature: drag a video link straight onto the Dock icon and have it download right away, without opening the app window first. Save to Downloads, notify when done. Single links first. Playlists and channels keep going through the normal Pinchflat source rules.

## Planned approach

First route to test: ex_tauri plus Burrito. Tauri provides the native window. The Phoenix app is bundled as a sidecar binary with Burrito. yt-dlp and ffmpeg are bundled per platform. The Tauri window opens the local Phoenix UI.

Fallback if that proves too heavy: elixir-desktop, with mix desktop.installer for a .dmg and .app bundle.

## Build requirements, Mac only

A Mac with Xcode command line tools, Elixir with OTP 27, Rust, Zig 0.16.0 exactly for Burrito, and the Tauri prerequisites. A Linux VM cannot build or test the .dmg. The Mac build and the Dock drop test need a Mac.

## Status

Scaffold only. No code yet, no build yet. Next step is to get Source running locally on a Mac, then wire the Tauri shell around it, then add the Dock drop handler.
