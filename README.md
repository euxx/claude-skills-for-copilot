# Claude Skills for Copilot

GitHub Copilot agent skills adapted from [Anthropic Claude plugins](https://github.com/anthropics/claude-plugins-official) and [OpenAI Codex Skills](https://github.com/openai/skills).

[![VS Code Marketplace](https://img.shields.io/badge/VS%20Code-Marketplace-blue?logo=visual-studio-code&logoColor=white)](https://marketplace.visualstudio.com/items?itemName=euxx.claude-skills-for-copilot)

## Skills

| Skill                    | Description                                                           | Source    |
| ------------------------ | --------------------------------------------------------------------- | --------- |
| `/code-review`           | Multi-agent code review: bugs, security, conventions-file violations  | Anthropic |
| `/code-simplifier`       | Simplify and refine code for clarity and maintainability              | Anthropic |
| `/comment-analyzer`      | Detect comment rot and misleading documentation                       | Anthropic |
| `/conventions-improver`  | Audit and improve AGENTS.md/CLAUDE.md/GEMINI.md quality               | Anthropic |
| `/feature-dev`           | Guided feature development with architecture design and review        | Anthropic |
| `/frontend-design`       | Create production-grade frontend interfaces with high design quality  | Anthropic |
| `/frontend-layout`       | Art-direction-led composition for landing pages and app UI            | OpenAI    |
| `/gh-address-comments`   | Fetch PR review comments and address the selected ones via gh CLI     | OpenAI    |
| `/gh-fix-ci`             | Debug failing GitHub Actions CI checks and fix after approval         | OpenAI    |
| `/mcp-server-dev`        | Guide building MCP servers: deployment model, tool patterns, scaffold | Anthropic |
| `/review-toolkit`        | Orchestrated end-to-end review (up to 6 specialist sub-skills)        | Anthropic |
| `/security-threat-model` | AppSec-grade threat model: trust boundaries, abuse paths, mitigations | OpenAI    |
| `/silent-failure-hunter` | Find silent failures, swallowed errors, and unjustified fallbacks     | Anthropic |
| `/test-analyzer`         | Behavioral test coverage review — gaps, not just line coverage        | Anthropic |
| `/type-design-analyzer`  | Type design quality: invariants, encapsulation, enforcement           | Anthropic |

## Usage

**Slash command** — type `/` to see available skills, then select or type the skill name (e.g., `/code-review`).

**Auto-invocation** — in agent mode, Copilot automatically uses skills relevant to your task.

**By instruction** — explicitly tell the agent in natural language (e.g., "Please review code changes by code review skill").

## Adaptation Details

Skills are adapted from two open-source repositories, both rewritten for VS Code Copilot agent skills format.

### From Anthropic Claude Plugins

Source: [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official) (Apache 2.0). The original plugins target Claude Code (CLI).

#### Common changes

Every skill from this source went through the following transformations:

| Change            | Original (Claude Code)                                             | Adapted (VS Code Copilot)                                                                             |
| ----------------- | ------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------- |
| File format       | `agent`/`command` YAML frontmatter with `model:`, `allowed-tools:` | `SKILL.md` with `name`, `description`, `argument-hint`, `user-invocable`                              |
| Tooling           | `Bash(gh pr view …)`, `TodoWrite`, `$ARGUMENTS`                    | Generic VS Code Copilot tool references                                                               |
| Description field | Long XML `<example>` chat transcripts embedded in frontmatter      | Single-sentence description                                                                           |
| Convention file   | `CLAUDE.md`                                                        | `AGENTS.md` or similar (`CLAUDE.md`, `GEMINI.md`) with priority `AGENTS.md -> CLAUDE.md -> GEMINI.md` |
| Model names       | `claude-haiku-4-5`, `claude-sonnet-4-5`, `claude-opus-4-5`         | Generic sub-agent references                                                                          |

#### `/code-review`

**Source:** [`plugins/code-review/commands/code-review.md`](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/code-review/commands/code-review.md)

The original is a PR-focused command that reads the diff via `gh pr diff` and spawns multiple Haiku sub-agents to review different aspects. It is tightly coupled to GitHub workflow — it requires an open PR and the `gh` CLI.

**Changes:**

- Removed all `gh` CLI usage; the skill now works on any file, directory, diff, or codebase passed as an argument
- Replaced named Anthropic model invocations with generic sub-agent calls
- Replaced single-file CLAUDE.md convention lookup with AGENTS.md or similar conventions files (`CLAUDE.md`, `GEMINI.md`) in priority order: `AGENTS.md -> CLAUDE.md -> GEMINI.md`

---

#### `/code-simplifier`

**Source:** [`plugins/code-simplifier/agents/code-simplifier.md`](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/code-simplifier/agents/code-simplifier.md)

The original agent is scoped to "recently modified files" and references Anthropic-internal project conventions (ES modules, arrow functions, specific test patterns). The `model: claude-opus-4-5` instruction makes it a high-quality but heavyweight agent.

**Changes:**

- Removed hardcoded project-specific coding standards from the prompt body
- Broadened scope from "recently modified code" to any file, diff, or codebase segment specified by the user
- Converted from agent (with `model:`) to a skill

---

#### `/comment-analyzer`

**Source:** [`plugins/pr-review-toolkit/agents/comment-analyzer.md`](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/pr-review-toolkit/agents/comment-analyzer.md)

The original agent embeds lengthy XML `<example>` transcripts inside the `description` frontmatter field to illustrate expected output. The body mixes free-form instruction with example scenarios.

**Changes:**

- Stripped all XML examples from the frontmatter description
- Restructured the body as a numbered analysis framework (accuracy verification → completeness assessment → long-term value evaluation → misleading element identification)
- Added `argument-hint` for user-facing guidance

---

#### `/conventions-improver`

**Source:** [`plugins/claude-md-management/skills/claude-md-improver/SKILL.md`](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/claude-md-management/skills/claude-md-improver/SKILL.md)

The original skill audits and improves `CLAUDE.md` files in Claude Code projects. It references Claude Code–specific file types (`.claude.local.md`, `~/.claude/CLAUDE.md`), the `#` key shortcut for incorporating learnings, and uses tool declarations (`Read`, `Glob`, `Grep`, `Bash`, `Edit`). Quality criteria and templates are stored in separate `references/` files.

**Changes:**

- Renamed from `claude-md-improver` to `conventions-improver` to reflect the broader scope
- Generalised from `CLAUDE.md`-only to support `AGENTS.md`, `CLAUDE.md`, and `GEMINI.md` with priority order `AGENTS.md -> CLAUDE.md -> GEMINI.md`
- Removed Claude Code–specific file types (`.claude.local.md`, global `~/.claude/CLAUDE.md`, the `#` shortcut tip)
- Removed tool declarations from YAML frontmatter
- Embedded quality-criteria and templates content inline (single self-contained `SKILL.md` rather than split across `references/` files)

---

#### `/feature-dev`

**Source:** [`plugins/feature-dev/commands/feature-dev.md`](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/feature-dev/commands/feature-dev.md)

The original command uses the Claude Code–specific `TodoWrite` built-in tool to manage a persistent task list, and receives the feature description via the `$ARGUMENTS` shell variable.

**Changes:**

- Replaced `TodoWrite` invocations with a generic "maintain a todo list" instruction
- Replaced `$ARGUMENTS` with a natural-language argument hint in the frontmatter
- Converted from command to skill format

---

#### `/frontend-design`

**Source:** [`plugins/frontend-design/skills/frontend-design/SKILL.md`](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/frontend-design/skills/frontend-design/SKILL.md)

This is the closest to a direct port — the source is already a `SKILL.md` intended for the Claude Code skill system, which shares a similar format to VS Code Copilot skills.

**Changes:**

- Removed the `license: Complete terms in LICENSE.txt` frontmatter field (not used by VS Code Copilot)
- Added `argument-hint` and `user-invocable: true` frontmatter fields
- Minor formatting normalisation (bullet indentation)

---

#### `/review-toolkit`

**Sources:**

- [`plugins/pr-review-toolkit/commands/review-pr.md`](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/pr-review-toolkit/commands/review-pr.md) — orchestrator command
- [`plugins/pr-review-toolkit/agents/`](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/pr-review-toolkit/agents) — 6 specialist agent files (code-reviewer, silent-failure-hunter, comment-analyzer, pr-test-analyzer, code-simplifier, type-design-analyzer)

The original is a PR review orchestrator: it reads a PR with `gh pr view`, then spawns the 6 specialist agents by filename. Each agent file is standalone and referenced by path.

**Changes:**

- Completely rewritten: dispatches the other 5 Copilot skills (not agent files) by skill name
- Removed all PR/GitHub context; works on any codebase, directory, or diff
- Replaced `gh pr view` with instructions to infer scope from argument or current workspace
- Added a summary table format for the final aggregated report

---

#### `/silent-failure-hunter`

**Source:** [`plugins/pr-review-toolkit/agents/silent-failure-hunter.md`](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/pr-review-toolkit/agents/silent-failure-hunter.md)

The original agent has an extensive `description` field with XML `<example>` blocks showing sample review dialogues. The body defines detection patterns for swallowed errors and missing fallbacks.

**Changes:**

- Stripped XML examples from the frontmatter description
- Restructured the body as a numbered review process (identify error handling → scrutinize each handler → check for hidden failure patterns)
- Added `argument-hint` for user-facing guidance

---

#### `/test-analyzer`

**Source:** [`plugins/pr-review-toolkit/agents/pr-test-analyzer.md`](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/pr-review-toolkit/agents/pr-test-analyzer.md)

The original is named `pr-test-analyzer` and is framed around reviewing tests in the context of a pull request diff. It focuses on whether new tests were added for changed code.

**Changes:**

- Renamed from `pr-test-analyzer` to `test-analyzer` to reflect the broader, non-PR scope
- Removed all PR framing; the skill now reviews any test file, directory, or codebase
- Added DAMP (Descriptive And Meaningful Phrases) principles as an explicit evaluation criterion
- Added `argument-hint` for user-facing guidance

---

#### `/type-design-analyzer`

**Source:** [`plugins/pr-review-toolkit/agents/type-design-analyzer.md`](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/pr-review-toolkit/agents/type-design-analyzer.md)

The original agent has an informal instruction style and uses XML `<example>` transcripts in its `description` to illustrate expected output quality.

**Changes:**

- Replaced verbose description with a single-sentence summary
- Reformatted the analysis framework as numbered steps with a 4-dimension rating rubric (invariants, encapsulation, enforcement, expressiveness; each scored 1–10)
- Added `argument-hint` for user-facing guidance

---

#### `/mcp-server-dev`

**Source:** [`plugins/mcp-server-dev/skills/build-mcp-server/SKILL.md`](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/mcp-server-dev/skills/build-mcp-server/SKILL.md)

The original skill is structured around Claude Code's tool ecosystem and references six companion `references/` files (remote-http-scaffold, deploy-cloudflare-workers, tool-design, auth, elicitation, server-capabilities). It also hands off to two peer skills (`build-mcp-app`, `build-mcpb`) that exist in the same plugin but are not yet adapted.

**Changes:**

- Renamed from `build-mcp-server` to `mcp-server-dev` to match the parent plugin name and our naming convention
- Removed `version:` frontmatter field (not used by VS Code Copilot)
- Added `argument-hint` and `user-invocable: true` frontmatter fields
- Removed all `references/xxx.md` internal file pointers; Phase 5 now includes inline scaffolding guidance for each deployment path, and replaced the References section with links to public MCP SDK repositories and the MCP specification
- Removed hand-off instructions for `build-mcp-app` and `build-mcpb` (those sub-skills don't exist in this repo); replaced with inline guidance to continue implementing the chosen approach

---

### From OpenAI Codex Skills

Source: [openai/skills](https://github.com/openai/skills) (Apache 2.0 per-skill). The original skills target the OpenAI Codex agent.

#### Common changes

| Change            | Original (OpenAI Codex)                                                                      | Adapted (VS Code Copilot)                                               |
| ----------------- | -------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| File format       | Separate `agents/openai.yaml` for UI config (display name, icons, prompt)                    | Removed; `argument-hint` and `user-invocable: true` added to `SKILL.md` |
| Bundled scripts   | Python `scripts/` helpers called by the workflow (gh-fix-ci, gh-address-comments)            | Replaced with inline `gh` CLI commands                                  |
| Reference files   | Companion `references/` files carrying output contracts and guidance (security-threat-model) | Inlined into `SKILL.md`                                                 |
| Platform-specific | `create-plan` skill, `sandbox_permissions=require_escalated` auth commands                   | Removed                                                                 |

#### `/gh-address-comments`

**Source:** [`skills/.curated/gh-address-comments/SKILL.md`](https://github.com/openai/skills/blob/main/skills/.curated/gh-address-comments/SKILL.md)

The original skill uses a bundled Python script (`scripts/fetch_comments.py`) to fetch PR comments via the GitHub GraphQL API. It also refers to OpenAI Codex–specific auth sandbox commands (`sandbox_permissions=require_escalated`).

**Changes:**

- Replaced the Python script call with an inline `gh api graphql` command block that fetches conversation comments, reviews, and review threads directly
- Added a REST fallback (`gh pr view --json reviews,comments`) for environments where GraphQL is unavailable
- Removed Codex sandbox-specific auth instructions
- Added `argument-hint` and `user-invocable: true` frontmatter fields

---

#### `/gh-fix-ci`

**Source:** [`skills/.curated/gh-fix-ci/SKILL.md`](https://github.com/openai/skills/blob/main/skills/.curated/gh-fix-ci/SKILL.md)

The original skill orchestrates a Python script (`scripts/inspect_pr_checks.py`, 509 lines) to fetch failing check logs. It also references the Codex-specific `create-plan` skill for the approval step.

**Changes:**

- Replaced the Python script call with the manual `gh` CLI commands already present in the original as a fallback (the script is a convenience wrapper around the same `gh run view` and `gh api` calls)
- Replaced `create-plan` skill invocation with an inline plan-and-approval step
- Removed Codex sandbox-specific auth instructions
- Added `argument-hint` and `user-invocable: true` frontmatter fields

---

#### `/security-threat-model`

**Source:** [`skills/.curated/security-threat-model/SKILL.md`](https://github.com/openai/skills/blob/main/skills/.curated/security-threat-model/SKILL.md)

The original skill references two companion files (`references/prompt-template.md` and `references/security-controls-and-assets.md`) that carry the output contract and quality rules.

**Changes:**

- Inlined the output format contract and evidence-grounding rules from `prompt-template.md` directly into `SKILL.md` (VS Code skills support only a single `SKILL.md` file)
- Inlined the Mermaid diagram rules and required output section list
- Removed references to the separate `references/` directory
- Added `argument-hint` and `user-invocable: true` frontmatter fields

---

#### `/frontend-layout`

**Source:** [`skills/.curated/frontend-skill/SKILL.md`](https://github.com/openai/skills/blob/main/skills/.curated/frontend-skill/SKILL.md)

The original skill is already in `SKILL.md` format and focuses on art direction, composition restraint, and image-led hierarchy — complementing the existing `/frontend-design` skill which emphasises bold aesthetic direction.

**Changes:**

- Renamed from `frontend-skill` to `frontend-layout` to clarify the complementary scope alongside the existing `frontend-design` skill
- Added `argument-hint` and `user-invocable: true` frontmatter fields
- Minor formatting normalisation (table alignment, bullet indentation)

## You may also like

- <img src="https://github.com/euxx/github-copilot-usage/raw/main/images/icon.png" width="20" align="absmiddle"> [**GitHub Copilot Usage**](https://marketplace.visualstudio.com/items?itemName=euxx.github-copilot-usage) - Shows Copilot Premium request quota usage in the VS Code status bar
- <img src="https://github.com/euxx/editor-tweaks/raw/main/images/icon.png" width="20" align="absmiddle"> [**Editor Tweaks**](https://marketplace.visualstudio.com/items?itemName=euxx.editor-tweaks) - A collection of small VS Code editor utilities packed into a single extension

## License

This project is licensed under [MIT](LICENSE).

Portions of the skill definitions are adapted from [claude-plugins-official](https://github.com/anthropics/claude-plugins-official) by Anthropic and from [openai/skills](https://github.com/openai/skills) by OpenAI, both licensed under the [Apache License 2.0](LICENSE-APACHE). Changes were made to adapt the content to VS Code Copilot agent skills format.
