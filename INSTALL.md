# Installation

How to install Surf FED on each platform.

## Option 1 — Download a prebuilt installer (recommended)

1. Go to the [Releases page](https://github.com/Surf-FED/Surf-FED-Internet-Browser-With-Extensions/releases).
2. Download the file for your OS:
   - **Windows**: `Surf FED.exe` (portable — no installer, just run it).
   - **macOS**: `Surf FED.dmg`.
   - **Linux**: `Surf FED.AppImage`.
3. See the platform notes below for first-run caveats (especially macOS
   Gatekeeper).

### Windows

- Just double-click `Surf FED.exe`. It's a portable build — no installation
  step. You can place it anywhere.
- SmartScreen may warn for unsigned builds; click **More info → Run anyway**.

### macOS

- Open `Surf FED.dmg` and drag **Surf FED** to **Applications**.
- The current build is **unsigned**, so Gatekeeper will block the first
  launch. To open it anyway:
  - Right-click **Surf FED** → **Open** → **Open** in the dialog, **or**
  - Run once in Terminal:
    ```bash
    xattr -dr com.apple.quarantine "/Applications/Surf FED.app"
    ```
- Signed + notarized builds are planned (see `ROADMAP.md` / `DEPLOYMENT.md`).

### Linux

- Make the AppImage executable and run it:
  ```bash
  chmod +x "Surf FED.AppImage"
  ./Surf FED.AppImage
  ```
- On first run, some distributions prompt to integrate the AppImage into your
  application menu — accept if you want a launcher entry.
- If you built from source on Ubuntu 24.04, the runtime libraries listed in
  `BUILD.md` are required.

## Option 2 — Run from source (developers)

Requirements: Node.js 18+ and npm.

```bash
git clone https://github.com/Surf-FED/Surf-FED-Internet-Browser-With-Extensions.git
cd Surf-FED-Internet-Browser-With-Extensions
npm install
npm start
```

See `BUILD.md` for how to package your own installer, and `CONTRIBUTING.md`
if you plan to contribute.

## Verifying it works

After launch you should see the Surf FED window with a toolbar (back, forward,
reload, address bar, new tab, dark mode, extensions) and a single Google tab.
Click the 🧩 button to open the Extensions Manager — the four built-in
extensions (Ad Blocker, Dark Reader, FED-GRAM, Page Info) should be listed and
enabled. See `usage.md` for everyday use.

## Troubleshooting

- **App won't open on macOS** — see the Gatekeeper note above.
- **Extensions missing in the manager** — make sure you're running a complete
  build (from source: `npm install` first; from a release: re-download the
  full artifact). Built-in extensions ship inside the app.
- **Linux AppImage won't run** — run `chmod +x` on it; on some distros you may
  also need FUSE (`sudo apt install libfuse2`).
- **"Extension directory not found" on a custom build** — you likely packaged
  without `asarUnpack`; re-read the packaging section of `BUILD.md`.
