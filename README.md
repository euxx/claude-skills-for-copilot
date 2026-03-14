# Claude Skills for Copilot

GitHub Copilot agent skills adapted from the [official Claude plugins](https://github.com/anthropics/claude-plugins-official).

[![VS Code Marketplace](https://img.shields.io/badge/VS%20Code-Marketplace-blue?logo=visual-studio-code&logoColor=white)](https://marketplace.visualstudio.com/items?itemName=euxx.claude-skills-for-copilot)

## Skills

| Skill                    | Description                                                          |
| ------------------------ | -------------------------------------------------------------------- |
| `/code-review`           | Multi-agent code review: bugs, security, AGENTS.md violations        |
| `/code-simplifier`       | Simplify and refine code for clarity and maintainability             |
| `/comment-analyzer`      | Detect comment rot and misleading documentation                      |
| `/feature-dev`           | Guided feature development with architecture design and review       |
| `/frontend-design`       | Create production-grade frontend interfaces with high design quality |
| `/review-toolkit`        | Orchestrated end-to-end review (up to 6 specialist sub-skills)       |
| `/silent-failure-hunter` | Find silent failures, swallowed errors, and unjustified fallbacks    |
| `/test-analyzer`         | Behavioral test coverage review — gaps, not just line coverage       |
| `/type-design-analyzer`  | Type design quality: invariants, encapsulation, enforcement          |

## Usage

**Slash command** — type `/` to see available skills, then select or type the skill name (e.g., `/code-review`).

**Auto-invocation** — in agent mode, Copilot automatically uses skills relevant to your task.

## Adaptation Details

All skills are adapted from [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official) (Apache 2.0). The source plugins target Claude Code (CLI); these versions are rewritten for VS Code Copilot agent skills format.

### Common changes

Every skill went through the following transformations:

| Change            | Original (Claude Code)                                             | Adapted (VS Code Copilot)                                                |
| ----------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------ |
| File format       | `agent`/`command` YAML frontmatter with `model:`, `allowed-tools:` | `SKILL.md` with `name`, `description`, `argument-hint`, `user-invocable` |
| Tooling           | `Bash(gh pr view …)`, `TodoWrite`, `$ARGUMENTS`                    | Generic VS Code Copilot tool references                                  |
| Description field | Long XML `<example>` chat transcripts embedded in frontmatter      | Single-sentence description                                              |
| Convention file   | `CLAUDE.md`                                                        | `AGENTS.md`                                                              |
| Model names       | `claude-haiku-4-5`, `claude-sonnet-4-5`, `claude-opus-4-5`         | Generic sub-agent references                                             |

### Per-skill details

#### `/code-review`

**Source:** [`plugins/code-review/commands/code-review.md`](https://github.com/anthropics/claude-plugins-official/blob/main/plugins/code-review/commands/code-review.md)

The original is a PR-focused command that reads the diff via `gh pr diff` and spawns multiple Haiku sub-agents to review different aspects. It is tightly coupled to GitHub workflow — it requires an open PR and the `gh` CLI.

**Changes:**

- Removed all `gh` CLI usage; the skill now works on any file, directory, diff, or codebase passed as an argument
- Replaced named Anthropic model invocations with generic sub-agent calls
- Replaced CLAUDE.md with AGENTS.md as the project conventions file

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
- Restructured the body as a numbered analysis framework (comment types → accuracy check → completeness check → maintainability check → scoring)
- Added `argument-hint` for user-facing guidance

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
- Restructured the body as a numbered review process with explicit confidence threshold (report only if confidence ≥ 80)
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

## License

This project is licensed under [MIT](LICENSE).

Portions of the skill definitions are adapted from [claude-plugins-official](https://github.com/anthropics/claude-plugins-official) by Anthropic, which is licensed under the [Apache License 2.0](LICENSE-APACHE). Changes were made to adapt the content from Claude Code agent skills to VS Code Copilot agent skills format.
