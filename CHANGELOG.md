# Changelog

All notable changes to this project will be documented in this file.

The format is based on Keep a Changelog, and this project adheres to Semantic Versioning.

## [Unreleased]

## [0.0.2] - 2026-03-25

### Added
- Semantic highlighting support in the extension manifest and theme definition.
- Tracking document for the highlighting improvement plan.

### Changed
- Expanded semantic token coverage across common symbol categories and selected language-specific selectors.
- Improved TextMate token coverage for Rust, JavaScript/TypeScript, Python, Shell, Markdown, CSS, data formats, HTML, and diff views.
- Tuned fallback token mappings using scope coverage patterns mined from the Catppuccin VS Code themes and verified manually in representative files.

## [0.0.1] - 2026-02-27

### Added
- Initial Miasma VS Code theme extension release.
- Changelog and SemVer release scripts.

### Changed
- Standardized packaging and publish workflow to use `--no-yarn --no-dependencies`.
