# Project Summary

**Surf FED** is a lightweight, tabbed web browser built with **Electron** that
ships with built-in support for **Chrome-style extensions (Manifest V2/V3)**.
It is designed to be a small, hackable browser where you can drop in your own
unpacked Chrome extensions and have them actually run on the pages you browse.

## What makes it work

The browser is built on two Electron-specific pillars that together make
extensions function on real web pages:

1. **Tabs are `<webview>` elements**, each bound to a shared Chromium session
   partition called `persist:surf-fed`.
2. **Extensions are loaded into that same partition** with Electron's
   `session.loadExtension()`, so content scripts, `declarativeNetRequest`
   rules, and popups execute inside the pages you visit.

Because extensions need real files on disk at runtime, the build config
unpacks `extensions/builtin/**/*` out of `app.asar`, and `main.js` resolves
that path defensively across dev and packaged builds.

## What ships with it

Four built-in Manifest V3 extensions under `extensions/builtin/`:

| Extension | What it does |
|-----------|--------------|
| **Ad Blocker** | Blocks common ad/tracking domains via `declarativeNetRequest`. |
| **Dark Reader** | Darkens pages via a content script at `document_start`. |
| **FED-GRAM** | Toolbar popup that downloads images from public Instagram posts. |
| **Page Info** | Popup showing the active page's title, URL, description, link/image counts. |

## Platforms

Built and shipped for **Windows** (portable `.exe`), **macOS** (`.dmg`), and
**Linux** (`.AppImage`) via GitHub Actions. See `BUILD.md` and
`DEPLOYMENT.md`.

## Key facts

- **Runtime:** Electron 28.x, Chromium-based.
- **Entry point:** `main.js` (main process) → `preload.js` (bridge) →
  `index.html` + `renderer.js` (UI).
- **Security posture:** `contextIsolation: true`, `nodeIntegration: false`,
  preload bridge is the only Node channel. This is a starting point, not a
  security-hardened browser — review Electron's security docs before
  serious use.
- **Versioning:** Semantic Versioning; releases cut by pushing a `v*` tag.
- **License:** see `LICENSE` and `COPYING.md`.

## Where to go next

| If you want to… | Read |
|------------------|------|
| Install it | `INSTALL.md` |
| Use it | `usage.md`, `EXTENSIONS.md` |
| Build from source | `BUILD.md` |
| Understand releases | `DEPLOYMENT.md` |
| Contribute | `CONTRIBUTING.md`, `CLAUDE.md`, `AGENTS.md` |
| Know the architecture decisions | `ADR.md` |
| See what's planned | `ROADMAP.md` |
| Report a bug / request a feature | `.github/ISSUE_TEMPLATE/` |
| Ask a question | Discussions (`.github/DISCUSSION_WELCOME_README.md`) |
| Understand the Tauri question | `SURF_FED_TAURI_FEASIBILITY_STUDY.md` |
