# Surf FED — Add Tauri macOS Project (Option B)

## Setup
- [x] Inspect workspace and locate the actual project source
- [x] Confirm icon assets exist (icon-1024x1024.png available)
- [x] Create a clean working copy of the project at project root

## Tauri project files
- [x] package.json — add `tauri` script + `@tauri-apps/cli` devDep
- [x] src-tauri/Cargo.toml
- [x] src-tauri/build.rs
- [x] src-tauri/src/main.rs
- [x] src-tauri/tauri.conf.json
- [x] src-tauri/capabilities/default.json
- [x] src-tauri/icons/ — icon.png (copied), icon.icns (real ICNS generated), icon.ico (multi-res generated)

## Workflow
- [x] build.option-b-tauri-mac-electron-win-linux.yml — replaced + copied into project .github/workflows/

## Verify
- [x] Validate all new files are in place and well-formed (JSON, TOML, Rust)
- [x] Confirm workflow's verify steps would pass against new structure (all PASS)
- [x] Package complete project into zip
