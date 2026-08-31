# Governance

This document describes how decisions are made in the Surf FED project and how
the maintainer team operates. It is intentionally lightweight.

## 1. Project status

Surf FED is a community-driven, open-source Electron browser with built-in
Chrome-style extension support. It is currently maintained by a small core
team (see `MAINTAINERS.md`).

## 2. Roles

| Role | Who | What they can do |
|------|-----|------------------|
| **Contributor** | Anyone | Open issues, discussions, and pull requests. |
| **Triage** | Maintainers | Label, close, and reassign issues; edit issue templates. |
| **Maintainer** | Listed in `MAINTAINERS.md` | Review and merge PRs, push tags, manage releases, manage repo settings. |
| **Security contact** | `security@fedpromptly.com` | Receives and triages private vulnerability reports (`SECURITY.md`). |

## 3. How decisions are made

1. **Small changes** (bug fixes, docs, new built-in extensions that don't
   change core APIs): any maintainer can merge after a passing CI run and at
   least one approving review.

2. **Larger changes** (new core features, changes to the extension-loading
   model, build/CI restructures, dependency additions): open a Discussion or
   an RFC-style issue first. A maintainer summarizes consensus; if there's no
   objection within a week, the change proceeds via normal PRs.

3. **Architectural changes** (anything touching `<webview>` usage,
   `session.loadExtension()`, the `persist:surf-fed` partition, or a move to a
   different desktop runtime such as Tauri): require explicit maintainer
   agreement and a written decision record in `ADR.md`. The feasibility study
   in `SURF_FED_TAURI_FEASIBILITY_STUDY.md` is the template for this kind of
   analysis.

## 4. Releases

- Releases are cut by pushing a Git tag matching `v*` (e.g. `v1.2.0`).
- The `Build Desktop Packages` workflow builds Windows, macOS, and Linux
  artifacts and creates a GitHub Release with auto-generated notes.
- Versioning follows [Semantic Versioning](https://semver.org/). See
  `CHANGELOG.md` for the history.

## 5. Becoming a maintainer

Maintainers are invited from consistent, trusted contributors. There is no
formal application — if you've been reviewing PRs, triaging issues, and
submitting high-quality changes over time, a current maintainer may nominate
you. Nominations are discussed among maintainers and confirmed by consensus.

## 6. Stepping down

Maintainers may step down at any time. Past maintainers are thanked in the
**Emeritus** section of `MAINTAINERS.md`.

## 7. Code of Conduct

All interactions in this project are governed by `CODE_OF_CONDUCT.md`.
Enforcement is the responsibility of the maintainer team.
