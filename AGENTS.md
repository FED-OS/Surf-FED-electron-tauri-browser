# AGENTS.md

Operating guide for AI agents (Claude, Codex, Cursor, Aider, etc.) working on
this repo. Start here, then read `CLAUDE.md` for the deeper architecture notes.

## TL;DR rules

1. **This is an Electron browser that loads Chrome MV3 extensions.** The
   `<webview>` tag and `session.loadExtension()` are core, not legacy.
2. **Do not rewrite to Tauri/WebKit** without explicit user instruction — it
   breaks the extension system. (See `SURF_FED_TAURI_FEASIBILITY_STUDY.md`.)
3. **Never commit** `node_modules/`, `dist/`, build artifacts, or `.DS_Store`.
4. **Run `npm install && npm start`** to confirm the app launches before
   declaring a UI/main-process change done.
5. **Security issues are private.** Do not open a public issue; use GitHub
   Security Advisories (see `SECURITY.md`).

## Where things live

```
main.js                 Electron main (window, IPC, extension loading)
preload.js              contextBridge -> window.electronAPI
renderer.js             Tab engine + Extensions Manager UI (uses <webview>)
index.html              UI shell
styles.css              UI styles
package.json            scripts + electron-builder config (asarUnpack for exts)
extensions/builtin/     shipped Chrome MV3 extensions (validated by CI)
.github/workflows/      CI (Build Desktop Packages)
```

## The one invariant to never break

`WEBVIEW_PARTITION` must be identical in `main.js` and `renderer.js`
(`persist:surf-fed`). Extensions are loaded into that partition; webviews are
bound to it. If they diverge, extensions silently stop running on pages.

## Safe edit checklist

- [ ] Editing `main.js` IPC? Add the handler AND expose it in `preload.js`.
- [ ] Adding a UI control? Wire it in `index.html`, style in `styles.css`,
      logic in `renderer.js`.
- [ ] Adding a built-in extension? Put it in `extensions/builtin/<name>/` with
      a valid `manifest.json`, add to `package.json` `build.files` and
      `asarUnpack`, document in `EXTENSIONS.md`.
- [ ] Touching packaging? Re-read `main.js`'s `builtinExtDir()` — packaged
      builds read from `app.asar.unpacked`, not `app.asar`.

## Build commands

```bash
npm install
npm start
npm run dist -- --win|--mac|--linux   # requires the matching OS runner
```

## Don't

- Don't enable `nodeIntegration: true` or disable `contextIsolation`.
- Don't load remote URLs in the UI shell (the only remote script in
  `index.html` is the injected analytics tag; leave it unless told otherwise).
- Don't add Electron dependencies without explaining why in the PR.
- Don't "fix" the placeholder `appId` `com.yourname.browser` without being
  asked — it's a known TODO.

## References

- Architecture & Tauri feasibility: `SURF_FED_TAURI_FEASIBILITY_STUDY.md`
- Contributing: `CONTRIBUTING.md`
- Extensions: `EXTENSIONS.md`
- Build/CI: `BUILD.md`, `DEPLOYMENT.md`
- Roadmap: `ROADMAP.md`
