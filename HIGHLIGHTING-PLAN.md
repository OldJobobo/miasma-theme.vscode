# Highlighting Improvement Plan

This plan covers the best practical improvements available in this repository as it exists today.

The key constraint from the VS Code docs is that syntax highlighting has two parts:
- Tokenization: provided by TextMate grammars or semantic token providers
- Theming: provided by a color theme mapping scopes and semantic token selectors to colors/styles

This extension currently contributes a theme, not language grammars or a semantic token provider. That means the improvement path here is to make `tokenColors` and `semanticTokenColors` stronger, not to change tokenization behavior.

Sources:
- https://code.visualstudio.com/api/language-extensions/syntax-highlight-guide
- https://code.visualstudio.com/api/language-extensions/semantic-highlight-guide

## Goals

- Improve readability and symbol differentiation across common languages
- Make semantic highlighting materially more complete
- Improve TextMate fallback behavior for languages without strong semantic support
- Preserve the Miasma palette and overall identity

## Plan

1. Audit the current baseline.
   Use VS Code Scope Inspector on representative files in TypeScript, JavaScript, JSON, Markdown, Python, Rust, Go, HTML/CSS, Shell, and diff views.
   Record which TextMate scopes and semantic token selectors are common, missing, or collapsing into the same color.

2. Expand semantic highlighting first.
   Add coverage for standard semantic token types and modifiers that the current theme does not handle well.
   Prioritize:
   - `namespace`, `struct`, `decorator`, `event`, `macro`, `label`
   - `comment`, `string`, `number`, `regexp`, `operator`
   - `*.declaration`, `*.definition`, `*.readonly`, `*.static`, `*.deprecated`, `*.abstract`, `*.async`, `*.modification`, `*.defaultLibrary`
   Add targeted combinations where they improve symbol clarity:
   - `variable.readonly`
   - `variable.readonly.defaultLibrary`
   - `property.readonly`
   - `function.defaultLibrary`
   - `class.defaultLibrary`
   - `method.async`
   - `type.defaultLibrary`

3. Strengthen TextMate scope coverage for fallback and non-semantic languages.
   Expand or tune `tokenColors` for common scopes so the theme remains strong even when semantic tokens are absent.
   Prioritize:
   - `comment`, `comment.documentation`
   - `string`, `string.regexp`, `constant.character.escape`
   - `constant.numeric`, `constant.language`, `constant.character`
   - `keyword`, `keyword.control`, `keyword.operator`
   - `storage`, `storage.type`, `storage.modifier`
   - `entity.name.function`, `entity.name.function.member`
   - `entity.name.type`, `entity.name.type.class`, `entity.name.type.interface`, `entity.name.namespace`
   - `variable.parameter`, `variable.other.property`, `variable.other.constant`, `variable.other.enummember`
   - `support.function`, `support.type`, `support.class`, `support.constant`
   - `meta.diff`, `markup.*`, and selective language-specific scopes when common scopes are insufficient

4. Align semantic and TextMate intent.
   Keep semantic token colors and matching TextMate scope colors consistent so semantic highlighting refines the theme instead of fighting it.
   Keep these categories visually distinct:
   - Types and namespaces
   - Functions and methods
   - Variables and properties
   - Readonly/constants/enum members
   - Keywords and operators
   - Strings, numbers, regex, and escapes

5. Preserve palette identity while improving role separation.
   Reuse the existing Miasma palette rather than inventing new accent colors.
   Increase contrast by assigning clearer semantic roles inside the existing palette.
   Use font styles sparingly and only where they add signal, such as documentation comments or deprecated symbols.

6. Add language-specific exceptions only after common scopes are exhausted.
   Prefer standard scopes first.
   Only add language-specific rules for real gaps such as:
   - TypeScript/JavaScript decorators and builtin globals
   - Rust macros and lifetimes
   - Python decorators and magic methods
   - Markdown fenced code punctuation and inline code
   - Git diff token scopes

7. Verify with semantic highlighting both on and off.
   Test with:
   - `editor.semanticHighlighting.enabled = configuredByTheme`
   - `editor.semanticHighlighting.enabled = true`
   - `editor.semanticHighlighting.enabled = false`
   Confirm the theme works well:
   - before semantic tokens arrive
   - after semantic tokens load
   - in languages with weak or absent semantic token providers

8. Package and manually review before release.
   Validate JSON, build the VSIX, install locally, and manually verify that:
   - the theme still loads cleanly
   - semantic tokens are active where expected
   - comments, strings, keywords, types, functions, and errors remain readable
   - diff and Markdown views still feel intentional

## Repo Work Order

1. Expand `semanticTokenColors` in `themes/base16-miasma-color-theme.json`
2. Refactor `tokenColors` to improve fallback coverage and align with semantic intent
3. Run manual Scope Inspector passes across target languages
4. Make a second tuning pass based on real scope collisions
5. Package with `vsce` and perform final manual verification

## Current Backlog

### Rust Remaining

- Add `meta.function.call.rust` as its own rule, not only `entity.name.function.rust`
- Add `meta.macro.rust support.function.rust` explicitly
- Add `entity.name.type.lifetime` alongside `entity.name.type.lifetime.rust`
- Split `storage.type.source.rust` from `meta.enum.rust storage.type.source.rust`
- Split `entity.name.trait.rust` from broader type scopes if they still read too similarly
- Add Rust semantic selectors where available from real inspector output:
  - `builtinAttribute.attribute.library:rust`
  - `generic.attribute:rust`

### Rust Follow-Up Testing

- Method calls
- Macros
- Traits
- Enums and enum members
- Module paths and `use` imports
- Locals vs parameters vs closure variables
- `self`
- Lifetimes
- Format string placeholders
- Attributes

### Other Language TextMate Opportunities

- TypeScript/JavaScript:
  - decorators
  - builtin globals and default-library functions
  - class properties vs locals
  - readonly fields and constants
  - JSX/TSX tag names vs component names
- Python:
  - decorators
  - builtin types and functions
  - magic methods
  - type hints
  - `self` and `cls`
- Go:
  - builtin types and default-library symbols
  - struct fields vs locals
  - interfaces vs concrete types
- Shell:
  - variables vs env vars
  - command names vs builtins
  - flags/options
  - heredoc markers
- JSON/TOML/YAML:
  - keys vs values
  - table and section headers
  - booleans, numbers, and null
- Markdown:
  - fenced language names
  - inline code vs fenced code
  - links vs headings vs blockquotes
- CSS/SCSS:
  - property names
  - class selectors
  - IDs
  - pseudo-selectors
  - variables/custom properties
- Diff/Git:
  - inserted/deleted/modified content
  - diff headers
  - inline emphasis in hunks

### Semantic Token Opportunities

- Add or tune more generic semantic selectors:
  - `operator`
  - `decorator`
  - `event`
  - `macro`
  - `label`
  - `comment`
  - `regexp`
- Add or tune more semantic modifier combinations:
  - `*.definition`
  - `*.static`
  - `*.abstract`
  - `*.async`
  - `*.modification`
- Add language-specific semantic overrides where useful:
  - JS/TS readonly and default-library symbols
  - Python decorators and type hints
  - Go default-library symbols
  - Rust attributes and other selectors surfaced by inspection

### Reference Sources

- Catppuccin VS Code Rust token rules
- Catppuccin VS Code semantic token rules
- VS Code semantic token scope map and standard token/modifier model
- Real Scope Inspector output from Miasma in representative files

## Constraint

If the goal expands from better theming to better tokenization accuracy, this extension would need to contribute `grammars` in `package.json` or ship a semantic token provider as part of a language extension. That is outside the scope of a theme-only pass.
