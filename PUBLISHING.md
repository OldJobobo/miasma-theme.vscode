# Publishing And Updating Miasma

This guide is for publishing updates to the VS Code extension:
`oldjobobo.miasma-theme`.

## Prerequisites

- You have a VS Marketplace publisher: `oldjobobo`
- You have a valid PAT with `Marketplace -> Manage`
- You are in Node LTS (recommended: Node 22 via `mise`)

## One-Time Login

```bash
npm i -g @vscode/vsce
vsce login oldjobobo
```

## Update Workflow

1. Make your theme/metadata changes.
2. Run a local package build.
3. Verify package contents.
4. Publish version bump (`patch`, `minor`, or `major`).

### 1) Package

```bash
npm run package
```

### 2) Verify VSIX Contents

```bash
unzip -l miasma-theme-*.vsix
```

Expected: includes extension files such as:
- `extension/package.json`
- `extension/themes/base16-miasma-color-theme.json`

If you only see manifest files, stop and fix your Node/vsce environment before publishing.

### 3) Publish Update

Use one of:

```bash
vsce publish patch --no-yarn --no-dependencies
vsce publish minor --no-yarn --no-dependencies
vsce publish major --no-yarn --no-dependencies
```

Or publish exact version:

```bash
vsce publish 0.0.1 --no-yarn --no-dependencies
```

## Post-Publish Checks

1. Confirm listing opens:
   - `https://marketplace.visualstudio.com/items?itemName=oldjobobo.miasma-theme`
2. In VS Code, verify the theme appears as:
   - `Miasma`

## Notes

- Run all `vsce` commands from the repository root.
- Keep extension ID stable: `oldjobobo.miasma-theme`.
- Keep `package.json` theme contribution aligned with:
  - `"label": "Miasma"`
  - `"path": "./themes/base16-miasma-color-theme.json"`

## Versioning Workflow (Recommended)

Use npm scripts to keep SemVer + Git tags consistent:

```bash
npm run version:patch   # or version:minor / version:major
```

For one command that versions and builds:

```bash
npm run release:patch   # or release:minor / release:major
```

After bumping, update `CHANGELOG.md`, then publish with `vsce publish <version|patch|minor|major> --no-yarn --no-dependencies`.
