# Mac app

Standalone Mac packaging for the Pinchflat fork in Source. Mac first, because alex is on Mac. No Docker, no self hosting setup. The app runs the Pinchflat server locally inside the app and shows its web UI.

## What you have already

Source is the whole upstream Pinchflat tree, 1351 files, fork point 67d8bd55. Elixir 1.18.4, OTP 27.2.4, Phoenix 1.7, SQLite, yt-dlp, ffmpeg, Deno. The Mac work does not change Source. It wraps it.

## First test run

Goal: the Mac build starts, the UI opens at localhost:8945, you add one small test source, and it downloads one video. Then we add the Dock drop.

What I need for that test, from alex:
- Mac chip: Apple Silicon or Intel, and macOS version. First build target is Apple Silicon on macos-14 unless he says Intel.
- One small test link, like the 10 video channel he already used.
- Downloads go to Downloads by default for the test.

What I do not need: no Docker, no TrueNAS, no server. The test build comes from GitHub Actions on a Mac runner, because this Linux VM cannot build or run a Mac app. It has no Elixir and no Rust, and it cannot test Dock drop.

## Known Mac blockers, found in Source

- sqlean extensions in Source are Linux only: sqlean-linux-arm and sqlean-linux-x86. On Mac, runtime.exs tries to load sqlean-linux-arm/sqlean, which will not load on macOS. The Mac build must fetch or build sqlean for macOS, or skip that extension for the first test. Title regex filtering uses it.
- Deno is required for YouTube downloads in the Docker build. The Mac build must bundle Deno or install it.
- yt-dlp and ffmpeg must be bundled per platform, macOS builds, not the Linux ones in the Dockerfile.
- The Mac app will be unsigned for the first test. macOS Gatekeeper will block it on first open. Right click, Open, is the workaround until signing is set up.

## Build route

First route: ex_tauri plus Burrito. Tauri gives the native window. The Phoenix app is bundled as a sidecar binary. yt-dlp, ffmpeg, and Deno are bundled with it.

Fallback: elixir-desktop, with mix desktop.installer for a .dmg and .app bundle.

Simpler smoke test first: GitHub Actions builds a plain Mac release with mix release, starts it, and curls localhost:8945/healthcheck. That proves Pinchflat runs natively on Mac before we add the Tauri window. See BUILD-PLAN.md and the workflow file Mac/mac-test-build.yml. That file lives in Mac because the GitHub token I use cannot write .github/workflows files, so it has to be copied to .github/workflows by hand in the GitHub web UI before it can run.

## Status

Scaffold and build plan only. No .app or .dmg built yet.
