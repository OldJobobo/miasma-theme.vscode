# Miasma VS Code Theme Extension

VS Code theme extension for `Miasma`.
Colors are derived from `miasma.nvim` (primarily `extras/miasma.toml` and `colors/miasma.vim`).

## Install

Install from the VS Code Marketplace (after first publish):

```bash
code --install-extension oldjobobo.miasma-theme
```

Or install the bundled `.vsix` directly:

```bash
code --install-extension ./miasma-theme-*.vsix
```

## Rebuild VSIX (maintainers)

Requires `@vscode/vsce`.

```bash
npm run package
```

## Versioning

This project uses Semantic Versioning (`MAJOR.MINOR.PATCH`).

- `npm run version:patch`: backward-compatible fixes (`0.0.1` -> `0.0.2`)
- `npm run version:minor`: backward-compatible features (`0.0.1` -> `0.1.0`)
- `npm run version:major`: breaking changes (`0.0.1` -> `1.0.0`)

Each command updates `package.json`, creates a Git commit, and creates a Git tag.
Use `npm run release:patch|minor|major` to version and build the VSIX in one step.
