# Repository Guidelines

## Project Structure & Module Organization
- `package.json`: VS Code extension manifest (name, publisher, engine compatibility, theme contribution).
- `themes/base16-miasma-color-theme.json`: primary source of truth for UI and syntax colors.
- `README.md`: quick install/build notes for contributors.
- `PUBLISHING.md`: release and Marketplace workflow.
- `miasma-theme-*.vsix`: packaged artifact; do not edit manually.

Keep changes focused: color edits belong in `themes/`, extension metadata changes belong in `package.json`.

## Build, Test, and Development Commands
- `vsce package --no-yarn --no-dependencies`: build a local `.vsix` from the current sources.
- `unzip -l miasma-theme-*.vsix`: inspect package contents before publishing.
- `code --install-extension oldjobobo.miasma-theme`: install from Marketplace for verification.
- `code --install-extension ./miasma-theme-*.vsix`: install a local package for manual verification.
- `vsce publish patch --no-yarn --no-dependencies` (or `minor` / `major`): publish a version bump.

Run commands from the repository root unless a release note says otherwise.

## Coding Style & Naming Conventions
- Use 2-space indentation in JSON files.
- Keep keys grouped logically: metadata first, then token/UI colors.
- Prefer stable, descriptive color token names and keep Base16 intent intact.
- File naming pattern: lowercase with hyphens (example: `base16-miasma-color-theme.json`).
- Validate JSON before commit (example: `jq . themes/base16-miasma-color-theme.json >/dev/null`).

## Testing Guidelines
This repository has no automated test suite yet. Required checks are manual:
- Package builds successfully with `vsce`.
- VSIX includes `extension/package.json` and theme JSON.
- Theme loads in VS Code and key scopes (comments, strings, keywords, errors) remain readable.

Document manual verification steps in the PR description.

## Commit & Pull Request Guidelines
Git history is not available in this workspace snapshot, so use this convention consistently:
- Commit format: `type(scope): summary` (examples: `feat(theme): tune string contrast`, `fix(metadata): correct displayName`).
- Keep commits atomic; avoid mixing metadata and palette refactors.
- PRs should include: purpose, before/after screenshots, tested VS Code version, and any Marketplace impact.
- Link related issue(s) when applicable and note version bump intent (`patch`/`minor`/`major`).
