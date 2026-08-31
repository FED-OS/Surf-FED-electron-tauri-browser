# Usage Guide — Surf FED

A quick tour of everyday use. For installing, see `INSTALL.md`; for
extensions, see `EXTENSIONS.md`.

## Launching

- **Windows:** double-click `Surf FED.exe`.
- **macOS:** open **Surf FED** from Applications (see `INSTALL.md` for the
  Gatekeeper workaround on unsigned builds).
- **Linux:** `chmod +x "Surf FED.AppImage" && ./Surf FED.AppImage`.

You'll start on a Google search tab.

## The toolbar

| Control | What it does |
|---------|--------------|
| ◀ Back | Go back in the current tab's history. |
| ▶ Forward | Go forward. |
| ⟳ Reload | Reload the current page. |
| Address bar | Type a URL or a search term (non-URL input becomes a Google search) and press **Enter**. |
| ＋ New tab | Open a new tab. |
| 🌙 Dark mode | Toggle a dark theme for the browser UI. |
| 🧩 Extensions | Open the Extensions Manager. |

## Tabs

- **New tab:** click ＋. Each tab is a separate page with its own
  back/forward history and its own webview.
- **Switch tabs:** click a tab in the tab bar.
- **Close a tab:** click the × on the tab. If you close the last tab, a new
  one opens automatically.

## Address bar

- If your input looks like a URL (contains a `.`), it's loaded as a URL.
- If it doesn't look like a URL, it's sent to Google as a search.
- Inputs without a scheme are prefixed with `https://`.

## Dark mode

Click 🌙 to toggle a dark browser UI. (This toggles the chrome's dark theme;
the built-in **Dark Reader** extension darkens *web pages* separately —
enable it from the Extensions Manager if you want darkened pages.)

## Extensions Manager (🧩)

Open with the 🧩 button. You can:

- **Enable/disable** any extension with the toggle. Choices persist.
- **Load an unpacked extension** you wrote — pick the folder containing
  `manifest.json`.
- **Open your extensions folder** to drop folders in by hand, then **Reload
  list**.
- **Remove** a user-added extension (built-in ones can only be disabled).

See `EXTENSIONS.md` for the full guide and supported manifest features.

## Built-in extensions at a glance

| Extension | On by default? | Effect |
|-----------|----------------|--------|
| Ad Blocker | yes | Blocks common ad/tracking domains. |
| Dark Reader | yes | Darkens pages via a content script. |
| FED-GRAM | yes | Popup to download images from public Instagram posts. |
| Page Info | yes | Popup showing the active page's metadata. |

## Tips

- Reload a page after enabling a content-script extension — content scripts
  attach on load.
- Your personal extensions live in the OS user-data folder and survive app
  updates.
- This is a starting-point browser; for sensitive work, mind the security
  note in `NOTICE.md` and `SUMMARY.md`.

## Troubleshooting

See `INSTALL.md` (install/Gatekeeper/AppImage issues) and `FAQ.md`.
