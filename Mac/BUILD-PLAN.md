# Mac build plan, first test run

## Source facts used here

- Source/mix.exs: app pinchflat, version 2025.9.26, elixir ~> 1.17
- Source/docker/selfhosted.Dockerfile: builder uses Elixir 1.18.4, OTP 27.2.4, Debian bookworm, Node 20, yarn, Hex, Rebar, ffmpeg from yt-dlp FFmpeg-Builds. Runner installs ffmpeg, ffprobe, python3, pipx, apprise, yt-dlp, Deno, locales. Start is /app/bin/docker_start, which runs migrate then PHX_SERVER=true ./pinchflat start. Port 8945.
- Source/config/runtime.exs: server only if PHX_SERVER is set. SECRET_KEY_BASE from env. Media path from MEDIA_PATH, default /downloads. Config path from CONFIG_PATH, default /config. Database path from DATABASE_PATH, default config/db/pinchflat.db. yt-dlp path from System.find_executable("yt-dlp"). sqlean load path is sqlean-linux-arm or sqlean-linux-x86, Linux only.
- Source/rel/overlays/bin/migrate runs ./pinchflat eval Pinchflat.Release.migrate.

## Mac first test, step by step

1. GitHub Actions, macos-14, Apple Silicon, manual trigger only.
2. Install Elixir 1.18.4 and OTP 27.2.4 with erlef/setup-beam. Install Node 20 and yarn. Install Deno, ffmpeg, and yt-dlp with Homebrew.
3. In Source, run mix local.hex --force, mix local.rebar --force, mix deps.get --only prod, mix deps.compile, yarn install in assets, mix assets.deploy, mix compile, then MIX_ENV=prod mix release.
4. For the test only, set CONFIG_PATH and MEDIA_PATH under the runner temp dir, generate a SECRET_KEY_BASE, set PHX_SERVER=true and PORT=8945, run bin/pinchflat eval Pinchflat.Release.migrate, then start bin/pinchflat start in the background.
5. Curl http://localhost:8945/healthcheck until it answers, then stop the server. Upload the release dir as a workflow artifact.
6. If the healthcheck passes, the core works natively on Mac. Then add the Tauri window and the Dock drop handler. If it fails on sqlean, the fix is to fetch sqlean macOS binaries or skip that extension for the test, and record the exact error.

## Mac app, after the smoke test

- Bundle the release with Burrito into one executable per platform.
- Wrap it in Tauri. The Tauri window shows the local Phoenix UI. The Tauri sidecar starts the Phoenix server on a free local port.
- Bundle yt-dlp_macos, the macOS ffmpeg build, and Deno inside the app. Put them on PATH for the app, and set yt-dlp path so Pinchflat finds them.
- Set Mac defaults: config under Library/Application Support/Pinchflat, media under Downloads/Pinchflat for the test, database under the config dir.
- Add the Dock drop: the app accepts a dropped URL, even when the window is closed, and queues a single video download right away. It shows a notification when done.
- Signing and notarization are out of scope for the first test. The test build is unsigned.

## What still needs a Mac

- The .app and .dmg build, the Dock drop test, and the real download test. All three need a Mac. This Linux VM can write the code and the workflow, but it cannot run them.
