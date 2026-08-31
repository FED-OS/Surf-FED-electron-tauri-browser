<!--
  Root-level pull request template (GitHub also uses .github/PULL_REQUEST_TEMPLATE.md).
  This copy keeps the same content for forks/mirrors that look in the repo root.
-->

## Summary

<!-- What does this change do, in one or two sentences? -->

## Motivation

<!-- Why is this change needed? Link related issues, e.g. "Closes #123". -->

## Type of change

- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation
- [ ] New / updated built-in extension
- [ ] Build / CI change

## Changes

-

## Testing

- OS tested:
- Steps:

```
1.
2.
3.
```

## Extension checklist (only if you added/changed an extension)

- [ ] Folder is under `extensions/builtin/<name>/` with a valid `manifest.json`
- [ ] Folder listed in `package.json` -> `build.files` and `asarUnpack`
- [ ] Tested via the 🧩 Extensions Manager
- [ ] Documented in `EXTENSIONS.md`

## Checklist

- [ ] Read `CONTRIBUTING.md` and `CODE_OF_CONDUCT.md`
- [ ] `npm install && npm start` launches with no console errors
- [ ] No `node_modules/`, `dist/`, or build artifacts committed
- [ ] Did not break the `persist:surf-fed` session partition invariant
