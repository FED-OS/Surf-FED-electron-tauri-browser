# CLAUDE.md

Guidance for Claude (and other AI coding agents) working in this repository.

## What this project is

Surf FED is an **Electron-based web browser** (`package.json` description:
"an Electron web browser with custom extension support"). It is NOT a generic
desktop app and NOT a Tauri app. The two pillars of the architecture are:

1. **Chromium rendering via `<webview>` tags** — each tab is a `<webview>`
   element (Electron-only) bound to the session partition `persist:surf-fed`.
2. **Chrome MV3 extensions via `session.loadExtension()`** — built-in
   extensions live in `extensions/builtin/` and are loaded into the same
   partition so their content scripts/rules run on browsed pages.

Do not propose replacing `<webview>` or `session.loadExtension()` with
Tauri/WebKit equivalents without flagging that it breaks extensions. The full
analysis lives in `SURF_FED_TAURI_FEASIBILITY_STUDY.md`.

## File map

| File | Role | Notes |
|------|------|-------|
| `main.js` | Electron main process | Window creation, IPC handlers, extension loading, `builtinExtDir()` path resolution. ~428 lines, Electron-only APIs throughout. |
| `preload.js` | Context bridge | Exposes `window.electronAPI` (window chrome + `extensions.*`). Electron-only. |
| `renderer.js` | Browser UI | Tabs, navigation, Extensions Manager UI. Uses `<webview>` + `window.electronAPI`. |
| `index.html` | UI shell | Toolbar, tab bar, webview container, extensions panel. |
| `styles.css` | UI styling | |
| `package.json` | Scripts + electron-builder config | `build.files` lists extension folders; `asarUnpack` extracts them so `session.loadExtension()` can read real files. |
| `extensions/builtin/*` | Built-in Chrome MV3 extensions | `ad-blocker`, `dark-reader`, `fed-gram`, `page-info`. |
| `.github/workflows/build.yml` | CI build | See `BUILD.md` and the provided `build.option-a-electron-all.yml`. |

## Architecture rules (respect these)

- **Electron-only APIs are load-bearing.** `session`, `ipcMain`,
  `contextBridge`, `<webview>`, `dialog`, `shell` are intentional. Don't
  "modernize" them away.
- **The session partition is sacred.** Every `<webview>` uses
  `persist:surf-fed` (defined as `WEBVIEW_PARTITION` in both `main.js` and
  `renderer.js`). Extensions load into that same partition. If you change one,
  change both.
- **Extensions must be real files on disk at runtime.** That's why
  `package.json` has `asarUnpack: ["extensions/builtin/**/*"]` and `main.js`
  has the `builtinExtDir()` multi-candidate resolver. Any new shipped
  extension must be added to `build.files` and kept in `asarUnpack`.
- **`contextIsolation: true`, `nodeIntegration: false`.** Keep these. The
  preload bridge is the only sanctioned channel to Node.
- **Manifest validation.** CI (`build.yml`) validates every
  `extensions/builtin/*/manifest.json` parses as JSON. Any new extension
  folder must include a valid `manifest.json` or CI fails.

## How to run / build

```bash
npm install
npm start                 # run in dev
npm run dist -- --win     # build Windows portable EXE
npm run dist -- --mac     # build macOS .dmg
npm run dist -- --linux   # build Linux .AppImage
```

See `BUILD.md` and `INSTALL.md` for details.

## Conventions

- Match the existing code style of the file you're editing — there is no
  formatter config yet.
- Tabs are the indentation in `renderer.js`/`main.js`'s newer sections; keep
  consistency within a function.
- Commit messages: short imperative summary, e.g.
  `feat(extensions): add grayscale-reader built-in`.
- Do not commit `node_modules/`, `dist/`, or `*.app`/`*.dmg`/`*.exe`/`*.AppImage`.

## When in doubt

- Read `CONTRIBUTING.md` before opening a PR.
- Read `SURF_FED_TAURI_FEASIBILITY_STUDY.md` before any Tauri/WebKit proposal.
- Read `EXTENSIONS.md` before touching extension code.
- Security issues go through private advisories, never a public issue (see
  `SECURITY.md`).
