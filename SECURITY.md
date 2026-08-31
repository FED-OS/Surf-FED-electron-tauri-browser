# Security Policy

## Supported Versions

We release patches for security vulnerabilities in the latest stable version
only.

| Version | Supported |
| :--- | :--- |
| Latest stable (v1.x) | ✅ Yes |
| Older versions | ❌ No |

## Reporting a Vulnerability

If you discover a security vulnerability, please **do not** open a public
issue.

Instead, send a private report via one of:

- **GitHub Security Advisories:** use the **"Report a vulnerability"** feature
  under the **Security** tab of the repository (preferred).
- **Email:** `security@fedpromptly.com`

Please include:

- A clear description of the vulnerability.
- Steps to reproduce it.
- Any potential impact or exploit scenario.
- Your Surf FED version and OS.

We will respond within **48 hours** to confirm receipt and will work on a fix
as soon as possible. Once fixed, we will credit you in the release notes
unless you prefer to remain anonymous.

## Disclosure Policy

- We will privately acknowledge the reporter.
- We will fix the issue and test it.
- We will release a patch and publicly disclose the vulnerability **after**
  the fix is released.

## Security posture of Surf FED

Surf FED is a starting point, not a security-hardened browser. It does keep
sensible defaults:

- `contextIsolation: true` — the renderer is isolated from Node.
- `nodeIntegration: false` — no Node APIs in web pages.
- A `preload.js` bridge (`window.electronAPI`) is the only sanctioned channel
  to native/Electron APIs.

However, before relying on Surf FED for sensitive browsing, review
Electron's [security guidelines](https://www.electronjs.org/docs/latest/tutorial/security).
Loading third-party Chrome extensions is powerful but carries risk — only
load extensions you trust, since content scripts and rules run on the pages
you visit.

Thank you for helping keep Surf FED secure! 🔒
