# Changelog

All notable changes to this project will be documented in this file.

## [Unreleased]

## [0.3.0] - 2026-03-17

### Added

- Added `/security-threat-model` skill — AppSec-grade threat modeling: trust boundaries, abuse paths, mitigations, and a Markdown threat model with Mermaid diagram (adapted from OpenAI Codex Skills)

### Changed

- Updated GitHub Actions CI and release workflows, including support for overwriting an existing release when rerunning the release workflow

## [0.2.0] - 2026-03-17

### Added

- Added `/gh-fix-ci` skill — inspects failing GitHub Actions CI on a PR, fetches logs, summarizes root cause, and proposes a fix with user approval (adapted from OpenAI Codex Skills)
- Added `/gh-address-comments` skill — fetches PR review comments via GraphQL, presents a numbered list, and applies selected fixes (adapted from OpenAI Codex Skills)
- Updated README with Source column and separate Adaptation Details sections for Anthropic and OpenAI skills
- Documented skill registration process in DEVELOPMENT.md

### Changed

- Registered all 12 skills in `package.json` `contributes.chatSkills` (conventions-improver, gh-address-comments, gh-fix-ci were previously missing)

## [0.1.0] - 2026-03-16

### Added

- Added `/conventions-improver` skill — audits and improves project conventions files (`AGENTS.md`, `CLAUDE.md`, `GEMINI.md`) with quality scoring (6 criteria, 100-point rubric) and targeted update suggestions

### Changed

- Simplified wording in README usage instructions

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
