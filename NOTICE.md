# NOTICE

This product includes software developed by the following projects:

## Surf FED Internet Browser

- **Project:** Surf FED — an Electron web browser with custom extension support
- **Version:** 1.1.0
- **License:** See `LICENSE` and `COPYING.md`.

## Third-party software

Surf FED is built on and/or bundles the following open-source software. We are
grateful to these projects and their contributors.

### Runtime & build

- **Electron** — © Electron contributors. Licensed under the MIT License.
  <https://www.electronjs.org/>
- **electron-builder** — © electron-builder contributors. Licensed under the
  MIT License. <https://www.electron.build/>
- **Chromium** — © The Chromium Authors. Licensed under the BSD-style
  Chromium license. Bundled as part of Electron.
- **Node.js** — © OpenJS Foundation and Node.js contributors. Licensed under
  the MIT License. <https://nodejs.org/>

### Bundled extensions

- **Surf FED Ad Blocker** (`extensions/builtin/ad-blocker/`) — uses a
  `declarativeNetRequest` ruleset. Bundled with Surf FED.
- **Surf FED Dark Reader** (`extensions/builtin/dark-reader/`) — content
  script. Bundled with Surf FED.
- **Surf FED Page Info** (`extensions/builtin/page-info/`) — popup. Bundled
  with Surf FED.
- **FED-GRAM** (`extensions/builtin/fed-gram/`) — Instagram image downloader,
  originally a Python + Streamlit application. The original `app.py`,
  `requirements.txt`, `README.md`, and `LICENSE` are preserved under
  `extensions/builtin/fed-gram/original/` for reference. See that folder's
  `LICENSE` for the original project's license terms.

## Disclaimer

Surf FED is a starting point, not a security-hardened browser. It keeps
`contextIsolation` enabled and `nodeIntegration` disabled, but users should
review Electron's [security guidelines](https://www.electronjs.org/docs/latest/tutorial/security)
before relying on it for sensitive browsing.

## Trademarks

Any product names, logos, brands, and other trademarks referred to in this
project are the property of their respective trademark holders and are not
affiliated with Surf FED. Use of these names is for identification and
description only.
