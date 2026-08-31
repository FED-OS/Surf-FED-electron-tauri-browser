# Roadmap

This is a living document. Priorities shift based on contributor interest and
user feedback (Discussions). Nothing here is a hard commitment.

## Now (v1.1.x maintenance)

- Keep the `Build Desktop Packages` CI green on Windows, macOS, and Linux.
- Triage and fix reported bugs from the issue tracker.
- Improve robustness of the `builtinExtDir()` resolver for edge-case installs.
- Code-sign and notarize macOS builds (currently unsigned `.dmg`).

## Next (v1.2)

- **Settings page** — persist dark mode, default search engine, startup page,
  and homepage across restarts.
- **Tab improvements** — pin tabs, mute tabs, drag-to-reorder, tab restore on
  restart.
- **Downloads manager** — visible download list with progress and open/show.
- **Bookmarks** — basic bookmark bar with import/export.
- **Keyboard shortcuts** — configurable, with sane defaults (Ctrl/Cmd+T, +W,
  +R, +L, etc.).

## Later (v1.3+)

- **Extension marketplace UI** — browse/install community extensions from
  Discussions or a curated list.
- **Per-extension permissions UI** — surface what each extension can access.
- **Privacy features** — per-site cookie/container isolation, clear-on-exit.
- **Updater** — auto-update via GitHub Releases (electron-updater).
- **Cross-platform consistency pass** — keyboard shortcuts, native menus,
  file dialogs.

## Investigating (no commitment)

- **Tauri/macOS track** — a smaller, native macOS binary. Blocked on
  reimplementing the Chrome MV3 extension runtime for WebKit. See
  `SURF_FED_TAURI_FEASIBILITY_STUDY.md` and `TAURI_MIGRATION_CHECKLIST.md`.
  Decision recorded in `ADR.md` (ADR-003: staying on Electron for now).
- **MV2 deprecation handling** — ensure user-loaded MV2 extensions still work
  as Electron's Chromium advances.

## How to influence the roadmap

Open a Discussion under the **Ideas** category, or a feature-request issue.
Sustained contributors who help implement a roadmap item get priority say in
its design. See `GOVERNANCE.md` for how decisions are made.
