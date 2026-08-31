# FAQ

## General

**What is Surf FED?**
A lightweight, tabbed web browser built with Electron that ships with built-in
support for Chrome-style extensions. You can also load your own unpacked
Chrome extensions and have them actually run on the pages you browse.

**Is it a security-hardened browser?**
No. It's a capable starting point that keeps `contextIsolation` on and
`nodeIntegration` off, but it is not hardened like a mainstream browser.
Review Electron's [security guidelines](https://www.electronjs.org/docs/latest/tutorial/security)
before relying on it for sensitive browsing.

**Which platforms are supported?**
Windows (portable `.exe`), macOS (`.dmg`), and Linux (`.AppImage`). See
`INSTALL.md`.

**Is it free / open source?**
Yes. See `LICENSE` and `COPYING.md`.

## Extensions

**What kind of extensions work?**
Unpacked Chrome extensions — Manifest V3 preferred, Manifest V2 still works.
Supported features include content scripts, background service workers (MV3)
and background pages (MV2), `action`/`browser_action` popups,
`declarativeNetRequest` rule sets, `storage`, `tabs`, `scripting`,
`activeTab`, and options pages. See `EXTENSIONS.md` for the full list.

**How do I add my own extension?**
Open the 🧩 Extensions Manager and click **＋ Load unpacked extension…**, then
select the folder that *directly contains* `manifest.json`. Or copy the
folder into your extensions folder (opened via **📂 Open extensions folder**)
and click **⟳ Reload list**.

**My extension loaded but its content script isn't running.**
Content scripts attach on page load, not retroactively. Reload the page or
open a new tab.

**My extension shows a ⚠ load error.**
The error is shown inline. Common causes: an unsupported permission or a
`manifest.json` typo. Note that blocking `chrome.webRequest` listeners need
`webRequestBlocking`, which Chromium restricts; prefer
`declarativeNetRequest` (see the built-in Ad Blocker).

**Where are my personal extensions stored?**
In the OS user-data directory (e.g.
`%APPDATA%\Surf FED\extensions\` on Windows). They survive app updates.
Enabled/disabled state lives in `extension-state/state.json`.

## Build & Tauri

**Will you switch macOS to Tauri?**
We studied it in depth and decided to stay on Electron for all platforms
(ADR-003). The reason: Surf FED's whole extension system relies on
`session.loadExtension()` and `<webview>`, neither of which exists in Tauri's
WebKit. Moving only macOS to Tauri would break every built-in extension
there. The full analysis is in `SURF_FED_TAURI_FEASIBILITY_STUDY.md`.

**How do I build installers myself?**
See `BUILD.md`. In short: `npm run dist -- --win|--mac|--linux` on the
matching OS. CI does this for releases automatically (see `DEPLOYMENT.md`).

**Why is the macOS build blocked by Gatekeeper?**
The macOS build is currently unsigned. See `INSTALL.md` for the one-line
workaround (`xattr -dr com.apple.quarantine …`). Signed + notarized builds
are on the roadmap.

## Contributing

**How do I contribute?**
Read `CONTRIBUTING.md`. Small fixes and docs are always welcome; for
architecture changes, check `ADR.md` and `CLAUDE.md`/`AGENTS.md` first.

**Can I add a built-in extension?**
Yes — put it under `extensions/builtin/<name>/` with a valid `manifest.json`,
add it to `package.json` `build.files` and `asarUnpack`, document it in
`EXTENSIONS.md`, and open a PR. CI validates manifests.

**Who maintains the project?**
See `MAINTAINERS.md` and `GOVERNANCE.md`.
