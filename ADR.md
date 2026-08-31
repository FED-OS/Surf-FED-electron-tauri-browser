# Architecture Decision Records (ADR)

This file records the significant architectural decisions made in Surf FED,
in reverse chronological order. New decisions go at the top.

The template for a new record:

```
## ADR-NNN: Title
- **Date:** YYYY-MM-DD
- **Status:** Proposed | Accepted | Deprecated | Superseded by ADR-NNN
- **Context:** Why this decision is being considered.
- **Decision:** What we decided.
- **Consequences:** What follows (positive, negative, neutral).
```

---

## ADR-003: Stay on Electron for all three desktop platforms
- **Date:** 2025-08-30
- **Status:** Accepted
- **Context:** We considered splitting the build so macOS uses Tauri (smaller
  binary, native Rust backend) while Windows and Linux keep Electron. A full
  study was performed by reading every source file.
- **Decision:** Keep Electron on Windows, macOS, and Linux. Do not move macOS
  to Tauri.
- **Consequences:**
  - Positive: All four built-in Chrome MV3 extensions (`ad-blocker`,
    `dark-reader`, `fed-gram`, `page-info`) continue to work on every
    platform, because `session.loadExtension()` and the Chromium engine are
    preserved.
  - Positive: One codebase, one `<webview>` tab engine, one IPC bridge — no
    platform-specific runtime fork.
  - Negative: Larger macOS binary (~150 MB) than a Tauri build would be.
  - Future: A Tauri/macOS track remains documented in
    `SURF_FED_TAURI_FEASIBILITY_STUDY.md` and
    `TAURI_MIGRATION_CHECKLIST.md` if we later choose to invest in a WebKit
    reimplementation of extensions.
- **Reference:** `SURF_FED_TAURI_FEASIBILITY_STUDY.md`,
  `build.option-a-electron-all.yml`.

## ADR-002: Load built-in extensions from `app.asar.unpacked`
- **Date:** (early project, carried into v1.1.0)
- **Status:** Accepted
- **Context:** Electron's `session.loadExtension()` requires extension files
  to exist as real files on disk; it cannot read from inside `app.asar`.
- **Decision:** Configure electron-builder with
  `asarUnpack: ["extensions/builtin/**/*"]` and implement a defensive
  `builtinExtDir()` resolver in `main.js` that checks several candidate paths
  (unpacked, resourcesPath, dev) and returns the first that exists.
- **Consequences:**
  - Positive: Extensions load reliably in packaged Windows/macOS/Linux builds
    despite path/packaging differences.
  - Negative: Two copies of extension files on disk (assembled + unpacked);
    acceptable for the small extension sizes involved.
  - Maintenance: Any new shipped extension must be added to `build.files` and
    kept inside `asarUnpack`.

## ADR-001: Use a single shared session partition for webviews + extensions
- **Date:** (v1.1.0 extension support)
- **Status:** Accepted
- **Context:** For Chrome-style extensions loaded via
  `session.loadExtension()` to actually run on the pages a user browses, the
  extensions and the page renderers must share a Chromium session.
- **Decision:** Define `WEBVIEW_PARTITION = 'persist:surf-fed'` and use it both
  when loading extensions (`session.fromPartition(WEBVIEW_PARTITION)`) and on
  every `<webview>` (`setAttribute('partition', WEBVIEW_PARTITION)`).
- **Consequences:**
  - Positive: Content scripts, `declarativeNetRequest` rules, and popups run
    on real browsed pages.
  - Invariant: `WEBVIEW_PARTITION` must remain identical in `main.js` and
    `renderer.js`; diverging silently breaks extensions.
  - Positive: The `persist:` prefix means cookies/storage survive restarts.
