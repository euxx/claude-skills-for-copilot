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

## License

This project is licensed under [MIT](LICENSE).

Portions of the skill definitions are adapted from [claude-plugins-official](https://github.com/anthropics/claude-plugins-official) by Anthropic, which is licensed under the [Apache License 2.0](LICENSE-APACHE). Changes were made to adapt the content from Claude Code agent skills to VS Code Copilot agent skills format.
