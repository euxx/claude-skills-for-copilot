# Changelog

All notable changes to this project will be documented in this file.

## [Unreleased]

## [0.0.7] - 2026-03-16

### Changed

- Expanded conventions file support across skills to `AGENTS.md`, `CLAUDE.md`, and `GEMINI.md` with priority `AGENTS.md -> CLAUDE.md -> GEMINI.md`
- Updated review and simplification skill guidance from AGENTS-only checks to `AGENTS.md` or similar conventions files
- Updated README usage with a direct-invocation example for explicitly selecting a skill

## [0.0.6] - 2026-03-14

### Added

- Added GitHub Actions workflows for CI and automated release

### Changed

- Standardized icon spec
- Synced config and docs from template

## [0.0.5] - 2026-03-14

### Changed

- Updated README: added adaptation details with per-skill breakdowns

## [0.0.4] - 2026-03-14

### Added

- Added `LICENSE-APACHE` (Apache 2.0 license text) for adapted skill content
- Added attribution notice in README for upstream source (anthropics/claude-plugins-official)

## [0.0.3] - 2026-03-14

### Changed

- Added `npm run package` to the Scripts table in DEVELOPMENT.md
- Updated package description

## [0.0.2] - 2026-03-14

### Changed

- Improved README: clarified description, expanded Usage section, added License
- Updated package description

## [0.0.1] - 2026-03-13

### Added

- Initial release with 9 agent skills adapted from [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official):
  - `code-review`
  - `code-simplifier`
  - `comment-analyzer`
  - `feature-dev`
  - `frontend-design`
  - `review-toolkit`
  - `silent-failure-hunter`
  - `test-analyzer`
  - `type-design-analyzer`
