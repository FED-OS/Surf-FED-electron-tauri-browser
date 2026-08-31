# Deployment

How Surf FED gets from a commit on `main` to installers in users' hands.

## 1. Continuous build (every push to `main` and manual runs)

The **Build Desktop Packages** workflow (`.github/workflows/build.yml`) runs a
three-platform matrix:

| Job | Runner | Output artifact |
|-----|--------|-----------------|
| Electron (Windows) | `windows-2022` | `Surf FED.exe` (portable) |
| Electron (macOS) | `macos-14` | `Surf FED.dmg` |
| Electron (Linux) | `ubuntu-24.04` | `Surf FED.AppImage` |

A separate `validate-extensions` job parses every
`extensions/builtin/*/manifest.json` before any packaging step, so a malformed
manifest fails the build early.

On ordinary pushes and manual (`workflow_dispatch`) runs, artifacts are
uploaded to the workflow run (30-day retention) — no public release is
created. Download them from the Actions tab of the run.

## 2. Cutting a release

Releases are **tag-driven**. To publish:

```bash
# 1. Make sure main is green and CHANGELOG.md is updated.
# 2. Tag a semantic version:
git tag v1.2.0
git push origin v1.2.0
```

Pushing a tag matching `v*` triggers the same matrix build, then a `release`
job that:

1. Downloads all three platform artifacts.
2. Flattens `.exe`, `.dmg`, `.AppImage`, `.deb`, `.snap`, and `latest*.yml`
   into a `release/` folder.
3. Creates a GitHub Release named `Surf FED <tag>` with auto-generated notes
   and attaches the files.

That's it. The release is public immediately.

## 3. macOS signing & notarization (TODO)

Current macOS builds are **unsigned**. macOS Gatekeeper will warn or block
them. To ship a smooth public macOS build, the `macos-14` job needs:

- An Apple Developer ID Application certificate and notarization credentials
  stored as GitHub Actions secrets (`APPLE_CERTIFICATE`,
  `APPLE_CERTIFICATE_PASSWORD`, `APPLE_ID`, `APPLE_PASSWORD`,
  `APPLE_TEAM_ID`).
- `electron-builder` `mac.notarize` / signing config in `package.json`.

This is tracked in `ROADMAP.md`. Until then, users can right-click → Open (or
`xattr -dr com.apple.quarantine "Surf FED.app"`) to bypass Gatekeeper for
testing.

## 4. Intel vs Apple Silicon

`macos-14` produces an Apple Silicon (arm64) build. For Intel Mac support, add
a second target (`--x64`) in the macOS matrix step and ship both DMGs. Tracked
as a follow-up.

## 5. Auto-update (future)

Not yet implemented. When added, the `latest*.yml` files already uploaded by
the workflow are what `electron-updater` reads to detect new versions. See
`ROADMAP.md`.

## 6. Manual / local builds

Contributors do not need to produce installers to contribute. If you want to
build locally (must be on the target OS):

```bash
npm install
npm run dist -- --win      # on Windows
npm run dist -- --mac      # on macOS
npm run dist -- --linux    # on Linux
```

See `BUILD.md` for the full local-build guide.
