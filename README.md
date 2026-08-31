<img width="2560" height="1440" alt="surf-fed-ui-mockup" src="https://github.com/user-attachments/assets/4493f2d8-f50c-4bc7-afbe-ff9ae4b4d6df" />

# Surf FED 🏄

A lightweight, tabbed web browser built with **Electron** that ships with
**built-in support for Chrome-style extensions (Manifest V2/V3)**. Drop in your
own unpacked Chrome extensions and they actually run on the pages you browse.

> **New in v1.1.0 — Extension support!** Surf FED can now load Chrome-style
> extensions. See **[EXTENSIONS.md](./EXTENSIONS.md)** for the full guide, or
> click the 🧩 button in the toolbar to open the Extensions Manager. Four
> built-in example extensions are included (Ad Blocker, Dark Reader, FED-GRAM,
> Page Info).

---

<a href='https://ko-fi.com/YOUR_USERNAME' target='_blank'>
    <img height='36' style='border:0px;height:36px;' src='https://ko-fi.com/img/githubbutton_sm.svg' border='0' alt='Buy Me a Coffee at ko-fi.com' />
</a>

> Replace `YOUR_USERNAME` with your Ko-fi page name.

---

## Quick start

```bash
npm install
npm start
```

Or grab a prebuilt installer from the
[Releases page](https://github.com/Surf-FED/Surf-FED-Internet-Browser-With-Extensions/releases)
— Windows portable `.exe`, macOS `.dmg`, Linux `.AppImage`. See
[INSTALL.md](./INSTALL.md).

## Highlights

- **Tabbed browsing** — each tab is a separate `<webview>` with its own
  back/forward history.
- **Chrome MV3 extensions** — loaded via Electron's `session.loadExtension()`
  into the shared `persist:surf-fed` session, so content scripts and
  `declarativeNetRequest` rules run on real pages.
- **Extensions Manager UI** — enable/disable, load unpacked extensions, open
  the extensions folder, reload, remove.
- **Four built-in extensions** — Ad Blocker, Dark Reader, FED-GRAM, Page Info.
- **Cross-platform builds** via GitHub Actions (Windows, macOS, Linux).
- **Dark mode** toggle for the browser chrome.

## Built-in extensions

| Extension | What it does |
|-----------|--------------|
| **Surf FED Ad Blocker** | Blocks common ad/tracking domains via `declarativeNetRequest`. |
| **Surf FED Dark Reader** | Darkens pages via a content script at `document_start`. |
| **FED-GRAM** | Toolbar popup that downloads images from public Instagram posts. |
| **Surf FED Page Info** | Popup showing the active page's title, URL, description, link/image counts. |

## Project layout

```
Surf-FED-Internet-Browser-With-Extensions/
├── main.js              # Electron main process (window, IPC, extension loading)
├── preload.js           # Safe bridge -> window.electronAPI
├── index.html           # Browser chrome (toolbar, tab bar, webviews, extensions panel)
├── renderer.js          # Tab management, navigation, Extensions Manager UI
├── styles.css           # Browser UI styling
├── assets/icons/        # App + toolbar icons (placeholders you can swap)
├── extensions/
│   └── builtin/         # Shipped Chrome MV3 extensions (validated by CI)
│       ├── ad-blocker/
│       ├── dark-reader/
│       ├── fed-gram/
│       └── page-info/
├── .github/workflows/   # CI ("Build Desktop Packages")
└── package.json         # Scripts + electron-builder config (asarUnpack for exts)
```

## Building distributables

```bash
npm run dist -- --win      # Windows portable EXE
npm run dist -- --mac      # macOS DMG
npm run dist -- --linux    # Linux AppImage
```

See [BUILD.md](./BUILD.md) for details and platform dependencies, and
[DEPLOYMENT.md](./DEPLOYMENT.md) for how releases are cut (push a `v*` tag).

## Replacing the placeholder icons

Overwrite files in place, keeping the same filenames and (ideally) pixel sizes:

- **App icon:** `assets/icons/icon.png` and each `icon-<size>x<size>.png`.
- **Toolbar icons:** `assets/icons/ui/` (back, forward, reload, home,
  new-tab, close) — keep 24×24 or update the `#toolbar img` size in
  `styles.css`.
- **Packaged icon** (`.ico`/`.icns`): generate from your final artwork:
  ```bash
  npx electron-icon-builder --input=assets/icons/icon.png --output=build --flatten
  ```

## Community & contributing

| Want to… | Go to |
|----------|-------|
| Ask a question / share an idea | [Discussions](https://github.com/Surf-FED/Surf-FED-Internet-Browser-With-Extensions/discussions) — see [DISCUSSION_WELCOME_README.md](./.github/DISCUSSION_WELCOME_README.md) |
| Report a bug | [Bug Report template](./.github/ISSUE_TEMPLATE/bug_report.md) |
| Request a feature | [Feature Request template](./.github/ISSUE_TEMPLATE/feature_request.md) |
| Contribute code / extensions | [CONTRIBUTING.md](./CONTRIBUTING.md) |
| Read the architecture rules | [CLAUDE.md](./CLAUDE.md) · [AGENTS.md](./AGENTS.md) |
| Understand decisions | [ADR.md](./ADR.md) |
| See what's planned | [ROADMAP.md](./ROADMAP.md) |
| Get help | [SUPPORT.md](./SUPPORT.md) · [FAQ.md](./FAQ.md) |
| Report a security issue privately | [SECURITY.md](./SECURITY.md) |

All interactions are governed by our [Code of Conduct](./CODE_OF_CONDUCT.md).

## Architecture note (the Tauri question)

Surf FED's extension system is built on Electron's `session.loadExtension()`
and `<webview>` tags, which do not exist in Tauri's WebKit. We studied moving
macOS to Tauri and decided to stay on Electron for all platforms (ADR-003) so
extensions keep working everywhere. The full analysis is in
[SURF_FED_TAURI_FEASIBILITY_STUDY.md](./SURF_FED_TAURI_FEASIBILITY_STUDY.md),
with a migration checklist in [TAURI_MIGRATION_CHECKLIST.md](./TAURI_MIGRATION_CHECKLIST.md).

## Notes

- The address bar accepts URLs or search terms (non-URL input falls back to a
  Google search).
- Each tab is a separate `<webview>` with its own back/forward history.
- This is a starting point, not a security-hardened browser — review
  Electron's [security guidelines](https://www.electronjs.org/docs/latest/tutorial/security)
  before shipping anything real.

## License

MIT — see [LICENSE](./LICENSE), [COPYING.md](./COPYING.md), and
[NOTICE.md](./NOTICE.md).
