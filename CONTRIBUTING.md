# Contributing to Surf FED

Thanks for your interest in contributing! Surf FED is an Electron-based web
browser with built-in Chrome-style extension support, and there are plenty of
ways to help — code, extensions, docs, triage, and testing.

## Quick start

```bash
git clone https://github.com/Surf-FED/Surf-FED-Internet-Browser-With-Extensions.git
cd Surf-FED-Internet-Browser-With-Extensions
npm install
npm start
```

Requirements: Node.js 18+ and npm. See `INSTALL.md` for platform specifics.

## Ways to contribute

- **Bug reports & feature requests** — use the issue templates under
  `.github/ISSUE_TEMPLATE/`.
- **Questions & ideas** — use Discussions, not issues (see
  `.github/DISCUSSION_WELCOME_README.md`).
- **Code fixes** — pick an issue labeled `good first issue` or `help wanted`.
- **Extensions** — add a built-in extension under `extensions/builtin/` or
  share one in Discussions. Read `EXTENSIONS.md` first.
- **Docs** — typos and clarifications are always welcome.

## Before you write code

Please read `CLAUDE.md` (or `AGENTS.md`) for the architecture rules. The two
non-negotiable invariants:

1. **`WEBVIEW_PARTITION` (`persist:surf-fed`) must stay identical in
   `main.js` and `renderer.js`.** Extensions load into that partition and
   webviews bind to it; if they diverge, extensions silently stop working.
2. **`contextIsolation: true` and `nodeIntegration: false` stay on.** The
   preload bridge (`window.electronAPI`) is the only sanctioned channel to
   Node APIs.

## Adding a built-in extension

1. Create `extensions/builtin/<your-extension>/` containing a valid
   `manifest.json` (Manifest V3 preferred; see existing extensions for
   patterns). CI validates that every `extensions/builtin/*/manifest.json`
   parses as JSON.
2. Add the folder to `package.json` -> `build.files` and keep it in
   `asarUnpack` — `session.loadExtension()` needs real files on disk, which
   is why `asarUnpack: ["extensions/builtin/**/*"]` exists.
3. Document the extension in `EXTENSIONS.md`.
4. Test it: run `npm start`, open the 🧩 Extensions Manager, and confirm it
   loads and runs on a real page.

## Adding a UI feature

1. Add the control in `index.html`.
2. Style it in `styles.css`.
3. Add the logic in `renderer.js`.
4. If it needs Node/Electron capabilities, expose a handler in `main.js`
   via `ipcMain` and bridge it in `preload.js` under `window.electronAPI`.

## Commit & branch conventions

- Branch from `main`: `git checkout -b feat/short-description`.
- Commits: short imperative summary, optionally scoped:
  `feat(extensions): add grayscale-reader built-in`.
- Keep PRs focused; one logical change per PR.

## Pull request checklist

- [ ] `npm install && npm start` launches with no console errors.
- [ ] No new files under `node_modules/`, `dist/`, or build artifacts.
- [ ] If you added an extension, `manifest.json` is valid and the folder is in
      `build.files` + `asarUnpack`.
- [ ] You filled in the PR template (`.github/PULL_REQUEST_TEMPLATE.md`).
- [ ] You read `CODE_OF_CONDUCT.md`.

## Building & releasing

Building locally: `npm run dist -- --win|--mac|--linux` (must run on the
matching OS). Releases are produced automatically by the
`Build Desktop Packages` workflow when a `v*` tag is pushed — contributors do
not need to build installers themselves. See `BUILD.md` and `DEPLOYMENT.md`.

## Security

Do not open a public issue for security vulnerabilities. Use GitHub Security
Advisories or email `security@fedpromptly.com`. See `SECURITY.md`.

## Licensing

By contributing, you agree your contributions are licensed under the project's
license (see `LICENSE` and `COPYING.md`).
