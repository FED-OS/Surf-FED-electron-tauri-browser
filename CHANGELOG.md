# Changelog

All notable changes to Surf FED are recorded here. The format is based on
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project
adheres to [Semantic Versioning](https://semver.org/).

## [Unreleased]

### Added
- Community & documentation scaffolding: issue templates, PR template,
  Discussions welcome README, `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`,
  `GOVERNANCE.md`, `MAINTAINERS.md`, `AUTHORS.md`, `CLAUDE.md`, `AGENTS.md`,
  `ADR.md`, `ROADMAP.md`, `BUILD.md`, `INSTALL.md`, `DEPLOYMENT.md`,
  `SUMMARY.md`, `usage.md`, `FAQ.md`, `SUPPORT.md`, `NOTICE.md`,
  `COPYING.md`, `CITATIONS.md`, and an updated `README.md` with community
  links and a Ko-fi support button.
- Tauri feasibility study (`SURF_FED_TAURI_FEASIBILITY_STUDY.md`) and
  migration checklist (`TAURI_MIGRATION_CHECKLIST.md`), plus two example CI
  workflows (`build.option-a-electron-all.yml`,
  `build.option-b-tauri-mac-electron-win-linux.yml`).

### Changed
- Decision recorded (ADR-003): stay on Electron for all three platforms;
  Tauri/macOS documented but not adopted. See the feasibility study.

## [1.1.0] — Extension support

### Added
- Built-in Chrome-style extension support via Electron's
  `session.loadExtension()`, loaded into the shared `persist:surf-fed`
  session partition so extensions run on browsed pages.
- Extensions Manager UI (🧩 toolbar button): list, enable/disable, load
  unpacked extension, open extensions folder, reload list, remove.
- Four built-in Manifest V3 extensions: Ad Blocker (`declarativeNetRequest`),
  Dark Reader (content script), FED-GRAM (Instagram image downloader popup),
  Page Info (page metadata popup).
- Persistent enabled/disabled state in `extension-state/state.json`.
- `asarUnpack` for `extensions/builtin/**/*` so packaged builds can load
  extensions from real files on disk; defensive `builtinExtDir()` resolver.

## [1.0.0] — Initial browser

### Added
- Tabbed Electron browser: back/forward/reload, address bar with URL-or-search
  fallback to Google, new-tab/close-tab, dark-mode toggle.
- Placeholder icon set (`assets/icons/` and `assets/icons/ui/`) with
  regeneration scripts.
- electron-builder packaging config for Windows (portable), macOS (dmg),
  Linux (AppImage).
- GitHub Actions workflow to build and release.
