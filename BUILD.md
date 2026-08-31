# Build Guide

Everything you need to build Surf FED from source, locally or in CI.

## Prerequisites

- **Node.js 18+** and **npm** (the CI uses Node 20).
- Platform-specific tooling only if you are packaging for that platform (see
  below). For just running the app in dev, Node + npm is enough.

## Run in development

```bash
npm install
npm start
```

This launches Electron from the project root using `main.js` as the entry
point (set via `"main": "main.js"` in `package.json`).

## Local packaging

Packaging must happen on the target OS (electron-builder builds for the host
platform natively). CI handles cross-platform builds for you; you usually do
not need to do this locally.

```bash
npm run dist -- --win       # Windows -> dist/Surf FED.exe (portable)
npm run dist -- --mac       # macOS   -> dist/Surf FED.dmg
npm run dist -- --linux     # Linux   -> dist/Surf FED.AppImage
```

All outputs land in `dist/` (configured via `build.directories.output`).

### Windows

- Run on Windows.
- Output: a portable `.exe` (target `portable` in `package.json`).
- No special system dependencies.

### macOS

- Run on macOS.
- Output: a `.dmg` (target `dmg`).
- Unsigned by default. For distribution, see `DEPLOYMENT.md` (signing +
  notarization). To produce an `.icns` from your icon:
  ```bash
  npx electron-icon-builder --input=assets/icons/icon.png --output=build --flatten
  ```

### Linux

- Run on Linux. On Ubuntu 24.04 specifically, install:
  ```bash
  sudo apt-get update
  sudo apt-get install -y \
    libgtk-3-dev libnotify-dev libnss3 libxss1 libxtst6 xauth \
    libavcodec-extra libgbm1 libasound2t64
  ```
  (Note: Ubuntu 24.04 uses `libasound2t64`, not `libasound2`.)
- Output: an `.AppImage` (target `AppImage`). `.deb`/`.snap` can be enabled by
  changing the `linux.target` in `package.json`.

## How extensions get packaged

`package.json` -> `build` includes this:

```json
"files": [ "main.js", "preload.js", "index.html", "styles.css",
           "renderer.js", "extensions/builtin/**/*" ],
"asarUnpack": [ "extensions/builtin/**/*" ]
```

`asarUnpack` is essential: Electron's `session.loadExtension()` reads
extension files from disk, so they are extracted out of `app.asar` into
`app.asar.unpacked/`. `main.js`'s `builtinExtDir()` resolves that path at
runtime. **Any new shipped extension must be added to both `files` and kept in
`asarUnpack`**, or it will not load in a packaged build.

## CI

`.github/workflows/build.yml` ("Build Desktop Packages") runs the matrix
above on every push to `main`, on `v*` tags, and on manual dispatch. The
`validate-extensions` job checks every built-in `manifest.json` parses as JSON
before packaging. On a `v*` tag, a `release` job publishes a GitHub Release.
See `DEPLOYMENT.md`.

## Regenerating placeholder icons (optional)

```bash
npm run icons                       # assets/icons/*
python3 scripts/make_ui_icons.py    # assets/icons/ui/*
```

Swap in your own artwork by overwriting the files in place (keep filenames and
ideally the same pixel sizes).
